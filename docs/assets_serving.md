# Asset serving

> [!WARNING]
> This section is unfinished. The [Rapid](#rapid) section is complete; the other
> diagrams are up to date, but do not yet have textual explanations.

### Legend

![Assets Serving Legend](diagrams/assets_serving_legend.d2){pad=20 scale=0.6}

### Overview

![Assets Serving](diagrams/assets_serving.d2){pad=10}

## Files

![Files](diagrams/files_serving.d2)

## Rapid

[Rapid](https://springrts.com/wiki/Rapid) is the format and protocol that the
client uses to download game versions. A repo contains content-addressed files,
packages that list the files in each version, and a `versions.gz` file that maps
tags such as `byar:test` to those packages. Clients ([pr-downloader]) start
with the `repos.gz` master list of repos and then fetch only the files they do
not already have.

![Rapid](diagrams/rapid_serving.d2)

### Serving

Players download repos from <https://repos-cdn.beyondallreason.dev/>, a
[Bunny](https://bunny.net/) pull zone backed by a Bunny storage zone. The
builder writes `<repo>/pool`, `<repo>/packages`, and `<repo>/versions.gz` to the
storage zone. Players communicate only with the CDN. The `repos.gz` master list
is maintained manually in the storage zone, not by the builder, so it can list
repos that are no longer built.

Replication from the storage zone to the edge regions lags behind writes.
Immediately after a build, the CDN can therefore serve a stale `versions.gz`,
causing clients to fetch a version that is no longer current. To avoid this,
each build uploads the same file under a unique name,
`<repo>/fresh/versions_<stamp>.gz`, because new files are visible in edge
locations immediately. It waits until the CDN serves that copy and then updates
an [edge rule](https://bunny.net/docs/cdn/edge-rules) to redirect
`<repo>/versions.gz` to it. Edge rule changes propagate globally within a
minute.

### Building

Builds run on a single server managed by the [rapid-hosting] Ansible playbook.
The server also hosts one of the [SPADS] instances, and all services run as
Podman quadlets. Two services are involved:

- **Caddy** terminates TLS, serves the builder's store as the origin for the
  repos at <https://repos.beyondallreason.dev/>, and reverse-proxies `/build` to
  the builder without buffering as required by the [build
  API][rapid-builder-api].
- **[rapid-builder]** is an HTTP service that builds and publishes a commit on
  request: it runs `rapid-buildgit` from [RapidTools] and uploads the result to
  Bunny. It keeps a Git clone and a Rapid store for each repo, so a build
  processes only changed content. See its [README][rapid-builder] for a single
  build step by step.

A GitHub Actions workflow in the game repository triggers a build by calling the
[composite action][rapid-build-action] from the same repo. The action sends the
workflow's OIDC token to `/build`, so GitHub and the server do not share a
secret. What a repo may publish is decided by a [CEL](https://cel.dev) policy
defined in the playbook's group variables, see [the authorization
docs][rapid-builder-authorization]. Currently, a push to `stable` in
[Beyond-All-Reason] publishes `byar:test`; a manual run on `master` publishes
`byar:pr-<number>` or `byar:br-<name>`; and a push to `master` in [BYAR-Chobby]
publishes `byar-chobby:test`.

The builder sends its metrics and logs to the [monitoring
server][ansible-monitoring].

[pr-downloader]: https://github.com/beyond-all-reason/pr-downloader
[rapid-hosting]: https://github.com/beyond-all-reason/rapid-hosting
[rapid-builder]: https://github.com/beyond-all-reason/rapid-hosting/tree/main/rapid-builder
[rapid-build-action]: https://github.com/beyond-all-reason/rapid-hosting/tree/main/action
[rapid-builder-api]: https://github.com/beyond-all-reason/rapid-hosting/blob/main/rapid-builder/docs/api.md
[rapid-builder-authorization]: https://github.com/beyond-all-reason/rapid-hosting/blob/main/rapid-builder/docs/authorization.md
[RapidTools]: https://github.com/beyond-all-reason/RapidTools
[SPADS]: https://github.com/beyond-all-reason/ansible-spads-setup
[ansible-monitoring]: https://github.com/beyond-all-reason/ansible-monitoring
[Beyond-All-Reason]: https://github.com/beyond-all-reason/Beyond-All-Reason
[BYAR-Chobby]: https://github.com/beyond-all-reason/BYAR-Chobby

## Rowy

![Rowy](diagrams/rowy.d2)
