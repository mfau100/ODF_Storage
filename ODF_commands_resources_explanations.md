### ODF info.

OCSInitialization ocsinit:
- OCSInitialization is an ODF custom resource
- ocsinit is the name of the CR
- This CR controls global ODF initialization settings
```
[student@workstation ~]$ oc get OCSInitialization
NAME      AGE    PHASE   CREATED AT
ocsinit   271d   Ready   2025-04-11T10:02:45Z
```
```
[student@workstation ~]$ oc get OCSInitialization ocsinit
NAME      AGE    PHASE   CREATED AT
ocsinit   271d   Ready   2025-04-11T10:02:45Z
```
What the command does (plain English)

- It patches the OCSInitialization custom resource to turn on Ceph tools support, which causes ODF to deploy the rook-ceph-tools pod.
- That pod gives you access to the Ceph CLI (ceph, rados, rbd, etc.) inside the cluster.
```
[student@workstation ~]$ oc patch OCSInitialization ocsinit \
  -n openshift-storage \
  --type json \
  --patch '[{ "op": "replace", "path": "/spec/enableCephTools", "value": true }]'
ocsinitialization.ocs.openshift.io/ocsinit patched
```
