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
    Biletomat(("Biletomat"))
    UseCase1["Potwierdzenie zakończenia transakcji"]
    UseCase2["Generowanie potwierdzenia"]
    UseCase3["Informacja o potwierdzeniu"]
    UseCase4["Oczekiwanie na odbiór"]
    UseCase5["Generowanie biletu"]
    UseCase6["Błąd generowania"]

    Biletomat --> UseCase1
    UseCase1 --> UseCase2
    UseCase2 --> UseCase3
    UseCase3 --> UseCase4

    %% Include relationship
    UseCase2 --include--> UseCase5

    %% Extend relationship
    UseCase6 -.extend.-> UseCase2

    subgraph System płatniczy
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
    UseCase1 --include--> UseCase5
    UseCase1 --include--> UseCase6

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
    UseCase2 --include--> UseCase6

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
    UseCase1 --include--> UseCase2

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
    UseCase5[Anulowanie transakcji]

    User --> UseCase1
    User --> UseCase2
    User --> UseCase3
    User --> UseCase4
    UseCase2 --include-->UseCase5
    
    subgraph System
        UseCase1
        UseCase2
        UseCase3
        UseCase4
        UseCase5
    end
```
---

## DIAGRAMY SEKWENCJI
### 1. DIAGRAM SEKWENCJI DLA PRZYPADKU UŻYCIA GENEROWANIA POTWIERDZENIA ZAKUPU
• **AKTOR:** UŻYTKOWNIK.  
• **OBIEKTY:** INTERFEJS BILETOMATU, BILETOMAT, SYSTEM TRANSAKCYJNY.  
• **KOLEJNOŚĆ KOMUNIKATÓW:**  
  1 **UŻYTKOWNIK** dokonuje płatności w **INTERFEJSIE BILETOMATU**.  
  2 **INTERFEJS BILETOMATU** przekazuje dane do **BILETOMATU**.  
  3 **BILETOMAT** wysyła żądanie autoryzacji płatności do **SYSTEMU TRANSAKCYJNEGO**.  
  4 **SYSTEM TRANSAKCYJNY** przesyła potwierdzenie zakończenia transakcji do **BILETOMATU**.  
  5 **BILETOMAT** generuje potwierdzenie zakupu.  
  6 **BILETOMAT** informuje **INTERFEJS BILETOMATU** o sukcesie transakcji.  
  7 **INTERFEJS BILETOMATU** wyświetla użytkownikowi powiadomienie o możliwości odbioru potwierdzenia.  
  8 **UŻYTKOWNIK** odbiera potwierdzenie z **BILETOMATU**.  
  9 **INTERFEJS BILETOMATU** wyświetla użytkownikowi powiadomienie o możliwości odbioru biletu.  
  10 **UŻYTKOWNIK** odbiera bilet z **BILETOMATU**.  

#### **SCENARIUSZ ALTERNATYWNY 1 (BŁĄD GENEROWANIA POTWIERDZENIA):**  
  5a **BILETOMAT** wykrywa błąd podczas generowania potwierdzenia.  
  6a **BILETOMAT** informuje **INTERFEJS BILETOMATU** o błędzie.  
  7a **INTERFEJS BILETOMATU** wyświetla użytkownikowi komunikat o błędzie.  
  8a **INTERFEJS BILETOMATU** zgłasza błąd do **SYSTEMU TRANSAKCYJNEGO**.  
  9a **SYSTEM TRANSAKCYJNY** potwierdza otrzymanie zgłoszenia błędu.  
  10a **UŻYTKOWNIK** wybiera opcję ponownej próby lub zwrotu środków w **INTERFEJSIE BILETOMATU**.  

### WIZUALIZACJA DIAGRAMU SEKWENCJI
```mermaid
sequenceDiagram
    participant U as Użytkownik
    participant IB as Interfejs Biletomatu
    participant B as Biletomat
    participant ST as System Transakcyjny

        U->>IB: Dokonanie płatności
        IB->>B: Przekazanie danych do biletomatu
        B->>ST: Żądanie autoryzacji płatności
        ST-->>B: Potwierdzenie zakończenia transakcji
    alt Prawidłowy przebieg procesu
        B->>B: Generowanie potwierdzenia
        B-->>IB: Informacja o sukcesie transakcji
        IB-->>U: Powiadomienie o odbiorze podtwierdzenia
        U->>B: Odbiór potwierdzenia
        IB-->>U: Powiadomienie o odbiorze biletu
        U->>B: Odbiór biletu
    else Błąd generowania potwierdzenia
        B->>B: Wykrycie błędu podczas generowania
        B->>IB: Informacja o błędzie
        IB-->>U: Komunikat o błędzie
        IB->>ST: Zgłoszenie błędu do systemu transakcyjnego
        ST-->>B: Potwierdzenie otrzymania zgłoszenia
        U->>IB: Wybór opcji (ponowna próba lub zwrot środków)
    end
```

