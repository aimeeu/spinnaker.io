---

title:  "Configure Maven Artifact Credentials"
description: Spinnaker supports artifacts stored in a Maven repository.
---

Spinnaker can be configured to deploy artifacts stored in a Maven repository.

You can configure more than one artifact account, each with separate
credentials. Specify which account to use in the configuration for the stage
that reads the data.

## Prerequisites

* A Maven repository

## Edit your artifact settings

1. Enable [artifact support](/docs/reference/artifacts/#enabling-artifact-support).

2. Enable the Maven artifact provider:

   ```bash
   hal config artifact maven enable
   ```

3. Add an artifact account:

   ```bash
   hal config artifact maven account add my-maven-account \
       --repository-url https://my.repo.example.com
   ```

There are more options described
[here](/docs/reference/halyard/commands#hal-config-artifact-maven-account-edit)
if you need more control over your configuration.
