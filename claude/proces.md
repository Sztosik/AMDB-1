
# Dokumentacja procesu — historia rozmowy z LLM (planowanie)

Format: **zapytano → dostano → zaakceptowano / odrzucono → dlaczego**.
Uczestnicy: użytkownik (projektant) i LLM (Claude, tryb planowania). LLM proponował warianty z rekomendacją, decyzję podejmował użytkownik.

## 1. Zlecenie początkowe
- **Zapytano:** przygotować wieloetapowy plan, według którego kolejny agent zbuduje konceptualny ERD dla opisu z `reality.md`; plan ma stosować zasady warsztatu, ale nie odwoływać się do materiałów warsztatowych, tylko do `reality.md`.
- **Dostano:** szkielet planu: twarde zasady dla agenta (poziom konceptualny, zero halucynacji, jawne założenia, flagowanie zamiast rozstrzygania) i numeracja zdań opisu Z1–Z20 do śledzenia pokrycia.
- **Zaakceptowano:** szkielet (bez uwag).

## 2. Tryb pracy, notacja, format wyniku
| Pytanie | Propozycje LLM | Decyzja | Dlaczego |
|---|---|---|---|
| Tryb pracy agenta | iteracyjny CoT (rekomendacja) / CoT w jednym przebiegu / jeden prompt | **Odrzucono wszystkie.** Własny wariant: najpierw razem piszemy i omawiamy plan fragment po fragmencie, potem agent tylko implementuje i flaguje nieścisłości | decyzje modelowe zostają po stronie projektanta; LLM nie może „przemycić” własnych rozstrzygnięć |
| Notacja | Mermaid `erDiagram` (rekomendacja) / PlantUML | **Mermaid** | renderuje się w Markdown/GitHub, crow's foot |
| Pliki wynikowe | jeden `erd.md` (rekomendacja) / osobne pliki | **jeden `erd.md`** | diagram i dokumentacja w jednym miejscu |

## 3. Fragment: encje
- **Dostano:** pewne encje UZYTKOWNIK, POJAZD, WYPOZYCZENIE, PLATNOSC oraz 3 kwestie sporne.

| Kwestia | Propozycje | Decyzja | Odrzucono i dlaczego |
|---|---|---|---|
| Cennik (Z18) | **TYP_POJAZDU** ze stawką (rek.) / encja CENNIK / tylko reguła | TYP_POJAZDU | CENNIK: związek przez wartość typu, słabszy; tylko reguła: stawka nie ma miejsca w modelu |
| Metoda płatności vs karta (Z6–Z7) | **KARTA_PLATNICZA** (rek.) / METODA_PLATNOSCI / nadtyp + podtyp | KARTA_PLATNICZA | opis zna tylko karty; ogólna nazwa lub nadtyp to halucynacja struktury |
| Lokalizacja GPS | **atrybut złożony** (rek.) / encja LOKALIZACJA | atrybut złożony | lokalizacja nie ma własnej tożsamości w opisie |

## 4. Fragment: atrybuty i identyfikatory
- **Dostano:** propozycja atrybutów z pokryciem w Z# i 4 kwestie sporne.

| Kwestia | Propozycje | Decyzja | Odrzucono i dlaczego |
|---|---|---|---|
| Typ wymagany przez Mermaid | **dziedziny pojęciowe** (rek.) / neutralny placeholder | dziedziny pojęciowe | placeholder niesie mniej informacji; typy SQL = mieszanie poziomów abstrakcji |
| Brak identyfikatorów w opisie (WYPOZYCZENIE, PLATNOSC, KARTA) | **naturalne + założenia** (rek.) / sztuczne id / brak id | naturalne (A1–A3), encje słabe | sztuczne id nie mają pokrycia w opisie; brak id zostawia model niepełnym |
| Kod CVC vs PCI DSS | **uwzględnić i oflagować** (rek.) / pominąć | uwzględnić + N1 | atrybut ma pokrycie w opisie; konflikt z praktyką to pytanie do klienta, nie decyzja LLM |
| Atrybut wyliczany `czas_trwania` | nie dodawać (rek.) / dodać jako pochodny | **Odrzucono rekomendację LLM** → dodać jako pochodny | projektant chce, żeby wielkość z Z18 była widoczna w modelu |

## 5. Fragment: związki i liczności
- **Dostano:** R1–R6 w notacji crow's foot, rozróżnienie związków identyfikujących i nieidentyfikujących, brak związków N:M.

| Kwestia | Propozycje | Decyzja | Odrzucono i dlaczego |
|---|---|---|---|
| R5: płatności na wypożyczenie | **0..N + nieścisłość** (rek.) / 0..1 + nieścisłość | 0..N + N2 | status „nieudana” sugeruje ponowienia; 0..1 by je wykluczało |
| R1: minimalna liczba kart | **0..N + reguła** (rek.) / 1..N | 0..N + B2 | 1..N jest sprzeczne z kolejnością rejestracja → logowanie → karta |

## 6. Fragment: reguły biznesowe i nieścisłości
- **Dostano:** reguły B1–B7 (z pokryciem) i nieścisłości N1–N8 (pytania do klienta).
- **Kwestia:** czy „oczywiste” N5 (końcowa lokalizacja aktualizuje pojazd) i N6 (wypożyczyć można tylko dostępny pojazd) mają być regułami.
  - **Decyzja:** zostają nieścisłościami (rek.). **Odrzucono:** przeniesienie do reguł jako założeń, bo byłoby to dodanie wymagań „bo zwykle tak jest”.
- **Zaakceptowano:** listy B1–B7 i N1–N8 jako kompletne.

## 7. Fragment: struktura `erd.md` i weryfikacja
- **Dostano:** 10 sekcji `erd.md` (cel, diagram, słownik, atrybuty, związki, reguły, założenia, nieścisłości, macierz pokrycia, log decyzji); weryfikacja przez render `mermaid-cli` i listę kontrolną (brak FK i typów SQL, pokrycie Z1–Z20).
- **Zaakceptowano:** strukturę i weryfikację bez zmian; język dokumentu: **polski** (odrzucono angielski).

## 8. Zakończenie planowania
- **Zapytano (LLM):** o zatwierdzenie planu do implementacji.
- **Odrzucono:** projektant nie zatwierdził przejścia do implementacji, tylko poprosił o zapisanie planu do pliku Markdown.
- **Dostano:** plan istnieje w tym pliku; zapis w repozytorium był zablokowany przez tryb planowania, więc LLM zaproponował polecenie `cp` albo wyjście z trybu planowania.
- **Następnie zapytano:** o streszczenie historii rozmowy w formacie „co zapytano / dostano / zaakceptowano / odrzucono / dlaczego” (ta sekcja).

## Obserwacje z procesu
- Rekomendacje LLM przyjęto w 13 z 14 decyzji. Odrzucono jedną (`czas_trwania` dodany jako pochodny) oraz cały zaproponowany zestaw trybów pracy.
- LLM sam zgłaszał miejsca, gdzie łatwo o halucynację (sztuczne id, CENNIK, nadtyp metody płatności, N5/N6), i proponował wariant zgodny z opisem.
- Ryzyko efektu automatyzacji: wysoki odsetek przyjętych rekomendacji. Warto przy ocenie krzyżowej sprawdzić, czy któraś z nich nie była błędna.
