## 1. Historie dla użytkownika
1. Jako użytkownik, chcę płacić za bilet kartą, gotówką lub telefonem, aby mieć większą elastyczność w wyborze metody płatności.
2. Jako użytkownik, chcę otrzymać wyraźne instrukcje na ekranie, aby wiedzieć, jak dokonać zakupu krok po kroku.
3. Jako użytkownik, chcę widzieć czas pozostały na decyzję (np. wyświetlany licznik czasu), aby móc szybko podjąć działanie.
4. Jako użytkownik, chcę szybko wybrać rodzaj biletu, aby zminimalizować czas spędzony przy biletomacie.
5. Jako użytkownik, chcę mieć możliwość wyboru języka, aby móc korzystać z biletomatu bez względu na znajomość języka lokalnego.
6. Jako użytkownik, chcę sprawdzić poprawność transakcji przed jej finalizacją, aby uniknąć pomyłek.
7. Jako użytkownik, chcę otrzymać potwierdzenie zakupu (np. wydruk biletu lub elektroniczny bilet), aby móc korzystać z transportu zgodnie z przepisami.

## Diagram przypadków użycia dla użytkownika

### 1. Szybki wybór rodzaju biletu
```mermaid
graph TB
    User((Użytkownik))

    UseCase1[Rozpoczęcie interakcji]
    UseCase2[Wybór kategorii biletu]
    UseCase3[Wybór biletu]
    UseCase4[Wyświetlenie podsumowania]
    UseCase5[Potwierdzenie wyboru]
    UseCase6[Anulowanie transakcji]
    UseCase7[Podpowiedź interfejsu]
    UseCase8[Weryfikacja dostępnych biletów]

    User --> UseCase1
    User --> UseCase6
    UseCase1 --> UseCase2
    UseCase2 -.include.-> UseCase8
    UseCase2 --> UseCase3
    UseCase3 --> UseCase4
    UseCase4 --> UseCase5
    UseCase7 -.extend.-> UseCase3

    subgraph System
        UseCase1
        UseCase2
        UseCase3
        UseCase4
        UseCase5
        UseCase6
        UseCase7
        UseCase8
    end
```

### 2. Anulowanie transakcji
```mermaid
graph TD
    User(("Użytkownik"))
    UseCase1["Rozpoczęcie interakcji"]
    UseCase2["Wybranie opcji anulowania"]
    UseCase3["Komunikat o anulowaniu"]
    UseCase4["Reset interfejsu"]
    UseCase5["Zapis powodu anulowania"]

    User --> UseCase1
    UseCase1 --> UseCase2
    UseCase2 --> UseCase4

    %% Include relationship
    UseCase2 -.include.-> UseCase3

    %% Extend relationship
    UseCase5 -.extend.-> UseCase2

    subgraph System
        UseCase1
        UseCase2
        UseCase3
        UseCase4
        UseCase5
    end
```
### 3.  Płatność za bilet
```mermaid
graph TD
    User(("Użytkownik"))
    UseCase1["Wybór metody płatności"]
    UseCase2["Weryfikacja metody płatności"]
    UseCase3["Realizacja płatności"]
    UseCase4["Potwierdzenie transakcji"]
    UseCase5["Anulowanie transakcji"]
    UseCase6["Obsługa błędów płatności"]

    User --> UseCase1
    User --> UseCase5
    UseCase1 --> UseCase3
    UseCase3 --> UseCase4
    
    %% Include relationships
    UseCase1 -.include.-> UseCase2
    
    
    %% Extend relationship
    UseCase6 -.extend.-> UseCase3
    
    subgraph System Płatności
        UseCase1
        UseCase2
        UseCase3
        UseCase4
        UseCase5
        UseCase6
    end
```
### 4. Sprawdzenie poprawności transakcji
```mermaid
graph TB
    User((Użytkownik))

    UseCase1[Wybór biletu i płatności]
    UseCase2[Wyświetlenie podsumowania]
    UseCase3[Potwierdzenie lub cofnięcie]
    UseCase4[Kontynuacja]
    UseCase5[Anulowanie transakcji]
    UseCase6[Ostrzeżenie o błędzie]

    User --> UseCase1
    UseCase1 -.include.-> UseCase5
    UseCase1 --> UseCase2
    UseCase2 --> UseCase3
    UseCase3 --> UseCase4
    UseCase3 -.extend.-> UseCase6

    subgraph System
        UseCase1
        UseCase2
        UseCase3
        UseCase4
        UseCase5
        UseCase6
    end
```

### 5. Wspólny diagram
```mermaid
graph TB
    User((Użytkownik))

    UseCase1[Szybki wybór rodzaju biletu]
    UseCase2[Anulowanie transakcji]
    UseCase3[Płatność za bilet]
    UseCase4[Sprawdzenie poprawności transakcji]



    User --> UseCase1
    User --> UseCase3
    User --> UseCase4
    UseCase1 -.include.->UseCase2
    UseCase3 -.include.-> UseCase2
    UseCase4 -.include.-> UseCase2

    subgraph System
        UseCase1
        UseCase2
        UseCase3
        UseCase4
    end
```

