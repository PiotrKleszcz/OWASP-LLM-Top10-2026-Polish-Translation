## LLM10:2026 Nieprawidłowe przetwarzanie wyników

### Opis

Nieprawidłowe przetwarzanie wyników odnosi się konkretnie do niewystarczającej walidacji, oczyszczania i przetwarzania wyników generowanych przez duże modele językowe przed przekazaniem ich do innych komponentów i systemów. Ponieważ treści generowane przez LLM mogą być kontrolowane za pomocą poleceń, zachowanie to jest podobne do zapewnienia użytkownikom pośredniego dostępu do dodatkowych funkcji.
Nieprawidłowe przetwarzanie wyników dotyczy niebezpiecznego wykorzystania wyników modelu przed przekazaniem ich dalej, natomiast LLM07:2026 Dezinformacja dotyczy wyników nieprawidłowych lub wprowadzających w błąd. Walidację i sanityzację danych wejściowych modelu omawia LLM01:2026 Wstrzyknięcie polecenia.
Wykorzystanie luki w zabezpieczeniach związanej z nieprawidłowym przetwarzaniem danych wyjściowych może skutkować atakami XSS i CSRF w przeglądarkach internetowych, a także atakami SSRF, eskalacją uprawnień lub zdalnym wykonaniem kodu w systemach zaplecza.
Następujące warunki mogą zwiększyć wpływ tej luki w zabezpieczeniach:

* Nadmierne uprawnienia aplikacyjne przyznane LLM, umożliwiające eskalację uprawnień lub zdalne wykonanie kodu.
* Podatność na pośrednie wstrzyknięcie polecenia, które może umożliwić atakującemu uzyskanie uprzywilejowanego dostępu do środowiska docelowego użytkownika.
* Niezwalidowane dane wejściowe z narzędzi stron trzecich.
* Brak kodowania wyników właściwego dla danego kontekstu (np. HTML, JavaScript, SQL).
* Niewystarczające monitorowanie i rejestrowanie wyników LLM.
* Brak ograniczania częstotliwości żądań lub wykrywania anomalii w zakresie korzystania z LLM.
* Miejsca docelowe w postaci terminala, logów lub IDE, które renderują wyniki modelu bez neutralizowania znaków sterujących, takich jak sekwencje ucieczki ANSI.
* Mechanizmy renderujące po stronie klienta (przeglądarka, interfejs czatu, IDE, terminal), które automatycznie pobierają zasoby zewnętrzne, do których odwołują się wyniki modelu (np. obrazy w formacie Markdown, podglądy linków, ramki iframe), umożliwiając eksfiltrację danych z kontekstu za pomocą żądań wychodzących.

### Typowe przykłady ryzyka

1. Wyniki LLM są wprowadzane bezpośrednio do powłoki systemowej lub podobnej funkcji, takiej jak exec lub eval, co powoduje zdalne wykonanie kodu.
2. LLM generuje kod JavaScript lub Markdown i zwraca go użytkownikowi. Kod jest następnie interpretowany przez przeglądarkę, co powoduje atak XSS.
3. Zapytania SQL generowane przez LLM są wykonywane bez odpowiedniej parametryzacji, co prowadzi do wstrzyknięcia kodu SQL.
4. Wyniki LLM są wykorzystywane do tworzenia ścieżek plików bez odpowiedniej sanitizacji, co może potencjalnie skutkować lukami w zabezpieczeniach związanych z przechodzeniem ścieżek.
5. Treści generowane przez LLM są wykorzystywane w szablonach wiadomości e-mail bez odpowiedniego filtrowania lub oczyszczania, co może potencjalnie prowadzić do ataków phishingowych.
6. Wynik LLM zawierający sekwencje ucieczki ANSI lub inne znaki sterujące zostaje zapisany w terminalu, przeglądarce logów lub panelu IDE, które je interpretują, umożliwiając podszywanie się wizualne (visual spoofing), przejęcie schowka (np. OSC 52) lub wykorzystanie znanych podatności emulatorów terminala (Rehberger, 2024b).
7. Interfejs czatu automatycznie renderuje obrazy w formacie Markdown lub podglądy linków, do których odwołują się wyniki modelu, co pozwala atakującemu kontrolującemu część kontekstu modelu na eksfiltrację danych z rozmowy za pośrednictwem nazwy hosta lub ciągu zapytania w adresie URL obrazu (Rehberger, 2024a).

### Strategie zapobiegania i ograniczania skutków

