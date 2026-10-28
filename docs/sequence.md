# Диаграмма последовательности: Запись на услугу

Показывает по шагам, как сервисы общаются между собой при создании новой записи, включая обработку ошибок (например, если время уже занято).

```mermaid
sequenceDiagram
    actor C as Клиент
    participant GW as api-gateway
    participant BS as booking-service
    participant CS as catalog-service
    participant DB as БД записей
    participant NS as notification-service

    C->>GW: POST /bookings + токен
    GW->>GW: проверить токен
    GW->>BS: POST /bookings
    BS->>CS: GET услуга и график
    CS-->>BS: данные

    alt Время занято
        BS-->>GW: 409 Конфликт
        GW-->>C: 409 Выберите другое время
    else Успех
        BS->>DB: сохранить запись (транзакция)
        BS-)NS: событие booking.created
        BS-->>GW: 201 Создано
        GW-->>C: 201 Запись подтверждена
    end
