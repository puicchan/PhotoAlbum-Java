# Architecture Diagram

This diagram represents the high-level architecture of the Photo Album application, a Spring Boot web application backed by an Oracle database for photo storage and metadata management.

## Application Architecture

```mermaid
flowchart TD
    Browser["Web Browser\n(User Interface)"]

    subgraph App["Spring Boot Application (Java 8, Port 8080)"]
        subgraph Presentation["Presentation Layer"]
            HC["HomeController\n(Photo Gallery)"]
            DC["DetailController\n(Photo Detail)"]
            PFC["PhotoFileController\n(Upload / Download)"]
            TH["Thymeleaf Templates\n(HTML Views)"]
            Static["Static Assets\n(CSS, JS)"]
        end

        subgraph Business["Business Logic Layer"]
            PS["PhotoService\n(Upload, Retrieve, Validate)"]
            Valid["Bean Validation\n(File type, size, count)"]
        end

        subgraph Data["Data Access Layer"]
            PR["PhotoRepository\n(Spring Data JPA)"]
            JPA["Spring Data JPA\n/ Hibernate ORM"]
        end
    end

    subgraph Storage["Data Storage"]
        OracleDB[("Oracle Database\n(FREEPDB1 / XE)\nPhoto binary data\n+ metadata")]
        LocalFS["Local File System\nsrc/main/resources/static/uploads\n(dev fallback)"]
    end

    Browser -->|"HTTP requests"| HC
    Browser -->|"HTTP requests"| DC
    Browser -->|"Multipart upload / image download"| PFC
    HC --> TH
    DC --> TH
    PFC --> PS
    HC --> PS
    DC --> PS
    PS --> Valid
    PS --> PR
    PS --> LocalFS
    PR --> JPA
    JPA -->|"ojdbc8 JDBC"| OracleDB
```
