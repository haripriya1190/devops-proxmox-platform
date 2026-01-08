# Day 1 – Proxmox Golden VM Template

## Objective

The objective of Day 1 was to create a reusable, cloud-init–enabled Golden VM template on Proxmox that serves as the immutable base image for all infrastructure virtual machines.

This template is designed to be:
- Terraform-compatible
- Kubernetes-ready
- Secure and reproducible
- Free from application-specific configuration

---

## Base Operating System

- **OS:** Ubuntu Server 22.04 LTS
- **Image Type:** Official Ubuntu cloud image
- **Provisioning Method:** cloud-init

---

## Storage Design

The Golden VM template is stored on an existing dedicated Proxmox storage backend (`sas-storage`) backed by SAS disks.

This storage backend is used for:
- Cloud images
- VM disks
- cloud-init volumes

Separating VM data from the Proxmox system disk improves performance, scalability, and operational safety.

---

## VM Configuration

- **VM ID:** 9000
- **CPU:** 2 vCPUs
- **Memory:** 2 GB
- **Disk Interface:** virtio-scsi
- **Network Interface:** virtio
- **Boot Mode:** Serial console (cloud image compatible)

---

## cloud-init Configuration

The template uses cloud-init for first-boot automation:

- A non-root `devops` user is created
- SSH key-based authentication is injected
- Password-based authentication is intentionally disabled
- DHCP is used for initial networking

This ensures secure, automated access without manual intervention.

---

## Guest Agent Enablement

The QEMU guest agent was installed and enabled inside the VM to allow Proxmox and Terraform to:

- Retrieve VM IP addresses
- Perform lifecycle operations
- Improve observability and automation

---

## Kubernetes Readiness

To ensure Kubernetes compatibility across all cloned VMs:

- Swap was disabled at the OS level
- Swap was permanently removed from `/etc/fstab`

This guarantees that all nodes provisioned from the template are Kubernetes-ready by default.

---

## Template Finalization

After validation:
- The VM was cleanly shut down
- Converted into a Proxmox template

This Golden Template is now used as the immutable base image for all infrastructure VMs provisioned via Terraform.

---

## Outcome

At the end of Day 1:
- A production-ready Golden VM template was created
- VM provisioning time was reduced
- Configuration drift risk was eliminated
- Infrastructure automation readiness was established
