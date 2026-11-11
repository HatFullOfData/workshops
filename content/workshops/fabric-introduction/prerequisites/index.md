+++
title = "Prerequisites"
weight = 1
+++

## Before You Begin

This is a hands-on course, so you will need an environment where you can build along with the trainer. Please check the following before the first session so you can spend your time exploring Fabric rather than troubleshooting access.

Prior analytics knowledge is helpful but not required.

## Fabric Environment

If your organisation is providing the training environment, share these requirements with your Fabric administrator in advance.

| Requirement | What you need |
|---|---|
| **Fabric enabled** | Microsoft Fabric must be enabled in the tenant used for training, with the tenant settings allowing all learners to use it. |
| **Fabric capacity** | Access to a Fabric capacity, with each training workspace assigned to it. A Fabric trial capacity can be used for the core labs; the AI and Copilot content requires a paid-for capacity with Copilot available. |
| **A workspace for each learner** | Permission to create a workspace and assign it to the training capacity. If learners cannot create workspaces, an administrator must provide a separate, empty workspace for each learner before the course. |
| **Workspace access** | Permissions to create and manage items in the training workspace. Workspace Admin access is recommended so learners can explore the workspace settings covered in the first lab. |

## Sign-in and Training Accounts

Make sure you can sign in to [Microsoft Fabric](https://app.fabric.microsoft.com/) using the account you will use during the course.

If the trainer is providing training logins, you must be able to use those accounts on your training device and network. Check that your organisation's policies allow you to sign in to the training tenant, and complete any required authentication steps before the session.

> [!NOTE]
> A separate browser profile or private browsing window can help keep your training login separate from your usual work account. It does not bypass your organisation's sign-in or device policies.

## Access to the Workshop API

The ingestion labs use a public API to load sample business data into Fabric. You will need access to:

```text
https://fabric-workshop-api.agreeablebeach-0b5e9550.uksouth.azurecontainerapps.io/
```

Before the course, open the [API documentation](https://fabric-workshop-api.agreeablebeach-0b5e9550.uksouth.azurecontainerapps.io/docs) from the device and network you will use for training. The API does not require authentication.

If access is blocked by a firewall, proxy or web filter, ask your IT team to allow HTTPS access to this host before the session. Your Fabric pipelines will also need to connect to it during the labs.

## Ready for the First Lab?

- You can sign in to Fabric with your course account, including a trainer-provided login if applicable.
- Fabric is enabled for your account in the training tenant.
- A Fabric capacity is available for your training workspace.
- You can create a workspace, or an empty workspace has been provided just for you.
- You have permission to create and manage items in that workspace.
- You can open the workshop API documentation.

If any of these checks are not complete, let the trainer or your Fabric administrator know before the course.
