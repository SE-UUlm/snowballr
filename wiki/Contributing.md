On this page, we explain how to contribute to the SnowballR project. We cover the following topics:

- [Contribution Workflow \& Conventions](#contribution-workflow--conventions)
  - [Workflow](#workflow)
  - [Commits \& Branches](#commits--branches)
- [Deployment](#deployment)
  - [Service Overview](#service-overview)
  - [Routing](#routing)
- [Versioning Guideline](#versioning-guideline)
- [Teamscale Integration](#teamscale-integration)

## Contribution Workflow & Conventions

### Workflow

Starting from an issue, we create a branch with the name of the issue (see [Commits & Branches](#commits--branches)).
It's up to you, whether you create a draft pull request immediately or wait until you are finished with the
implementation. While creating a draft pull request gives you direct feedback from the CI/CD pipeline, it also clutters
the pull request list. So it's up to you whether you want to create a draft pull request or not.

When starting to work on an issue, ensure that the issue is assigned to you and part of our project SnowballR.
Furthermore, make sure you set the status to `In progress` and the iteration to the current one (if that is not already
done). **Prefer** to work on issues that are already assigned to you and part of the current iteration/sprint.

When you are finished with the implementation, create a pull request (when not already done) and fill out the template.
If other branches were merged into `develop` while you were working on the issue, make sure to rebase your branch onto
the `develop` branch (`git rebase origin/develop`) and resolve any conflicts. Make sure that you don't rebase your
branch after you requested a review, as we experienced that the comments are hard to find afterward. Continue with
setting the status of the issue to `To review`. One other team member will then assign themselves as reviewer and set
the status to `In review`.

The reviewing process works as follows:

1. The reviewer will check the code and provide feedback. This can be done by adding comments to the pull request,
   preferably annotating the code directly. The reviewer can also approve the pull request if everything is fine.
2. If the reviewer requests changes, the author of the pull request (you) will either implement the changes or
   provide a reason why the changes are not necessary. In either case, the author should respond to all comments. The
   author should never resolve any comments themselves as this is the responsibility of the reviewer.
3. Once the reviewer is satisfied with the changes, they will approve the pull request. You can then merge the pull
   request into the `develop` branch. Make sure to use merge commits and not squash or rebase.
4. If there were updates to the `develop` branch while the pull request was in review, you will need to rebase your
   branch onto the `develop` branch again and resolve any conflicts. Make sure this is discussed with the reviewer.
5. After merging the pull request, the issue is automatically closed and the status is set to `Done`.

### Commits & Branches

For commits, we follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) specification. The
commits are automatically checked by the [`Semantic PRs`](https://github.com/Ezard/semantic-prs) GitHub App when
creating a pull request.

A branch name should be `<prefix>/<issue-number>-<short-description>`, e.g. `fix/1234-fix-bug-in-component`. `prefix`
signals the type of the issue. For that we use the type of
[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) that best fits the issues. For instance, if the
issue is a bug, we use `fix/`, if it is a feature, we use `feat/`, etc. **Prefer** using the GitHub functionality to
create branches from an issue as it already provides `<issue-number>-<short-description>` and you only have to add the
`prefix/` part.

## Deployment

This project uses [Docker](https://www.docker.com/) for local and production deployments. All services are orchestrated
via [Docker Compose](https://docs.docker.com/compose/), with [Caddy](https://caddyserver.com/) serving as a reverse
proxy to route HTTP traffic to the appropriate service.

Currently, deployments are performed nightly, using the latest version of the API and the current state of the `develop`
branch of the [frontend](https://github.com/SE-UUlm/snowballr-frontend/tree/develop). In the future, the versions should
be pinned to and aligned with releases of the system parts to ensure stability and reproducibility.

### Service Overview

| Service        | Description                | Port                     |
|----------------|----------------------------|--------------------------|
| `frontend`     | Svelte-based GUI           | `8000`                   |
| `mock-backend` | Mock backend (for testing) | `8002`                   |
| `api-docs`     | Sabledocs for gRPC docs    | `8000` with path `/docs` |

### Routing

Caddy handles incoming HTTP(S) traffic and routes it based on the request path with the host
[snowballr.informatik.uni-ulm.de](https://snowballr.informatik.uni-ulm.de/).

- `/docs/*` → forwards to api-docs
- `/api/*` → forwards to mock-backend
- All other paths → served by the frontend service

## Versioning Guideline

For the versioning we follow [Semantic Versioning](https://semver.org/).
When a new version should be published, document the changes in a changelog (following the guidelines of
[Common Changelog](https://common-changelog.org/)), tag the code in Git and a release will be automatically created and
published.

>**Note:** At the moment only the [API](https://github.com/SE-UUlm/snowballr-api) is correctly versioned.

## Teamscale Integration

We use [Teamscale](https://teamscale.com/) for analyzing, monitoring and improving the quality of
our project. To set up the integration with your IDE follow the instructions online:

- [IntelliJ IDEA](https://docs.teamscale.com/howto/integrating-with-your-ide/intellij/)
- [VS Code](https://docs.teamscale.com/howto/integrating-with-your-ide/visual-studio-code/)

Note that the configuration file was already added, and you only have to connect the plugin to the server.
