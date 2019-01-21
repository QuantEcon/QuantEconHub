## Setup Instructions

0. Make sure you have a Google Cloud account (potentially with \$400 in [free credits](https://cloud.google.com/free/)). An individual account is fine. Once that's done, create a project, and don't set a default compute location for it.

:warning: Make sure you're aware of how pricing works before deciding whether you want to do this. I think Google Cloud charges by second (so our setup is \$0.40/hr, without sleep). But the pricing structure is intricate, and this didn't involve academic discounts, so I'm uncertain. 

1. Go to the [Kubernetes Engine API](https://console.cloud.google.com/apis/api/container.googleapis.com/overview) and hit "Enable." This may take a few minutes. **Note:** You may need to wait a few minutes before doing this for Google Cloud to become "aware" of your new project.

2. From your local terminal, pull the Docker image with `docker pull quantecon/google-hub`. If you don't have Docker installed, follow the instructions [here](https://lectures.quantecon.org/jl/tools_editors.html#Docker).

3. From your local terminal, start the container with `docker run -it quantecon/google-hub`.

4. In the container, run the setup script with `./setup.sh` and follow the on-screen instructions. Hit **1** for all the Google Cloud numeric options, and **n** for the default compute region.

5. After the script exits, run the following in the container:

```
RELEASE=jhub
NAMESPACE=jhub

helm upgrade --install $RELEASE jupyterhub/jupyterhub --namespace $NAMESPACE --version 0.7.0 --values config.yaml
```

6. That's it! To find the public IP for your hub, run `kubectl get service --namespace jhub` in the container and note the `proxy-public` external IP address.

It might say `<pending>`, which means that your IP is being assigned. Running the command again in a few minutes should clear it up.

:warning: Make sure you don't close the terminal window until you're done setting up things like authentication below). Once you exit the container, you'll lose the secret token necessary to continue working with the JupyterHub (although you will still be able to administrate through Google Cloud; i.e. handling budgeting, shutting down the project, etc.)

## Customization

> What kind of security options are available?

The easiest authentication setup is to use GitHub, with an optional whitelist of users (anyone else will get a `403: Forbidden` error if they try to log in).

The instructions for GitHub setup are [here](https://zero-to-jupyterhub.readthedocs.io/en/latest/authentication.html). For the whitelist, you can just add a list of github user IDs to the `config.yaml` (see below on how to edit it).

**Note:** When you're setting it up on GitHub, the hub address is just `http://youripaddress` (from step 6) and the callback URL is just that followed by `/hub/oauth_callback`.

Both involve changes to `config.yaml`. You can open this file by running `nano config.yaml` in the container. Make your changes, and then hit `Ctrl + X` to exit. It'll ask you to hit `Y` to save.

After running the changes, you will need to "recompile" (apply the changes) by running the following command:

```
RELEASE=jhub
helm upgrade $RELEASE jupyterhub/jupyterhub --version=0.7.0 --values config.yaml
```

Here is an example of what your `config.yaml` might look like with GitHub authentication and a whitelist. 

```
proxy:
  secretToken: "some_secret_string"
singleuser:
  image:
    name: quantecon/base
    tag: jupyterhub2
auth:
  type: github
  github:
    clientId: "bcfbff53591612ae69da"
    clientSecret: "some_secret_string"
    callbackUrl: "http://35.239.254.168/hub/oauth_callback"
  whitelist:
    users:
      - arnavs
      - jlperla
```

> How do I give my user account special privileges?

You can do this by setting yourself up in the `config.yaml` to be an [admin user](https://zero-to-jupyterhub.readthedocs.io/en/latest/user-management.html#admin-users). This will let you start/stop students' servers, and access their notebooks.

As above, after changing the `config.yaml` you will need to [apply the changes](https://zero-to-jupyterhub.readthedocs.io/en/latest/extending-jupyterhub.html).

> How do I budget compute for user accounts?

You can do this by editing the `singleuser` part of the `config.yaml` (which we've already created, to tell JupyterHub to start new users with the `quantecon/base`).

See [this page](https://zero-to-jupyterhub.readthedocs.io/en/latest/user-resources.html) for details. At the bottom, there's a command you can run to scale the cluster itself up or down.
