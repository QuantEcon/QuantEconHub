> What is the pricing like?

This [tool](https://cloud.google.com/products/calculator/) from Google allows you to estimate pricing of Kubernetes workloads, but filling it out is nontrivial.  

Google offers some support for soft budgeting, and you can find the instructions [here](https://cloud.google.com/billing/docs/how-to/budgets).

> How do I restart the container after stopping it? 

1. Run `docker ps -a`. You should see something like the following (**note:** we have reports that this command might not work on some Windows setups)

```
CONTAINER ID        IMAGE                  COMMAND             CREATED             STATUS                        PORTS               NAMES
99bdc50375a6        quantecon/google-hub   "bash"              18 minutes ago      Exited (130) 20 seconds ago                       silly_lalande
```

2. Run `docker start -ai silly_lalande` (where you input the name of the container in your case).

:warning: This will stop working if you delete the stopped container (i.e., if you `docker rm` it). If you do that without writing down the secret token from the container's `config.yaml`, then you will need to tear down the entire JupyterHub setup (i.e., shut down the Google Cloud project) and start over if you want to make changes.

> What resources are installed by default? 

On the Julia side-

```
    Status `~/.julia/environments/v1.0/Project.toml`
  [6e4b80f9] BenchmarkTools v0.4.1
  [3da002f7] ColorTypes v0.7.5
  [34da2185] Compat v1.4.0
  [a93c6f00] DataFrames v0.16.0
  [1313f7d8] DataFramesMeta v0.4.0
  [39dd38d3] Dierckx v0.4.1
  [31c24e10] Distributions v0.16.4
  [2fe49d83] Expectations v1.0.2
  [28b8d3ca] GR v0.37.0
  [7073ff75] IJulia v1.15.2
  [43edad99] InstantiateFromURL v0.2.1
  [a98d9a8b] Interpolations v0.11.1
  [5ab0869b] KernelDensity v0.5.1
  [b964fa9f] LaTeXStrings v1.0.3
  [76087f3c] NLopt v0.5.1
  [2774e3e8] NLsolve v3.0.1
  [429524aa] Optim v0.17.2
  [d96e819e] Parameters v0.10.3
  [91a5bcdd] Plots v0.22.4
  [f27b6e38] Polynomials v0.5.1
  [1fd47b50] QuadGK v2.0.3
  [fcd29c91] QuantEcon v0.15.0
  [295af30f] Revise v1.0.2
  [f2b01f46] Roots v0.7.4
  [60ddc479] StatPlots v0.8.2
  [90137ffa] StaticArrays v0.10.2
  [37e2e46d] LinearAlgebra 
  [9a3f8284] Random 
  [2f01184e] SparseArrays 
  [10745b16] Statistics 
  [8dfed614] Test 
```

This also ships with a standard scientific computing environment in Python, and a set of common CLI tools (`git`, `curl`, `vim`, etc.). You can see the `quantecon/base:jupyterhub2` Docker image for more detail.

> How do I nuke/reset things?

* To shut down the Google Cloud setup, there are instructions for deleting the project [here](https://cloud.google.com/resource-manager/docs/creating-managing-projects). Basically, go to the settings page, select the project, and hit shut down.

* To reset things on your local machine (in a \*nix terminal, like git bash or macOS or Linux), run `docker rm -f $(docker ps -aq)` to get rid of all containers, and `docker rmi -f $(docker images -aq)` to get rid of all images. Alternately, `docker rmi quantecon/google-hub` will delete just the relevant image.
