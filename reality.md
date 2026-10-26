Opis Rzeczywistości Biznesowej
 Wyobraźmy sobie firmę "MobiGo", która oferuje wynajem rowerów elektrycznych i hulajnóg na minuty w mieście.

Proces korzystania z usług firmy wygląda następująco:
 Nowy użytkownik, aby móc korzystać z systemu, musi najpierw zarejestrować się w aplikacji mobilnej. Podczas rejestracji podaje swoje imię, nazwisko, adres e-mail
oraz numer telefonu. Każdy adres e-mail w systemie musi być unikalny, ponieważ służy on jako login. System automatycznie nadaje każdemu użytkownikowi unikalny
identyfikator i zapisuje datę rejestracji.
 Po zalogowaniu użytkownik musi dodać metodę płatności, aby opłacać przejazdy. Może dodać jedną lub więcej kart kredytowych, podając numer karty, datę jej
ważności oraz kod CVC. System przechowuje te dane w bezpieczny sposób.
 W mieście rozmieszczone są pojazdy – rowery elektryczne i hulajnogi. Każdy pojazd ma unikalny numer identyfikacyjny (widoczny też jako kod QR), który pozwala
go zidentyfikować. Dla każdego pojazdu system przechowuje informacje o jego typie (rower/hulajnoga), aktualnym poziomie naładowania baterii (w procentach),
statusie (dostępny, wypożyczony, w serwisie) oraz jego ostatniej znanej lokalizacji (współrzędne GPS).
 Użytkownik, chcąc rozpocząć przejazd, skanuje kod QR na pojeździe. To rozpoczyna proces wypożyczenia. System zapisuje, który użytkownik wypożyczył który
pojazd, oraz dokładną datę i godzinę rozpoczęcia wynajmu. Status pojazdu zmienia się na "wypożyczony".
 Po zakończeniu jazdy użytkownik kończy wypożyczenie w aplikacji. System zapisuje datę i godzinę zakończenia oraz końcową lokalizację pojazdu. Status pojazdu
wraca na "dostępny".
 Natychmiast po zakończeniu przejazdu system generuje płatność. Koszt przejazdu jest obliczany na podstawie czasu jego trwania oraz cennika obowiązującego dla
danego typu pojazdu (np. inna stawka za minutę dla roweru, inna dla hulajnogi). System tworzy rekord płatności, który zawiera obliczoną kwotę, datę transakcji oraz
jej status (np. "udana", "nieudana"). Płatność jest powiązana z konkretnym wypożyczeniem oraz metodą płatności wybraną przez użytkownika
