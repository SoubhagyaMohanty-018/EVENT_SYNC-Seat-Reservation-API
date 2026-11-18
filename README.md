
# EventSync – Concurrent Event & Seat Reservation API

![Java](https://shields.io)
![Spring Boot](https://shields.io)
![MySQL](https://shields.io)
![REST API](https://shields.io)
![Postman](https://shields.io)

Backend REST API built with **Java** and **Spring Boot** for managing college events and handling simultaneous seat bookings without race conditions.

## Architecture & Applied DSA
- **ACID Transactions (`@Transactional`):** Guarantees seat count updates and booking records commit together or roll back cleanly on failure.
- **Optimistic Locking (`@Version`):** Prevents double-booking race conditions when multiple users request the final seat concurrently.
- **Rate Limiting (`ConcurrentHashMap`):** Tracks per-user request counts in memory with thread-safe O(1) lookups.
- **Fair Waitlist (`Queue`):** Uses a FIFO queue to assign cancelled seats automatically in arrival order.
- **Query Speed:** Uses MySQL B-tree indexing on event dates and SQL joins for fast seat-availability checks.

## Core API Endpoints

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/api/events` | Fetch active events and available seat counts |
| `POST` | `/api/bookings/reserve` | Book a seat with `@Transactional` lock check |
| `POST` | `/api/bookings/{id}/cancel` | Cancel booking and trigger FIFO waitlist assignment |
