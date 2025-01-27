## 2. Historie dla biletomatu (urządzenia)
1. Jako biletomat, chcę posiadać czytelny ekran dotykowy, aby użytkownik mógł łatwo nawigować po interfejsie.
2. Jako biletomat, chcę być wyposażony w różne metody płatności (terminal kart, czytnik gotówki, NFC), aby obsługiwać różnorodne transakcje.
3. Jako biletomat, chcę wydawać resztę w gotówce, jeśli użytkownik zapłaci nadmiarowo, aby transakcja była zgodna z oczekiwaniami.
4. Jako biletomat, chcę automatycznie aktualizować listę dostępnych biletów i ich cen, aby zapewnić zgodność z polityką przewoźnika.
5. Jako biletomat, chcę rejestrować wszystkie transakcje i wysyłać raporty do systemu centralnego, aby umożliwić monitoring i kontrolę operacji.

## Diagram przypadków użycia dla biletomatu

### 1. Generowanie potwierdzenia zakupu
```mermaid
graph TD
    ST(("System Transakcyjny"))
    UseCase1["Potwierdzenie zakończenia transakcji"]
    UseCase2["Generowanie potwierdzenia"]
    UseCase3["Informacja o potwierdzeniu"]
    UseCase4["Oczekiwanie na odbiór"]
    UseCase5["Generowanie biletu"]
    UseCase6["Błąd generowania"]

    ST --> UseCase1
    UseCase1 --> UseCase2
    UseCase2 --> UseCase3
    UseCase3 --> UseCase4

    %% Include relationship
    UseCase2 -.include.-> UseCase5

    %% Extend relationship
    UseCase6 -.extend.-> UseCase2

    subgraph Biletomat
        UseCase1
        UseCase2
        UseCase3
        UseCase4
        UseCase5
        UseCase6
    end
```

### 2. Realizacja płatności
```mermaid
graph TD
    Biletomat(("Biletomat"))
    UseCase1["Inicjowanie płatności"]
    UseCase2["Przesłanie danych transakcji"]
    UseCase3["Oczekiwanie na odpowiedź"]
    UseCase4["Potwierdzenie płatności"]
    UseCase5["Błędy płatności"]
    UseCase6["Anulowanie transakcji"]
    UseCase7["Obsługa alternatyw"]

    Biletomat --> UseCase1
    UseCase1 --> UseCase2
    UseCase2 --> UseCase3
    UseCase3 --> UseCase4

    %% Include relationships
    UseCase1 -.include.-> UseCase5
    UseCase1 -.include.-> UseCase6

    %% Extend relationship
    UseCase7 -.extend.-> UseCase1

    subgraph System płatniczy
        UseCase1
        UseCase2
        UseCase3
        UseCase4
        UseCase5
        UseCase6
        UseCase7
    end
```
### 3. Wyświetlenie dostępnych biletów
```mermaid
graph TB
    Biletomat((Biletomat))

    UseCase1[Uruchomienie ekranu powitalnego]
    UseCase2[Pobranie listy biletów]
    UseCase3[Wyświetlenie biletów]
    UseCase4[Oczekiwanie na wybór użytkownika]
    UseCase5[Ostrzeżenie o braku danych]
    UseCase6[Aktualizacja biletów]

    Biletomat --> UseCase1
    UseCase1 --> UseCase2
    UseCase2 --> UseCase3
    UseCase3 --> UseCase4

    %% Include relationship
    UseCase2 -.include.-> UseCase6

    %% Extend relationship
    UseCase5 -.extend.-> UseCase2

    subgraph System Biletowy
        UseCase1
        UseCase2
        UseCase3
        UseCase4
        UseCase5
        UseCase6
    end
```
### 4. Reset interfejsu po anulowaniu transakcji
```mermaid
graph TB
    Biletomat((Biletomat))

    UseCase1[Rejestracja anulowania]
    UseCase2[Komunikat o anulowaniu]
    UseCase3[Reset interfejsu]
    UseCase4[Zapis powodu anulowania]

    Biletomat --> UseCase1
    UseCase1 --> UseCase3

    %% Include relationship
    UseCase1 -.include.-> UseCase2

    %% Extend relationship
    UseCase4 -.extend.-> UseCase1

    subgraph System Biletowy
        UseCase1
        UseCase2
        UseCase3
        UseCase4
    end
```
### 5. Wspólny diagram
```mermaid
graph TB
    User((Biletomat))

    UseCase1[Generowanie potwierdzenia zakupu]
    UseCase2[Realizacja płatności]
    UseCase3[Wyświetlenie dostępnych biletów]
    UseCase4[Reset interfejsu po anulowaniu transakcji]

    User --> UseCase1
    User --> UseCase2
    User --> UseCase3
    User --> UseCase4
    
    subgraph System
        UseCase1
        UseCase2
        UseCase3
        UseCase4
    end
```
## DIAGRAMY SEKWENCJI
### 1. DIAGRAM SEKWENCJI DLA PRZYPADKU UŻYCIA GENEROWANIA POTWIERDZENIA ZAKUPU
• **AKTOR:** BILETOMAT.  
• **OBIEKTY:** INTERFEJS BILETOMATU, UŻYTKOWNIK, SYSTEM TRANSAKCYJNY.  
• **KOLEJNOŚĆ KOMUNIKATÓW:**  
  1. **BILETOMAT** wysyła żądanie autoryzacji płatności do **SYSTEMU TRANSAKCYJNEGO**.  
  2. **SYSTEM TRANSAKCYJNY** przesyła potwierdzenie zakończenia transakcji do **BILETOMATU**.  
  3. **BILETOMAT** generuje potwierdzenie zakupu.  
  4. **BILETOMAT** informuje **INTERFEJS BILETOMATU** o sukcesie transakcji.  
  5. **INTERFEJS BILETOMATU** wyświetla użytkownikowi powiadomienie o możliwości odbioru potwierdzenia.  
  6. **UŻYTKOWNIK** odbiera potwierdzenie z **BILETOMATU**.  
  7. **INTERFEJS BILETOMATU** wyświetla użytkownikowi powiadomienie o możliwości odbioru biletu.  
  8. **UŻYTKOWNIK** odbiera bilet z **BILETOMATU**.  

