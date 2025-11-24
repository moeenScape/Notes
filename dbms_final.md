# Database Management System - Complete Course Material

## Course Overview
This comprehensive guide covers essential DBMS topics for university-level study, including ER diagrams, functional dependencies, normalization, B+ trees, and transaction management.

---

## Table of Contents
1. [ER Diagram (Entity-Relationship Diagram)](#1-er-diagram)
2. [Functional Dependency](#2-functional-dependency)
3. [Closure Method](#3-closure-method)
4. [Minimal Cover of FD](#4-minimal-cover-of-fd)
5. [Anomalies in Databases](#5-anomalies-in-databases)
6. [Normalization](#6-normalization)
7. [B+ Tree Operations](#7-b-tree-operations)
8. [ACID Properties](#8-acid-properties)
9. [Conflict Serializability](#9-conflict-serializability)
10. [Practice Problems](#10-practice-problems)
11. [Solved Exam Problems](#11-solved-exam-problems)

---

## 1. ER Diagram (Entity-Relationship Diagram)

### Definition
An ER diagram is a visual representation of entities, their attributes, and relationships in a database system.

### Components
- **Entity**: A real-world object (Rectangle)
- **Attribute**: Properties of entities (Oval)
- **Relationship**: Association between entities (Diamond)
- **Primary Key**: Uniquely identifies an entity (Underlined)

### Types of Attributes
1. **Simple**: Cannot be divided (e.g., Age)
2. **Composite**: Can be divided (e.g., Name → First Name, Last Name)
3. **Derived**: Calculated from other attributes (e.g., Age from DOB)
4. **Multi-valued**: Multiple values (e.g., Phone Numbers)

### Cardinality Ratios
- **One-to-One (1:1)**: One entity relates to one entity
- **One-to-Many (1:N)**: One entity relates to many entities
- **Many-to-Many (M:N)**: Many entities relate to many entities


### Real-World Example: University Database

```
Entities:
- STUDENT (StudentID, Name, DOB, Email, Phone)
- COURSE (CourseID, CourseName, Credits)
- PROFESSOR (ProfessorID, Name, Department, Salary)
- DEPARTMENT (DeptID, DeptName, Building)

Relationships:
- STUDENT enrolls in COURSE (M:N)
- PROFESSOR teaches COURSE (1:N)
- DEPARTMENT has PROFESSOR (1:N)
- DEPARTMENT offers COURSE (1:N)
```

### ER Diagram Notation
```
[STUDENT] ----< enrolls >----< [COURSE]
    |                              |
StudentID (PK)              CourseID (PK)
Name                        CourseName
DOB                         Credits
Email
Phone (multi-valued)

[PROFESSOR] ----< teaches >---- [COURSE]
    |
ProfessorID (PK)
Name
Department
Salary

[DEPARTMENT] ----< has >---- [PROFESSOR]
    |
DeptID (PK)
DeptName
Building
```

---

## 2. Functional Dependency

### Definition
A functional dependency (FD) X → Y means that the value of X uniquely determines the value of Y.

### Types of Functional Dependencies

1. **Trivial FD**: Y is a subset of X
   - Example: {StudentID, Name} → {StudentID}

2. **Non-Trivial FD**: Y is not a subset of X
   - Example: {StudentID} → {Name}

3. **Completely Non-Trivial**: X and Y have no common attributes
   - Example: {StudentID} → {Name, Email}


### Armstrong's Axioms

**Primary Rules:**
1. **Reflexivity**: If Y ⊆ X, then X → Y
2. **Augmentation**: If X → Y, then XZ → YZ
3. **Transitivity**: If X → Y and Y → Z, then X → Z

**Secondary Rules (Derived):**
4. **Union**: If X → Y and X → Z, then X → YZ
5. **Decomposition**: If X → YZ, then X → Y and X → Z
6. **Pseudo-transitivity**: If X → Y and WY → Z, then WX → Z

### Real-World Example: Employee Database

```
Relation: EMPLOYEE(EmpID, Name, DeptID, DeptName, Salary, ManagerID)

Functional Dependencies:
- EmpID → Name, DeptID, Salary, ManagerID
- DeptID → DeptName
- EmpID → DeptName (by transitivity)
```

---

## 3. Closure Method

### Definition
The closure of a set of attributes X (denoted X⁺) is the set of all attributes that can be functionally determined by X.

### Algorithm to Find Attribute Closure

```
Input: Set of FDs F, Set of attributes X
Output: X⁺ (closure of X)

Steps:
1. Initialize result = X
2. Repeat until no more attributes can be added:
   For each FD (A → B) in F:
       If A ⊆ result, then add B to result
3. Return result
```

### Example Problem

**Given:**
- Relation: R(A, B, C, D, E)
- FDs: {A → B, B → C, CD → E}
- Find: (AD)⁺

**Solution:**
```
Step 1: result = {A, D}
Step 2: Check A → B, A ⊆ {A,D}, so add B → result = {A, B, D}
Step 3: Check B → C, B ⊆ {A,B,D}, so add C → result = {A, B, C, D}
Step 4: Check CD → E, CD ⊆ {A,B,C,D}, so add E → result = {A, B, C, D, E}
Step 5: No more attributes to add

Answer: (AD)⁺ = {A, B, C, D, E}
```


### Finding Candidate Keys Using Closure

**Steps:**
1. Find attributes that appear only on the left side of FDs (must be in key)
2. Find attributes that appear only on the right side (cannot be in key)
3. Find attributes that appear on both sides (may or may not be in key)
4. Compute closure of combinations to find candidate keys

**Example:**
```
Relation: R(A, B, C, D)
FDs: {A → B, B → C, C → D}

Left only: A (must be in key)
Right only: D (not in key)
Both sides: B, C

Check (A)⁺:
- Start: {A}
- A → B: {A, B}
- B → C: {A, B, C}
- C → D: {A, B, C, D}

Since (A)⁺ = {A, B, C, D} = all attributes
Candidate Key = {A}
```

---

## 4. Minimal Cover of FD

### Definition
A minimal cover (canonical cover) is a simplified set of functional dependencies that is equivalent to the original set but has no redundancy.

### Properties of Minimal Cover
1. **Right side is single attribute**: Each FD has only one attribute on the right
2. **No extraneous attributes**: Removing any attribute breaks equivalence
3. **No redundant FDs**: Removing any FD breaks equivalence

### Algorithm to Find Minimal Cover

```
Input: Set of FDs F
Output: Minimal cover Fc

Steps:
1. Decompose RHS: Convert X → YZ to X → Y and X → Z
2. Remove extraneous attributes from LHS:
   For each FD X → A:
       For each attribute B in X:
           If ((X - B) → A) can be derived, remove B from X
3. Remove redundant FDs:
   For each FD X → A:
       If X → A can be derived from F - {X → A}, remove it
```


### Example Problem

**Given:**
```
F = {A → BC, B → C, A → B, AB → C}
```

**Solution:**

**Step 1: Decompose RHS**
```
F = {A → B, A → C, B → C, A → B, AB → C}
```

**Step 2: Remove extraneous attributes from LHS**
- Check AB → C: Can we derive from A → C? Yes!
  - (A)⁺ = {A, B, C}, so A → C exists
  - Remove AB → C or simplify to A → C (already exists)
```
F = {A → B, A → C, B → C}
```

**Step 3: Remove redundant FDs**
- Check A → C: Can we derive it from {A → B, B → C}?
  - A → B and B → C, by transitivity A → C
  - Yes, so remove A → C
```
Minimal Cover Fc = {A → B, B → C}
```

---

## 5. Anomalies in Databases

### Definition
Anomalies are problems that occur in poorly designed databases due to data redundancy.

### Types of Anomalies

#### 1. Insertion Anomaly
Cannot insert data without the presence of other data.

**Example:**
```
EMPLOYEE_DEPT(EmpID, Name, DeptID, DeptName, DeptLocation)

Problem: Cannot add a new department without hiring an employee first.
```

#### 2. Deletion Anomaly
Deleting data unintentionally removes other important data.

**Example:**
```
If we delete the last employee of a department, we lose department information.
```

#### 3. Update Anomaly
Updating data in one place requires updates in multiple places, leading to inconsistency.

**Example:**
```
If a department changes location, we must update all employee records in that department.
If we miss one, data becomes inconsistent.
```


### Real-World Example: Student Enrollment

**Poor Design:**
```
ENROLLMENT(StudentID, StudentName, CourseID, CourseName, InstructorName, Grade)

Sample Data:
| StudentID | StudentName | CourseID | CourseName | InstructorName | Grade |
|-----------|-------------|----------|------------|----------------|-------|
| 101       | Alice       | CS101    | DBMS       | Dr. Smith      | A     |
| 101       | Alice       | CS102    | OS         | Dr. Jones      | B     |
| 102       | Bob         | CS101    | DBMS       | Dr. Smith      | A     |

Problems:
- Insertion: Can't add a course without enrolling a student
- Deletion: If Bob drops CS101, we lose course info
- Update: If Dr. Smith's name changes, update multiple rows
```

**Solution: Normalize the database (see Normalization section)**

---

## 6. Normalization

### Definition
Normalization is the process of organizing data to minimize redundancy and dependency by dividing large tables into smaller ones and defining relationships.

### Normal Forms

#### 1NF (First Normal Form)
**Rules:**
- Each cell contains atomic (indivisible) values
- Each column contains values of the same type
- Each column has a unique name
- Order doesn't matter

**Example:**
```
❌ Not in 1NF:
STUDENT(StudentID, Name, Phones)
| StudentID | Name  | Phones           |
|-----------|-------|------------------|
| 101       | Alice | 123-456, 789-012 |

✅ In 1NF:
STUDENT(StudentID, Name, Phone)
| StudentID | Name  | Phone   |
|-----------|-------|---------|
| 101       | Alice | 123-456 |
| 101       | Alice | 789-012 |
```


#### 2NF (Second Normal Form)
**Rules:**
- Must be in 1NF
- No partial dependency (non-prime attributes must depend on the entire candidate key)

**Example:**
```
❌ Not in 2NF:
ENROLLMENT(StudentID, CourseID, StudentName, CourseName, Grade)
Primary Key: (StudentID, CourseID)

FDs:
- StudentID, CourseID → Grade ✓ (full dependency)
- StudentID → StudentName ✗ (partial dependency)
- CourseID → CourseName ✗ (partial dependency)

✅ In 2NF:
STUDENT(StudentID, StudentName)
COURSE(CourseID, CourseName)
ENROLLMENT(StudentID, CourseID, Grade)
```

#### 3NF (Third Normal Form)
**Rules:**
- Must be in 2NF
- No transitive dependency (non-prime attributes should not depend on other non-prime attributes)

**Example:**
```
❌ Not in 3NF:
EMPLOYEE(EmpID, Name, DeptID, DeptName)
Primary Key: EmpID

FDs:
- EmpID → Name, DeptID ✓
- DeptID → DeptName ✗ (transitive: EmpID → DeptID → DeptName)

✅ In 3NF:
EMPLOYEE(EmpID, Name, DeptID)
DEPARTMENT(DeptID, DeptName)
```

#### BCNF (Boyce-Codd Normal Form)
**Rules:**
- Must be in 3NF
- For every FD X → Y, X must be a super key

**Example:**
```
❌ Not in BCNF:
TEACHING(StudentID, Course, Instructor)
FDs:
- StudentID, Course → Instructor
- Instructor → Course (Instructor teaches only one course)

Problem: Instructor → Course, but Instructor is not a super key

✅ In BCNF:
STUDENT_INSTRUCTOR(StudentID, Instructor)
INSTRUCTOR_COURSE(Instructor, Course)
```


### Normalization Process Example

**Original Table:**
```
STUDENT_COURSE(StudentID, StudentName, StudentCity, CourseID, CourseName, InstructorID, InstructorName, Grade)

Sample Data:
| SID | SName | SCity  | CID  | CName | IID | IName     | Grade |
|-----|-------|--------|------|-------|-----|-----------|-------|
| 1   | Alice | NYC    | C101 | DBMS  | I1  | Dr. Smith | A     |
| 1   | Alice | NYC    | C102 | OS    | I2  | Dr. Jones | B     |
| 2   | Bob   | LA     | C101 | DBMS  | I1  | Dr. Smith | A     |
```

**Step 1: Convert to 1NF** (Already in 1NF - atomic values)

**Step 2: Convert to 2NF**
```
Primary Key: (StudentID, CourseID)

Partial Dependencies:
- StudentID → StudentName, StudentCity
- CourseID → CourseName, InstructorID, InstructorName

Split into:
STUDENT(StudentID, StudentName, StudentCity)
COURSE(CourseID, CourseName, InstructorID, InstructorName)
ENROLLMENT(StudentID, CourseID, Grade)
```

**Step 3: Convert to 3NF**
```
Transitive Dependency in COURSE:
- CourseID → InstructorID → InstructorName

Split into:
STUDENT(StudentID, StudentName, StudentCity)
INSTRUCTOR(InstructorID, InstructorName)
COURSE(CourseID, CourseName, InstructorID)
ENROLLMENT(StudentID, CourseID, Grade)
```

---

## 7. B+ Tree Operations

### Definition
A B+ tree is a self-balancing tree data structure that maintains sorted data and allows searches, insertions, and deletions in logarithmic time.

### Properties
- All data is stored in leaf nodes
- Internal nodes only store keys for navigation
- Leaf nodes are linked (allows range queries)
- Order m means: max m children, max m-1 keys per node
- Min keys in internal node: ⌈m/2⌉ - 1
- Min keys in leaf node: ⌈(m-1)/2⌉


### B+ Tree Insertion Algorithm

```
1. Find the appropriate leaf node for the key
2. If leaf has space (< m-1 keys):
   - Insert key in sorted order
3. If leaf is full:
   - Split the leaf into two nodes
   - Copy middle key up to parent
   - If parent is full, split parent recursively
```

### Insertion Example (Order 3: max 3 children, max 2 keys)

**Insert: 10, 20, 30, 40, 50**

```
Step 1: Insert 10
[10]

Step 2: Insert 20
[10, 20]

Step 3: Insert 30 (Node full, split)
      [20]
     /    \
  [10]    [20, 30]

Step 4: Insert 40
      [20]
     /    \
  [10]    [20, 30, 40]  (Full, will split on next insert)

Step 5: Insert 50 (Split leaf)
        [20, 30]
       /    |    \
    [10]  [20]  [30, 40, 50]  (Full again, split)

Final:
        [20, 40]
       /    |    \
    [10]  [20,30] [40,50]
```

### B+ Tree Deletion Algorithm

```
1. Find and delete the key from leaf
2. If leaf has enough keys (≥ ⌈(m-1)/2⌉):
   - Done
3. If leaf has too few keys:
   - Try to borrow from sibling
   - If cannot borrow, merge with sibling
   - Update parent recursively
```

### Deletion Example (Order 3)

**Starting Tree:**
```
        [20, 40]
       /    |    \
    [10]  [20,30] [40,50]
```

**Delete 30:**
```
        [20, 40]
       /    |    \
    [10]  [20]   [40,50]
(Leaf [20] has only 1 key, min required = 1, so OK)
```

**Delete 20:**
```
Leaf becomes empty, must merge or borrow
Borrow from right sibling [40,50]:

        [40]
       /    \
    [10]   [40,50]
```


### Real-World Application
B+ trees are used in:
- Database indexing (MySQL, PostgreSQL)
- File systems (NTFS, ext4)
- Fast range queries and sorted data retrieval

---

## 8. ACID Properties

### Definition
ACID properties ensure reliable transaction processing in database systems.

### A - Atomicity
**"All or Nothing"**
- A transaction is treated as a single unit
- Either all operations complete successfully, or none do

**Example:**
```
Bank Transfer: $100 from Account A to Account B

Transaction:
1. Deduct $100 from A
2. Add $100 to B

If step 2 fails, step 1 must be rolled back.
Otherwise, $100 disappears!
```

### C - Consistency
**"Valid State to Valid State"**
- Database must remain in a consistent state before and after transaction
- All integrity constraints must be maintained

**Example:**
```
Constraint: Account balance cannot be negative

Transaction: Withdraw $500 from account with $300
Result: REJECTED (would violate consistency)
```

### I - Isolation
**"Concurrent Transactions Don't Interfere"**
- Concurrent transactions execute as if they were serial
- Intermediate states are not visible to other transactions

**Example:**
```
Transaction T1: Transfer $100 from A to B
Transaction T2: Read balance of A

T2 should see either:
- Balance before T1 (if T1 not committed)
- Balance after T1 (if T1 committed)

NOT the intermediate state where A is debited but B not credited!
```


### D - Durability
**"Permanent Changes"**
- Once a transaction is committed, changes are permanent
- Survive system failures (power loss, crashes)

**Example:**
```
After confirming "Payment Successful", the payment record must persist
even if the server crashes immediately after.

Achieved through:
- Write-ahead logging
- Database backups
- Redundant storage
```

### ACID in Real-World Banking System

```
Scenario: Online money transfer

BEGIN TRANSACTION;
  -- Atomicity: Both must succeed or both fail
  UPDATE accounts SET balance = balance - 1000 WHERE account_id = 'A123';
  UPDATE accounts SET balance = balance + 1000 WHERE account_id = 'B456';
  
  -- Consistency: Check constraints
  IF (SELECT balance FROM accounts WHERE account_id = 'A123') < 0 THEN
    ROLLBACK;  -- Maintain consistency
  END IF;
  
COMMIT;  -- Durability: Changes are permanent

-- Isolation: Other transactions see consistent state
```

---

## 9. Conflict Serializability

### Definition
A schedule is conflict serializable if it can be transformed into a serial schedule by swapping non-conflicting operations.

### Conflicting Operations
Two operations conflict if:
1. They belong to different transactions
2. They access the same data item
3. At least one is a WRITE operation

**Conflict Types:**
- Read-Write (R-W) conflict
- Write-Read (W-R) conflict
- Write-Write (W-W) conflict

**Non-conflicting:**
- Read-Read (R-R) - can be swapped


### Testing Conflict Serializability - Precedence Graph Method

**Algorithm:**
1. Create a node for each transaction
2. Draw edge Ti → Tj if Ti conflicts with Tj and Ti's operation comes first
3. If graph has a cycle → NOT conflict serializable
4. If graph is acyclic → Conflict serializable

### Example 1: Conflict Serializable

**Schedule S:**
```
T1: R(A)  W(A)        R(B)  W(B)
T2:             R(A)              W(A)
```

**Step-by-step:**
```
1. T1: R(A) - T2: R(A) → No conflict (both reads)
2. T1: W(A) - T2: R(A) → Conflict! T1 → T2
3. T1: R(B) - T2: W(A) → No conflict (different items)
4. T1: W(B) - T2: W(A) → No conflict (different items)
```

**Precedence Graph:**
```
T1 → T2
```
No cycle → **Conflict Serializable**
Equivalent serial schedule: T1, T2

### Example 2: NOT Conflict Serializable

**Schedule S:**
```
T1: R(A)  W(A)        R(B)
T2:             R(B)        W(B)  R(A)
```

**Conflicts:**
```
1. T1: W(A) - T2: R(A) → T1 → T2
2. T1: R(B) - T2: W(B) → T1 → T2
3. T2: R(B) - T1: R(B) → No conflict (both reads)
4. T2: W(B) - T1: R(B) → T2 → T1 (T2's write comes after T1's read in schedule)
```

**Precedence Graph:**
```
T1 ⇄ T2  (cycle!)
```
Has cycle → **NOT Conflict Serializable**


### Example 3: Three Transactions

**Schedule S:**
```
T1: R(X)        W(X)
T2:       R(Y)        W(Y)
T3:                         R(X)  R(Y)
```

**Conflicts:**
```
1. T1: W(X) - T3: R(X) → T1 → T3
2. T2: W(Y) - T3: R(Y) → T2 → T3
```

**Precedence Graph:**
```
T1 → T3
T2 → T3
```
No cycle → **Conflict Serializable**
Equivalent serial schedules: T1,T2,T3 or T2,T1,T3

---

## 10. Practice Problems

### Problem 1: ER Diagram
**Design an ER diagram for a Library Management System with:**
- Books (ISBN, Title, Author, Publisher, Year)
- Members (MemberID, Name, Address, Phone)
- Loans (LoanID, LoanDate, DueDate, ReturnDate)
- A member can borrow multiple books
- A book can be borrowed by multiple members (at different times)

### Problem 2: Functional Dependencies
**Given Relation R(A, B, C, D, E) with FDs:**
```
F = {A → B, BC → E, ED → A}
```
**Tasks:**
a) Find (BC)⁺
b) Find all candidate keys
c) Is the FD A → E derivable from F?

### Problem 3: Minimal Cover
**Find the minimal cover of:**
```
F = {A → BC, B → C, A → B, AB → C, AC → D}
```


### Problem 4: Normalization
**Normalize the following table to 3NF:**
```
ORDER(OrderID, OrderDate, CustomerID, CustomerName, CustomerCity, 
      ProductID, ProductName, Quantity, UnitPrice, TotalPrice)

FDs:
- OrderID, ProductID → Quantity, UnitPrice, TotalPrice
- OrderID → OrderDate, CustomerID
- CustomerID → CustomerName, CustomerCity
- ProductID → ProductName, UnitPrice
- Quantity, UnitPrice → TotalPrice
```

### Problem 5: B+ Tree
**Given a B+ tree of order 4 (max 4 children, max 3 keys):**
Insert the following keys in order: 5, 15, 25, 35, 45, 55, 10, 20
Draw the final B+ tree structure.

### Problem 6: Conflict Serializability
**Determine if the following schedule is conflict serializable:**
```
T1: R(A)  W(A)              R(C)
T2:             R(B)  W(B)        W(A)
T3:       R(A)                    W(C)
```

---

## 11. Solved Exam Problems

### Solved Problem 1: Closure and Candidate Keys

**Question:**
Given R(A, B, C, D, E) with FDs: {A → BC, CD → E, B → D, E → A}
a) Find (A)⁺
b) Find all candidate keys

**Solution:**

**Part a) Find (A)⁺**
```
Step 1: result = {A}
Step 2: A → BC, so result = {A, B, C}
Step 3: B → D, so result = {A, B, C, D}
Step 4: CD → E, CD ⊆ {A,B,C,D}, so result = {A, B, C, D, E}

Answer: (A)⁺ = {A, B, C, D, E}
```

**Part b) Find all candidate keys**
```
Analysis:
- Left only: None
- Right only: None
- Both sides: A, B, C, D, E

Since (A)⁺ = {A, B, C, D, E}, A is a candidate key.

Check if any subset of A works: No (A is single attribute)
Check other single attributes:
- (B)⁺ = {B, D} ✗
- (C)⁺ = {C} ✗
- (D)⁺ = {D} ✗
- (E)⁺ = {E, A, B, C, D} = all attributes ✓

Candidate Keys: {A}, {E}
```


### Solved Problem 2: Minimal Cover

**Question:**
Find the minimal cover of F = {A → BC, B → C, A → B, AB → C, AC → D}

**Solution:**

**Step 1: Decompose RHS**
```
F = {A → B, A → C, B → C, A → B, AB → C, AC → D}
Remove duplicate A → B:
F = {A → B, A → C, B → C, AB → C, AC → D}
```

**Step 2: Remove extraneous attributes from LHS**

Check AB → C:
```
Can we derive from A → C?
(A)⁺ = {A, B, C} (using A → B, A → C, B → C)
Yes! A → C exists, so AB → C is redundant
Remove AB → C
F = {A → B, A → C, B → C, AC → D}
```

Check AC → D:
```
Can we derive from A → D?
(A)⁺ = {A, B, C} (no D)
Cannot remove C from LHS

Can we derive from C → D?
(C)⁺ = {C} (no D)
Cannot remove A from LHS

Keep AC → D
```

**Step 3: Remove redundant FDs**

Check A → C:
```
Can we derive from {A → B, B → C, AC → D}?
(A)⁺ using {A → B, B → C} = {A, B, C}
Yes! A → C can be derived by transitivity
Remove A → C
F = {A → B, B → C, AC → D}
```

Check if any other FD is redundant:
```
A → B: Cannot derive without it
B → C: Cannot derive without it
AC → D: Cannot derive without it
```

**Answer: Minimal Cover = {A → B, B → C, AC → D}**


### Solved Problem 3: Normalization to BCNF

**Question:**
Normalize R(Student, Course, Instructor, Room) to BCNF
```
FDs:
- Student, Course → Instructor, Room
- Instructor → Course
- Course → Room
```

**Solution:**

**Check for BCNF violations:**
```
Primary Key: (Student, Course)? No, because Instructor → Course
Let's find actual candidate key:

(Student, Instructor)⁺:
- Instructor → Course: {Student, Instructor, Course}
- Course → Room: {Student, Instructor, Course, Room}
Candidate Key: (Student, Instructor)

Check each FD:
1. Student, Instructor → Course, Room ✓ (LHS is super key)
2. Instructor → Course ✗ (Instructor is not a super key) - VIOLATION!
3. Course → Room ✗ (Course is not a super key) - VIOLATION!
```

**Decomposition:**

**Step 1: Handle Instructor → Course**
```
R1(Instructor, Course)
R2(Student, Instructor, Room)

Check R2 FDs:
- Student, Instructor → Room ✓
- But we still have Course → Room issue
```

**Step 2: Need to reconsider**
```
Better decomposition:

R1(Instructor, Course)  [Instructor → Course]
R2(Course, Room)        [Course → Room]
R3(Student, Instructor) [Student, Instructor → all via joins]

Verify BCNF:
- R1: Instructor is key ✓
- R2: Course is key ✓
- R3: (Student, Instructor) is key ✓
```

**Answer:**
```
R1(Instructor, Course)
R2(Course, Room)
R3(Student, Instructor)
```


### Solved Problem 4: B+ Tree Operations

**Question:**
Given a B+ tree of order 3 (max 3 children, max 2 keys per node).
Insert: 10, 20, 30, 40, 50, 60
Then delete: 20, 30

**Solution:**

**Insertions:**

```
Insert 10:
[10]

Insert 20:
[10, 20]

Insert 30 (split required):
      [20]
     /    \
  [10]    [20, 30]

Insert 40:
      [20]
     /    \
  [10]    [20, 30, 40]  (full)

Insert 50 (split leaf):
        [20, 30]
       /    |    \
    [10]  [20]  [30, 40, 50]  (full)

Insert 60 (split leaf):
        [20, 40]
       /    |    \
    [10]  [20,30] [40,50,60]  (full)

After split:
          [20, 40]
         /    |    \
      [10] [20,30] [40,50] [60]
```

**Deletions:**

```
Delete 20:
          [20, 40]
         /    |    \
      [10]  [30]  [40,50] [60]

[30] has only 1 key (min required = 1), OK

Delete 30:
[30] becomes empty, must merge or borrow

Merge [10] and [40,50]:
        [40]
       /    \
    [10]   [40,50,60]

Or borrow from sibling:
          [40]
         /    \
      [10]   [40,50,60]
```

**Final Tree:**
```
        [40]
       /    \
    [10]   [40,50,60]
```


### Solved Problem 5: Conflict Serializability

**Question:**
Check if the following schedule is conflict serializable:
```
S: R1(A), R2(A), W1(A), R3(B), W2(B), R1(B), W3(A), W1(B)
```

**Solution:**

**Step 1: Identify all operations**
```
T1: R(A), W(A), R(B), W(B)
T2: R(A), W(B)
T3: R(B), W(A)
```

**Step 2: Find conflicts**
```
1. R1(A) - R2(A): No conflict (both reads)
2. R1(A) - W3(A): Conflict! R1 before W3 → T1 → T3
3. R2(A) - W1(A): Conflict! R2 before W1 → T2 → T1
4. R2(A) - W3(A): Conflict! R2 before W3 → T2 → T3
5. W1(A) - W3(A): Conflict! W1 before W3 → T1 → T3
6. R3(B) - W2(B): Conflict! R3 before W2 → T3 → T2
7. R3(B) - W1(B): Conflict! R3 before W1 → T3 → T1
8. W2(B) - R1(B): Conflict! W2 before R1 → T2 → T1
9. W2(B) - W1(B): Conflict! W2 before W1 → T2 → T1
```

**Step 3: Build precedence graph**
```
Edges:
T1 → T3 (from conflicts 2, 5)
T2 → T1 (from conflicts 3, 8, 9)
T2 → T3 (from conflict 4)
T3 → T2 (from conflict 6)
T3 → T1 (from conflict 7)
```

**Step 4: Check for cycles**
```
T2 → T3 → T2  (CYCLE!)
```

**Answer: NOT Conflict Serializable** (due to cycle T2 ⇄ T3)


### Solved Problem 6: Complete Normalization Example

**Question:**
Given the relation and normalize to 3NF:
```
HOSPITAL(PatientID, PatientName, DoctorID, DoctorName, DoctorSpecialty, 
         AppointmentDate, RoomNo, RoomType, RoomCharge)

FDs:
1. PatientID, DoctorID, AppointmentDate → RoomNo
2. PatientID → PatientName
3. DoctorID → DoctorName, DoctorSpecialty
4. RoomNo → RoomType, RoomCharge
```

**Solution:**

**Step 1: Identify candidate key**
```
(PatientID, DoctorID, AppointmentDate)⁺ = all attributes
Candidate Key: (PatientID, DoctorID, AppointmentDate)
```

**Step 2: Check for 2NF violations (partial dependencies)**
```
Partial dependencies:
- PatientID → PatientName ✗
- DoctorID → DoctorName, DoctorSpecialty ✗
```

**Decompose to 2NF:**
```
PATIENT(PatientID, PatientName)
DOCTOR(DoctorID, DoctorName, DoctorSpecialty)
APPOINTMENT(PatientID, DoctorID, AppointmentDate, RoomNo, RoomType, RoomCharge)
```

**Step 3: Check for 3NF violations (transitive dependencies)**
```
In APPOINTMENT:
- PatientID, DoctorID, AppointmentDate → RoomNo ✓
- RoomNo → RoomType, RoomCharge ✗ (transitive dependency)
```

**Decompose to 3NF:**
```
PATIENT(PatientID, PatientName)
DOCTOR(DoctorID, DoctorName, DoctorSpecialty)
ROOM(RoomNo, RoomType, RoomCharge)
APPOINTMENT(PatientID, DoctorID, AppointmentDate, RoomNo)
```

**Final Answer (3NF):**
```
1. PATIENT(PatientID, PatientName)
2. DOCTOR(DoctorID, DoctorName, DoctorSpecialty)
3. ROOM(RoomNo, RoomType, RoomCharge)
4. APPOINTMENT(PatientID, DoctorID, AppointmentDate, RoomNo)
```


---

## Exam Tips and Common Mistakes

### For ER Diagrams
✅ **Do:**
- Clearly identify entities, attributes, and relationships
- Mark primary keys with underline
- Show cardinality ratios correctly
- Use proper notation (rectangles, ovals, diamonds)

❌ **Avoid:**
- Mixing attributes with entities
- Forgetting to show cardinality
- Creating redundant relationships

### For Functional Dependencies
✅ **Do:**
- Write closure step-by-step
- Check all FDs systematically
- Verify candidate keys by computing closure

❌ **Avoid:**
- Skipping steps in closure computation
- Forgetting Armstrong's axioms
- Not checking if closure equals all attributes

### For Normalization
✅ **Do:**
- Start from 1NF and progress sequentially
- Identify all FDs before decomposing
- Verify each normal form's conditions
- Ensure lossless join decomposition

❌ **Avoid:**
- Jumping directly to 3NF without checking 2NF
- Losing functional dependencies during decomposition
- Creating tables that cannot be joined back

### For B+ Trees
✅ **Do:**
- Remember: order m = max m children, max m-1 keys
- Calculate minimum keys: ⌈m/2⌉ - 1 for internal, ⌈(m-1)/2⌉ for leaf
- Show all intermediate steps during insertion/deletion
- Link leaf nodes in your diagram

❌ **Avoid:**
- Confusing B-tree with B+ tree (data only in leaves for B+)
- Forgetting to split when node is full
- Not maintaining minimum key requirements after deletion

### For Conflict Serializability
✅ **Do:**
- List all operations in order
- Identify all conflicting pairs systematically
- Draw precedence graph carefully
- Check for cycles thoroughly

❌ **Avoid:**
- Missing R-R operations (they don't conflict!)
- Drawing edges in wrong direction
- Declaring serializable when cycle exists
- Forgetting to check all transaction pairs


---

## Quick Reference Formulas

### B+ Tree
```
Order = m
Max keys per node = m - 1
Max children per node = m
Min keys in internal node = ⌈m/2⌉ - 1
Min keys in leaf node = ⌈(m-1)/2⌉
Min children in internal node = ⌈m/2⌉
```

### Normal Forms Summary
```
1NF: Atomic values, no repeating groups
2NF: 1NF + No partial dependency
3NF: 2NF + No transitive dependency
BCNF: 3NF + Every determinant is a candidate key
```

### Conflict Types
```
R-R: No conflict ✓
R-W: Conflict ✗
W-R: Conflict ✗
W-W: Conflict ✗
```

---

## Additional Practice Problems (Unsolved)

### Problem 7: Mixed Concepts
```
Given R(A, B, C, D, E, F) with FDs:
F = {AB → C, C → D, D → E, E → F, F → A}

a) Find (AB)⁺
b) Find all candidate keys
c) What is the highest normal form of R?
d) Decompose R into BCNF
```

### Problem 8: Complex B+ Tree
```
Order 4 B+ tree (max 4 children, max 3 keys)
Insert: 8, 5, 1, 7, 3, 12, 9, 6, 2, 10
Delete: 5, 1, 8
Draw the final tree.
```

### Problem 9: Serializability
```
Given schedule:
S: R1(X), R2(Y), W1(Y), R3(X), W2(X), W3(Y), W1(X)

a) Is it conflict serializable?
b) If yes, give an equivalent serial schedule
c) If no, explain why
```

### Problem 10: Real-World Design
```
Design a complete database for an E-commerce system:
- Customers can place orders
- Orders contain multiple products
- Products belong to categories
- Track inventory for each product
- Maintain order history and payment information

Provide:
a) ER Diagram
b) Relational schema in 3NF
c) Sample SQL CREATE TABLE statements
```

---

## Conclusion

This course material covers all essential DBMS topics for university exams. Practice the solved problems, attempt the unsolved ones, and review the exam tips before your test.

**Key Takeaways:**
1. Master closure computation for FD problems
2. Understand normalization step-by-step
3. Practice B+ tree operations with different orders
4. Use precedence graphs for serializability
5. Remember ACID properties with real examples

**Good luck with your exam! 🎓**

