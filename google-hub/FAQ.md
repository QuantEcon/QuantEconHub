> What is the pricing like?

This [tool](https://cloud.google.com/products/calculator/) from Google allows you to estimate pricing of Kubernetes workloads, but filling it out is nontrivial.  

Google offers some support for soft budgeting, and you can find the instructions [here](https://cloud.google.com/billing/docs/how-to/budgets).

> How do I nuke/reset things?

* To shut down the Google Cloud setup, there are instructions for deleting the project [here](https://cloud.google.com/resource-manager/docs/creating-managing-projects). Basically, go to the settings page, select the project, and hit shut down.

* To reset things on your local machine (in a \*nix terminal, like git bash or macOS or Linux), run `docker rm -f $(docker ps -aq)` to get rid of all containers, and `docker rmi -f $(docker images -aq)` to get rid of all images. Alternately, `docker rmi quantecon/google-hub` will delete just the relevant image.