#### **SCENARIUSZ ALTERNATYWNY 1 (BŁĄD GENEROWANIA POTWIERDZENIA):**  
  3a. **BILETOMAT** wykrywa błąd podczas generowania potwierdzenia.  
  4a. **BILETOMAT** informuje **INTERFEJS BILETOMATU** o błędzie.  
  5a. **INTERFEJS BILETOMATU** wyświetla użytkownikowi komunikat o błędzie.  
  6a. **INTERFEJS BILETOMATU** zgłasza błąd do **SYSTEMU TRANSAKCYJNEGO**.  
  7a. **SYSTEM TRANSAKCYJNY** potwierdza otrzymanie zgłoszenia błędu.    

### WIZUALIZACJA DIAGRAMU SEKWENCJI
```mermaid
sequenceDiagram
    participant B as Biletomat
    participant ST as System Transakcyjny
    participant IB as Interfejs Biletomatu
    participant U as Użytkownik

    B->>ST: Żądanie autoryzacji płatności
    ST-->>B: Potwierdzenie zakończenia transakcji
    alt Scenariusz główny
        B->>B: Generowanie potwierdzenia
        B->>IB: Informacja o potwierdzeniu zakupu  
        IB->>U: Powiadomienie o odbiorze potwierdzenia
        U->>B: Odbiór potwierdzenia
        B-->>U:  
        U-->>IB:    
        IB->>U: Powiadomienie o odbiorze biletu
        U->>B: Odbiór biletu
        B-->>U:  
        U-->>IB: 
        IB-->>B:  

    else Błąd podczas generowania potwierdzenia
        B->>B: Wykrycie błędu generowania
        B->>IB: Informacja o błędzie
        IB->>U: Komunikat o błędzie
        U-->>IB: 
        IB-->>ST: Zgłoszenie błędu do systemu transakcyjnego
        ST-->>B: Potwierdzenie otrzymania zgłoszenia
    end
```
### 2. SCENARIUSZ DLA PRZYPADKU UŻYCIA WSWIETLENIE DOSTĘPNYCH BILETÓW

• **AKTOR:** Biletomat, Użytkownik.
• **OBIEKTY:** Serwer aplikacji, Baza danych.
• **KOLEJNOŚĆ KOMUNIKATÓW:**  
  1. **Użytkownik**: Podchodzi do biletomatu i inicjuje interakcję.
  2. **Biletomat**: Wyświetla ekran powitalny użytkownikowi.
  3. **Biletomat**: Wysyła zapytanie do serwera aplikacji o listę dostępnych biletów.
  4. **Serwer aplikacji**: Przekazuje zapytanie do bazy danych w celu uzyskania aktualnej listy biletów.
  5. **Baza danych**: Zwraca listę dostępnych biletów do serwera aplikacji.
  6. **Serwer aplikacji**: Przekazuje listę biletów do biletomatu.
  7. **Biletomat**: Wyświetla listę kategorii biletów i szczegóły na ekranie.
  8. **Biletomat**: Czeka na wybór użytkownika.

