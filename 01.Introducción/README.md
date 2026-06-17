# Repositorio de Gobernanza Terraform.


# 1. Introducción.

# ¿Qué es Terraform? - Descripción Detallada con Arquitectura.

## 📌 Definición Fundamental.

**Terraform** es una herramienta de **Infraestructura como Código (IaC)** de código abierto desarrollada por HashiCorp que permite definir, provisionar y gestionar infraestructura en cualquier nube o entorno on-premise utilizando un lenguaje declarativo llamado **HCL (HashiCorp Configuration Language)**.

### Características Esenciales:
- **Declarativo**: Describes el "qué" (estado final deseado), no el "cómo" (pasos para llegar).
- **Independiente de plataforma**: Mismo flujo para AWS, Azure, GCP, vSphere, etc.
- **Manejo de estado**: Mantiene un mapa de tus recursos reales vs. configuración.
- **Plan de ejecución**: Muestra qué cambios hará antes de aplicarlos.
- **Grafo de dependencias**: Calcula automáticamente el orden correcto de operaciones.

---

## 🏗️ Arquitectura Flowchart de Terraform.

### Diagrama de Alto Nivel.

```mermaid

flowchart TB
    subgraph USER["👤 USUARIO/DESARROLLADOR"]
        HCL["📝 Archivos .tf\n(HCL Configuration)"]
        CLI["💻 Terraform CLI\n(apply, plan, destroy)"]
    end

    subgraph CORE["⚙️ NÚCLEO DE TERRAFORM"]
        PARSER["📖 Parser & Loader\n(Lectura de archivos)"]
        GRAPH["🕸️ Graph Builder\n(Construye dependencias)"]
        PLAN["📋 Plan Engine\n(Compara estado actual vs deseado)"]
        EXEC["🚀 Execution Engine\n(Ejecuta en paralelo)"]
    end

    subgraph STATE["🗃️ SISTEMA DE ESTADO"]
        LOCAL["💾 Estado Local\n(terraform.tfstate)"]
        REMOTE["☁️ Backend Remoto\n(S3, GCS, AzureRM, Consul)"]
        LOCK["🔒 State Locking\n(DynamoDB, etcd, Table Service)"]
    end

    subgraph PROVIDERS["🔌 PROVEEDORES (Providers)"]
        AWS["AWS Provider"]
        AZURE["Azure Provider"]
        GCP["GCP Provider"]
        ONS["On-Premise\n(vSphere, K8s, etc.)"]
    end

    subgraph RESOURCES["🖥️ RECURSOS FÍSICOS/VIRTUALES"]
        R_AWS["EC2, S3, RDS\nLambda, VPC"]
        R_AZURE["VM, Storage\nAKS, Functions"]
        R_GCP["GCE, GCS\nGKE, Cloud Run"]
        R_ONS["VMware VMs\nK8s Pods, LB"]
    end

    HCL --> PARSER
    CLI --> PARSER
    CLI --> EXEC
    
    PARSER --> GRAPH
    GRAPH --> PLAN
    PLAN <--> STATE
    STATE --> LOCAL
    STATE --> REMOTE
    REMOTE --> LOCK
    
    PLAN --> EXEC
    EXEC --> PROVIDERS
    PROVIDERS --> AWS
    PROVIDERS --> AZURE
    PROVIDERS --> GCP
    PROVIDERS --> ONS
    
    AWS --> R_AWS
    AZURE --> R_AZURE
    GCP --> R_GCP
    ONS --> R_ONS
    
    R_AWS -.->|"API Calls"| PLAN
    R_AZURE -.->|"API Calls"| PLAN
    R_GCP -.->|"API Calls"| PLAN
    R_ONS -.->|"API Calls"| PLAN

    style USER fill:#e1f5fe
    style CORE fill:#fff3e0
    style STATE fill:#f3e5f5
    style PROVIDERS fill:#e8f5e9
    style RESOURCES fill:#ffebee
```

---

## 🔄 Flujo de Ejecución Detallado.

### Diagrama de Secuencia (Terraform Apply).

