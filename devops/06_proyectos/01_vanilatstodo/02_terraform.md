
1. tiene reglas especificas de espacios y alineado de simbologia como el simbolo igual
2. instale algunas extensiones en vscode para que me ayudara con el formateo pero ninguna funciono
3. la forma que logré resolver es crear un contenedor local y correr un script que me ayuda en los distintos stages
4. informacion sobre estos elementos estan en mi repo de devopssources
5. detalle sobre los comandos de terraform:

# Understanding Terraform Command Purposes

Let's clarify what each command does:

### 1. `fmt`
- Rewrites Terraform configuration files to a canonical format
- Applies consistent indentation and spacing
- Does NOT validate configuration logic
```bash
./tf-codeformat.sh fmt
```

### 2. `init`
- Initializes a working directory containing Terraform configuration files
- Downloads required providers
- Sets up backend for state storage
- Does NOT validate configuration logic
```bash
./tf-codeformat.sh init
```

### 3. `validate`
- Checks configuration syntax
- Validates internal consistency of attribute names and value types
- Checks resource references
- Does NOT access any remote services, APIs, or state
```bash
./tf-codeformat.sh validate
```

### 4. `plan`
- Shows what changes Terraform will make to infrastructure
- Checks against actual resource state
- Requires provider authentication (AWS credentials)
- Actually connects to AWS to compare state
```bash
./tf-codeformat.sh plan
```

### Command Flow for Validation
```mermaid
graph LR
    A[fmt] -->|Format Code| B[init]
    B -->|Download Providers| C[validate]
    C -->|Check Syntax| D[plan]
    D -->|Preview Changes| E[apply]
```

`validate` only checks syntax and logic, while `plan` shows actual infrastructure changes.