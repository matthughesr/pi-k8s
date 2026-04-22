Goal: create an ansible playbook to setup k0s on a raspberry pi. I should be able to nuke my pi and get back up running quickly

ansible playbook for k8s on raspberry pi

Ansible will be uses to autoamate setting up k0s networking, services
Ansible cannot flash the OS. Ansible steps in once we can ssh into it


This is running k0s
why k0s?  
- k0s runs upstream Kubernetes with minimal modifications. k3s changes defaults (storage, ingress, networking) to reduce complexity — creates subtle differences from  
  "real" Kubernetes.                                                                                                                                                     
  - Your EKS background transfers directly — the YAML, the concepts, the behavior all work the same way. With k3s you'd hit unexpected differences.                      
  - GitOps friendly — k0s config lives in a YAML file (k0s.yaml) that sits naturally in git. You're already using it for the SAN config you just added. 