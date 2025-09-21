# Linux and Kubernetes (kubectl) basic commnads

List of basic commands of Linux and Kubernetes/K8s's kubectl.

## Linux basic commands

### ls

- `ls -l`
- `ls -la`
- `ls -lh`

### Add (create) user

`useradd USERNANE`

### chmod

`chmod ABC FOLDER`

- A: owner
- B: group
- C: others
- folder

`chmod 755 test_folder`

### chown

`chown USER:GROUP FOLDER_NAME`

`chown root:root /srv/ftp/upload`

### Add user to group (usermod)

`usermod -a -G GROUP USER`

`usermod -a -G web-server webadminstrator`

## Kubernetes (kubectl) - basic commands

This is short list of basic commands for checking pods (applications).
I assuming everything is configured in `.kube/config`

### Configuring default namespace

This is how you can set **default** namespace.
`kubectl config use-namespace SOME-NAMESPACE-NAME`

### Switching between default namespaces

If you don't want use default namespace or you often switch between namespaces you can use this option in commands:
`kubectl -n SOME_NAMESPACE get pods`

I like this form, because you executing command: where and what.
You can easily rewrite what you want since namespace is in the beginning of command.

The `-n SOME_NAMESPACE` use different namespace from default (`-n` as _namespace_)

### List of pods

`kubectl get pods`

### List of comfigmaps

`kubectl get configmaps`

### Logs

`kubectl logs -f POD`

### Events

`kubectl events`

### Shell in pod

`kubectl exec -it POD -- sh`

### Restart of deployment

`kubectl rollout restart deployment DEPLOYMENT-NAME`
