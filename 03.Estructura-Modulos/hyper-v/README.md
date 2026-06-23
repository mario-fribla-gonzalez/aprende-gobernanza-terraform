# Repostorio de Gobernanza Terraform.

## On-Premise Microsoft Hyper-V (Hyper-V), estructura del directorio de modulos de Terraform.

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
 │   ├── hyperv/
 │   │   ├── modules/
 │   │   │   │
 │   │   │   ├── host/                             # 🖥️ Host Hyper-V
 │   │   │   │    ├── hyperv-host/                   # Configuración del host (gestión)
 │   │   │   │    ├── hyperv-features/               # Características de Hyper-V (habilitación)
 │   │   │   │    ├── hyperv-services/               # Servicios de Hyper-V (VM Management, etc.)
 │   │   │   │    ├── hyperv-firewall/               # Reglas de firewall para WinRM/SSH
 │   │   │   │    └── hyperv-winrm/                  # Configuración de WinRM (acceso remoto)
 │   │   │   ├── compute/                          # 💻 Cómputo
 │   │   │   │   ├── vm/                             # Virtual Machine (VM individual)
 │   │   │   │   ├── vm-generation/                  # VM Generación 1 o 2
 │   │   │   │   ├── vm-cluster/                     # Clúster de VMs (múltiples instancias)
 │   │   │   │   ├── vm-checkpoint/                  # Checkpoints (Snapshots)
 │   │   │   │   ├── vm-replication/                 # Replicación de VM (DR)
 │   │   │   │   ├── vm-export/                      # Exportación de VM
 │   │   │   │   ├── vm-import/                      # Importación de VM
 │   │   │   │   ├── vm-remote/                      # VM en host remoto (SSH/WinRM)
 │   │   │   │   └── vm-migration/                   # Migración de VM (Live Migration)
 │   │   │   ├── storage/                          # 💾 Almacenamiento
 │   │   │   │   ├── vhd/                            # Virtual Hard Disk (VHD/VHDX)
 │   │   │   │   ├── vhd-dynamic/                    # VHD Dinámico (expansible)
 │   │   │   │   ├── vhd-fixed/                      # VHD Fijo (pre-asignado)
 │   │   │   │   ├── vhd-differencing/               # VHD de Diferenciación (parent-child)
 │   │   │   │   ├── vhd-resize/                     # Redimensionamiento de VHD
 │   │   │   │   ├── vhd-convert/                    # Conversión de VHD (VHD → VHDX)
 │   │   │   │   ├── iso-image/                      # ISO Image (CD/DVD)
 │   │   │   │   ├── image-file/                     # Image File (VHDX/ISO)
 │   │   │   │   ├── storage-pool/                   # Storage Pool (almacenamiento agrupado)
 │   │   │   │   └── storage-reservation/            # Reserva de almacenamiento
 │   │   │   ├── networking/                       # 🌐 Redes
 │   │   │   │   ├── virtual-switch/                 # Virtual Switch (External/Internal/Private)
 │   │   │   │   ├── virtual-switch-external/        # Switch Externo (conexión a red física)
 │   │   │   │   ├── virtual-switch-internal/        # Switch Interno (comunicación host-VMs)
 │   │   │   │   ├── virtual-switch-private/         # Switch Privado (solo entre VMs)
 │   │   │   │   ├── virtual-switch-nat/             # Switch NAT (traducción de direcciones)
 │   │   │   │   ├── network-adapter/                # Network Adapter (NIC virtual)
 │   │   │   │   ├── vlan/                           # VLAN (trunk/access)
 │   │   │   │   ├── nat-static-mapping/             # NAT Static Mapping (port forwarding)
 │   │   │   │   ├── network-team/                   # NIC Teaming (agregación de enlaces)
 │   │   │   │   ├── mac-address/                    # Dirección MAC (gestión)
 │   │   │   │   └── dhcp/                           # DHCP Server (servidor DHCP)
 │   │   │   ├── security/                         # 🔒 Seguridad
 │   │   │   │   ├── secure-boot/                    # Secure Boot (protección de arranque)
 │   │   │   │   ├── encryption/                     # VM Encryption (BitLocker)
 │   │   │   │   ├── shielded-vm/                    # Shielded VM (VM protegida)
 │   │   │   │   ├── guardian/                       # Host Guardian Service (HGS)
 │   │   │   │   ├── key-protector/                  # Key Protector (TPM)
 │   │   │   │   ├── vTPM/                           # Virtual TPM (Trusted Platform Module)
 │   │   │   │   ├── permissions/                    # Permisos de Hyper-V (ACL)
 │   │   │   │   ├── roles/                          # Roles (Hyper-V Administrators)
 │   │   │   │   ├── groups/                         # Grupos de seguridad
 │   │   │   │   └── certificates/                   # Certificados SSL/TLS (WinRM/SSH)
 │   │   │   ├── automation/                       # 🤖 Automatización
 │   │   │   │   ├── dsc/                            # PowerShell DSC (Desired State Configuration)
 │   │   │   │   ├── ansible/                        # Ansible (provisioning)
 │   │   │   │   ├── puppet/                         # Puppet (configuration management)
 │   │   │   │   ├── chef/                           # Chef (configuration management)
 │   │   │   │   ├── custom-script/                  # Custom Scripts (PowerShell/Bash)
 │   │   │   │   ├── cloud-init/                     # Cloud-Init (Linux cloud images)
 │   │   │   │   ├── sysprep/                        # Sysprep (Windows generalization)
 │   │   │   │   └── unattend/                       # Unattend.xml (Windows automated setup)
 │   │   │   ├── governance/                       # 📋 Gobernanza
 │   │   │   │   ├── tags/                           # Tags (etiquetas de organización)
 │   │   │   │   ├── custom-attributes/              # Custom Attributes (atributos personalizados)
 │   │   │   │   ├── annotations/                    # Annotations (notas)
 │   │   │   │   ├── folders/                        # Folders (organización de VMs)
 │   │   │   │   ├── resource-groups/                # Resource Groups (agrupación de VMs)
 │   │   │   │   ├── quotas/                         # Quotas (límites de recursos)
 │   │   │   │   ├── reservations/                   # Reservations (reserva de CPU/RAM)
 │   │   │   │   ├── limits/                         # Limits (límites de recursos)
 │   │   │   │   ├── monitoring/                     # Monitoring (monitoreo de VMs)
 │   │   │   │   ├── logging/                        # Logging (registros de eventos)
 │   │   │   │   ├── reporting/                      # Reporting (reportes de VMs)
 │   │   │   │   └── audit/                          # Audit (auditoría de VMs)
 │   │   │   └── container/                        # 🐳 Contenedores
 │   │   │       ├── windows-containers/             # Windows Containers (containers nativos)
 │   │   │       ├── hyper-v-containers/             # Hyper-V Containers (aislamiento)
 │   │   │       ├── docker/                         # Docker Engine en Windows
 │   │   │       ├── kubernetes/                     # Kubernetes en Hyper-V (minikube, k3s)
 │   │   │       ├── images/                         # Container Images (gestión)
 │   │   │       └── registries/                     # Container Registries (ACR, Docker Hub)
```
---
### Declaración

Los directorios de estos modulos se complementaran con el tiempo, si usted considera que puede ser un aportar en unos de estos modules, contacteme.

Los directorios que esten para su referencia, en este readme se indocaran con un ✅ y aquellos que se desconoce si se implementaran con ⚠️.

---

Mario Fribla

***DevOps***
