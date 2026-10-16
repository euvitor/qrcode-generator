# QR Code Generator API

Uma API REST desenvolvida em **Java 21** e **Spring Boot** para geração de QR Codes e upload automático para a nuvem via **Amazon S3**.

---

## 🚀 Tecnologias Utilizadas

- **Java 21**
- **Spring Boot 4**
- **Google ZXing** (Geração de imagens de QR Code)
- **AWS SDK for Java v2** (Integração com Amazon S3)
- **Docker** (Containerização e Multi-stage Build)
- **Maven** (Gerenciador de dependências e build)

---

## 📂 Arquitetura do Projeto

O projeto adota princípios de **Clean Architecture / Ports and Adapters (Hexagonal)**:

```text
src/main/java/dev/euvitor/qrcode/generator/
├── Controller/               # Adaptadores de entrada (REST Controller)
│   └── QrCodeController.java
├── dto/                      # Objetos de transferência de dados (Records)
│   ├── QrCodeGenerateRequest.java
│   └── QrCodeGenerateResponse.java
├── service/                  # Regra de negócio (geração do QR Code)
│   └── QrCodeGeneratorService.java
├── ports/                    # Interfaces/Contratos (Porta de saída)
│   └── StoragePort.java
└── infrastructure/           # Adaptador de saída (Implementação AWS S3)
    └── S3StorageAdapter.java
```

---

## ⚙️ Pré-requisitos

- **Java 21** instalado (para execução local)
- **Docker** instalado
- Uma conta na **AWS** com:
  - Um Bucket S3 criado.
  - Credenciais IAM (`AWS_ACCESS_KEY_ID` e `AWS_SECRET_ACCESS_KEY`) com permissão para `s3:PutObject` no bucket.

---

## 🔐 Configuração das Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto baseado no `.env.example`:

```env
AWS_ACCESS_KEY_ID=seu_access_key_id_aqui
AWS_SECRET_ACCESS_KEY=seu_secret_access_key_aqui
AWS_REGION=us-east-2
AWS_BUCKET_NAME=nome-do-seu-bucket
```

> ⚠️ **Atenção:** O arquivo `.env` contém credenciais sensíveis e está incluído no `.gitignore`. Nunca envie este arquivo para o repositório público.

---

## 🐳 Executando com Docker

### 1. Construir a imagem Docker

```bash
docker build -t qrcode-generator:latest .
```

### 2. Executar o container com as variáveis do `.env`

```bash
docker run -d --name qrcode-api \
  --env-file .env \
  -p 8080:8080 \
  qrcode-generator:latest
```

---

## 💻 Executando Localmente (Sem Docker)

Exporte as variáveis de ambiente no seu terminal:

```bash
export AWS_ACCESS_KEY_ID="seu_access_key_id"
export AWS_SECRET_ACCESS_KEY="seu_secret_access_key"
export AWS_REGION="us-east-2"
export AWS_BUCKET_NAME="nome-do-seu-bucket"
```

E inicie a aplicação com o Maven Wrapper:

```bash
./mvnw spring-boot:run
```

A aplicação estará acessível em `http://localhost:8080`.

---

## 📡 Endpoints da API

### `POST /qrcode`

Gera um QR Code a partir de um texto/URL e envia para o S3.

#### Requisição:

- **URL:** `http://localhost:8080/qrcode`
- **Método:** `POST`
- **Headers:** `Content-Type: application/json`
- **Body:**

```json
{
  "text": "https://github.com/euvitor"
}
```

#### Resposta de Sucesso (`200 OK`):

```json
{
  "url": "https://seu-bucket.s3.us-east-2.amazonaws.com/qrcode-uuid-gerado.png"
}
```

---

## 👤 Autor

Desenvolvido por **Vitor**  
- GitHub: [@euvitor](https://github.com/euvitor)
