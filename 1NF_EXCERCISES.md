## 🧩 Zadanie: Normalizacja do 1NF

Na podstawie poniższych **nienormalnych struktur tabel** (nie spełniających 1NF), dokonaj ich transformacji tak, aby spełniały założenia **pierwszej postaci normalnej (1NF)**.

📌 **Przypomnienie:**
Tabela w 1NF nie powinna zawierać:
- powtarzających się grup,
- wielowartościowych atrybutów,
- zagnieżdżonych struktur danych.

---

### 📄 Tabela 1: `PaymentStructure`

| InvoiceId | 🗓️ PaymentDate | 💵 CashPayment | 💳 CardPayment | 💸 TransferPayment |
|-----------|---------------|----------------|----------------|--------------------|
| 101       | 2023-04-01    | 10.00          | 29.99          | NULL               |
| 102       | 2023-04-02    | NULL           | NULL           | 40.00              |
| 103       | 2023-04-03    | 10.00          | 20.00          | NULL               |
| 104       | 2023-04-04    | 5.00           | NULL           | NULL               |
| 105       | 2023-04-05    | NULL           | 50.00          | NULL               |

> ❌ **Problem:** osobne kolumny dla różnych metod płatności zamiast jednego, elastycznego atrybutu.

```sql
-- Tworzenie tabeli StrangePaymentStructure
CREATE TABLE PaymentStructure (
    InvoiceId INT PRIMARY KEY,
    PaymentDate DATE,
    CashPayment DECIMAL(10,2),
    CardPayment DECIMAL(10,2),
    TransferPayment DECIMAL(10,2)
);

-- Wstawienie przykładowych danych
INSERT INTO PaymentStructure (InvoiceId, PaymentDate, CashPayment, CardPayment, TransferPayment) VALUES
(101, '2023-04-01', 10.00, 29.99, NULL),
(102, '2023-04-02', NULL, NULL, 40.00),
(103, '2023-04-03', 10.00, 20.00, NULL),
(104, '2023-04-04', 5.00, NULL, NULL),
(105, '2023-04-05', NULL, 50.00, NULL);
```

---

### 👤 Tabela 2: `UserAccountRoles`

| AccountId | CustomerId | 👥 Username | 🛡️ Roles               |
|-----------|------------|-------------|------------------------|
| 1         | 34         | user34      | user, admin            |
| 2         | 55         | anowak      | user                   |
| 3         | 67         | jdoe        | user, editor, viewer   |

> ❌ **Problem:** kolumna `Roles` zawiera wiele wartości oddzielonych przecinkami, co łamie 1NF.

```sql
-- Tworzenie tabeli UserAccountRoles
CREATE TABLE UserAccountRoles (
    AccountId INT PRIMARY KEY,
    CustomerId INT,
    Username VARCHAR(50),
    Roles TEXT
);

-- Wstawienie przykładowych danych
INSERT INTO UserAccountRoles (AccountId, CustomerId, Username, Roles) VALUES
(1, 34, 'user34', 'user, admin'),
(2, 55, 'anowak', 'user'),
(3, 67, 'jdoe', 'user, editor, viewer');
```

---

### 🎯 Twoje zadanie:
Dla każdej z tabel:

1️⃣ Zidentyfikuj, co łamie zasady 1NF.  
2️⃣ Zaproponuj jednego lub więcej **nowych schematów tabel**, które będą spełniać wymagania 1NF.  
3️⃣ Zdefiniuj odpowiednie **klucze główne i obce**.  
4️⃣ Użyj **transakcji**, aby przekopiować dane z oryginalnej tabeli do nowej, znormalizowanej struktury (przy użyciu `BEGIN`, `INSERT`, `COMMIT`).

---

🎓 **Powodzenia!** 💪

