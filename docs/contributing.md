# Contributing to BAR Infrastructure

Welcome! We're a volunteer-driven project, and we're excited you're here to help. This page will guide you on how to get started with the Beyond All Reason infrastructure.

> [!TIP]
> This documentation is a new and evolving resource. If you find anything unclear or have unanswered questions, we welcome your feedback! Please [open an issue](https://github.com/beyond-all-reason/infrastructure/issues) to share your thoughts. Contributing to the docs is also a fantastic way to make your first contribution.

Our core philosophy is applying software engineering to operations. We try to build robust solutions, and that often means changing code or building new tools, not just managing servers.

## Communication

- **Discord:** For general questions and discussions, join us in the development channels on the [BAR Discord server](https://discord.gg/beyond-all-reason). We have dedicated channels for different parts of the infrastructure, such as `#new-client` and `#teiserver-spads`. If no existing channel fits just post in `#dev-main`.
- **GitHub:** For tracking work and having more permanent discussions, we use GitHub Issues.

At the moment the main point of contact for infrastructure is **Marek** ([p2004a](https://github.com/p2004a)).

## Getting Started

### 1. Understand the Big Picture

Before diving in, take some time to browse this documentation site to understand how the system works. We recommend starting with the [Home](index.md) page for a high-level overview, and then moving on to the [Runtime Infrastructure](current_infra.md) page for more details.

### 2. Find Something to Work On

A great way to start is by tackling an existing issue. Check the issue trackers in the repositories listed in the [Component Index](components.md) that interest you.

Look for issues tagged with `good first issue`, as these are well-suited for new contributors. While not all repositories may have them, it's a good starting point.

Some larger cross-system issues/projects are tracked in the [`infrastructure`](https://github.com/beyond-all-reason/infrastructure/issues) repository.

#### Can't Find Anything?

If you've looked for an issue but are still unsure where to start, feel free to ask. To help us point you in the right direction, please tell us a bit about your interests and what parts of the project you've already looked at.

### 3. Taking Initiative

Many potential improvements aren't tracked in an issue yet. If you have an idea or find something to improve, please bring it up for discussion! We value proactive contributions.

> [!EXAMPLE]
> To give you a sense of the possibilities, here are a couple of _examples_ of areas that just aren't scoped as issues at the moment:
>
> - Finishing the [Recoil Engine docker build](https://raw.githubusercontent.com/beyond-all-reason/RecoilEngine/refs/heads/master/docker-build-v2/README.md) (e.g., adding Windows testing or Podman support).
> - Reducing the complexity of the game's infrastructure build process, which has some quirks for historical reasons.
>
> As you learn more about the project and how things are connected together issues like this will pop up to you very quickly.

## The Contribution Process

1.  **Find or create an issue.** This helps track the work.
2.  **Check for a `CONTRIBUTING.md` file.** It might contain specific guidelines relevant to that repository.
3.  **For larger changes, agree on a plan first.** This ensures we're all on the same page and you don't waste your time.
4.  **Fork the repo, create a branch, write your code, submit a pull request.** Following the standard [GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow) process.

We look forward to your contributions!
