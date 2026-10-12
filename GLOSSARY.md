# Xmap-vibe Domain Glossary

This file defines the canonical domain vocabulary for the **Xmap-vibe** project.
All agents and developers should use these terms consistently and avoid forbidden synonyms.

## Core Domain Concepts

**Spot (Спот)**:
A specific physical location suitable for action sports (street, skatepark, DIY, bowl, pumptrack, hill/slope). A Spot has geographical coordinates, a title, description, supported disciplines, obstacle tags, and associated photos.
_Avoid_: Place, location, point, pin

**Discipline (Дисциплина / Вид катания)**:
The type of extreme or action sport practiced at a spot (e.g., BMX, Skate, Stunt Scooter, Snowboard, Aggressive Inline). A single spot may support multiple disciplines.
_Avoid_: Sport, category, game

**Spot Media (Медиафайлы спота)**:
Photos or videos uploaded by users to showcase obstacles, surface quality, and conditions of a Spot. Stored in S3-compatible object storage with metadata in PostgreSQL.
_Avoid_: File, image attachment, picture

**Spot Conditions (Состояние спота)**:
Current status or usability of a spot (e.g., dry, wet, covered in snow, security guards present, under construction/destroyed).
_Avoid_: Status, weather

**Obstacle (Препятствие / Фигура)**:
Specific elements present at the spot: ledge, rail, stairs, quarter-pipe, bank, gap, box, spine, bowl.
_Avoid_: Item, element, object
