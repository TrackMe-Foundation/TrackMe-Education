# TrackMe Education
Copyright TrackMe 2025

## Requirements
- Must provide options for single sign on (SAMLv2)
- Must provide functionality to manage:
    - Schools
    - Rooms
        - Room number
        - Room capacity (seats only)
        - Room capacity (computers)
        - Room facilities
    - Courses
    - Classes/Groups
    - Class Schedules
    - Users (teacher, student, admin)
    - Reports
        - Grades
        - Progress
        - Interventions
- Must be simple to deploy

## Decisions Made
|Question|Decision|
|---|---|
|What database software?|MongoDB - Lightweight with ample feature set|
|Hosting method(s)?|<table><tr><td>Self-Hosted</td><td>Docker [compose] - Simple deployment and allows for simple CI/CD process</td><tr><td>SaaS</td><td>Not currently supported</td></tr></table>
|

## Database Documentation
### Diagram
::: mermaid
erDiagram
    %% entities
    KEY {
        datatype fieldname PK "SAMPLE DATA"
    }
    Schools {
        int Id PK "1"
        string Name "School 1"
        string Location "Staffordshire, UK"
    }
    Rooms {
        int Id PK "1"
        int SchoolId FK "1"
        string Name "Room 1"
        int Seats "30"
        int Computers "25"
    }
    RoomBookings { 
        int Id PK "1"
        int RoomId FK "1"
        int GroupId FK "1"
        datetime BookingStart "2025-03-03 09:00:00"
        datetime BookingEnd "2025-03-03 10:00:00"
    }
    Courses {
        int Id PK "1"
        string Name "Computer Science"
    }
    Groups {
        int Id PK "1"
        int CourseId FK "1"
        string Name "CS1"
    }
    Students {
        int Id PK "1"
        string Forename "Bob"
        string Surname "Roberts"
        string Email "broberts@acme.com"
    }
    Teachers {
        int Id PK "1"
        string Forename "Alice"
        string Surname "Thomson"
        string Email "athomson@acme.com"
    }
    Admins {
        int Id PK "1"
        string Forename "Francis"
        string Surname "Elliot"
        string Email "felliot@acme.com"
    }
    ReportTypes {
        int Id PK "1"
        string TypeName "Progress Report"
    }
    Reports {
        int Id PK "1"
        int ReportTypeId FK "1"
        int StudentId FK "1"
        int TeacherId FK "1"
        int AdminId FK
        string Grade
        string Feedback "Good student"
        string InterventionsMade
    }
    TeacherXGroup {
        int Id PK "1"
        int TeacherId FK "1"
        int GroupId FK "1"
    }
    StudentXGroup {
        int Id PK "1"
        int StudentId FK "1"
        int GroupId FK "1"
    }
    SchoolXCourse {
        int Id PK "1"
        int SchoolId FK "1"
        int CourseId FK "1"
    }

    %% relationships
    Schools ||--|{ Rooms : ""
    SchoolXCourse ||--|{ Schools : ""
    SchoolXCourse ||--|{ Courses : ""
    Courses ||--|{ Groups : ""
    StudentXGroup ||--|{ Students : ""
    StudentXGroup ||--|{ Groups : ""
    TeacherXGroup ||--|{ Teachers: ""
    TeacherXGroup ||--|{ Groups: ""
    RoomBookings ||--|| Groups: ""
    RoomBookings ||--|| Rooms : ""
    ReportTypes ||--|{ Reports : ""
    Students ||--|{ Reports : ""
    Teachers ||--|{ Reports : ""
    Schools }|--|{ Admins : ""
:::

## TrackMe Education (TME) - FAQ
| | |
|---|---|
|How do I deploy TME?|To deploy TME, please follow [this guide](./tme_deployment.md).|
|Who developed TME?|TME was primarily developed by the TrackMe Foundation but is open-source and aims to allow others to aid in its development on Github.|
|Why use TME?|TME is an open-source, collaboratively developed application. We aim to develop additional features when requests are made to ensure teachers, schools and administrative staff have every tool they need in one place.|
|Is there support for TME?|There is no formal support for the TME software, as such, we advise regular backups are taken as per best IT best practice. For information on backup and recovery, please see [this page](./tme_backup.md).