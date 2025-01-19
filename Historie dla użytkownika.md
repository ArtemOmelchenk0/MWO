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
    User ----> UseCase6
    UseCase1 --> UseCase2
    UseCase2 --include--> UseCase8
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
    User --> UseCase2
    UseCase2 --> UseCase3
    UseCase3 --> UseCase4

    %% Extend relationship
    UseCase5 -.extend.-> UseCase2

    %% Include relationship
    UseCase2 --include--> UseCase3

    subgraph System
        UseCase1
        UseCase2
        UseCase3
        UseCase4
        UseCase5
    end

```