```mermaid
sequenceDiagram
    participant User as Usuario
    participant CLI as Terraform CLI
    participant State as Backend de Estado
    participant Graph as Motor de Grafos
    participant Providers as Providers
    participant APIs as APIs de Nube

    User->>CLI: terraform apply
    CLI->>CLI: terraform init (si es necesario)
    CLI->>State: terraform refresh (obtener estado actual)
    State-->>CLI: Estado actual (JSON)
    
    CLI->>CLI: Cargar archivos .tf
    CLI->>Graph: Construir grafo de dependencias
    Graph-->>CLI: Grafo resuelto
    
    CLI->>CLI: Calcular diferencias<br/>(Estado actual vs Configuración)
    CLI->>User: Mostrar Plan de ejecución<br/>(+ create, ~ update, - destroy)
    
    User->>CLI: Confirmar (yes)
    
    CLI->>Providers: Iniciar ejecución en paralelo
    loop Para cada recurso en orden
        Providers->>APIs: API Call (Create/Update/Delete)
        APIs-->>Providers: Respuesta + IDs
        Providers-->>CLI: Resultado del recurso
        CLI->>State: Actualizar estado en memoria
    end
    
    CLI->>State: Persistir nuevo estado
    State-->>CLI: Confirmación escritura
    
    CLI->>User: ✅ Apply complete!<br/>Resources: X added, Y changed, Z destroyed
```

---

## 🧩 Componentes Arquitectónicos Clave.

### 1. **Núcleo (Core)** - Diagrama Interno.

```mermaid
flowchart LR
    subgraph INPUT["ENTRADA"]
        CONFIG["📁 Configuración\n(main.tf, variables.tf)"]
        STATE_IN["📄 Estado Actual\n(terraform.tfstate)"]
    end

    subgraph TERRAFORM_CORE["TERRAFORM CORE"]
        direction TB
        LOADER["Loader\n(parset HCL + JSON)"]
        
        subgraph EVAL["Evaluación"]
            VAR_EVAL["Variables Evaluation"]
            MOD_EXP["Module Expansion"]
            COUNT_EVAL["count & for_each\nEvaluation"]
        end
        
        GRAPH_BUILDER["Graph Builder\n(DFS + Topological Sort)"]
        
        subgraph DIFF["Diff Engine"]
            COMPARE["Comparison\n(Actual vs Deseado)"]
            ACTION_DETECT["Action Detection\n(Create/Update/Delete)"]
        end
        
        EXECUTOR["Executor\n(Worker Pool + Parallelism)"]
    end

    subgraph OUTPUT["SALIDA"]
        PLAN["📋 Plan\n(terraform plan)"]
        STATE_OUT["📄 Nuevo Estado\n(Actualizado)"]
        LOGS["📝 Execution Logs"]
    end

    CONFIG --> LOADER
    LOADER --> VAR_EVAL
    VAR_EVAL --> MOD_EXP
    MOD_EXP --> COUNT_EVAL
    COUNT_EVAL --> GRAPH_BUILDER
    
    STATE_IN --> COMPARE
    
    GRAPH_BUILDER --> COMPARE
    COMPARE --> ACTION_DETECT
    ACTION_DETECT --> PLAN
    ACTION_DETECT --> EXECUTOR
    EXECUTOR --> STATE_OUT
    EXECUTOR --> LOGS

    style TERRAFORM_CORE fill:#fff9c4
```

### 2. **Sistema de Estado (State Management)**.

```mermaid
flowchart TB
    subgraph STATE_MGMT["GESTIÓN DE ESTADO"]
        
        subgraph BACKEND_CONFIG["Configuración Backend"]
            BC["backend s3 bucket = tf-state-prod  key    = vpc/terraform.tfstate  region = us-east-1  dynamodb_table = tf-locks}"]
        end
        
        subgraph OPERATIONS["Operaciones"]
            OP1["📤 Push: Guardar estado"]
            OP2["📥 Pull: Leer estado"]
            OP3["🔒 Lock: Evitar escritura concurrente"]
            OP4["🔓 Unlock: Liberar lock"]
        end
        OP1 --> OP2 
        OP2 --> OP3
        OP3 --> OP4
        subgraph STATE_FORMAT["Formato de Estado (JSON)"]
            SF["{version 4, terraform_version: 1.5.0,
                resources: [ { mode: managed,
                               type: aws_instance,
                               name: web,
                               instances: [ { attributes: { id: i-12345,
                                                           ami: ami-0c55b159cbfafe1f0,
                                                 instance_type: t2.micro } } ] } ] }"]
        end
        
        BC --> OPERATIONS
        OPERATIONS --> STATE_FORMAT
    end
    
    style STATE_MGMT fill:#e8eaf6
```