1. Traktuj model jak każdego innego użytkownika, stosując podejście oparte na zerowym zaufaniu i stosuj odpowiednią walidację danych wejściowych w odpowiedziach pochodzących z modelu do funkcji zaplecza.
2. Postępuj zgodnie z wytycznymi OWASP ASVS (Application Security Verification Standard), aby zapewnić skuteczną walidację i sanityzację danych wejściowych (OWASP, b.d.).
3. Koduj dane wyjściowe modelu z powrotem do użytkowników, aby ograniczyć niepożądane wykonywanie kodu przez JavaScript lub Markdown. OWASP ASVS zawiera szczegółowe wytyczne dotyczące kodowania danych wyjściowych.
4. Wdrażaj kodowanie danych wyjściowych z uwzględnieniem kontekstu, w zależności od miejsca wykorzystania danych wyjściowych LLM (np. kodowanie HTML dla treści internetowych, kodowanie JavaScript dla kontekstów skryptów w przeglądarce).
5. Używaj zapytań parametrycznych lub przygotowanych instrukcji dla wszystkich operacji baz danych związanych z wynikami LLM.
6. Stosuj ścisłe zasady bezpieczeństwa treści (CSP), aby ograniczyć ryzyko ataków XSS z treści generowanych przez LLM.
7. Wdrażaj systemy rejestrowania i monitorowania w celu wykrywania nietypowych wzorców w wynikach LLM, które mogą wskazywać na próby wykorzystania luk.
8. Usuwaj z wyników modelu znaki sterujące (sekwencje ucieczki ANSI, BEL, OSC, backspace, powrót karetki) oraz inne niedrukowalne bajty, zanim zostaną zapisane w terminalach, plikach logów lub innych interpretujących je miejscach docelowych. Jeśli muszą zostać zachowane, koduj je w widocznej postaci.
9. W mechanizmach renderujących po stronie klienta (interfejsy czatu, IDE, klienty poczty e-mail, aplikacje mobilne) zapobiegaj sytuacji, w której wyniki modelu automatycznie wywołują żądania wychodzące do endpointów kontrolowanych przez atakującego. Domyślnie wyłącz automatyczne renderowanie obrazów w formacie Markdown, podglądów linków, ramek iframe i podobnych elementów. Tam, gdzie renderowanie jest konieczne, ogranicz pobieranie do jawnej listy dozwolonych źródeł lub kieruj je przez pośredniczący mechanizm pobierania po stronie serwera, który usuwa z adresów parametry zapytania przenoszące dane.

### Przykładowe scenariusze ataków

#### Scenariusz nr 1

Aplikacja wykorzystuje narzędzie LLM do generowania odpowiedzi dla funkcji chatbota. Narzędzie oferuje również szereg funkcji administracyjnych dostępnych dla innego uprzywilejowanego LLM. LLM ogólnego przeznaczenia przekazuje swoją odpowiedź bezpośrednio, bez odpowiedniej walidacji danych wyjściowych, do narzędzia, powodując jego zamknięcie w celu konserwacji.

#### Scenariusz nr 2

Użytkownik korzysta z narzędzia do tworzenia streszczeń stron internetowych opartego na LLM w celu wygenerowania zwięzłego streszczenia artykułu. Strona internetowa zawiera polecenie wstrzyknięcia, które nakazuje LLM przechwycenie poufnych treści ze strony internetowej lub z rozmowy użytkownika. Następnie LLM może zakodować poufne dane i wysłać je, bez żadnej weryfikacji wyników ani filtrowania, do serwera kontrolowanego przez atakującego.

#### Scenariusz nr 3

LLM umożliwia użytkownikom tworzenie zapytań SQL do źródłowej bazy danych za pomocą funkcji przypominającej czat. Użytkownik żąda zapytania o usunięcie wszystkich tabel bazy danych. Jeśli zapytanie utworzone przez LLM nie zostanie dokładnie sprawdzone, wszystkie tabele bazy danych zostaną usunięte.

#### Scenariusz nr 4

Aplikacja internetowa wykorzystuje LLM do generowania treści na podstawie poleceń tekstowych użytkownika bez oczyszczania danych wyjściowych. Atakujący może przesłać spreparowane polecenie, które spowoduje, że LLM zwróci nieoczyszczoną zawartość JavaScript, co doprowadzi do ataku XSS po wyrenderowaniu w przeglądarce ofiary. Atak ten był możliwy dzięki niewystarczającej walidacji i niewłaściwemu kodowaniu wyników modelu.

#### Scenariusz nr 5

LLM jest używany do generowania dynamicznych szablonów wiadomości e-mail na potrzeby kampanii marketingowej. Atakujący manipuluje LLM, aby umieścić złośliwy kod JavaScript w treści wiadomości e-mail. Jeśli aplikacja nie oczyści prawidłowo danych wyjściowych LLM, może to doprowadzić do ataków XSS na odbiorców, którzy wyświetlają wiadomości e-mail w podatnych na ataki klientach poczty elektronicznej.

#### Scenariusz nr 6

Aplikacja automatycznie kompiluje i wdraża kod wygenerowany przez LLM bez przeglądu przez człowieka i bez testów bezpieczeństwa. Ponieważ wynik jest uznawany za zaufany i wykonywany bez walidacji, niebezpieczny kod trafia do środowiska produkcyjnego i zostaje wykorzystany przez atakujących.
