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

:warning: Make sure you don't _delete_ (i.e., `docker rm`) the container. If you want to make modifications to the JupyterHub, restart the container by running `docker ps -a`, noting the name of the `quantecon/google-hub` image (e.g., something like `admiring_mirzakhani`), and running `docker start -ai admiring_mirzakhani`).

## Customization

> What kind of security options are available?

The easiest authentication setup is to use GitHub, with an optional whitelist of users (anyone else will get a `403: Forbidden` error if they try to log in).

The instructions for GitHub setup are [here](https://zero-to-jupyterhub.readthedocs.io/en/latest/authentication.html) and the whitelist are [here](https://zero-to-jupyterhub.readthedocs.io/en/latest/authentication.html#adding-a-whitelist).

Both involve changes to `config.yaml` (an example with both the GitHub auth and the whitelist is below), and after running the changes you need to recompile by following [these instructions](https://zero-to-jupyterhub.readthedocs.io/en/latest/extending-jupyterhub.html).

**Note:** When you're setting it up on GitHub, the hub address is just `http://youripaddress` (from step 6) and the callback is just that followed by `/hub/oauth_callback`.

```
auth:
  type: github
  github:
    clientId: "bcfbff53591612ae69da"
    clientSecret: "some_secret"
    callbackUrl: "http://35.239.254.168/hub/oauth_callback"
  whitelist:
    users:
      - arnavs
      - jlperla
```

You can open this file by running `nano config.yaml` in the container. Make your changes, and then hit `Ctrl + X` to exit. It'll ask you to hit `Y` to save.

> How do I give my user account special privileges?

You can do this by setting yourself up in the `config.yaml` to be an [admin user](https://zero-to-jupyterhub.readthedocs.io/en/latest/user-management.html#admin-users). This will let you start/stop students' servers, and access their notebooks.

As above, after changing the `config.yaml` you will need to [apply the changes](https://zero-to-jupyterhub.readthedocs.io/en/latest/extending-jupyterhub.html).

> How do I budget compute for user accounts?

You can do this by editing the `singleuser` part of the `config.yaml` (which we've already created, to tell JupyterHub to start new users with the `quantecon/base`).

See [this page](https://zero-to-jupyterhub.readthedocs.io/en/latest/user-resources.html) for details. At the bottom, there's a command you can run to scale the cluster itself up or down.