#### Scenariusze alternatywne:


**Scenariusz alternatywny 1**: Brak połączenia z serwerem aplikacji
  3a. **Biletomat**: Wysyła zapytanie do serwera aplikacji o listę biletów.
  4a. **Serwer aplikacji**: Nie odpowiada z powodu awarii sieci lub błędu.
  5a. **Biletomat**: Wyświetla komunikat o braku możliwości pobrania danych i proponuje kontynuację w trybie offline (np. wyświetlenie wcześniej zapisanych danych).
  6a. **Biletomat**: Wyświetla listę biletów zapisanych lokalnie na ekranie.


**Scenariusz alternatywny 2**: Brak biletów w bazie danych
  3b. **Biletomat**: Wysyła zapytanie do serwera aplikacji o listę biletów.
  4b. **Serwer aplikacji**: Przekazuje zapytanie do bazy danych.
  5b. **Baza danych**: Zwraca pustą listę biletów.
  6b. **Serwer aplikacji**: Przekazuje informację o braku biletów do biletomatu.
  7b. **Biletomat**: Wyświetla komunikat o braku dostępnych biletów i sugeruje spróbowanie ponownie później.

### WIZUALIZACJA DIAGRAMU SEKWENCJI
```mermaid
sequenceDiagram
    participant U as Użytkownik
    participant B as Biletomat
    participant S as Serwer aplikacji
    participant D as Baza danych

    Note over U,D: Scenariusz główny
    U->>B: Inicjuje interakcję
    B->>U: Wyświetla ekran powitalny
    U-->>B: 
    
    B->>S: Wysyła zapytanie o listę biletów
    alt Scenariusz główny
        S->>D: Przekazuje zapytanie o listę biletów
        D-->>S: Zwraca listę dostępnych biletów
        S-->>B: Przekazuje listę biletów
        B-->>U: Czeka na wybór użytkownika
    else Scenariusz alternatywny 1: Brak połączenia
        S-->>B: Brak odpowiedzi (timeout)
        B->>U: Przekazanie komunikatu o błędzie
        B->>U: Przekazuje lokalnie zapisane bilety
    else Scenariusz alternatywny 2: Brak biletów
        S->>D: Przekazuje zapytanie o listę biletów
        D-->>S: Zwraca pustą listę
        S-->>B: Przekazuje informację o braku biletów
        B->>U: Wyświetla komunikat o braku biletów
        
    end
```
## Diagramy klas

### Analiza przypadku użycia "Generowanie potwierdzenia zakupu"

#### Klasy:
1. **Biletomat**
   - **Atrybuty:**
     - `string transactionId` – identyfikator transakcji.
     - `string confirmationType` – typ potwierdzenia (np. drukowane, elektroniczne).
   - **Metody:**
     - `void generujPotwierdzenie()` – generuje potwierdzenie zakupu.
     - `void informujUzytkownika()` – informuje użytkownika o możliwości odbioru potwierdzenia.
     - `void oczekujNaOdbior()` – czeka na odbiór potwierdzenia przez użytkownika.
     - `void obsluzBladGenerowania()` – obsługuje błąd podczas generowania potwierdzenia.

2. **System Transakcyjny**
   - **Atrybuty:**
     - `map<string, Transaction> transactions` – mapa przechowująca transakcje.
   - **Metody:**
     - `bool potwierdzZakonczenieTransakcji(string transactionId)` – potwierdza zakończenie transakcji.
     - `void zglosBlad(string transactionId, string errorMessage)` – zgłasza błąd do systemu.

3. **Interfejs Biletomatu**
   - **Atrybuty:**
     - `string currentScreen` – aktualnie wyświetlany ekran.
   - **Metody:**
     - `void wyswietlPowiadomienie(string komunikat)` – wyświetla powiadomienie użytkownikowi.
     - `void wyswietlBlad(string komunikat)` – wyświetla komunikat o błędzie.

4. **Użytkownik**
   - **Atrybuty:**
     - `string username` – nazwa użytkownika.
   - **Metody:**
     - `void odbierzPotwierdzenie()` – odbiera potwierdzenie zakupu.
     - `void odbierzBilet()` – odbiera bilet.

#### Relacje:
- **Biletomat** komunikuje się z **Systemem Transakcyjnym** w celu potwierdzenia zakończenia transakcji.
- **Biletomat** współpracuje z **Interfejsem Biletomatu**, aby informować użytkownika o potwierdzeniu zakupu i ewentualnych błędach.
- **Użytkownik** odbiera potwierdzenie i bilet z **Biletomatu**.

