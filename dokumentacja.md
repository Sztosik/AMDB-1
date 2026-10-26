# MobiGo — dokumentacja diagramu ERD

## Cel i zakres

Diagram to konceptualny model danych MobiGo: 6 encji i 6 związków, każdy element wynika z opisu rzeczywistości biznesowej (`reality.md`). Model nie zawiera kluczy obcych, typów danych ani tabel technicznych.

Notacja: crow's foot (Mermaid). Na diagramie są tylko nazwy atrybutów.

| Symbol | Znaczenie |
| --- | --- |
| dwie kreski | dokładnie 1 |
| kółko + „kurza stopka” | 0..N |
| linia ciągła | związek identyfikujący — encja po stronie N jest słaba |
| linia przerywana | związek nieidentyfikujący |


## Encje

Identyfikatory są naturalne — jedynym sztucznym jest `id_uzytkownika`, bo opis go wymienia.

| Encja | Co oznacza | Identyfikator | Atrybuty |
| --- | --- | --- | --- |
| UZYTKOWNIK | osoba zarejestrowana w aplikacji | id_uzytkownika (email unikalny — login) | imie, nazwisko, email, telefon, data_rejestracji |
| KARTA_PLATNICZA | karta dodana do opłacania przejazdów („metoda płatności”) | numer_karty | data_waznosci, kod_cvc |
| TYP_POJAZDU | rower elektryczny lub hulajnoga z własną stawką („cennik”) | nazwa | stawka_za_minute |
| POJAZD | konkretny pojazd w mieście | nr_identyfikacyjny (= kod QR) | poziom_baterii, status, ostatnia_lokalizacja |
| WYPOZYCZENIE | jeden przejazd od skanu QR do zakończenia | pojazd + data_godzina_rozpoczecia (encja słaba) | data_godzina_zakonczenia, lokalizacja_koncowa, czas_trwania (pochodny) |
| PLATNOSC | obciążenie karty za zakończony przejazd | wypożyczenie + data_transakcji (encja słaba) | kwota, status |

Lokalizacje to atrybuty złożone (szerokość, długość GPS), nie osobne encje.

## Związki i liczności

Wszystkie związki są typu 1:N, więc nie potrzeba encji asocjacyjnych — WYPOZYCZENIE łączy użytkownika z pojazdem.

| # | Związek | Liczność | Uzasadnienie |
| --- | --- | --- | --- |
| R1 | UZYTKOWNIK posiada KARTA_PLATNICZA | 1 : 0..N | karta należy do jednego użytkownika; konto istnieje, zanim doda kartę |
| R2 | UZYTKOWNIK dokonuje WYPOZYCZENIE | 1 : 0..N | każdy przejazd ma jednego użytkownika |
| R3 | POJAZD jest wypożyczany w WYPOZYCZENIE | 1 : 0..N, identyfikujący | przejazd dotyczy jednego pojazdu |
| R4 | TYP_POJAZDU klasyfikuje POJAZD | 1 : 0..N | pojazd ma jeden typ, typ wyznacza stawkę |
| R5 | WYPOZYCZENIE jest rozliczane przez PLATNOSC | 1 : 0..N, identyfikujący | 0 — przejazd trwa; N — ponowienia po nieudanej płatności |
| R6 | KARTA_PLATNICZA obciążana PLATNOSC | 1 : 0..N | płatność obciąża jedną wybraną kartę |

## Reguły biznesowe

Tych reguł z opisu nie da się narysować na diagramie, więc obowiązują obok niego.

- **B1** E-mail jest unikalny i służy jako login.
- **B2** Wypożyczenie wymaga co najmniej jednej karty.
- **B3** Rozpoczęcie zmienia status pojazdu na „wypożyczony”, zakończenie na „dostępny”.
- **B4** Kwota = czas_trwania × stawka_za_minute typu wypożyczonego pojazdu.
- **B5** Płatność powstaje dopiero po zakończeniu wypożyczenia.
- **B6** Obciążana karta musi należeć do użytkownika, który wypożyczył pojazd.
- **B7** Zakończenie jest późniejsze niż rozpoczęcie.

## Założenia i decyzje

- Ta sama karta nie występuje u dwóch użytkowników, więc numer karty ją identyfikuje.
- Pojazd nie może mieć dwóch wypożyczeń rozpoczętych w tej samej chwili.
- Płatność identyfikuje wypożyczenie i data transakcji.
- Cennik to stawka w TYP_POJAZDU, nie osobna encja — opis nie mówi o historii cen.
- KARTA_PLATNICZA zamiast ogólnej METODA_PLATNOSCI — opis zna tylko karty.
- kod_cvc zostaje, bo jest w opisie, mimo konfliktu z PCI DSS (pytanie poniżej).
- Celowo pominięto encje bez pokrycia w opisie: stacje, strefy, promocje, osobny cennik.
