---

title:  "DC/OS"
sidebar:
  nav: setup
aliases: 
    - /setup/providers/dcos/
---



DC/OS configuration for Spinnaker consists of a set of DC/OS
clusters and a set of [Accounts](/docs/concepts/providers/#accounts) that have
credentials to authenticate to one or more of those clusters.
Additionally, each account has a set of [Docker Registry](/docs/setup/install/providers/docker-registry/)
accounts to be used as a source of images.

## Prerequisites

### DC/OS Cluster

You need to have a DC/OS cluster running version 1.8 or greater.  This setup guide
assumes the use of Enterprise DC/OS as the authentication methods described
below are only supported in the that version.  The provider may work with
the open-source DC/OS but only if authentication is disabled.  However, it
has not been extensively tested.

<!-- TODO: link to the reference guide section about permissions required for the account-->

### Docker registries

Follow the steps under the [Docker Registry](/docs/setup/install/providers/docker-registry/)
provider to add any registries containing images you want to deploy. If
you have already done so, you can verify that these accounts exist by running:

```bash
hal config provider docker-registry account list
```

## Set up the DC/OS provider

First, enable the provider:

```bash
hal config provider dcos enable
```

Next we need to add our DC/OS cluster:

```bash
hal config provider dcos cluster add my-dcos-cluster \
    --dcos-url $CLUSTER_URL \
    --skip-tls-verify
    # For simplicity we won't worry about the
    # certificate for the cluster but this would not be recommended
    # for a production deployment
```

Create a Spinnaker account that has credentials for the cluster.  The
credentials can either be for a service account or a user account.

If you are using a service account:

```bash
hal config provider dcos account add my-dcos-account \
    --cluster my-dcos-cluster \
    --docker-registries my-docker-registry \
    --uid $DCOS_SERVICE_ACCOUNT \
    --service-key-file $PATH_TO_PRIVATE_KEY
```

If you are using a user account:

```bash
hal config provider dcos account add my-dcos-account \
    --cluster my-dcos-cluster \
    --docker-registries my-docker-registry \
    --uid $DCOS_USER \
    --password $DCOS_PASSWORD
```


Note: Make sure that your DC/OS user has permission to deploy applications
under a group in Marathon named after the Spinnaker account name
(e.g. `/my-dcos-account`)


## Advanced account settings

If you are looking for more configurability, please see the other options
listed in the [Halyard
Reference](/docs/reference/halyard/commands/#hal-config-provider-dcos)
for adding [accounts](/docs/reference/halyard/commands/#hal-config-provider-dcos-account-add)
and [clusters](/docs/reference/halyard/commands/#hal-config-provider-dcos-cluster-add).

## Next steps

Optionally, you can [set up another cloud provider](/docs/setup/install/providers/),
but otherwise you're ready to [choose an environment](/docs/setup/install/environment/)
in which to install Spinnaker.
