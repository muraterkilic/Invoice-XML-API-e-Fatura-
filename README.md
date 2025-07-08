# Invoice XML API (e-Fatura)

A Spring Boot API for receiving, validating, and storing electronic invoices (e-Faktura) in XML format encoded as base64.

---

## 🔧 Technologies Used

- Java 17
- Spring Boot 3.x
- Spring Web
- Spring Data JPA
- PostgreSQL
- JAXB (XML to Java object)
- Swagger (OpenAPI)
- JUnit & Mockito (unit testing)
- Docker & Docker Compose

---

## 🚀 Running the Project

### ✅ Using Maven
```bash
mvn clean install
mvn spring-boot:run
```

### 🐳 Using Docker Compose
```bash
docker-compose up --build
```
This runs both the Spring Boot application and a PostgreSQL database.

---

## 📦 API Endpoint

### POST `/api/invoices`
**Content-Type:** `application/json`

#### 🔸 Example Request (Valid)
```json
{
  "base64xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiPz4KPEZha3R1cmEgeG1sbnM9Imh0dHA6Ly9jcmQuZ292LnBsL3d6b3IvMjAyMy8wNi8yOS8xMjY0OC8iPgogIDxQb2RtaW90MT4KICAgIDxEYW5lSWRlbnR5ZmlrYWN5am5lPgogICAgICA8TklQPjEyMzQ1Njc4OTA8L05JUD4KICAgIDwvRGFuZUlkZW50eWZpa2FjeWpuZT4KICA8L1BvZG1pb3QxPgogIDxGYT4KICAgIDxQXzE+MjAyMy0wOC0zMTwvUF8xPgogICAgPFBfMj5GSzIwMjMvMDgvMzE8L1BfMj4KICA8L0ZhPgo8L0Zha3R1cmE+"
}
```

#### 🔹 Example Response (Success)
```json
{
  "message": "Fatura başarıyla kaydedildi"
}
```

---

### 🔻 Example Request (Invalid Base64)
```json
{
  "base64xml": "PHNwZWNpZmljYXRpb24geG1sbnM9Imh0dHA6Ly9...=="
}
```

### 🔴 Example Response (Decode Failure)
```json
{
  "decode_hatası": "Input byte array has incorrect ending byte at 40",
  "örnek_xml": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<Faktura xmlns=\"http://crd.gov.pl/wzor/2023/06/29/12648/\">\n  <Podmiot1>\n    <DaneIdentyfikacyjne>\n      <NIP>1234567890</NIP>\n    </DaneIdentyfikacyjne>\n  </Podmiot1>\n  <Fa>\n    <P_1>2023-08-31</P_1>\n    <P_2>FK2023/08/31</P_2>\n  </Fa>\n</Faktura>",
  "gönderilen_veri": "PHNwZWNpZmljYXRpb24geG1sbnM9Imh0dHA6Ly9...==",
  "gönderilen_veri_ilk_10": "PHNwZWNpZm",
  "error": "Base64 çözümleme hatası: Input byte array has incorrect ending byte at 40. Doğru Base64 örneği: PD94bWwgdmVyc2lvbj0iMS4wIiBlbm...",
  "type": "XML_PROCESSING_ERROR",
  "ipucu": "Base64 verilerinizi daha detaylı analiz etmek için /api/diagnostic/analyze-base64 endpoint'ini kullanabilirsiniz."
}
```

---

## 🧪 Testing

To run unit tests:
```bash
mvn test
```

---

## 🧾 Swagger UI

Once app is running:
```
http://localhost:8080/swagger-ui/index.html#
```

---

## 📁 Project Structure
```
src/
├── main/java/com/example/invoiceapi
│   ├── controller
│   ├── dto
│   ├── model
│   ├── repository
│   ├── service
│   ├── config
│   └── InvoiceApiApplication.java
├── resources
│   ├── application.yml
│   └── schemat.xsd
└── test/java/com/example/invoiceapi/service
```

---

## 📦 Docker Files

### Dockerfile
```Dockerfile
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY target/invoice-xml-api.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### docker-compose.yml
```yaml
version: '3.8'
services:
  db:
    image: postgres:14
    environment:
      POSTGRES_DB: invoicedb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
    ports:
      - "5432:5432"
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      SPRING_DATASOURCE_URL: jdbc:postgresql://db:5432/invoicedb
      SPRING_DATASOURCE_USERNAME: user
      SPRING_DATASOURCE_PASSWORD: pass
    depends_on:
      - db
```

---

## 📌 Notes
- `base64xml` field must contain XML encoded in base64
- XML must conform to the `schemat.xsd` structure
- JAXB is used to unmarshal and map fields such as `NIP`, `P_1`, `P_2`, `KodWaluty`, etc.

---

## 📬 Contact
Feel free to contribute or report issues via GitHub.

---

MIT License
