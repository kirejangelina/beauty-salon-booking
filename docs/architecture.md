# Архитектура системы (Микросервисы)

Показывает, из каких сервисов состоит система и как они общаются между собой.

```mermaid
flowchart LR
    User["Клиент / Мастер / Администратор"]
    GW["api-gateway<br/>проверка токена"]

    subgraph US["user-service"]
        US_API["Пользователи"]
        US_DB[("БД пользователей")]
        US_API --> US_DB
    end

    subgraph CS["catalog-service"]
        CS_API["Каталог услуг"]
        CS_DB[("БД каталога")]
        CS_API --> CS_DB
    end

    subgraph BS["booking-service"]
        BS_API["Записи"]
        BS_DB[("БД записей")]
        BS_API --> BS_DB
    end

    User -->|HTTP| GW
    GW -->|"/users"| US_API
    GW -->|"/catalog"| CS_API
    GW -->|"/bookings"| BS_API
    BS_API -->|"REST"| CS_API