---

### Diagram klas dla przypadku użycia "Generowanie potwierdzenia zakupu"

```mermaid
classDiagram
    class Biletomat {
        - string transactionId
        - string confirmationType
        + void generujPotwierdzenie()
        + void informujUzytkownika()
        + void oczekujNaOdbior()
        + void obsluzBladGenerowania()
    }

    class SystemTransakcyjny {
        - map~string, Transaction~ transactions
        + bool potwierdzZakonczenieTransakcji(string transactionId)
        + void zglosBlad(string transactionId, string errorMessage)
    }

    class InterfejsBiletomatu {
        - string currentScreen
        + void wyswietlPowiadomienie(string komunikat)
        + void wyswietlBlad(string komunikat)
    }

    class Użytkownik {
        - string username
        + void odbierzPotwierdzenie()
        + void odbierzBilet()
    }

    Biletomat --> SystemTransakcyjny : komunikuje się
    Biletomat --> InterfejsBiletomatu : współpracuje
    Użytkownik --> Biletomat : odbiera potwierdzenie i bilet
```
### Analiza przypadku użycia "Wyświetlenie dostępnych biletów"

#### Klasy:
1. **Biletomat**
   - **Atrybuty**: 
     - `String nazwaBiletomatu`
     - `List<Bilet> lokalnaListaBiletów`
   - **Metody**:
     - `void uruchomEkranPowitalny()`
     - `void pobierzListeBiletów()`
     - `void wyświetlBilety()`
     - `void czekajNaWybórUżytkownika()`
     - `void wyświetlKomunikatBłąd(String komunikat)`

2. **SerwerAplikacji**
   - **Atrybuty**:
     - `String adresSerwera`
   - **Metody**:
     - `List<Bilet> pobierzListeBiletów()`
     - `void przekażListeBiletów(Biletomat biletomat)`

3. **BazaDanych**
   - **Atrybuty**:
     - `Map<String, Bilet> bilety`
   - **Metody**:
     - `List<Bilet> zwróćListeBiletów()`
     - `void dodajBilet(Bilet bilet)`
     - `void usuńBilet(String idBiletu)`

4. **Bilet**
   - **Atrybuty**:
     - `String idBiletu`
     - `String kategoria`
     - `double cena`
     - `Date dataWażności`
   - **Metody**:
     - `String getKategoria()`
     - `double getCena()`
     - `Date getDataWażności()`

5. **Użytkownik**
   - **Atrybuty**:
     - `String nazwaUżytkownika`
   - **Metody**:
     - `void inicjujInterakcję(Biletomat biletomat)`

#### Relacje:
- **Biletomat** jest powiązany z **SerwerAplikacji** (asocjacja) w celu pobrania listy biletów.
- **SerwerAplikacji** jest powiązany z **BazaDanych** (asocjacja) w celu uzyskania listy biletów.
- **Biletomat** przechowuje lokalną listę biletów (agregacja) w przypadku braku połączenia z serwerem.
- **Użytkownik** inicjuje interakcję z **Biletomat** (asocjacja).

### Diagram klas

```mermaid
classDiagram
    class Biletomat {
        - String nazwaBiletomatu
        - List<Bilet> lokalnaListaBiletów
        + void uruchomEkranPowitalny()
        + void pobierzListeBiletów()
        + void wyświetlBilety()
        + void czekajNaWybórUżytkownika()
        + void wyświetlKomunikatBłąd(String komunikat)
    }

    class SerwerAplikacji {
        - String adresSerwera
        + List<Bilet> pobierzListeBiletów()
        + void przekażListeBiletów(Biletomat biletomat)
    }

    class BazaDanych {
        - Map<String, Bilet> bilety
        + List<Bilet> zwróćListeBiletów()
        + void dodajBilet(Bilet bilet)
        + void usuńBilet(String idBiletu)
    }

    class Bilet {
        - String idBiletu
        - String kategoria
        - double cena
        - Date dataWażności
        + String getKategoria()
        + double getCena()
        + Date getDataWażności()
    }

    class Użytkownik {
        - String nazwaUżytkownika
        + void inicjujInterakcję(Biletomat biletomat)
    }

    Biletomat --> SerwerAplikacji : pobiera listę biletów
    SerwerAplikacji --> BazaDanych : pobiera listę biletów
    Biletomat --> Bilet : przechowuje lokalnie
    Użytkownik --> Biletomat : inicjuje interakcję
```