### 3. **Providers y Plugin System**.

```mermaid
flowchart TD
    subgraph PLUGIN_SYS["SISTEMA DE PLUGINS"]
        CLI_TF["Terraform CLI"]
        
        subgraph PLUGIN_DIR["Directorio de Plugins: ~/.terraform.d/plugins/"]
            P1["provider-registry/namespace/version\n ej: registry.terraform.io/hashicorp/aws/5.0.0"]
            P2["🔌 provider-aws_v5.0.0_x5"]
            P3["🔌 provider-azurerm_v3.0.0_x5"]
            P4["🔌 provider-google_v5.0.0_x5"]
        end
        
        subgraph GRPC["gRPC Communication"]
            G1["Protocol Buffers"]
            G2["Streaming de operaciones"]
            G3["Handshake & Auth"]
        end
        
        subgraph PROVIDER_IMPL["Implementación del Provider"]
            RESOURCES["Resource CRUD\n(Create, Read, Update, Delete)"]
            DATA_SOURCES["Data Sources\n(Búsqueda de recursos existentes)"]
            SCHEMA["Resource Schema\n(Validación de argumentos)"]
        end
    end
    
    CLI_TF -->|"gRPC Plugin Discovery"| PLUGIN_DIR
    PLUGIN_DIR -->|"gRPC Calls"| GRPC
    GRPC --> PROVIDER_IMPL
    
    PROVIDER_IMPL -->|"HTTP/HTTPS"| CLOUD_API["☁️ Cloud Provider APIs"]
    
    style PLUGIN_SYS fill:#fce4ec
```

---

## 📊 Diagrama de Ciclo de Vida de un Recurso.

```mermaid
stateDiagram-v2
    [*] --> Configurado: Definir en HCL
    
    Configurado --> Planeado: terraform plan
    Planeado --> Aprobado: User confirmation
    
    Aprobado --> Creando: terraform apply
    Creando --> Creado: API call success
    Creado --> Drift: Cambio externo
    
    Drift --> Actualizando: terraform apply -refresh-only
    Actualizando --> Creado
    
    Creado --> Destruyendo: terraform destroy
    Destruyendo --> [*]
    
    Creado --> Actualizando: terraform apply (modificar .tf)
    Actualizando --> Creado
    
    note right of Creado
        Estado estable
        Registrado en .tfstate
    end note
    
    note right of Drift
        Recurso modificado
        fuera de Terraform
    end note
```
---

## DIAGRAMA DE ARQUITECTURA DE DATOS (Cómo fluye la información).

