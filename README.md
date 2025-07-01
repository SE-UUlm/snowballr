## SnowballR

Welcome to the **SnowballR** project! This page will give you a quick overview over the project.

**SnowballR** is a web-based tool supporting Systematic Literature Reviews (SLR). The goal of **SnowballR** is to automate reference retrieval, facilitate transparent inclusion/exclusion decisions, and maintain a clear audit trail of the review process..

Users can create dedicated **projects** and invite other collaborators to participate. Each project follows a structured paper selection process based on the [snowballing](https://en.wikipedia.org/wiki/Snowball_sampling) methodology, which helps to identify relevant literature through citation networks.

Papers within a project are processed through multiple **review stages**. In each stage:

- New papers are discovered and added.
- Multiple reviewers assess each paper independently.
- Papers are **accepted** or **declined** based on consensus.
- Accepted papers proceed to the next stage.

This iterative process continues until a final, curated set of papers is selected that satisfies the project's research criteria.

The platform ensures transparency, collaboration, and efficiency in literature selection — making it especially suitable for systematic reviews, thesis preparation, and research group coordination.

### Project Structure

**SnowballR** consists of the following components. For a more detailed introduction into the single components, follow the links to the according repository:

- The **[frontend](https://github.com/SE-UUlm/snowballr-frontend)** implements a graphical user interface (GUI) for interacting with the application. It communicates with the backend via [gRPC](https://grpc.io/) to fetch data, trigger actions, and update the interface based on the system state.
- The **[API](https://github.com/SE-UUlm/snowballr-api)** exposes the implemented backend functionality and provides a structured, type-safe gRPC interface for communication between clients and the backend. 
- The **[backend](https://github.com/SE-UUlm/snowballr-backend)** listens for incoming gRPC requests, processes the requested operations, and responds with appropriate gRPC messages based on the application logic.


For a comprehensive overview of the project, see [Getting Started](./wiki/Getting-Started.md).