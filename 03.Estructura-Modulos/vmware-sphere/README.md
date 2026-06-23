# Repostorio de Gobernanza Terraform.

## VMware vSphere (vSphere), estructura del directorio de modulos de Terraform.

La siguiente estructura de directorios representa como se desea reflejar en este respositorio y como haremos referencia posteriormente en las practicas a esta estructura.

Cada directorio, tendrá como mínimo estos archivos obligatorios y otros opcionales de Terraform:
```text
- main.tf                 # (obligatorio)
- outputs.tf              # (obligatorio)
- variables.tf            # (obligatorio)
- backend.tf              # (opcional)
- locals.tf               # (opcional)
- providers.tf            # (opcional)
```

También en algunos directorios opcionales como: 

```text
 ├── templates/ 
 ├── format/
 ├── resource/
```
---

```text
 aprende-gobernanza-terraform/
 │
 ├── terraform/
 │   │
 │   ├── vsphere/
 │   │   ├── modules/
 │   │   │   │
 │   │   │   ├── infrastructure/                  # 🏗️ Infraestructura base
 │   │   │   │    ├── datacenter/                   # Datacenter (gestión de datacenters)
 │   │   │   │    ├── cluster/                      # Compute Cluster (DRS/HA)
 │   │   │   │    ├── host/                         # ESXi Hosts (configuración)
 │   │   │   │    ├── resource-pool/                # Resource Pools (CPU/RAM)
 │   │   │   │    ├── folder/                       # VM Folders (organización)
 │   │   │   │    └── license/                      # Licencias (gestión)
 │   │   │   ├── compute/                         # 💻 Cómputo
 │   │   │   │   ├── vm/                            # Virtual Machine (VM individual)
 │   │   │   │   ├── vm-template/                   # VM Template (desde plantilla)
 │   │   │   │   ├── vm-clone/                      # Clone de VM (desde VM existente)
 │   │   │   │   ├── vm-multiple/                   # Múltiples VMs (despliegue masivo)
 │   │   │   │   ├── vm-customization/              # Customización (Linux/Windows)
 │   │   │   │   ├── vm-advanced/                   # VM Avanzada (configuraciones especiales)
 │   │   │   │   ├── vm-snapshot/                   # Snapshots (gestión)
 │   │   │   │   ├── vm-migration/                  # Migración (vMotion, Storage vMotion)
 │   │   │   │   ├── vm-anti-affinity/              # Anti-Affinity Rules (DRS)
 │   │   │   │   ├── vm-drs-group/                  # DRS Groups (VM/Host groups)
 │   │   │   │   └── vm-overrides/                  # VM Overrides (HA/DRS específicos)
 │   │   │   ├── storage/                         # 💾 Almacenamiento
 │   │   │   │   ├── datastore/                     # Datastore (VMFS/NFS)
 │   │   │   │   ├── datastore-cluster/             # Datastore Cluster (Storage DRS)
 │   │   │   │   ├── virtual-disk/                  # Virtual Disk (disco adicional)
 │   │   │   │   ├── storage-policy/                # Storage Policies (SPBM)
 │   │   │   │   ├── vsan-cluster/                  # vSAN Cluster (almacenamiento HCI)
 │   │   │   │   ├── vsan-datastore/                # vSAN Datastore
 │   │   │   │   ├── vsan-disk-group/               # vSAN Disk Group
 │   │   │   │   ├── vsan-fault-domain/             # vSAN Fault Domain
 │   │   │   │   ├── vsan-performance-service/      # vSAN Performance Service
 │   │   │   │   ├── vsan-capacity-reservation/     # vSAN Capacity Reservation
 │   │   │   │   ├── vsan-hci-mesh/                 # vSAN HCI Mesh
 │   │   │   │   ├── vsan-esa/                      # vSAN ESA (Express Storage Arch)
 │   │   │   │   ├── vsan-file-services/            # vSAN File Services
 │   │   │   │   ├── vsan-iscsi/                    # vSAN iSCSI Target
 │   │   │   │   ├── vsan-remote-datastore/         # vSAN Remote Datastore
 │   │   │   │   ├── vsan-direct/                   # vSAN Direct (Disaggregated)
 │   │   │   │   └── vsan-topology/                 # vSAN Topology (Stretched Clusters)
 │   │   │   ├── networking/                      # 🌐 Redes
 │   │   │   │   ├── distributed-switch/            # Distributed Virtual Switch (vDS)
 │   │   │   │   ├── distributed-port-group/        # Distributed Port Group
 │   │   │   │   ├── standard-switch/               # Standard Virtual Switch (vSS)
 │   │   │   │   ├── standard-port-group/           # Standard Port Group
 │   │   │   │   ├── network/                       # Network (red existente)
 │   │   │   │   ├── host-network/                  # Host Network (vmknic, vswitch)
 │   │   │   │   ├── host-vswitch/                  # Host Virtual Switch
 │   │   │   │   ├── host-port-group/               # Host Port Group
 │   │   │   │   ├── host-vmnic/                    # Host vmnic (NICs físicas)
 │   │   │   │   ├── host-vmkernel/                 # Host VMkernel (vmk interfaces)
 │   │   │   │   ├── host-firewall/                 # Host Firewall (reglas)
 │   │   │   │   ├── host-ntp/                      # Host NTP (configuración)
 │   │   │   │   ├── host-dns/                      # Host DNS (configuración)
 │   │   │   │   ├── host-syslog/                   # Host Syslog (configuración)
 │   │   │   │   ├── host-iscsi/                    # Host iSCSI (configuración)
 │   │   │   │   ├── host-nfs/                      # Host NFS (configuración)
 │   │   │   │   ├── host-fcoe/                     # Host FCoE (configuración)
 │   │   │   │   ├── host-vsan/                     # Host vSAN (configuración)
 │   │   │   │   ├── host-pci/                      # Host PCI (passthrough, SR-IOV)
 │   │   │   │   ├── host-usb/                      # Host USB (passthrough)
 │   │   │   │   ├── host-gpu/                      # Host GPU (passthrough, vGPU)
 │   │   │   │   ├── host-tpm/                      # Host TPM (configuración)
 │   │   │   │   ├── host-credstore/                # Host Credential Store
 │   │   │   │   ├── host-active-directory/         # Host Active Directory (autenticación)
 │   │   │   │   ├── host-lockdown/                 # Host Lockdown Mode
 │   │   │   │   ├── host-quarantine/               # Host Quarantine Mode
 │   │   │   │   ├── host-maintenance/              # Host Maintenance Mode
 │   │   │   │   ├── host-power/                    # Host Power Management
 │   │   │   │   ├── host-powermanagement/          # Host Power Management (DPM)
 │   │   │   │   ├── host-ssl/                      # Host SSL (certificados)
 │   │   │   │   ├── host-ssh/                      # Host SSH (configuración)
 │   │   │   │   ├── host-esxcli/                   # Host ESXCLI (comandos)
 │   │   │   │   ├── host-syslog/                   # Host Syslog (configuración)
 │   │   │   │   ├── host-ntp/                      # Host NTP (configuración)
 │   │   │   │   ├── host-dns/                      # Host DNS (configuración)
 │   │   │   │   ├── host-iscsi/                    # Host iSCSI (configuración)
 │   │   │   │   ├── host-nfs/                      # Host NFS (configuración)
 │   │   │   │   ├── host-fcoe/                     # Host FCoE (configuración)
 │   │   │   │   ├── host-vsan/                     # Host vSAN (configuración)
 │   │   │   │   ├── host-pci/                      # Host PCI (passthrough, SR-IOV)
 │   │   │   │   ├── host-usb/                      # Host USB (passthrough)
 │   │   │   │   ├── host-gpu/                      # Host GPU (passthrough, vGPU)
 │   │   │   │   ├── host-tpm/                      # Host TPM (configuración)
 │   │   │   │   ├── host-credstore/                # Host Credential Store
 │   │   │   │   └── host-active-directory/         # Host Active Directory (autenticación)
 │   │   │   ├── security/                        # 🔒 Seguridad
 │   │   │   │   ├── role/                          # Roles (permisos)
 │   │   │   │   ├── permission/                    # Permisos (asignación)
 │   │   │   │   ├── global-permission/             # Global Permissions
 │   │   │   │   ├── tag-category/                  # Tag Categories (clasificación)
 │   │   │   │   ├── tag/                           # Tags (etiquetas)
 │   │   │   │   ├── vm-storage-policy/             # VM Storage Policy (SPBM)
 │   │   │   │   ├── content-library/               # Content Library (plantillas, ISO)
 │   │   │   │   ├── content-library-item/          # Content Library Item
 │   │   │   │   ├── certificate/                   # Certificados (SSL/TLS)
 │   │   │   │   ├── truststore/                    # Trust Store (CA certs)
 │   │   │   │   ├── encryption/                    # Encryption (VM encryption)
 │   │   │   │   ├── vm-encryption/                 # VM Encryption
 │   │   │   │   ├── vsan-encryption/               # vSAN Encryption
 │   │   │   │   ├── host-encryption/               # Host Encryption
 │   │   │   │   ├── kmip/                          # KMIP (Key Management)
 │   │   │   │   ├── kmip-cluster/                  # KMIP Cluster
 │   │   │   │   ├── kmip-server/                   # KMIP Server
 │   │   │   │   ├── kmip-client/                   # KMIP Client
 │   │   │   │   ├── kmip-provider/                 # KMIP Provider
 │   │   │   │   ├── kmip-certificate/              # KMIP Certificate
 │   │   │   │   ├── kmip-key/                      # KMIP Key
 │   │   │   │   ├── kmip-key-provider/             # KMIP Key Provider
 │   │   │   │   ├── kmip-key-server/               # KMIP Key Server
 │   │   │   │   ├── kmip-key-client/               # KMIP Key Client
 │   │   │   │   ├── kmip-key-store/                # KMIP Key Store
 │   │   │   │   ├── kmip-key-vault/                # KMIP Key Vault
 │   │   │   │   ├── kmip-key-rotation/             # KMIP Key Rotation
 │   │   │   │   ├── kmip-key-backup/               # KMIP Key Backup
 │   │   │   │   ├── kmip-key-recovery/             # KMIP Key Recovery
 │   │   │   │   └── firewall/                      # Firewall (NSX, Distributed Firewall)
 │   │   │   ├── automation/                      # 🤖 Automatización
 │   │   │   │   ├── orchestration/                 # Orchestration (vRO, vRA)
 │   │   │   │   ├── workflow/                      # Workflow (vRO workflows)
 │   │   │   │   ├── action/                        # Action (vRO actions)
 │   │   │   │   ├── policy/                        # Policy (vRA policies)
 │   │   │   │   ├── blueprint/                     # Blueprint (vRA blueprints)
 │   │   │   │   ├── catalog/                       # Catalog (vRA catalog)
 │   │   │   │   ├── deployment/                    # Deployment (vRA deployments)
 │   │   │   │   ├── resource/                      # Resource (vRA resources)
 │   │   │   │   ├── machine/                       # Machine (vRA machines)
 │   │   │   │   ├── software/                      # Software (vRA software)
 │   │   │   │   ├── network/                       # Network (vRA network)
 │   │   │   │   ├── storage/                       # Storage (vRA storage)
 │   │   │   │   ├── load-balancer/                 # Load Balancer (vRA LB)
 │   │   │   │   ├── security/                      # Security (vRA security)
 │   │   │   │   ├── monitoring/                    # Monitoring (vRA monitoring)
 │   │   │   │   ├── event/                         # Event (vRO events)
 │   │   │   │   ├── trigger/                       # Trigger (vRO triggers)
 │   │   │   │   ├── schedule/                      # Schedule (vRO schedules)
 │   │   │   │   ├── script/                        # Script (vRO scripts)
 │   │   │   │   ├── plugin/                        # Plugin (vRO plugins)
 │   │   │   │   ├── package/                       # Package (vRO packages)
 │   │   │   │   ├── module/                        # Module (vRO modules)
 │   │   │   │   ├── library/                       # Library (vRO libraries)
 │   │   │   │   ├── configuration/                 # Configuration (vRO config)
 │   │   │   │   ├── credential/                    # Credential (vRO credentials)
 │   │   │   │   ├── certificate/                   # Certificate (vRO certs)
 │   │   │   │   ├── key/                           # Key (vRO keys)
 │   │   │   │   ├── secret/                        # Secret (vRO secrets)
 │   │   │   │   ├── token/                         # Token (vRO tokens)
 │   │   │   │   ├── session/                       # Session (vRO sessions)
 │   │   │   │   ├── user/                          # User (vRO users)
 │   │   │   │   ├── group/                         # Group (vRO groups)
 │   │   │   │   ├── permission/                    # Permission (vRO permissions)
 │   │   │   │   ├── role/                          # Role (vRO roles)
 │   │   │   │   ├── tenant/                        # Tenant (vRO tenants)
 │   │   │   │   ├── organization/                  # Organization (vRO orgs)
 │   │   │   │   ├── project/                       # Project (vRO projects)
 │   │   │   │   ├── team/                          # Team (vRO teams)
 │   │   │   │   └── member/                        # Member (vRO members)
 │   │   │   ├── governance/                      # 📋 Gobernanza
 │   │   │   │   ├── folder/                        # Folders (organización)
 │   │   │   │   ├── tags/                          # Tags (etiquetas)
 │   │   │   │   ├── custom-attribute/              # Custom Attributes (atributos personalizados)
 │   │   │   │   ├── annotation/                    # Annotations (notas)
 │   │   │   │   ├── custom-field/                  # Custom Fields (campos personalizados)
 │   │   │   │   ├── metadata/                      # Metadata (metadatos)
 │   │   │   │   ├── label/                         # Labels (etiquetas)
 │   │   │   │   ├── category/                      # Categories (categorías)
 │   │   │   │   ├── classification/                # Classification (clasificación)
 │   │   │   │   ├── compliance/                    # Compliance (cumplimiento)
 │   │   │   │   ├── security/                      # Security (seguridad)
 │   │   │   │   ├── audit/                         # Audit (auditoría)
 │   │   │   │   ├── log/                           # Log (registros)
 │   │   │   │   ├── monitoring/                    # Monitoring (monitoreo)
 │   │   │   │   ├── alert/                         # Alert (alertas)
 │   │   │   │   ├── notification/                  # Notification (notificaciones)
 │   │   │   │   ├── report/                        # Report (reportes)
 │   │   │   │   ├── dashboard/                     # Dashboard (paneles)
 │   │   │   │   ├── view/                          # View (vistas)
 │   │   │   │   ├── filter/                        # Filter (filtros)
 │   │   │   │   ├── search/                        # Search (búsqueda)
 │   │   │   │   ├── query/                         # Query (consultas)
 │   │   │   │   ├── template/                      # Template (plantillas)
 │   │   │   │   └── policy/                        # Policy (políticas)
 │   │   │   ├── hybrid/                          # 🔄 Híbrido
 │   │   │   │   ├── vmc/                           # VMC on AWS
 │   │   │   │   ├── vmw-cloud/                     # VMware Cloud
 │   │   │   │   ├── vmw-alibaba/                   # VMware Cloud on Alibaba
 │   │   │   │   ├── vmw-aws/                       # VMware Cloud on AWS
 │   │   │   │   ├── vmw-azure/                     # VMware Cloud on Azure
 │   │   │   │   ├── vmw-gcp/                       # VMware Cloud on GCP
 │   │   │   │   ├── vmw-ibm/                       # VMware Cloud on IBM
 │   │   │   │   ├── vmw-oracle/                    # VMware Cloud on Oracle
 │   │   │   └── kubernetes/                      # ☸️ Kubernetes en vSphere
 │   │   │       ├── rke2/                         # RKE2 Cluster (multi-AZ)
 │   │   │       │   ├── master/                    # Master nodes (control plane)
 │   │   │       │   ├── worker/                    # Worker nodes (workers)
 │   │   │       │   ├── storage/                   # Storage nodes (storage)
 │   │   │       │   ├── nfs/                       # NFS server (storage)
 │   │   │       │   ├── lb/                        # Load Balancer (Kube-VIP)
 │   │   │       │   ├── ingress/                   # Ingress Controller (nginx)
 │   │   │       │   ├── cert-manager/              # Cert Manager (SSL)
 │   │   │       │   ├── metallb/                   # MetalLB (LoadBalancer)
 │   │   │       │   ├── csi/                       # CSI (Container Storage Interface)
 │   │   │       │   └── cni/                       # CNI (Container Network Interface)
 │   │   │       ├── tkg/                          # Tanzu Kubernetes Grid
 │   │   │       │   ├── cluster/                   # TKG Cluster
 │   │   │       │   ├── nodepool/                  # TKG Node Pool
 │   │   │       │   ├── workload/                  # TKG Workload Cluster
 │   │   │       │   ├── supervisor/                # TKG Supervisor Cluster
 │   │   │       │   ├── namespace/                 # TKG Namespace
 │   │   │       │   ├── storage/                   # TKG Storage
 │   │   │       │   ├── network/                   # TKG Network
 │   │   │       │   ├── security/                  # TKG Security
 │   │   │       │   ├── monitoring/                # TKG Monitoring
 │   │   │       │   ├── logging/                   # TKG Logging
 │   │   │       │   ├── backup/                    # TKG Backup
 │   │   │       │   ├── restore/                   # TKG Restore
 │   │   │       │   ├── upgrade/                   # TKG Upgrade
 │   │   │       │   ├── scale/                     # TKG Scale
 │   │   │       │   ├── autoscaling/               # TKG Autoscaling
 │   │   │       │   ├── addon/                     # TKG Addon
 │   │   │       │   ├── extension/                 # TKG Extension
 │   │   │       │   ├── operator/                  # TKG Operator
 │   │   │       │   ├── controller/                # TKG Controller
 │   │   │       │   ├── webhook/                   # TKG Webhook
 │   │   │       │   ├── admission/                 # TKG Admission
 │   │   │       │   ├── mutating/                  # TKG Mutating
 │   │   │       │   ├── validating/                # TKG Validating
 │   │   │       │   ├── conversion/                # TKG Conversion
 │   │   │       │   └── defaulting/                # TKG Defaulting
```
---
### Declaración

Los directorios de estos modulos se complementaran con el tiempo, si usted considera que puede ser un aportar en unos de estos modules, contacteme.

Los directorios que esten para su referencia, en este readme se indocaran con un ✅ y aquellos que se desconoce si se implementaran con ⚠️.

---

Mario Fribla

***DevOps***