```mermaid
flowchart TB
    subgraph SOURCE["📁 CÓDIGO FUENTE"]
        direction TB
        TF_FILES["main.tf<br/>variables.tf<br/>outputs.tf"]
        VARS["terraform.tfvars<br/>(valores específicos)"]
        SCRIPTS["scripts/startup.sh<br/>(script de bootstrap)"]
    end
    
    subgraph TERRAFORM_ENGINE["⚙️ MOTOR DE TERRAFORM"]
        direction TB
        PARSER["Parser HCL<br/>→ AST (Abstract Syntax Tree)"]
        
        subgraph EVAL["Evaluación"]
            VAR_EVAL["Variables:<br/>project_id, region, zone"]
            FUNC_EVAL["Funciones:<br/>templatefile(), timestamp()"]
            DEP_RES["Resolución de<br/>dependencias"]
        end
        
        STATE_MEM["Estado en Memoria<br/>(JSON temporal)"]
        DIFF_ENGINE["Motor de Diff<br/>→ Plan de ejecución"]
    end
    
    subgraph GCP_APIS["☁️ GCP APIs"]
        CM["Compute Engine API<br/>/instances, /disks, /firewalls"]
        NET["Network API<br/>/networks, /subnetworks"]
        IAM["IAM API<br/>/serviceAccounts, /roles"]
        SM["Secret Manager API<br/>/secrets"]
    end
    
    subgraph GCP_RESOURCES["🖥️ RECURSOS GCP"]
        NET_RES["VPC Network<br/>Subnet"]
        FW_RES["Firewall Rules<br/>(HTTP, HTTPS, SSH)"]
        VM["VM Instance<br/>- Disco boot (30GB)<br/>- Disco data (50GB SSD)<br/>- IP estática: 34.123.45.67"]
        SA["Service Account<br/>+ IAM Roles"]
        SECRET["Secret Manager<br/>(Clave SSH privada)"]
    end
    
    subgraph OUTPUT["📤 RESULTADOS"]
        IP["IP Pública: 34.123.45.67"]
        SSH["Comando SSH:<br/>ssh gcp-user@34.123.45.67"]
        URL["GCP Console URL"]
        LOG["deployment.log"]
    end
    
    TF_FILES --> PARSER
    VARS --> VAR_EVAL
    SCRIPTS --> FUNC_EVAL
    
    PARSER --> EVAL
    EVAL --> DEP_RES
    DEP_RES --> DIFF_ENGINE
    
    STATE_MEM <--> DIFF_ENGINE
    
    DIFF_ENGINE -->|Create/Update/Delete| GCP_APIS
    
    GCP_APIS --> CM
    GCP_APIS --> NET
    GCP_APIS --> IAM
    GCP_APIS --> SM
    
    CM --> VM
    NET --> NET_RES
    CM --> FW_RES
    IAM --> SA
    SM --> SECRET
    
    VM --> IP
    VM --> SSH
    VM --> URL
    
    VM -->|Estado actualizado| STATE_MEM
    STATE_MEM --> LOG
    
    style SOURCE fill:#e3f2fd
    style TERRAFORM_ENGINE fill:#fff3e0
    style GCP_APIS fill:#e8f5e9
    style GCP_RESOURCES fill:#f3e5f5
    style OUTPUT fill:#ffebee
```
---

## 🎯 Ejemplo Concreto con Flujo de Datos.

### Caso: Desplegar una VM en AWS.

```mermaid
flowchart LR
    subgraph STEP1["1️⃣ Definición"]
        HCL_CODE["
resource 'aws_instance' 'web' {
  ami           = 'ami-0c55b159cbfafe1f0'
  instance_type = 't2.micro'
  tags = {Name = 'web-server' } } "]
    end
    
    subgraph STEP2["2️⃣ Planificación"]
        PLAN_OUT["+ create aws_instance.web
              ami: 'ami-0c55b159cbfafe1f0'
    instance_type: 't2.micro'
        tags.Name: 'web-server' "]
    end
    
    subgraph STEP3["3️⃣ Ejecución"]
        API_CALL["POST /ec2/run-instances\nBody: {ImageId: 'ami...', InstanceType: 't2.micro'} "]
    end
    
    subgraph STEP4["4️⃣ Estado"]
        STATE_JSON["'aws_instance.web': { id: 'i-0a1b2c3d4e5f',
                             public_ip: '54.123.45.67' } "]
    end
    
    HCL_CODE -->|"terraform plan"| PLAN_OUT
    PLAN_OUT -->|"terraform apply"| API_CALL
    API_CALL -->|"response: instance_id"| STATE_JSON
    STATE_JSON -->|"referenciable en otros recursos"| HCL_CODE
    
    style STEP1 fill:#bbdefb
    style STEP2 fill:#c8e6c9
    style STEP3 fill:#ffecb3
    style STEP4 fill:#d1c4e9
```

### Caso: Desplegar una VM en GCP.

