## 🧩 Zadanie: Normalizacja do 3NF

Poniższe przykłady przedstawiają tabele, które spełniają wymagania **drugiej postaci normalnej (2NF)**, ale **nie spełniają założeń trzeciej postaci normalnej (3NF)**.

📌 **Przypomnienie:**
Tabela jest w 3NF, jeśli:
- spełnia 2NF,
- nie zawiera zależności przechodnich (czyli kolumny nie powinny zależeć od innych kolumn niekluczowych).

---

### 🌍 Tabela 1: `CustomerLocation`

| CustomerId | Address           | City      | Country     | CountryCode |
|------------|-------------------|-----------|-------------|-------------|
| 1          | Main St 1         | Berlin    | Germany     | DE          |
| 2          | Green Ave 42      | Warsaw    | Poland      | PL          |
| 3          | Sunset Blvd 88    | Lisbon    | Portugal    | PT          |

> ❌ **Problem (3NF):** `CountryCode` zależy od `Country`, a nie bezpośrednio od `CustomerId`.

```sql
-- Tabela CustomerLocation
CREATE TABLE CustomerLocation (
    CustomerId INT PRIMARY KEY,
    Address VARCHAR(100),
    City VARCHAR(50),
    Country VARCHAR(50),
    CountryCode CHAR(2)
);

INSERT INTO CustomerLocation (CustomerId, Address, City, Country, CountryCode) VALUES
(1, 'Main St 1', 'Berlin', 'Germany', 'DE'),
(2, 'Green Ave 42', 'Warsaw', 'Poland', 'PL'),
(3, 'Sunset Blvd 88', 'Lisbon', 'Portugal', 'PT');
```

---

### 🛒 Tabela 2: `ProductPricing`

| ProductId | ProductName       | PriceTier | DiscountPercent |
|-----------|--------------------|-----------|------------------|
| 101       | Wireless Headphones| Premium   | 20               |
| 102       | USB Cable          | Standard  | 10               |
| 103       | Vinyl Record       | Standard  | 10               |

> ❌ **Problem (3NF):** `DiscountPercent` zależy od `PriceTier`, nie od `ProductId`.

```sql
-- Tabela ProductPricing
CREATE TABLE ProductPricing (
    ProductId INT PRIMARY KEY,
    ProductName VARCHAR(100),
    PriceTier VARCHAR(50),
    DiscountPercent INT
);

INSERT INTO ProductPricing (ProductId, ProductName, PriceTier, DiscountPercent) VALUES
(101, 'Wireless Headphones', 'Premium', 20),
(102, 'USB Cable', 'Standard', 10),
(103, 'Vinyl Record', 'Standard', 10);
```

---

### 🎯 Twoje zadanie:
Dla każdej z tabel:

1️⃣ Zidentyfikuj zależność przechodnią.  
2️⃣ Zaproponuj nowy, znormalizowany schemat tabel.  
3️⃣ Zdefiniuj odpowiednie klucze główne i obce.  
4️⃣ Użyj **transakcji**, aby przekopiować dane z oryginalnej tabeli do nowej struktury (`BEGIN`, `INSERT`, `COMMIT`).

---

🧠 **Powodzenia!**

