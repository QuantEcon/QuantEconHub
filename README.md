# QuantEconHub
Test bed for the jupyterhub environment.  Public later or as necessary

## Basic Usage

Ask for your GitHub ID to be added to the whitelist (and make one if you haven't)

Go to `35.239.254.168` in your browser and supply any username and password.

You'll start off in an instance of the `quantecon/base` Docker image.

From there, use it like you would any other JupyterHub --- your work and package choices, etc., will persist

## Administrative Details

> Where are the instructions for setting this up?

We set this up using the (Google Cloud instructions) from the [Zero to JupyterHub with Kubernetes](https://zero-to-jupyterhub.readthedocs.io/en/latest/) guide.

> How do I add users to the whitelist?

Add them to the `whitelist/users` section of the `config.yaml`, and recompile.

> How do I upgrade the Docker image?

The `singleuser/image` part of the `config.yaml` contains a name (like `quantecon/base`) and a tag (like `jupyterhub2`). Edit this section and recompile.

:warning: We've had trouble trying to update to a new version of the same tag. That's why we had to create, e.g., `jupyterhub2` instead of rewriting the `jupyterhub`.

> How do I reset a single user's Docker container?

Unclear, but [these](https://stackoverflow.com/questions/46123457/kubernetes-restart-container-within-pod?rq=1) StackOverflow instructions may help.

> How do I exchange files with students?

The best way is through `nbgitpuller`, which works here exactly as it would on other Jupyter setups (and comes pre-installed in `quantecon/base`).

> How do I inspect user data?

If you can isolate a particular docker container with the user's data, you can log into it with `docker exec` (i.e., `docker exec -it container-id /bin/bash`)

> How do I budget compute for users?

See [here](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/).

Will fill in these details once they're implemented.
