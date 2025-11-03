This page describes important processes within the **SnowballR** project.
The goal is to provide a clear overview of how interactions between users and the system are structured,
from account creation to project collaboration and internal processes.

Each process is documented either as a diagram, a step-by-step description, or a combination of both.
Where useful, additional clarifications are provided to highlight the involved components and their responsibilities.
This helps developers and contributors understand where functionality is implemented and how the different parts of the
system interact.

---

### User Account Lifecycle

The user account lifecycle diagram provides a high-level overview of the different stages a user account goes through
within the system. Each box represents a specific state of the account. At the top, the corresponding internal
`UserStatus` enum value is shown (if the user object exists). Below it, a short description outlines the
characteristics or conditions of that state.

![snowballr-user-account-lifecycle-help.svg](assets/snowballr-user-account-lifecycle-help.svg)

The lifecycle is composed of six distinct states. The first three represent the primary phases of a user account, while
the remaining three correspond to the deletion and cleanup stages, which are initiated after an account has been
deleted and specific cron jobs have been triggered.

![snowballr-user-account-lifecycle.svg](assets/snowballr-user-account-lifecycle.svg)

### Project Invitation Process

The project invitation process provides a high-level overview of the different stages a project invitation goes through
within the system. Each box represents a specific state of the invitation process.

In contrast to the user account lifecycle diagram, each state only contains a description since there are multiple
entities involved.

![snowballr-project-invitation-process.svg](assets/snowballr-project-invitation-process.svg)
