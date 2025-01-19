## 2. Historie dla biletomatu (urządzenia)
1. Jako biletomat, chcę posiadać czytelny ekran dotykowy, aby użytkownik mógł łatwo nawigować po interfejsie.
2. Jako biletomat, chcę być wyposażony w różne metody płatności (terminal kart, czytnik gotówki, NFC), aby obsługiwać różnorodne transakcje.
3. Jako biletomat, chcę wydawać resztę w gotówce, jeśli użytkownik zapłaci nadmiarowo, aby transakcja była zgodna z oczekiwaniami.
4. Jako biletomat, chcę automatycznie aktualizować listę dostępnych biletów i ich cen, aby zapewnić zgodność z polityką przewoźnika.
5. Jako biletomat, chcę rejestrować wszystkie transakcje i wysyłać raporty do systemu centralnego, aby umożliwić monitoring i kontrolę operacji.

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
