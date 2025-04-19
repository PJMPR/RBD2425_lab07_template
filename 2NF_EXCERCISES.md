## 🧩 Zadanie: Normalizacja do 2NF

Poniższe przykłady przedstawiają tabele, które spełniają wymagania **pierwszej postaci normalnej (1NF)**, ale **nie spełniają założeń drugiej postaci normalnej (2NF)**.

📌 **Przypomnienie:**
Tabela jest w 2NF, jeśli:
- spełnia 1NF,
- każdy atrybut niebędący częścią klucza głównego zależy od **całego** klucza głównego.

---

### 🎤 Tabela: `ArtistMembers`

| ArtistId | MemberName       | Instrument      | ArtistName   |
|----------|------------------|------------------|--------------|
| 1        | Sting            | Vocals, Bass     | The Police   |
| 1        | Stewart Copeland | Drums            | The Police   |
| 2        | James Hetfield   | Vocals, Guitar   | Metallica    |
| 2        | Lars Ulrich      | Drums            | Metallica    |

> ❌ **Problem (2NF):** Kolumna `ArtistName` zależy tylko od `ArtistId`, a nie od całego klucza głównego (`ArtistId`, `MemberName`).

```sql
-- Tabela ArtistMembers
CREATE TABLE ArtistMembers (
    ArtistId INT,
    MemberName VARCHAR(100),
    Instrument VARCHAR(100),
    ArtistName VARCHAR(100),
    PRIMARY KEY (ArtistId, MemberName)
);

-- Przykładowe dane
INSERT INTO ArtistMembers (ArtistId, MemberName, Instrument, ArtistName) VALUES
(1, 'Sting', 'Vocals, Bass', 'The Police'),
(1, 'Stewart Copeland', 'Drums', 'The Police'),
(2, 'James Hetfield', 'Vocals, Guitar', 'Metallica'),
(2, 'Lars Ulrich', 'Drums', 'Metallica');
```

---

### 🧑‍💼 Tabela: `EmployeeTasks`

| EmployeeId | TaskId | TaskName             | EmployeeName     | Status      | StartDate  | EndDate    |
|------------|--------|----------------------|------------------|-------------|------------|------------|
| 5          | 301    | Process Invoice      | Maria Jenkins    | Completed   | 2024-01-02 | 2024-01-03 |
| 5          | 302    | Update Inventory     | Maria Jenkins    | In Progress | 2024-01-04 | NULL       |
| 6          | 303    | Client Support Call  | Daniel Moore     | Completed   | 2024-01-05 | 2024-01-05 |
| 6          | 304    | Prepare Report       | Daniel Moore     | Not Started | NULL       | NULL       |

> ❌ **Problem (2NF):** Kolumna `EmployeeName` zależy tylko od `EmployeeId`, a nie od całego klucza głównego (`EmployeeId`, `TaskId`).

```sql
-- Tabela EmployeeTasks
CREATE TABLE EmployeeTasks (
    EmployeeId INT,
    TaskId INT,
    TaskName VARCHAR(100),
    EmployeeName VARCHAR(100),
    Status VARCHAR(50),
    StartDate DATE,
    EndDate DATE,
    PRIMARY KEY (EmployeeId, TaskId)
);

-- Przykładowe dane
INSERT INTO EmployeeTasks (EmployeeId, TaskId, TaskName, EmployeeName, Status, StartDate, EndDate) VALUES
(5, 301, 'Process Invoice', 'Maria Jenkins', 'Completed', '2024-01-02', '2024-01-03'),
(5, 302, 'Update Inventory', 'Maria Jenkins', 'In Progress', '2024-01-04', NULL),
(6, 303, 'Client Support Call', 'Daniel Moore', 'Completed', '2024-01-05', '2024-01-05'),
(6, 304, 'Prepare Report', 'Daniel Moore', 'Not Started', NULL, NULL);
```

---

### 🎯 Twoje zadanie:
Dla każdej z tabel:

1️⃣ Zidentyfikuj, co łamie zasady 2NF.  
2️⃣ Zaproponuj nowy, znormalizowany schemat tabel.  
3️⃣ Zdefiniuj odpowiednie klucze główne i obce.  
4️⃣ Użyj **transakcji**, aby przekopiować dane z oryginalnej tabeli do nowej struktury (`BEGIN`, `INSERT`, `COMMIT`).

---

🎧 **Powodzenia!**

