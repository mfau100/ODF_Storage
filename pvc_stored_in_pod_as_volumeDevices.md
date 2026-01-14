# The vm referrences the pvc.
```
[student@workstation ~]$ oc get vm mariadb-server-backup -o yaml
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  name: mariadb-server-backup
  namespace: backup-vm
spec:
  runStrategy: Always
  template:
    metadata:
      creationTimestamp: null
    spec:
      architecture: amd64
      domain:
        devices: {}
        machine:
          type: pc-q35-rhel9.4.0
        memory:
          guest: 512Mi
        resources: {}
      terminationGracePeriodSeconds: 180
      volumes:
      - name: mariadb-server
        persistentVolumeClaim:
          claimName: mariadb-server
```
# The pod has the pvc info.
```
[student@workstation ~]$ oc get pod virt-launcher-mariadb-server-backup-nmgh2 -o yaml
apiVersion: v1
kind: Pod
metadata:
  name: virt-launcher-mariadb-server-backup-nmgh2
  namespace: backup-vm
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: node-labeller.kubevirt.io/obsolete-host-model
            operator: DoesNotExist
    volumeDevices:
    - devicePath: /dev/mariadb-server
      name: mariadb-server
    volumeMounts:
    - mountPath: /var/run/kubevirt-private
      name: private
    - mountPath: /var/run/kubevirt
      name: public
    - mountPath: /var/run/kubevirt-ephemeral-disks
      name: ephemeral-disks
    - mountPath: /var/run/kubevirt/container-disks
      mountPropagation: HostToContainer
      name: container-disks
    - mountPath: /var/run/libvirt
      name: libvirt-runtime
    - mountPath: /var/run/kubevirt/sockets
      name: sockets
    - mountPath: /var/run/kubevirt/hotplug-disks
      mountPropagation: HostToContainer
      name: hotplug-disks
  volumes:
  - emptyDir: {}
    name: private
  - emptyDir: {}
    name: public
  - emptyDir: {}
    name: sockets
  - emptyDir: {}
    name: virt-bin-share-dir
  - emptyDir: {}
    name: libvirt-runtime
  - emptyDir: {}
    name: ephemeral-disks
  - emptyDir: {}
    name: container-disks
  - name: mariadb-server
    persistentVolumeClaim:
      claimName: mariadb-server
  - emptyDir: {}
    name: hotplug-disks
```