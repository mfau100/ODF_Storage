### Backups


[student@workstation ~]$ oc get storageclasses | egrep "^NAME|csi"
NAME                                                  PROVISIONER                                   RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
ocs-external-storagecluster-ceph-rbd (default)        openshift-storage.rbd.csi.ceph.com            Delete          Immediate           true                   272d
ocs-external-storagecluster-ceph-rbd-virtualization   openshift-storage.rbd.csi.ceph.com            Delete          Immediate           true                   272d
ocs-external-storagecluster-cephfs                    openshift-storage.cephfs.csi.ceph.com         Delete          Immediate           true                   272d

[student@workstation ~]$ oc get volumesnapshotclasses
NAME                                                 DRIVER                                  DELETIONPOLICY   AGE
ocs-external-storagecluster-cephfsplugin-snapclass   openshift-storage.cephfs.csi.ceph.com   Delete           272d
ocs-external-storagecluster-rbdplugin-snapclass      openshift-storage.rbd.csi.ceph.com      Delete           272d
