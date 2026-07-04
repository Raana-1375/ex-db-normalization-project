# Database Normalization

## Original Table (Unnormalized)

The original dataset contains repeating groups (course1, course2, course3)
and partial/transitive dependencies that need to be resolved.

| id_student | name_student | classroom | classroom_description | course1 | course2 | course3 |
|---|---|---|---|---|---|---|
| 1 | Ana Martínez | A101 | Web Frontend | HTML | CSS | JavaScript |
| 2 | Luis Fernández | A102 | Web Backend | Java | Spring Framework | SQL |
| 3 | Carla Gómez | A101 | Web Frontend | HTML | CSS | JavaScript |
| 4 | Diego López | A103 | Desarrollo Mobile | Kotlin | Swift | Dart |
| 5 | Marta Sánchez | A102 | Web Backend | Java | Spring Framework | SQL |
| 6 | Javier Torres | A101 | Web Frontend | HTML | CSS | JavaScript |
| 7 | Laura Ruiz | A103 | Desarrollo Mobile | Kotlin | Swift | Dart |
| 8 | Pablo Ramírez | A102 | Web Backend | Java | Spring Framework | SQL |
| 9 | Sofía Navarro | A101 | Web Frontend | HTML | CSS | JavaScript |
| 10 | Tomás Ortega | A103 | Desarrollo Mobile | Kotlin | Swift | Dart |

---

## 1NF (First Normal Form)

**Rule:** Every field must be atomic (single value). No repeating column groups.

**Problem found:** `course1`, `course2`, `course3` are repeating groups representing
the same concept (a course).

**Solution:** Split each course into its own row.

| id_student | name_student | classroom | classroom_description | course |
|---|---|---|---|---|
| 1 | Ana Martínez | A101 | Web Frontend | HTML |
| 1 | Ana Martínez | A101 | Web Frontend | CSS |
| 1 | Ana Martínez | A101 | Web Frontend | JavaScript |
| ... | ... | ... | ... | ... |

Primary key: `(id_student, course)`

---

## 2NF (Second Normal Form)

**Rule:** Must satisfy 1NF. No partial dependency (every non-key column must
depend on the WHOLE primary key, not just part of it).

**Problem found:** `name_student`, `classroom`, and `classroom_description`
only depend on `id_student`, not on the full key `(id_student, course)`.
This is a partial dependency.

**Solution:** Split into two tables.

**Table: Students**

| id_student (PK) | name_student | classroom | classroom_description |
|---|---|---|---|
| 1 | Ana Martínez | A101 | Web Frontend |
| 2 | Luis Fernández | A102 | Web Backend |

**Table: Student_Courses**

| id_student (FK) | course |
|---|---|
| 1 | HTML |
| 1 | CSS |
| 1 | JavaScript |

---

## 3NF (Third Normal Form)

**Rule:** Must satisfy 2NF. No transitive dependency (a non-key column must
not depend on another non-key column).

**Problem found:** In the `Students` table, `classroom_description` and
`course` actually depend on `classroom`, not directly on `id_student`.
This is a transitive dependency: `id_student -> classroom -> classroom_description`.
The exercise statement confirms this: "Programming languages depend on the classroom."

**Solution:** Split into three final tables.

**Table: Students**

| id_student (PK) | name_student | classroom (FK) |
|---|---|---|
| 1 | Ana Martínez | A101 |
| 2 | Luis Fernández | A102 |
| 3 | Carla Gómez | A101 |
| 4 | Diego López | A103 |
| 5 | Marta Sánchez | A102 |
| 6 | Javier Torres | A101 |
| 7 | Laura Ruiz | A103 |
| 8 | Pablo Ramírez | A102 |
| 9 | Sofía Navarro | A101 |
| 10 | Tomás Ortega | A103 |

**Table: Classrooms**

| classroom (PK) | classroom_description |
|---|---|
| A101 | Web Frontend |
| A102 | Web Backend |
| A103 | Desarrollo Mobile |

**Table: Classroom_Courses**

| classroom (FK) | course |
|---|---|
| A101 | HTML |
| A101 | CSS |
| A101 | JavaScript |
| A102 | Java |
| A102 | Spring Framework |
| A102 | SQL |
| A103 | Kotlin |
| A103 | Swift |
| A103 | Dart |