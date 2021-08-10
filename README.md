# basic-gitops-operator

Step 1:

We made a new repo under github with the resources we want to create in our kubernetes cluster:
https://github.com/lcostea/basic-gitops-config

It contains a namespace `redis` and a deployment for 1 or more redis containers to create in the `redis` namespace.

Step 2:

We create a simple application that clones the repo and then applies it to the cluster it runs into and it keeps doing that in a loop at every 30 seconds.
Initialising our application using go modules:

`go mod init github.com/lcostea/basic-gitops-operator`
Create a new `main.go` file

Step 3:

For working with git repos: cloning them intially and keeping them in sync we are going to use this library:
https://github.com/go-git/go-git which is probably the main go client for working with git. 
And this library is also used by ArgoCD, though sometimes, probably for speed, they use the git client directly, invoking it from go code.
For our needs of cloning and pull a git repo this is more than enough.

Step 4:

We are not going to call the Kubernetes API because that will complicate things a lot, but instead we will call the `kubectl apply` binary from our go code to apply the folder contents where we pulled the gitops repo.

Step 5:

Create the loop for calling the git sync and manifests apply every 30 seconds.
