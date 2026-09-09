# Relational Database Normalization: 1NF – 5NF

### A practical guide with a single running example

---

### Database normalization

**Database normalization** is a systematic design process for organizing tables and columns in a relational database. Its goal is to reduce data duplication, prevent data errors, simplify queries. You break large tables into smaller, linked pieces so each fact is stored in only one place.

##### Unnormalized table:

![alt text](<images\Unnormalized table.JPG>)

The first thing to notice is this table serves many purposes
including:

- Identifying the organization's salespeople
- Listing the sales offices and phone numbers
- Associating a salesperson with an sales office
- Showing each salesperson's customers

#### Reasons to normalize:

##### Reason #1 - Data duplication

![alt text](<images\Unnormalized table duplication.JPG>)

Duplicated information presents two problems:

- It increases storage and decrease performance.
- It becomes more difficult to maintain data
  changes.

!!!_One symbol for table with 1 billion records costs 1 gygabyte of storage_!!!

##### Reason #2 - Data Anomalies (may cause data errors)

- **Insert Anomaly**

![alt text](<images\Insert Anomaly.JPG>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;There are facts we cannot record until we know information for the entire row. In our example we cannot record a new sales office until we also know the salesperson. Why? Because in order to create the record,we need provide a primary key. In our case this is the EmployeeID.

- **Update Anomaly**

![alt text](<images\Update Anomaly.JPG>)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The same information is recorded in multiple rows. For instance, if the office number changes, then there are multiple updates that need to be made. If these updates are not successfully completed across all rows, then an inconsistency occurs.

- **Deletion Anomaly**

![alt text](<images\Deletion Anomaly.JPG>)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Deletion of a row can cause more than one set of facts to be removed. For instance, if John Hunt retires, then deleting that row causes us to lose information about the New York office.

##### Reason #3 - Query Issues

![alt text](images\SearchAndSortIssue.JPG)

SELECT SalesOffice
FROM SalesStaff
WHERE Customer1 = 'Ford' OR
Customer2 = 'Ford' OR
Customer3 = 'Ford'

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;We will also have problems with sellers who work with more than three clients.

## 0. Why normalize?

Normalization is the process of organizing columns and tables in a relational database to **reduce data redundancy** and **eliminate update, insertion, and deletion anomalies**. Each normal form (NF) is a stricter rule set than the one before it — a table in 4NF is automatically in 3NF, 2NF, and 1NF.

Three anomaly types recur throughout this guide:

| Anomaly               | What it means                                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Insertion anomaly** | You cannot add a fact without also being forced to add unrelated/unknown facts (or duplicate existing ones).    |
| **Update anomaly**    | The same fact is stored in multiple rows; updating it in one place but not others leaves the data inconsistent. |
| **Deletion anomaly**  | Deleting a row accidentally destroys a fact that had nothing to do with the reason for the deletion.            |

---

## 1. The starting point: a denormalized table

Imagine a company that tracks student course enrollments, instructors, and campus rooms in **one flat spreadsheet-like table**.

**`Enrollments` (denormalized)**

| StudentID | StudentName | StudentPhones      | CourseID | CourseName | Instructor  | InstructorOffice | RoomID | RoomBuilding | Grade | AdvisorName | AdvisorDept |
| --------- | ----------- | ------------------ | -------- | ---------- | ----------- | ---------------- | ------ | ------------ | ----- | ----------- | ----------- |
| S1        | Anna Popova | 555-1111, 555-2222 | C10      | Databases  | Dr. Ivanov  | Ivanov-101       | R5     | Main Hall    | A     | Dr. Orlov   | CS          |
| S1        | Anna Popova | 555-1111, 555-2222 | C20      | Algorithms | Dr. Petrova | Petrova-202      | R6     | Main Hall    | B     | Dr. Orlov   | CS          |
| S2        | Boris Lee   | 555-3333           | C10      | Databases  | Dr. Ivanov  | Ivanov-101       | R5     | Main Hall    | C     | Dr. Orlov   | CS          |
| S3        | Chen Wu     | 555-4444           | C30      | Networks   | Dr. Ivanov  | Ivanov-101       | R7     | East Wing    | A     | Dr. Sokol   | EE          |

**Defects visible immediately:**

- **Repeating group**: `StudentPhones` holds multiple values in one cell.
- **Redundancy**: `StudentName`, `CourseName`, `Instructor`, `InstructorOffice`, `RoomBuilding`, `AdvisorName`, `AdvisorDept` all repeat across rows.
- **Mixed subjects**: one row describes a student, a course, an instructor, a room, _and_ an advisor all at once.
- **Update risk**: if Dr. Ivanov changes offices, every row containing him must be updated.
- **Delete risk**: if S3 withdraws from C30 (the only row with that course/instructor/room combination... actually R7/East Wing appears once), deleting that row could wipe out facts about the room or instructor that aren't recorded elsewhere.
- **Insert risk**: you cannot record a new course that has no students enrolled yet, because `CourseID` only appears attached to an enrollment row.

We'll now fix these, one normal form at a time.

---

## 2. First Normal Form (1NF)

### Definition

A table is in 1NF if:

1. Each column holds **atomic (indivisible) values** - no lists, sets, or repeating groups in a single cell (e.g., `Phone1`, `Phone2`, `Phone3`).
2. Each row is uniquely identifiable (has a primary key).
3. There are no repeating groups of columns

### Problems it solves

- Eliminates **multi-valued cells**, which break simple filtering, indexing, and aggregation (`WHERE Phone = '555-1111'` doesn't work reliably on a comma-packed field).
- Eliminates repeating column groups, which make queries like "find all phone numbers" require scanning an unbounded, hardcoded set of columns.

### Problems that remain

- Redundant data across rows (StudentName, CourseName, etc. still repeat).
- Update/delete/insert anomalies from mixed subjects — 1NF says nothing about _which_ facts belong together in a table.

### Applying it to our example

The repeating group `StudentPhones` is split into its own row per phone number. We need a new table because a student can have many phones (one column per phone would violate atomicity/1NF again).

**`Enrollments_1NF`**

| StudentID | StudentName | CourseID | CourseName | Instructor  | InstructorOffice | RoomID | RoomBuilding | Grade | AdvisorName | AdvisorDept |
| --------- | ----------- | -------- | ---------- | ----------- | ---------------- | ------ | ------------ | ----- | ----------- | ----------- |
| S1        | Anna Popova | C10      | Databases  | Dr. Ivanov  | Ivanov-101       | R5     | Main Hall    | A     | Dr. Orlov   | CS          |
| S1        | Anna Popova | C20      | Algorithms | Dr. Petrova | Petrova-202      | R6     | Main Hall    | B     | Dr. Orlov   | CS          |
| S2        | Boris Lee   | C10      | Databases  | Dr. Ivanov  | Ivanov-101       | R5     | Main Hall    | C     | Dr. Orlov   | CS          |
| S3        | Chen Wu     | C30      | Networks   | Dr. Ivanov  | Ivanov-101       | R7     | East Wing    | A     | Dr. Sokol   | EE          |

**`StudentPhones_1NF`**

| StudentID | Phone    |
| --------- | -------- |
| S1        | 555-1111 |
| S1        | 555-2222 |
| S2        | 555-3333 |
| S3        | 555-4444 |

**Primary key of `Enrollments_1NF`**: composite `(StudentID, CourseID)` — this is the natural key, since a student can enroll in many courses and a course has many students.

**What improved**: every cell now holds one value; phone numbers can be queried/indexed normally.
**What's still broken**: massive redundancy (Dr. Ivanov's office repeats every time he teaches a course someone is enrolled in), and several unrelated "themes" (student, course, instructor, room, advisor) are jammed into one table.

---

## 3. Second Normal Form (2NF)

### Definition

A table is in 2NF if:

1. It is in 1NF, **and**
2. Every non-key attribute is **fully functionally dependent on the whole primary key** — i.e., no non-key attribute depends on only _part_ of a composite key.

2NF only matters when the primary key is **composite** (more than one column). If the key is a single column, 1NF ⇒ 2NF automatically.

### Problems it solves

- **Partial dependency anomalies**: attributes that describe only part of the key (e.g., only the student, not the enrollment) were being repeated for every combination of the _other_ part of the key.

### Problems that remain

- Transitive dependencies: non-key attributes that depend on _other non-key attributes_, not directly on the key (e.g., `InstructorOffice` depends on `Instructor`, not on `(StudentID, CourseID)`).

### Applying it to our example

In `Enrollments_1NF`, the key is `(StudentID, CourseID)`. Check each non-key column:

| Column                                                         | Depends on                                                          |
| -------------------------------------------------------------- | ------------------------------------------------------------------- |
| StudentName                                                    | StudentID only → **partial dependency**                             |
| AdvisorName, AdvisorDept                                       | StudentID only → **partial dependency**                             |
| CourseName, Instructor, InstructorOffice, RoomID, RoomBuilding | CourseID only → **partial dependency**                              |
| Grade                                                          | the _whole_ key `(StudentID, CourseID)` → correctly full dependency |

Everything except `Grade` is a partial dependency. We split them out:

**`Students_2NF`**

| StudentID (PK) | StudentName | AdvisorName | AdvisorDept |
| -------------- | ----------- | ----------- | ----------- |
| S1             | Anna Popova | Dr. Orlov   | CS          |
| S2             | Boris Lee   | Dr. Orlov   | CS          |
| S3             | Chen Wu     | Dr. Sokol   | EE          |

**`Courses_2NF`**

| CourseID (PK) | CourseName | Instructor  | InstructorOffice | RoomID | RoomBuilding |
| ------------- | ---------- | ----------- | ---------------- | ------ | ------------ |
| C10           | Databases  | Dr. Ivanov  | Ivanov-101       | R5     | Main Hall    |
| C20           | Algorithms | Dr. Petrova | Petrova-202      | R6     | Main Hall    |
| C30           | Networks   | Dr. Ivanov  | Ivanov-101       | R7     | East Wing    |

**`Enrollments_2NF`**

| StudentID (FK) | CourseID (FK) | Grade |
| -------------- | ------------- | ----- |
| S1             | C10           | A     |
| S1             | C20           | B     |
| S2             | C10           | C     |
| S3             | C30           | A     |

(`StudentPhones_1NF` carries over unchanged.)

**What improved**: a student's name is stored once, not once per enrollment; a course's instructor/room is stored once, not once per enrolled student. Updating Anna Popova's name now touches exactly one row.
**What's still broken**: inside `Courses_2NF`, `InstructorOffice` depends on `Instructor`, not directly on `CourseID` — and `AdvisorDept` in `Students_2NF` depends on `AdvisorName`, not on `StudentID`. These are **transitive dependencies**.

---

## 4. Third Normal Form (3NF)

### Definition

A table is in 3NF if:

1. It is in 2NF, **and**
2. It has **no transitive dependencies** — every non-key attribute depends **directly** on the primary key, and not on another non-key attribute.

(Formally: for every functional dependency X → Y, either X is a superkey, or Y is part of a candidate key.)

### Problems it solves

- **Transitive-dependency anomalies**: a fact stored redundantly because it "hitchhikes" on another non-key attribute. E.g., every course taught by Dr. Ivanov repeats "Ivanov-101," so moving his office means updating every one of his courses.

### Problems that remain

- 3NF still permits certain anomalies when a table has **multiple overlapping candidate keys** with dependencies between their parts — the narrower **BCNF** rule (below) is needed for that edge case.

### Applying it to our example

- In `Courses_2NF`: `Instructor → InstructorOffice` and `RoomID → RoomBuilding` are transitive dependencies.
- In `Students_2NF`: `AdvisorName → AdvisorDept` is a transitive dependency.

Split further:

**`Instructors_3NF`**

| Instructor (PK) | InstructorOffice |
| --------------- | ---------------- |
| Dr. Ivanov      | Ivanov-101       |
| Dr. Petrova     | Petrova-202      |

**`Rooms_3NF`**

| RoomID (PK) | RoomBuilding |
| ----------- | ------------ |
| R5          | Main Hall    |
| R6          | Main Hall    |
| R7          | East Wing    |

**`Advisors_3NF`**

| AdvisorName (PK) | AdvisorDept |
| ---------------- | ----------- |
| Dr. Orlov        | CS          |
| Dr. Sokol        | EE          |

**`Courses_3NF`**

| CourseID (PK) | CourseName | Instructor (FK) | RoomID (FK) |
| ------------- | ---------- | --------------- | ----------- |
| C10           | Databases  | Dr. Ivanov      | R5          |
| C20           | Algorithms | Dr. Petrova     | R6          |
| C30           | Networks   | Dr. Ivanov      | R7          |

**`Students_3NF`**

| StudentID (PK) | StudentName | AdvisorName (FK) |
| -------------- | ----------- | ---------------- |
| S1             | Anna Popova | Dr. Orlov        |
| S2             | Boris Lee   | Dr. Orlov        |
| S3             | Chen Wu     | Dr. Sokol        |

(`Enrollments_2NF` and `StudentPhones_1NF` carry over unchanged.)

**What improved**: an instructor's office is stored once, period — regardless of how many courses they teach. Same for room→building and advisor→department. Every non-key column now describes only the entity named by the primary key.
**What's still broken**: nothing in _this_ example — our data happens not to trigger the BCNF edge case. We'll show that edge case separately below, since it requires a table with overlapping composite candidate keys.

---

## 5. Boyce-Codd Normal Form (BCNF) — the "3.5NF" checkpoint

### Definition

A table is in BCNF if, for every functional dependency **X → Y**, **X is a superkey** (i.e., X alone could determine every column in the table). It's a stricter version of 3NF that closes a loophole 3NF allows: 3NF tolerates a non-superkey X → Y dependency _if_ Y happens to be part of some candidate key; BCNF does not.

### Problems it solves

- Anomalies that only appear when a table has **two or more overlapping composite candidate keys**, where a non-key part of one key determines part of another.

### A short example, since our running tables don't naturally trigger it

Suppose we track tutoring sessions:

**`Tutoring`**

| StudentID | Subject | Tutor     |
| --------- | ------- | --------- |
| S1        | Math    | Dr. Orlov |
| S1        | Physics | Dr. Sokol |
| S2        | Math    | Dr. Orlov |

Rule: each `Tutor` teaches exactly **one** `Subject` (but a subject may have several tutors, and a student can have several tutors for different subjects). Candidate keys are `(StudentID, Subject)` and `(StudentID, Tutor)`. But `Tutor → Subject` holds, and `Tutor` is _not_ a superkey — this is 3NF-compliant (Subject is part of a candidate key) but **violates BCNF**.

**Anomaly**: if Dr. Orlov switches from teaching Math to teaching Chemistry, you must update _every row_ where Dr. Orlov appears, or the table becomes inconsistent (some rows say Math, some say Chemistry, for the same tutor).

**Decomposition into BCNF:**

`Tutors` (`Tutor` PK → `Subject`) and `Assignments` (`StudentID`, `Tutor` → composite PK). Now `Tutor → Subject` lives in exactly one place.

Our main running example (`Instructors_3NF`, `Rooms_3NF`, `Courses_3NF`, etc.) is already in BCNF — each has a single-column key that determines everything else, with no overlapping candidate keys.

---

## 6. Fourth Normal Form (4NF)

### Definition

A table is in 4NF if:

1. It is in BCNF, **and**
2. It has **no non-trivial multi-valued dependencies** — i.e., it doesn't force two or more _independent_ multi-valued facts about the same entity into one table.

A multi-valued dependency exists when, for a fixed value of one attribute, a set of values of another attribute exists **independently** of a third attribute.

### Problems it solves

- **Combinatorial redundancy**: when two independent one-to-many facts about the same entity are mixed into a single table, you're forced to store every _combination_ of them, even though they have nothing to do with each other.

### Applying it to our example

Suppose we want to track, per course, both the **set of textbooks used** and the **set of languages the course is offered in** — and these two facts are completely independent of each other (any textbook could pair with any language).

**Bad design (violates 4NF):**

| CourseID | Textbook            | Language  |
| -------- | ------------------- | --------- |
| C10      | "SQL Fundamentals"  | English   |
| C10      | "SQL Fundamentals"  | Ukrainian |
| C10      | "Relational Theory" | English   |
| C10      | "Relational Theory" | Ukrainian |

Notice: to add one more textbook for C10, you must add a row **for every existing language** (and vice versa) just to keep the cross-product consistent. That's the classic 4NF insertion anomaly.

**4NF fix** — split the two independent multi-valued facts into separate tables:

**`CourseTextbooks_4NF`**

| CourseID (FK) | Textbook          |
| ------------- | ----------------- |
| C10           | SQL Fundamentals  |
| C10           | Relational Theory |

**`CourseLanguages_4NF`**

| CourseID (FK) | Language  |
| ------------- | --------- |
| C10           | English   |
| C10           | Ukrainian |

**What improved**: adding a new textbook no longer requires touching language rows, and vice versa; no more forced cross-product rows.
**What's still possible**: a rarer case — a **join-dependency** that isn't reducible to simple multi-valued dependencies. That's what 5NF addresses.

---

## 7. Fifth Normal Form (5NF) — a.k.a. Project-Join Normal Form (PJ/NF)

### Definition

A table is in 5NF if:

1. It is in 4NF, **and**
2. It cannot be losslessly decomposed into **any** smaller set of tables without losing information encoded in a specific **join dependency** — informally, every join dependency in the table is implied by its candidate keys. If a table _can_ be split further without loss, and rejoining always reconstructs the original data exactly (no spurious rows), 5NF says you should split it.

### Problems it solves

- **Join anomalies caused by three-way (or more) circular business constraints** that can't be captured by simple functional or multi-valued dependencies — cases where pairwise combinations of two facts are _not_ independent of the third, but the three-way combination still has redundancy a plain 4NF table can't remove.

### A worked example for our scenario

Suppose the business rule is:

> An `Instructor` can teach a `Subject`. A `Subject` can be offered in a `Term` (semester). If an instructor teaches a subject, **and** that subject is offered in a term, **and** that instructor is teaching that term at all — then the instructor teaches that subject in that term.

This three-way circular rule is exactly the classic case a single table over-generalizes:

**`Teaching` (violates 5NF)**

| Instructor  | Subject   | Term     |
| ----------- | --------- | -------- |
| Dr. Ivanov  | Databases | Fall2026 |
| Dr. Ivanov  | Networks  | Fall2026 |
| Dr. Petrova | Databases | Fall2026 |

If we only know the three **pairwise** facts —

- Dr. Ivanov teaches {Databases, Networks}
- {Databases, Networks} offered in {Fall2026}
- Dr. Ivanov and Dr. Petrova both teach in {Fall2026}

— storing them as one flat three-column table forces us to either duplicate rows to represent all valid combinations, or risk implying a combination (e.g., "Dr. Petrova teaches Networks in Fall2026") that isn't actually true, purely as an artifact of the join.

**5NF fix** — decompose into the three pairwise relationships that actually hold independently:

**`InstructorSubject_5NF`**

| Instructor  | Subject   |
| ----------- | --------- |
| Dr. Ivanov  | Databases |
| Dr. Ivanov  | Networks  |
| Dr. Petrova | Databases |

**`SubjectTerm_5NF`**

| Subject   | Term     |
| --------- | -------- |
| Databases | Fall2026 |
| Networks  | Fall2026 |

**`InstructorTerm_5NF`**

| Instructor  | Term     |
| ----------- | -------- |
| Dr. Ivanov  | Fall2026 |
| Dr. Petrova | Fall2026 |

The actual valid `(Instructor, Subject, Term)` triples are recovered only where **all three pairwise facts agree** — this is the join-dependency property 5NF is built around. Any additional pairwise fact (say, Dr. Petrova starts teaching Networks) is added in exactly one small table, without touching the others, and without risk of the "phantom row" problem a flat table creates.

**What improved**: eliminated the last class of redundancy — one that only appears with three-or-more-way circular associations. Each independent pairwise fact is stored exactly once.
**What remains**: this is the final classical normal form; further "improvement" beyond 5NF (e.g., Domain-Key Normal Form, DKNF) is mostly of theoretical interest and rarely pursued in practice, since 5NF already removes all redundancy expressible via join dependencies.

---

## 8. Summary table

| NF   | Rule added                                         | Anomaly eliminated                                                   | Typical trigger                                         |
| ---- | -------------------------------------------------- | -------------------------------------------------------------------- | ------------------------------------------------------- |
| 1NF  | Atomic values, no repeating groups                 | Unqueryable multi-valued cells                                       | A column holding a list                                 |
| 2NF  | No partial dependency on a composite key           | Redundancy from attributes tied to only _part_ of the key            | Composite primary key                                   |
| 3NF  | No transitive dependency                           | Redundancy from attributes tied to _another non-key attribute_       | Non-key attribute determining another non-key attribute |
| BCNF | Every determinant is a superkey                    | Redundancy from overlapping composite candidate keys                 | Two overlapping candidate keys                          |
| 4NF  | No non-trivial multi-valued dependency             | Forced cross-product rows from two independent one-to-many facts     | Two independent multi-valued facts on one entity        |
| 5NF  | No non-trivial join dependency not implied by keys | Phantom/spurious rows from circular three-way (or more) associations | Circular constraints among 3+ tables                    |

## 9. Final schema (our running example, fully normalized to 5NF-equivalent design)

```
Students(StudentID PK, StudentName, AdvisorName FK)
StudentPhones(StudentID FK, Phone)
Advisors(AdvisorName PK, AdvisorDept)
Courses(CourseID PK, CourseName, Instructor FK, RoomID FK)
Instructors(Instructor PK, InstructorOffice)
Rooms(RoomID PK, RoomBuilding)
Enrollments(StudentID FK, CourseID FK, Grade)
CourseTextbooks(CourseID FK, Textbook)
CourseLanguages(CourseID FK, Language)
InstructorSubject(Instructor FK, Subject)
SubjectTerm(Subject FK, Term)
InstructorTerm(Instructor FK, Term)
```

Every fact now lives in exactly one place; every table's non-key columns depend on "the key, the whole key, and nothing but the key" (the classic mnemonic for 2NF–3NF), and no table forces artificial combinations of independent facts.

## 10. A practical note

In real-world schema design, most production databases stop at **3NF or BCNF** — 4NF and 5NF violations are relatively rare and often intentionally tolerated (denormalized) for read performance, with the anomalies managed at the application layer instead. Knowing the full ladder still matters: it tells you _exactly which anomaly you're choosing to accept_ when you denormalize on purpose.
