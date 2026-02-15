# 🏆 Sports Activity Scheduler & Management System

## 📘 ER Diagram & Database Schema Design

------------------------------------------------------------------------

# 📌 Overview

This document defines the **Low-Level Database Design (LLD)** and
**Entity Relationship Structure** for the Sports Activity Scheduler &
Management System.

The system supports:

-   Students
-   Parents
-   Coaches
-   Teams
-   Sports
-   Trainings
-   Matches
-   Schedules
-   Attendance
-   Player Analytics
-   Notifications
-   Activity Logs

------------------------------------------------------------------------

# 🧩 Core Architecture Layers

## 1️⃣ Identity Layer

-   users
-   students
-   parents
-   coaches
-   parent_student_map

## 2️⃣ Sports Domain Layer

-   sports
-   teams
-   team_members
-   trainings
-   matches
-   schedules

## 3️⃣ Analytics & Tracking Layer

-   attendance
-   player_stats
-   notifications
-   activity_logs

------------------------------------------------------------------------

# 🗂️ Database Tables

## 👤 users

-   id (PK) -- UUID\
-   email -- VARCHAR\
-   password_hash -- VARCHAR\
-   role -- ENUM (STUDENT, PARENT, COACH, ADMIN)\
-   status -- VARCHAR\
-   created_at -- TIMESTAMP

## 🎓 students

-   id (PK) -- UUID\
-   user_id (FK → users.id)\
-   date_of_birth -- DATE\
-   gender -- VARCHAR\
-   admission_date -- DATE

## 👨‍👩‍👧 parents

-   id (PK) -- UUID\
-   user_id (FK → users.id)\
-   phone_number -- VARCHAR

## 🧑‍🏫 coaches

-   id (PK) -- UUID\
-   user_id (FK → users.id)\
-   specialization -- VARCHAR\
-   experience_years -- INT

## 🔗 parent_student_map

-   id (PK) -- UUID\
-   parent_id (FK → parents.id)\
-   student_id (FK → students.id)

------------------------------------------------------------------------

# ⚽ Sports Domain

## 🏅 sports

-   id (PK) -- UUID\
-   name -- VARCHAR\
-   category -- VARCHAR\
-   max_players -- INT

## 👥 teams

-   id (PK) -- UUID\
-   sport_id (FK → sports.id)\
-   name -- VARCHAR\
-   age_group -- VARCHAR\
-   coach_id (FK → coaches.id)

## 👕 team_members

-   id (PK) -- UUID\
-   team_id (FK → teams.id)\
-   student_id (FK → students.id)\
-   joined_date -- DATE

------------------------------------------------------------------------

# 🏋️ Trainings & Matches

## 🏋️ trainings

-   id (PK) -- UUID\
-   team_id (FK → teams.id)\
-   title -- VARCHAR\
-   description -- TEXT

## 🏆 matches

-   id (PK) -- UUID\
-   team_id (FK → teams.id)\
-   opponent_name -- VARCHAR\
-   match_type -- VARCHAR\
-   result -- VARCHAR

## 🗓️ schedules

-   id (PK) -- UUID\
-   event_type -- ENUM (TRAINING, MATCH)\
-   event_id -- UUID\
-   start_time -- TIMESTAMP\
-   end_time -- TIMESTAMP\
-   location -- VARCHAR\
-   status -- VARCHAR

------------------------------------------------------------------------

# 📊 Attendance & Analytics

## 📌 attendance

-   id (PK) -- UUID\
-   schedule_id (FK → schedules.id)\
-   student_id (FK → students.id)\
-   status -- ENUM (PRESENT, ABSENT)\
-   remarks -- TEXT

## 📈 player_stats

-   id (PK) -- UUID\
-   student_id (FK → students.id)\
-   match_id (FK → matches.id)\
-   goals -- INT\
-   assists -- INT\
-   score -- INT\
-   rating -- DECIMAL\
-   coach_feedback -- TEXT

------------------------------------------------------------------------

# 🔔 Notifications & Logs

## 🔔 notifications

-   id (PK) -- UUID\
-   user_id (FK → users.id)\
-   title -- VARCHAR\
-   message -- TEXT\
-   is_read -- BOOLEAN\
-   created_at -- TIMESTAMP

## 📝 activity_logs

-   id (PK) -- UUID\
-   user_id (FK → users.id)\
-   action -- VARCHAR\
-   entity_type -- VARCHAR\
-   entity_id -- UUID\
-   timestamp -- TIMESTAMP

------------------------------------------------------------------------

# 📊 ER Diagram (Mermaid)

``` mermaid
erDiagram

users ||--|| students : has
users ||--|| parents : has
users ||--|| coaches : has

parents ||--o{ parent_student_map : maps
students ||--o{ parent_student_map : maps

sports ||--o{ teams : contains
coaches ||--o{ teams : manages

teams ||--o{ team_members : includes
students ||--o{ team_members : joins

teams ||--o{ trainings : conducts
teams ||--o{ matches : plays

trainings ||--|| schedules : scheduled
matches ||--|| schedules : scheduled

schedules ||--o{ attendance : records
students ||--o{ attendance : attends

matches ||--o{ player_stats : generates
students ||--o{ player_stats : has

users ||--o{ notifications : receives
users ||--o{ activity_logs : performs
```
