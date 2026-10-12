# Xmap-vibe Domain Glossary

This file defines the canonical domain vocabulary for the **Xmap-vibe** project.
All agents and developers should use these terms consistently and avoid forbidden synonyms.

## Core Domain Concepts

**Spot (Спот)**:
A specific physical location suitable for action sports (street, skatepark, DIY, bowl, pumptrack, hill/slope). A Spot has geographical coordinates (`Point`), a title, description, supported disciplines, spot type, obstacle tags, associated photos, and an approval status.
_Avoid_: Place, location, point, pin

**Spot Status (Статус спота)**:
The lifecycle state of a spot in the platform:
- `PENDING_REVIEW` (на модерации) — созданная заявка, ожидает проверки модератором.
- `APPROVED` (одобрен) — активный спот, отображается на публичной интерактивной карте.
- `REJECTED` (отклонен) — заявка отклонена модератором с указанием причины.
_Avoid_: State, condition, visibility

**Spot Type (Тип спота)**:
Physical categorization of the spot: `STREET`, `SKATEPARK`, `PUMPTRACK`, `DIY`, `BOWL`, `DIRT`.
_Avoid_: Category, kind

**Discipline (Дисциплина / Вид катания)**:
The type of extreme or action sport practiced at a spot: `BMX`, `SKATE`, `STUNT_SCOOTER`, `SNOWBOARD`, `AGGRESSIVE_INLINE`. A single spot may support multiple disciplines.
_Avoid_: Sport, category, game

**Spot Media (Медиафайлы спота)**:
Photos uploaded by riders to showcase obstacles, surface quality, and conditions of a Spot. Stored in S3-compatible object storage with metadata in PostgreSQL. Also undergo moderation before public display.
_Avoid_: File, image attachment, picture

**Rider (Райдер / Пользователь)**:
An authenticated user in the system (synchronized from Keycloak via JIT). Can submit spot proposals, upload photos, and view their submission status.
_Avoid_: Client, account, customer

**Moderator (Модератор)**:
A privileged user with `ROLE_MODERATOR` or `ROLE_ADMIN` permissions who reviews incoming spot proposals and media uploads, either approving them for the public map or rejecting them.
_Avoid_: Manager, supervisor, auditor

**Obstacle (Препятствие / Фигура)**:
Specific elements present at the spot: ledge, rail, stairs, quarter-pipe, bank, gap, box, spine, bowl.
_Avoid_: Item, element, object
