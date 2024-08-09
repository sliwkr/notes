# kubectl

## table of contents

- [Topic](#header-link)

## Update a deployment

### Header link

```sh
k get pods -n namespace  # list pods within a given namespace
k get pods -n namespace pod -o=yaml  # list configuration of a given pod
k exec -it -n namespace pod -- command  # execute a command within a given pod
k get deployments -n namespace  # get namespace deployments
k get deployments -A  # get all deployments
k get service -n namespace
k get ingress -n namespace

k explain deploy.spec
k explain deploy --recursive > deployment.txt
--dry-run -o yaml

k apply -f file.yaml
k get all
k get secret
k get configmap
k describe service SERVICE_NAME
k describe pod POD_NAME   # pod events, scheduling errors
k get node -o wide  # get node ip address

k get pods -A | grep -v 'Running'  # get pods that are not in Running state
k get events --watch --field-selector involvedObject.kind=Node  # get logs for nodes

k get statefulset -n NAMESPACE  # get all statefulsets of a namespace
k rollout restart statefulset -n NAMESPACE STATEFULSET_NAME  # force pods in stateful set to stop/start

k logs -n NAMESPACE POD_NAME -f  # logs from a pod

k scale statefulset -n NAMESPACE POD_NAME --replicas=2  # increase/decrease a number of replicas in a statefulset

k get pods --all-namespaces -o wide --field-selector spec.nodeName=
k get pods POD_NAME_HERE -o jsonpath='{.spec.containers[*].name}'  # get all containers in a pod

k api-resources  # list of all resources supported by kubectl
k edit configmap coredns -n kube-system  # edit a resource from a namespace
k cp ./folder -n namespace pod:/abs/location  # copy a directory to a pod
k config use-context minikube  # use a context defined in ~/.kube/config
```