## DIAGRAMY SEKWENCJI  
### 1. DIAGRAM SEKWENCJI DLA PRZYPADKU UŻYCIA ANULOWANIA TRANSAKCJI  
- **AKTOR:** UŻYTKOWNIK.  
- **OBIEKTY:** INTERFEJS BILETOMATU, BILETOMAT, SYSTEM TRANSAKCYJNY.  
- **KOLEJNOŚĆ KOMUNIKATÓW:**  
  1. **UŻYTKOWNIK** rozpoczyna proces zakupu biletu poprzez **INTERFEJS BILETOMATU**.  
  2. **UŻYTKOWNIK** wybiera opcję "Anuluj" w dowolnym momencie procesu.  
  3. **INTERFEJS BILETOMATU** przekazuje żądanie anulowania do **BILETOMATU**.  
  4. **BILETOMAT** wysyła żądanie anulowania transakcji do **SYSTEMU TRANSAKCYJNEGO**.  
  5. **SYSTEM TRANSAKCYJNY** przetwarza żądanie i zwraca potwierdzenie anulowania.  
  6. **BILETOMAT** przekazuje potwierdzenie do **INTERFEJSU BILETOMATU**.  
  7. **INTERFEJS BILETOMATU** wyświetla **UŻYTKOWNIKOWI** komunikat potwierdzający anulowanie transakcji.  
  8. **INTERFEJS BILETOMATU** resetuje się do ekranu głównego.  

#### **SCENARIUSZ OPCJONALNY (ZAPISANIE POWODU ANULOWANIA):**  
  5a. **SYSTEM TRANSAKCYJNY** prosi **BILETOMAT** o podanie powodu anulowania.  
  6a. **BILETOMAT** przekazuje prośbę do **INTERFEJSU BILETOMATU**.  
  7a. **INTERFEJS BILETOMATU** wyświetla **UŻYTKOWNIKOWI** formularz do wprowadzenia powodu anulowania.  
  8a. **UŻYTKOWNIK** wprowadza powód anulowania.  
  9a. **INTERFEJS BILETOMATU** przekazuje powód do **BILETOMATU**, a następnie do **SYSTEMU TRANSAKCYJNEGO**.  
  10a. **SYSTEM TRANSAKCYJNY** zapisuje powód anulowania i zwraca potwierdzenie.  
  11a. **INTERFEJS BILETOMATU** wyświetla komunikat o anulowaniu i resetuje się do ekranu głównego. 

### WIZUALIZACJA DIAGRAMU SEKWENCJI  
```mermaid
sequenceDiagram
    participant User as UŻYTKOWNIK
    participant UI as INTERFEJS BILETOMATU
    participant B as BILETOMAT
    participant ST as SYSTEM TRANSAKCYJNY

    User->>UI: Rozpoczęcie procesu zakupu biletu
    User->>UI: Wybór opcji "Anuluj"
    UI->>B: Przekazanie żądania anulowania
    B->>ST: Żądanie anulowania transakcji
    ST-->>B: Potwierdzenie anulowania
    B-->>UI: Przekazanie potwierdzenia
    UI-->>User: Wyświetlenie komunikatu o anulowaniu
    UI->>UI: Reset do ekranu głównego

    opt Zapisanie powodu anulowania
        ST->>B: Prośba o podanie powodu anulowania
        B->>UI: Przekazanie prośby
        UI-->>User: Wyświetlenie formularza powodu anulowania
        User->>UI: Wprowadzenie powodu anulowania
        UI->>B: Przekazanie powodu
        B->>ST: Przekazanie powodu
        ST-->>B: Potwierdzenie zapisu powodu
        B-->>UI: Przekazanie potwierdzenia
        UI-->>User: Wyświetlenie komunikatu o anulowaniu
        UI->>UI: Reset do ekranu głównego
    end
```

### 2. Scenariusz dla przypadku użycia " Szybki wybór rodzaju biletu"

**Aktor:** Użytkownik.

**Obiekty:** Interfejs biletomatu, serwer aplikacji, Baza danych biletów.

**Kolejność komunikatów:**

  1. Użytkownik rozpoczyna interakcję z biletomatem.
  2. Interfejs wyświetla dostępne kategorie biletów (np. jednorazowe, okresowe).
  3. Użytkownik wybiera kategorię biletu.
  4. Interfejs przesyła wybór do serwera.
  5. Serwer sprawdza dostępne bilety w bazie danych.
  6. Baza danych zwraca listę biletów do serwera.
  7. Serwer przekazuje listę biletów do interfejsu.
  8. Interfejs wyświetla listę biletów użytkownikowi.
  9. Użytkownik wybiera konkretny bilet.
  10. Interfejs wyświetla podsumowanie wyboru.
  11. Użytkownik potwierdza wybór.
  12. serwer przetwarza transakcję.
  13. Baza danych aktualizuje stan biletów.
  14. System biletowy zwraca potwierdzenie do interfejsu.
  15. Interfejs wyświetla potwierdzenie transakcji użytkownikowi.


### Diagram sekwencji dla przypadku użycia " Szybki wybór rodzaju biletu"

```mermaid
sequenceDiagram
    participant U as Użytkownik
    participant B as Interfejs biletomatu
    participant S as Serwer aplikacji
    participant D as Baza danych biletów

    Note over U,D: Scenariusz główny
    U->>B: Rozpoczyna interakcję
    B->>U: Wyświetla dostępne kategorie biletów
    U->>B: Wybiera kategorię biletu
    B->>S: Przesyła wybór kategorii
    S->>D: Sprawdza dostępne bilety
    D-->>S: Zwraca listę biletów
    S-->>B: Przekazuje listę biletów
    B->>U: Wyświetla listę biletów
    U->>B: Wybiera konkretny bilet
    B->>U: Wyświetla podsumowanie wyboru
    U->>B: Potwierdza wybór
    B->>S: Przesyła potwierdzenie
    S->>D: Aktualizuje stan biletów
    D-->>S: Zwraca potwierdzenie aktualizacji
    S-->>B: Przekazuje potwierdzenie transakcji
    B->>U: Wyświetla potwierdzenie transakcji
```
