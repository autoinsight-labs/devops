# AutoInsight API

## 🚀 Sobre o Projeto

A **AutoInsight API** é uma API RESTful desenvolvida em ASP.NET Core .NET 9.0 para gestão inteligente de pátios e frotas de motocicletas. A API fornece endpoints completos para gerenciamento de pátios, funcionários, veículos e suas relações, com integração ao banco de dados Oracle e documentação automática via OpenAPI/Scalar.

## 👥 Equipe de Desenvolvimento

| Nome | RM | E-mail | GitHub | LinkedIn |
|------|-------|---------|---------|----------|
| Arthur Vieira Mariano | RM554742 | arthvm@proton.me | [@arthvm](https://github.com/arthvm) | [arthvm](https://linkedin.com/in/arthvm/) |
| Guilherme Henrique Maggiorini | RM554745 | guimaggiorini@gmail.com | [@guimaggiorini](https://github.com/guimaggiorini) | [guimaggiorini](https://linkedin.com/in/guimaggiorini/) |
| Ian Rossato Braga | RM554989 | ian007953@gmail.com | [@iannrb](https://github.com/iannrb) | [ianrossato](https://linkedin.com/in/ianrossato/) |

## 🛠️ Tecnologias Utilizadas

### Stack Principal
- **.NET 9.0** - Framework principal
- **ASP.NET Core** - Minimal API
- **Entity Framework Core 9.0.4** - ORM
- **Oracle Database** - Banco de dados principal
- **AutoMapper 14.0.0** - Mapeamento de objetos
- **Scalar 2.3.0** - Documentação da API

### Arquitetura
- **Minimal API** - Implementação de rotas
- **Repository Pattern** - Abstração de acesso a dados
- **DTOs** - Transferência de dados
- **Migrations** - Controle de versão do banco

## 🗄️ Estrutura do Banco de Dados

O projeto utiliza Entity Framework Core com Oracle Database e inclui as seguintes entidades:

- **Yards** - Pátios de motocicletas
- **Vehicles** - Veículos/motocicletas
- **YardEmployees** - Funcionários dos pátios
- **YardVehicles** - Relação entre pátios e veículos
- **Addresses** - Endereços
- **QRCodes** - Códigos QR para identificação
- **Bookings** - Reservas
- **Models** - Modelos de motocicletas
- **EmployeeInvites** - Convites para funcionários

## 🚀 Como Executar o Projeto

### Pré-requisitos

- .NET 9.0 SDK
- .NET Entity Framework CLI
- Oracle Database
- Git

### Instalação

1. **Clone o repositório:**
   ```bash
   git clone https://github.com/autoinsight-labs/devops.git
   cd devops
   ```

2. **Configure a variável de ambiente:**
   ```bash
   # Crie um arquivo .env na raiz do projeto
   echo "ORACLE_CONNECTION_STRING=sua_connection_string_aqui" > .env
   ```

3. **Restaure as dependências:**
   ```bash
   dotnet restore
   ```

4. **Execute as migrations:**
   ```bash
   dotnet ef database update
   ```

5. **Execute o projeto:**
   ```bash
   dotnet run
   ```

6. **Acesse a documentação:**
   - Scalar UI: `http://localhost:5100/scalar`
   - OpenAPI JSON: `http://localhost:5100/openapi/v1.json`

### Sobre a Documentação OpenAPI/Scalar

- **Título e descrição**: O documento OpenAPI é gerado com título "AutoInsight API" e descrição completa do domínio e recursos.
- **Tags**: Endpoints organizados por `yard`, `employee` e `vehicle`.
- **Sumário e descrição**: Cada rota possui `.WithSummary()` e `.WithDescription()` detalhando comportamento, parâmetros e códigos de resposta.
- **OperationId**: Definida por rota para facilitar geração de clientes e rastreabilidade.
- **Exibição no Scalar**: Interface moderna para navegar por rotas, schemas e experimentar requisições.

## 📋 Rotas da API

### Health Check
| Método | Endpoint | Descrição | Retorno |
|--------|----------|-----------|---------|
| GET | `/health` | Verificação de saúde da API | 200 OK |

### 🏢 Yards (Pátios)
| Método | Endpoint | Descrição | Parâmetros                   | Retorno |
|--------|----------|-----------|------------------------------|---------|
| GET | `/yards` | Lista pátios com paginação | `pageNumber`, `pageSize`     | 200 OK, 400 BadRequest |
| GET | `/yards/{id}` | Busca pátio por ID | `id` (path)                  | 200 OK, 404 NotFound |
| POST | `/yards` | Cria novo pátio | Body: `YardDto`              | 201 Created, 500 InternalServerError |
| PUT | `/yards/{id}` | Atualiza pátio existente | `id` (path), Body: `YardDto` | 200 OK, 404 NotFound |
| DELETE | `/yards/{id}` | Remove pátio | `id` (path)                  | 204 NoContent, 404 NotFound |

### 👥 Employees (Funcionários)
| Método | Endpoint | Descrição | Parâmetros | Retorno |
|--------|----------|-----------|------------|---------|
| GET | `/yards/{id}/employees` | Lista funcionários do pátio | `id` (path), `pageNumber`, `pageSize` | 200 OK, 400 BadRequest, 404 NotFound |
| GET | `/yards/{id}/employees/{employeeId}` | Busca funcionário específico | `id`, `employeeId` (path) | 200 OK, 404 NotFound |
| POST | `/yards/{id}/employees` | Adiciona funcionário ao pátio | `id` (path), Body: `YardEmployeeDto` | 201 Created, 404 NotFound |
| PUT | `/yards/{id}/employees/{employeeId}` | Atualiza funcionário | `id`, `employeeId` (path), Body: `YardEmployeeDto` | 200 OK, 400 BadRequest, 404 NotFound |
| DELETE | `/yards/{id}/employees/{employeeId}` | Remove funcionário | `id`, `employeeId` (path) | 204 NoContent, 404 NotFound |

### 🏍️ Vehicles (Veículos)
| Método | Endpoint | Descrição | Parâmetros | Retorno |
|--------|----------|-----------|------------|---------|
| GET | `/vehicles` | Busca veículo por QR Code | `qrCodeId` (query) | 200 OK, 404 NotFound |
| GET | `/vehicles/{id}` | Busca veículo por ID | `id` (path) | 200 OK, 404 NotFound |

### 🏍️ Yard Vehicles (Veículos do Pátio)
| Método | Endpoint | Descrição | Parâmetros | Retorno |
|--------|----------|-----------|------------|---------|
| GET | `/yards/{id}/vehicles` | Lista veículos do pátio | `id` (path), `pageNumber`, `pageSize` | 200 OK, 400 BadRequest, 404 NotFound |
| GET | `/yards/{id}/vehicles/{yardVehicleId}` | Busca veículo específico do pátio | `id`, `yardVehicleId` (path) | 200 OK, 400 BadRequest, 404 NotFound |
| POST | `/yards/{id}/vehicles` | Adiciona veículo ao pátio | `id` (path), Body: `YardVehicleDto` | 201 Created, 400 BadRequest, 404 NotFound |
| PUT | `/yards/{id}/vehicles/{yardVehicleId}` | Atualiza veículo do pátio | `id`, `yardVehicleId` (path), Body: `YardVehicleDto` | 200 OK, 400 BadRequest, 404 NotFound |

## 🎯 Funcionalidades Implementadas

### ✅ CRUD Completo
- **Yards**: Criação, leitura, atualização e exclusão de pátios
- **Employees**: Gestão completa de funcionários por pátio
- **Vehicles**: Consulta e gestão de veículos

### ✅ Rotas Parametrizadas
- **Query Parameters**: Paginação (`pageNumber`, `pageSize`), filtros (`qrCodeId`)
- **Path Parameters**: IDs de recursos (`id`, `employeeId`, `yardVehicleId`)

### ✅ Retornos HTTP Adequados
- **200 OK**: Sucesso em consultas e atualizações
- **201 Created**: Criação de novos recursos
- **204 NoContent**: Exclusão bem-sucedida
- **400 BadRequest**: Parâmetros inválidos
- **404 NotFound**: Recurso não encontrado
- **500 InternalServerError**: Erro interno do servidor

### ✅ Integração com Oracle
- Entity Framework Core com provider Oracle
- Migrations para criação e versionamento das tabelas
- Connection string via variável de ambiente

### ✅ Documentação OpenAPI
- Scalar UI para interface gráfica moderna
- Documentação automática de todas as rotas
- Schemas de request/response definidos

## ☁️ Deploy na Azure

### Pré-requisitos

- Azure CLI instalado
- Docker instalado
- Conta Azure ativa

### 1. Configuração Inicial do Azure

```bash
# Login no Azure
az login

# Criar resource group
az group create \
  --name rg-challenge \
  --location eastus

# Confirmar criação
az group show --name rg-challenge
```

### 2. Criar Azure Container Registry

```bash
# Criar Azure Container Registry
az acr create \
  --resource-group rg-challenge \
  --name autoinsight \
  --sku Basic

# Habilitar admin user
az acr update --name autoinsight --admin-enabled true

# Obter credenciais
ACR_USERNAME=$(az acr credential show --name autoinsight --query username -o tsv)
ACR_PASSWORD=$(az acr credential show --name autoinsight --query 'passwords[0].value' -o tsv)

echo "Username: $ACR_USERNAME"
```

### 3. Build e Push da Imagem Docker

```bash
# Login no ACR
az acr login --name autoinsight

# Build da imagem
docker build -t autoinsight.azurecr.io/aspnet:latest .

# Push para o registry
docker push autoinsight.azurecr.io/aspnet:latest

# Verificar imagem no registry
az acr repository list --name autoinsight --output table
```

### 4. Deploy do Oracle Database

```bash
# Criar container Oracle
az container create \
  --resource-group rg-challenge \
  --name oracle-db \
  --image container-registry.oracle.com/database/express:21.3.0-xe \
  --location eastus \
  --os-type Linux \
  --cpu 4 \
  --memory 8 \
  --ports 1521 \
  --ip-address Public \
  --environment-variables \
    ORACLE_PWD='SecurePassword!' \
    ORACLE_CHARACTERSET='AL32UTF8'
```

**Aguarde a inicialização completa do Oracle (alguns minutos):**

```bash
# Monitorar logs do Oracle
az container logs \
  --resource-group rg-challenge \
  --name oracle-db \
  --follow
```

Aguarde até aparecer a mensagem **"DATABASE IS READY TO USE!"**

```bash
# Obter IP público do Oracle
ORACLE_IP=$(az container show \
  --name oracle-db \
  --resource-group rg-challenge \
  --query ipAddress.ip \
  -o tsv)

echo "Oracle IP: $ORACLE_IP"
```

### 5. Deploy da API

```bash
# Criar container da API
az container create \
  --resource-group rg-challenge \
  --name autoinsight-api \
  --image autoinsight.azurecr.io/aspnet:latest \
  --location eastus \
  --os-type Linux \
  --cpu 1 \
  --memory 1.5 \
  --ports 8080 \
  --ip-address Public \
  --registry-login-server autoinsight.azurecr.io \
  --registry-username $ACR_USERNAME \
  --registry-password $ACR_PASSWORD \
  --environment-variables \
    ORACLE_CONNECTION_STRING="User Id=system;Password=SecurePassword!;Data Source=$ORACLE_IP:1521/XE"

# Obter IP público da API
API_IP=$(az container show \
  --name autoinsight-api \
  --resource-group rg-challenge \
  --query ipAddress.ip \
  -o tsv)

echo "API disponível em: http://$API_IP:8080"
```

### 6. Verificar Status da API

```bash
# Verificar logs da API
az container logs \
  --resource-group rg-challenge \
  --name autoinsight-api \
  --tail 20

# Testar health check
curl "http://$API_IP:8080/health"
```

### 7. Demonstração CRUD Completo

#### CREATE - Criar um Yard

```bash
curl -X POST "http://$API_IP:8080/yards" \
  -H "Content-Type: application/json" \
  -d '{
    "ownerId": "11111111-1111-1111-1111-111111111111",
    "address": {
      "country": "BR",
      "state": "SP",
      "city": "São Paulo",
      "zipCode": "01311-000",
      "neighborhood": "Bela Vista",
      "complement": "Av. Paulista, 1106"
    }
  }'
```

**Copie o YardId retornado para os próximos comandos.**

#### READ - Consultar Yards

```bash
# Listar todos os yards
curl "http://$API_IP:8080/yards"

# Buscar yard específico
curl "http://$API_IP:8080/yards/[YARD-ID]"
```

#### UPDATE - Atualizar Yard

```bash
curl -X PUT "http://$API_IP:8080/yards/[YARD-ID]" \
  -H "Content-Type: application/json" \
  -d '{
    "ownerId": "11111111-1111-1111-1111-111111111111",
    "address": {
      "country": "BR",
      "state": "SP",
      "city": "São Paulo",
      "zipCode": "01311-000",
      "neighborhood": "Jardins",
      "complement": "Av. Paulista, 2000 - ATUALIZADO"
    }
  }'
```

#### DELETE - Remover Yard

```bash
curl -X DELETE "http://$API_IP:8080/yards/[YARD-ID]"
```

### 8. Verificar Dados no Banco Oracle

```bash
# Conectar ao container Oracle
az container exec \
  --resource-group rg-challenge \
  --name oracle-db \
  --exec-command "/bin/bash"
```

Dentro do container Oracle:

```bash
sqlplus 'system/SecurePassword!@XE'
```

Executar consultas SQL:

```sql
-- Listar yards
SET LINESIZE 200
SET PAGESIZE 50
SELECT * FROM "Yards";

-- Listar endereços
SELECT * FROM "Addresses";

-- Contar registros
SELECT COUNT(*) FROM "Yards";
```

### 9. Limpeza de Recursos

Após os testes, remover os recursos para evitar cobranças:

```bash
# Deletar resource group (remove todos os recursos)
az group delete --name rg-challenge --yes --no-wait
```

## 📊 Exemplo de Uso

```bash
# Listar pátios
curl -X GET "http://localhost:5100/yards?pageNumber=1&pageSize=10"

# Criar novo pátio
curl http://localhost:5100/yards \
  --request POST \
  --header 'Content-Type: application/json' \
  --data '{
  "address": {
    "country": "BR",
    "state": "SP",
    "city": "São Paulo",
    "zipCode": "01311-000",
    "neighborhood": "Bela Vista",
    "complement": "Avenida Paulista, 1106"
  },
  "ownerId": "d51bqcy66s8o1lokzvquy6ux"
}'

# Buscar veículo por QR Code
curl -X GET "http://localhost:5100/vehicles?qrCodeId=QR123"
```

## 📝 Padrões de Desenvolvimento

- **Repository Pattern** para abstração de dados
- **DTOs** para transferência segura de dados
- **AutoMapper** para mapeamento automático
- **Minimal APIs** para performance otimizada
- **Environment Variables** para configurações sensíveis

## 📄 Licença

Este projeto foi desenvolvido para fins acadêmicos como parte do challenge da Mottu FIAP.
