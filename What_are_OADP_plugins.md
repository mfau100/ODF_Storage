### OADP plug-ins allow OpenShift to back up resources that aren’t native. They extend the system’s capabilities to handle different resource types across multiple platforms and store backups on a variety of storage backends.

1️⃣ Resources (things you want to back up / restore)
```
Resource                        Type	    Plug-in	Notes

Persistent Volumes / PVCs	    csi	        Uses Kubernetes CSI Snapshot API to capture volume snapshots. Mandatory for backing up storage.

Volumes	kubevirt	Required for OpenShift Virtualization; handles VM specs, disks, DataVolumes.
OpenShift-specific resources	openshift	Handles ImageStreams, Routes, ServiceAccounts, internal registry images, and other OpenShift CRDs. Mandatory for Data Mover.
Secrets & ConfigMaps	Core OADP	No plug-in needed; handled natively.
Standard Kubernetes resources	Core OADP	Pods, Deployments, Services, Namespaces, etc.
Custom Resources	Optional plug-ins	Some CRDs may require additional plug-ins if special logic is needed for backup/restore.
```
2️⃣ Storage Backends (where backups are stored / snapshots managed)
Storage Backend	Plug-in	Notes
AWS (S3 + EBS)	aws	Stores backup data in S3-compatible storage; manages volume snapshots on EBS.
Google Cloud Platform (GCP)	gcp	Uses Google Cloud Storage (GCS) and GCE disk snapshots.
Azure	azure	Uses Azure Blob Storage and Managed Disk snapshots.
OpenShift internal registry / object storage	openshift	Can store container images or backup metadata.
On-prem / CSI-compatible storage	csi	Uses Kubernetes CSI Snapshot API to create snapshots on supported storage backends (Ceph, vSphere, etc.).
S3-compatible object storage	aws or generic S3 plug-ins	For private cloud or on-prem S3 storage solutions.
Mandatory plug-ins for Data Mover

csi → Volume snapshots

openshift → OpenShift-specific resources and images

kubevirt → VM / DataVolume backups (if using virtualization)

Optional storage plug-ins

aws → AWS S3 / EBS

gcp → Google Cloud Storage / GCE disks

azure → Azure Blob / Managed Disks

Quick mental model:

Resources → what you’re backing up (PVCs, VMs, CRDs, OpenShift objects)

Storage backends → where you’re putting it (S3, GCS, Azure, on-prem)

Plug-ins → the adapters that make the two talk