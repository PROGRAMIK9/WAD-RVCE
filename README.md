# Classic ER Diagram (Chen Notation)

Based on `prisma/schema.prisma`.

**Legend:**
- **Rectangles**: Entities
- **Diamonds**: Relationships
- **Ovals**: Attributes
- **Double Lines**: Total Participation
- **Single Lines**: Partial Participation
- **Underlined Attributes**: Primary Key

```mermaid
flowchart TD
    %% Entities
    User[User]
    Patient[Patient]
    Doctor[Doctor]
    Appointment[Appointment]
    MedicalRecord[MedicalRecord]

    %% Attributes for User
    Uid([<u>id</u>]) --- User
    Uemail([email]) --- User
    Uname([name]) --- User
    Urole([role]) --- User
    Upass([password]) --- User

    %% Attributes for Patient
    Pid([<u>id</u>]) --- Patient
    Pdob([dob]) --- Patient
    Pgender([gender]) --- Patient
    Paddr([address]) --- Patient
    Pbg([bloodGroup]) --- Patient

    %% Attributes for Doctor
    Did([<u>id</u>]) --- Doctor
    Dspec([specialty]) --- Doctor

    %% Attributes for Appointment
    Aid([<u>id</u>]) --- Appointment
    Adate([date]) --- Appointment
    Astatus([status]) --- Appointment
    Areason([reason]) --- Appointment

    %% Attributes for MedicalRecord
    Mid([<u>id</u>]) --- MedicalRecord
    Mtype([type]) --- MedicalRecord
    Mtitle([title]) --- MedicalRecord
    Mfile([fileUrl]) --- MedicalRecord

    %% Relationships

    %% User - Patient (1:1)
    %% User can exist without Patient (Partial), Patient must have User (Total)
    User ---|1| R_UserPat{has_profile}
    R_UserPat ===|1| Patient

    %% User - Doctor (1:1)
    %% User can exist without Doctor (Partial), Doctor must have User (Total)
    User ---|1| R_UserDoc{has_profile}
    R_UserDoc ===|1| Doctor

    %% Patient - Appointment (1:N)
    %% Patient can have marked appointments (Partial?), Appointment MUST have Patient (Total)
    Patient ---|1| R_PatApp{books}
    R_PatApp ===|N| Appointment

    %% Doctor - Appointment (1:N)
    %% Doctor can have appointments (Partial?), Appointment MUST have Doctor (Total)
    Doctor ---|1| R_DocApp{attends}
    R_DocApp ===|N| Appointment

    %% Patient - MedicalRecord (1:N)
    %% Patient has records (Partial), Record MUST belong to patient (Total)
    Patient ---|1| R_PatRec{has_record}
    R_PatRec ===|N| MedicalRecord

    %% Styling to mimic Chen
    classDef entity fill:#f9f,stroke:#333,stroke-width:2px;
    classDef relation fill:#fff,stroke:#333,stroke-width:2px,shape:diamond;
    classDef attribute fill:#fff,stroke:#333,stroke-width:1px,stroke-dasharray: 5 5;
    
    class User,Patient,Doctor,Appointment,MedicalRecord entity;
    class R_UserPat,R_UserDoc,R_PatApp,R_DocApp,R_PatRec relation;
```

## Description of Relationships and Constraints

1.  **User has Patient Profile (1:1)**
    *   **Entities**: User, Patient
    *   **Cardinality**: 1:1. A User can have at most one Patient profile. A Patient profile belongs to exactly one User.
    *   **Participation**:
        *   User: **Partial** (Not all users are patients).
        *   Patient: **Total** (Every patient profile must be linked to a user).

2.  **User has Doctor Profile (1:1)**
    *   **Entities**: User, Doctor
    *   **Cardinality**: 1:1. A User can have at most one Doctor profile.
    *   **Participation**:
        *   User: **Partial** (Not all users are doctors).
        *   Doctor: **Total** (Every doctor profile must be linked to a user).

3.  **Patient books Appointment (1:N)**
    *   **Entities**: Patient, Appointment
    *   **Cardinality**: 1:N. A Patient can book multiple appointments. An appointment is booked by one Patient.
    *   **Participation**:
        *   Patient: **Partial** (A patient might not have any appointments yet).
        *   Appointment: **Total** (An appointment must exist for a patient).

4.  **Doctor attends Appointment (1:N)**
    *   **Entities**: Doctor, Appointment
    *   **Cardinality**: 1:N. A Doctor can accept multiple appointments. An appointment is attended by one Doctor.
    *   **Participation**:
        *   Doctor: **Partial** (A doctor might not have appointments scheduled).
        *   Appointment: **Total** (An appointment must have a doctor assigned).

5.  **Patient has MedicalRecord (1:N)**
    *   **Entities**: Patient, MedicalRecord
    *   **Cardinality**: 1:N. A Patient can have multiple medical records. A record belongs to one Patient.
    *   **Participation**:
        *   Patient: **Partial** (A patient might be healthy/new and have no records).
        *   MedicalRecord: **Total** (A record cannot exist without a patient).
