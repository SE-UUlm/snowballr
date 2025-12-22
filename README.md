<div align="center">
    <picture>
        <img alt="SnowballR Logo" src="images/logo-with-text/snowballr-logo-with-text_512x1920.png" width="700"/>
    </picture>
</div>

<div align="center">
    <a href="https://github.com/SE-UUlm/snowballr/releases/latest">
        <img alt="Version" src="https://img.shields.io/github/v/release/SE-UUlm/snowballr?label=Version&color=light-green">
    </a>
    <a href="https://github.com/SE-UUlm/snowballr/actions/workflows/deploy-prod.yml">
        <img alt="Production Deployment Status" src="https://img.shields.io/github/actions/workflow/status/SE-UUlm/snowballr/deploy-prod.yml?logo=github&label=Production%20Deployment">
    </a>
    <a href="https://github.com/SE-UUlm/snowballr/actions/workflows/deploy-dev.yml">
        <img alt="Development Deployment Status" src="https://img.shields.io/github/actions/workflow/status/SE-UUlm/snowballr/deploy-dev.yml?logo=github&label=Development%20Deployment">
    </a>
    <a href="https://snowballr.informatik.uni-ulm.de">
        <img alt="Production Website" src="https://custom-icon-badges.demolab.com/badge/Production_Website-gray?logo=snowballr">
    </a>
    <a href="https://snowballr-dev.informatik.uni-ulm.de">
        <img alt="Development Website" src="https://custom-icon-badges.demolab.com/badge/Development_Website-gray?logo=snowballr">
    </a>
    <a href="https://github.com/SE-UUlm/snowballr/wiki">
        <img alt="GitHub Wiki" src="https://img.shields.io/badge/Wiki-grey?logo=github">
    </a>
    <a href="https://deepwiki.com/SE-UUlm/snowballr">
        <img alt="Ask DeepWiki" src="https://deepwiki.com/badge.svg">
    </a>
    <a href="https://github.com/SE-UUlm/snowballr/blob/main/LICENSE">
        <img alt="License" src="https://img.shields.io/github/license/SE-UUlm/snowballr?label=License">
    </a>
</div>

# SnowballR

Welcome to the **SnowballR** project! This page will give you a quick overview over the project.

**SnowballR** is a web-based tool supporting Systematic Literature Reviews (SLR). The goal of **SnowballR** is to
automate reference retrieval, facilitate transparent inclusion/exclusion decisions, and maintain a clear audit trail
of the review process.

Users can create dedicated **projects** and invite other collaborators to participate. Each project follows a structured
paper selection process based on the [snowballing](https://en.wikipedia.org/wiki/Snowball_sampling) methodology, which
helps to identify relevant literature through citation networks.

Papers within a project are processed through multiple **review stages**. In each stage:

- New papers are discovered and added.
- Multiple reviewers assess each paper independently.
- Papers are **accepted** or **declined** based on consensus.
- Accepted papers proceed to the next stage.

This iterative process continues until a final, curated set of papers is selected that satisfies the project's research
criteria.

The platform ensures transparency, collaboration, and efficiency in literature selection — making it especially suitable
for systematic reviews, thesis preparation, and research group coordination.

## Project Structure

**SnowballR** consists of the following components. For a more detailed introduction into the single components, follow
the links to the according repository:

- The **[frontend](https://github.com/SE-UUlm/snowballr-frontend)** implements a graphical user interface (GUI) for
  interacting with the application. It communicates with the backend via [gRPC](https://grpc.io/) to fetch data, trigger
  actions, and update the interface based on the system state.
- The **[API](https://github.com/SE-UUlm/snowballr-api)** exposes the implemented backend functionality and provides a
  structured, type-safe gRPC interface for communication between clients and the backend.
- The **[backend](https://github.com/SE-UUlm/snowballr-backend)** listens for incoming gRPC requests, processes the
  requested operations, and responds with appropriate gRPC messages based on the application logic.

For a comprehensive overview of the project, see
[Getting Started](https://github.com/SE-UUlm/snowballr/wiki/Getting-Started).
