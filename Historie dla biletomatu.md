## 2. Historie dla biletomatu (urządzenia)
1. Jako biletomat, chcę posiadać czytelny ekran dotykowy, aby użytkownik mógł łatwo nawigować po interfejsie.
2. Jako biletomat, chcę być wyposażony w różne metody płatności (terminal kart, czytnik gotówki, NFC), aby obsługiwać różnorodne transakcje.
3. Jako biletomat, chcę wydawać resztę w gotówce, jeśli użytkownik zapłaci nadmiarowo, aby transakcja była zgodna z oczekiwaniami.
4. Jako biletomat, chcę automatycznie aktualizować listę dostępnych biletów i ich cen, aby zapewnić zgodność z polityką przewoźnika.
5. Jako biletomat, chcę rejestrować wszystkie transakcje i wysyłać raporty do systemu centralnego, aby umożliwić monitoring i kontrolę operacji.

## Diagram przypadków użycia dla biletomatu

### 1. Szybki wybór rodzaju biletu
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
### 2. Reset interfejsu po anulowaniu transakcji
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

