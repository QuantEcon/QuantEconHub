# QuantEconHub
Test bed for the jupyterhub environment.  Public later or as necessary

## Basic Usage

Ask for your GitHub ID to be added to the whitelist (and make one if you haven't)

Go to `35.239.254.168` in your browser and supply any username and password.

You'll start off in an instance of the `quantecon/base` Docker image.

From there, use it like you would any other JupyterHub --- your work and package choices, etc., will persist

## Administrative Details

> Where are the instructions for setting this up?

We set this up using the (Google Cloud) instructions from the [Zero to JupyterHub with Kubernetes](https://zero-to-jupyterhub.readthedocs.io/en/latest/) guide.

> How do I find the public IP for students to access?

See instructions.

> How do I choose the Docker image for students to use?

By setting the relevant bit of `config.yaml` (see [here](https://zero-to-jupyterhub.readthedocs.io/en/latest/user-environment.html)) and recompiling.

If your Google Cloud Console is set up (i.e., it's within the 1-hour persistence window), then to recompile you will simply need to follow the instructions [here](https://zero-to-jupyterhub.readthedocs.io/en/latest/extending-jupyterhub.html).

> How do I add users to the whitelist?

Add them to the `whitelist/users` section of the `config.yaml` (i.e., [here](https://zero-to-jupyterhub.readthedocs.io/en/latest/authentication.html#adding-a-whitelist)), and recompile.

Note that the Google Cloud console doesn't persist your installation of `helm` beyond one hour of inactivity, so if you go that route, and you find that `helm` is missing (i.e., `which helm` doesn't return anything), then to recompile you will need to:

0. Make the relevant changes to the `config.yaml`
1. Run the `curl` and `helm init` commands from [here](https://zero-to-jupyterhub.readthedocs.io/en/latest/setup-helm.html).
2. Run the `helm repo add`, `helm repo update`, and `helm upgrade` commands from [here](https://zero-to-jupyterhub.readthedocs.io/en/latest/setup-jupyterhub.html).

> How do I delete the whitelist?

Nuke the relevant bit of the `config.yaml` and recompile.

> How do I exchange files with students?

The best way is through `nbgitpuller`, which works here exactly as it would on other Jupyter setups (and comes pre-installed in `quantecon/base`).
