# Useful commands

- I'll post some interesting commands I used in the past think that they are helpful at least for me :-)

## port forwarding

- access via jumphost

``` { .sh }
# on jumphost
k port-forward -n <NAMESPACE> services/<SERVICE> <LOCAL_PORT_JUMPHOST>:<SERVICE_PORT>

# on local machine
ssh -L <LOCAL_PORT>:localhost:<LOCAL_PORT_JUMPHOST> jumphost
```

## multi-attach error

- when you try to scale down nodes it might happen that some pods can't be moved because of [Multi-Attach error for volume](https://github.com/kubernetes-sigs/vsphere-csi-driver/blob/master/docs/book/known_issues.md#multi-attach-error-for-rwo-block-volume-when-node-vm-is-shutdown-before-pods-are-evicted-and-volumes-are-detached-from-node-vm)
- you can see these error by describing the pods and see events like below:

``` { .sh }
k describe pods -n <NAMESPACE> <POD>

# Events: Multi-Attach error for volume "pvc-uuid" Volume is already exclusively attached to one node and can't be attached to another.
```

- to solve this in a smei-automated way you can use the following lines:

``` { .sh }
NODE=<OLD_NODE>
k get pods -A -owide | grep $NODE
k get volumeattachments.storage.k8s.io -o custom-columns=":metadata.name,:spec.nodeName" | grep $NODE | cut -d" " -f1 | xargs -r -t -L1 kubectl patch volumeattachments.storage.k8s.io --patch '{"metadata":{"finalizers":null}}' --type merge
k get volumeattachments.storage.k8s.io -o custom-columns=":metadata.name,:spec.nodeName" | grep $NODE | cut -d" " -f1 | xargs -r -t -L1 kubectl delete volumeattachments.storage.k8s.io
k delete node $NODE
```

## Node `NotReady` taint

- nodes that are in `NotReady` state can be tainted by the following command to remove them in a correct way:

`kubectl taint node <NODENAME> node.kubernetes.io/out-of-service:NoExecute`

- for more information check the kubernetes documentation of [Non-graceful node shutdown handling](https://kubernetes.io/docs/concepts/cluster-administration/node-shutdown/#non-graceful-node-shutdown)
