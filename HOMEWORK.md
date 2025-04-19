## 🏠 Praca domowa: Normalizacja danych geograficznych

W bazie danych Chinook dane geograficzne (np. kraj, miasto, kod pocztowy) są **powielane** w wielu miejscach, m.in. w tabelach `Customer` oraz `Employee`. Takie rozwiązanie utrudnia utrzymanie spójności danych i ich późniejsze rozwijanie.

Twoim zadaniem będzie **zaprojektowanie lepszej struktury przechowywania danych adresowych** poprzez normalizację oraz wprowadzenie **rekursywnej tabeli hierarchii geograficznej** – czyli takiej, która umożliwia przechowywanie różnych poziomów lokalizacji (np. kraj, miasto, kod pocztowy) w jednej tabeli, wskazując nadrzędny poziom przez `ParentGeographyId`.

---

### 📌 Obecne podejście (uproszczone):

#### Tabela `Customer`
| CustomerId | FirstName | LastName | Address      | City     | PostalCode | Country   |
|------------|-----------|----------|--------------|----------|-------------|-----------|
| 1          | Anna      | Nowak    | Main St 12   | Berlin   | 10115       | Germany   |

#### Tabela `Employee`
| EmployeeId | FirstName | LastName | Address      | City     | PostalCode | Country   |
|------------|-----------|----------|--------------|----------|-------------|-----------|
| 3          | Maria     | Jenkins  | Office Blvd 8| Berlin   | 10115       | Germany   |

> ❌ **Problem:** dane o miejscowościach i krajach są powielane. Trudno kontrolować poprawność i spójność danych.

---

### 🎯 Twoje zadanie:

Zaprojektuj nową strukturę opartą na **rekursywnej hierarchii geograficznej**:

#### 1️⃣ Tabela `Geography`
Przechowuje wszystkie jednostki geograficzne (kraj, miasto, kod pocztowy itp.) w jednej tabeli.

```sql
CREATE TABLE Geography (
    GeographyId INT PRIMARY KEY,
    Name VARCHAR(100),
    Type VARCHAR(50),       -- Np. 'Country', 'City', 'PostalCode'
    ParentGeographyId INT,  -- Odwołanie do wyższego poziomu
    FOREIGN KEY (ParentGeographyId) REFERENCES Geography(GeographyId)
);
```

#### 🔍 Przykładowe dane dla tabeli `Geography`

| GeographyId | Name      | Type        | ParentGeographyId |
|-------------|-----------|-------------|--------------------|
| 4           | Poland    | Country     | NULL               |
| 5           | Warsaw    | City        | 4                  |
| 6           | 00-001    | PostalCode  | 5                  |

#### 2️⃣ Tabela `Address`
Zawiera dane dotyczące konkretnego adresu (ulica, numer), powiązane z najniższym poziomem geografii (np. kodem pocztowym).

```sql
CREATE TABLE Address (
    AddressId INT PRIMARY KEY,
    Street VARCHAR(100),
    BuildingNumber VARCHAR(10),
    ApartmentNumber VARCHAR(10),
    GeographyId INT,
    FOREIGN KEY (GeographyId) REFERENCES Geography(GeographyId)
);
```

#### 3️⃣ Powiązanie z `Customer` i `Employee`
Usuń kolumny `Address`, `City`, `PostalCode`, `Country` z oryginalnych tabel i wprowadź `AddressId` jako klucz obcy.

```sql
ALTER TABLE Customer ADD AddressId INT;
ALTER TABLE Employee ADD AddressId INT;

-- Potem:
ALTER TABLE Customer ADD FOREIGN KEY (AddressId) REFERENCES Address(AddressId);
ALTER TABLE Employee ADD FOREIGN KEY (AddressId) REFERENCES Address(AddressId);
```

---

### 📝 Co masz zrobić:

1️⃣ Zdefiniuj strukturę trzech powyższych tabel.  
2️⃣ Użyj danych przykładowych z tabel `Customer` i `Employee`, aby zapełnić `Geography` i `Address`.  
3️⃣ Przygotuj skrypty `INSERT` i `UPDATE`, aby powiązać istniejących klientów i pracowników z nową strukturą.  
4️⃣ (Opcjonalnie) Zaprojektuj widok SQL, który umożliwia wyświetlenie pełnych danych adresowych klienta/pracownika z połączenia kilku tabel przy użyciu `JOIN`ów rekurencyjnych.

---

🌍 **Powodzenia!** Pokaż, że umiesz projektować elastyczne i skalowalne struktury danych 😉