```mermaid
flowchart LR
    subgraph STEP1["1️⃣ Definición"]
        HCL_CODE[" resource 'google_compute_instance' 'web' {
  machine_type = 'e2-micro'
  zone         = 'us-central1-a'
  boot_disk { initialize_params {
      image = 'ubuntu-os-cloud/ubuntu-2204-lts' }  }
  network_interface { access_config {} }
  tags = ['http-server'] } "]
    end
    
    subgraph STEP2["2️⃣ Planificación"]
        PLAN_OUT["+ create google_compute_instance.web
         machine_type: 'e2-micro' 
         zone: 'us-central1-a' 
         boot_disk.image: 'ubuntu-2204-lts' 
         tags: ['http-server'] 
         + create google_compute_address.web 
         (IP pública automática)"]
    end
    
    subgraph STEP3["3️⃣ Ejecución"]
        API_CALL["POST /compute/v1/projects/{project}/zones/us-central1-a/instances
            Body: { 'machineType': 'zones/us-central1-a/machineTypes/e2-micro',
  'disks': [{'boot': true, 'initializeParams': {'sourceImage': 'projects/ubuntu-os-cloud/global/images/ubuntu-2204-lts'}}],
  'networkInterfaces': [{'accessConfigs': [{'type': 'ONE_TO_ONE_NAT'}]}],
  'tags': {'items': ['http-server']} }"]
    end
    
    subgraph STEP4["4️⃣ Estado"]
        STATE_JSON["'google_compute_instance.web': {
  id: 'projects/my-project/zones/us-central1-a/instances/web-server',
  instance_id: '1234567890123456789', network_interface: [{ access_config: [{nat_ip: '34.123.45.67'}],
    network_ip: '10.128.0.2' }],self_link: 'https://www.googleapis.com/compute/v1/projects/...'}"]
    end
    
    HCL_CODE -->|"terraform plan"| PLAN_OUT
    PLAN_OUT -->|"terraform apply"| API_CALL
    API_CALL -->|"response: instance_id + nat_ip"| STATE_JSON
    STATE_JSON -->|"referenciable en otros recursos"| HCL_CODE
    
    style STEP1 fill:#bbdefb
    style STEP2 fill:#c8e6c9
    style STEP3 fill:#ffecb3
    style STEP4 fill:#d1c4e9
```
---

### 🔄 Comparación visual del flujo: AWS vs GCP.

```mermaid
flowchart TB
    subgraph AWS["☁️ AWS FLOW"]
        direction LR
        A1["resource 'aws_instance'"] --> A2["terraform plan"] --> A3["POST /ec2/run-instances"] --> A4["state: {public_ip}"]
    end
    
    subgraph GCP["☁️ GCP FLOW"]
        direction LR
        G1["resource 'google_compute_instance'"] --> G2["terraform plan"] --> G3["POST /compute/.../instances"] --> G4["state: {nat_ip}"]
    end
    
    style AWS fill:#ffecb3,color:#000
    style GCP fill:#bbdefb,color:#fff
```

## 📝 Diferencias clave AWS vs GCP

| Elemento | AWS | GCP |
|----------|-----|-----|
| **Recurso principal** | `aws_instance` | `google_compute_instance` |
| **Tipo de máquina** | `t2.micro` | `e2-micro` |
| **Imagen/AMI** | AMI ID (`ami-0c55...`) | Ruta de imagen (`ubuntu-os-cloud/ubuntu-2204-lts`) |
| **IP pública** | Automática (por defecto) | Requiere `access_config {}` explícito |
| **Firewall** | Security Group separado | Tags + Firewall Rules separados |
| **Zona/Región** | Solo región | Zona obligatoria (`us-central1-a`) |
| **ID del recurso** | `i-0a1b2c3d4e5f` | Ruta completa: `projects/.../instances/...` |
| **IP pública obtenida** | `public_ip` | `nat_ip` dentro de `access_config` |

---

## 🔐 Resumen de Conceptos Clave.

| Componente | Función | Ejemplo |
|------------|---------|---------|
| **Configuration (.tf)** | Define el estado deseado. | `resource "aws_s3_bucket" "google_compute_instance" "data" {...}` |
| **Provider** | Traduce Terraform → API específica. | `hashicorp/aws`, `hashicorp/azurerm` |
| **State** | Mapa de recursos reales. | `terraform.tfstate` (JSON) |
| **Backend** | Almacenamiento remoto del estado.| S3, GCS, Azure Storage, Consul |
| **Modules** | Contenedores reutilizables de recursos. | `module "vpc" { source = "terraform-aws-modules/vpc/aws" }` |
| **Plan** | Lista de cambios propuestos. | `terraform plan -out=tfplan` |

---

## 🎓 Entendimiento Final.

**Terraform NO es un simple script de aprovisionamiento**, sino un **orquestador declarativo con gestión de estado** que:

1. **Lee** tu configuración HCL
2. **Calcula** un grafo de dependencias automáticamente
3. **Compara** el estado actual con el deseado
4. **Genera** un plan de ejecución
5. **Ejecuta** solo las API calls necesarias, en orden óptimo y paralelo
6. **Persiste** el nuevo estado para futuras operaciones

---

Mario Fribla
***DevOps***
