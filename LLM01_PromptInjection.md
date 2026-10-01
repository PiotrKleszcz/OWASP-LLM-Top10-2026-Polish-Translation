## LLM01:2026 Wstrzyknięcie polecenia

### Opis

**Podatność na wstrzyknięcie polecenia (prompt injection)** występuje wtedy, gdy dane wejściowe dużego modelu językowego (LLM) — niezależnie od tego, czy są to bezpośrednie dane od użytkownika, pobrane treści, wyniki narzędzi, treści graficzne, dźwiękowe lub wideo, pośrednie rozumowanie czy pamięć trwała — zmieniają zachowanie modelu w sposób niezamierzony przez twórcę aplikacji. Modele LLM nie rozróżniają na poziomie architektury „instrukcji” i „danych” (jedne i drugie są tokenami w tym samym strumieniu), dlatego nie istnieje czysty odpowiednik zapytań parametryzowanych (NCSC, 2025). Aby wpłynąć na model, dane wejściowe nie muszą być czytelne dla człowieka, nie muszą pochodzić bezpośrednio od użytkownika i nie muszą być widoczne w wyrenderowanym interfejsie.

Podatności na wstrzyknięcie polecenia wynikają ze sposobu, w jaki modele przetwarzają dane wejściowe, oraz z tego, jak te dane mogą zmusić model do nieprawidłowego przekazywania danych lub instrukcji innym częściom systemu. Trzy właściwości ujawniające się na etapie wdrożenia pogłębiają ten problem. Po pierwsze, **łączenie w oknie kontekstowym (context-window pooling)**: model traktuje monit systemowy, dane wejściowe użytkownika, pobrane dokumenty, wyniki narzędzi, historię rozmowy i pamięć jako jeden strumień tokenów, bez egzekwowanej granicy zaufania. Po drugie, **trwałość pamięci**: wstrzyknięcie, które zapisuje dane w pamięci długotrwałej, korpusie RAG, bazie wektorowej lub hostowanej usłudze pamięci, skaża każdą kolejną sesję odczytującą dane z tego magazynu. Po trzecie, **wykonanie agentowe**: gdy wynik modelu steruje wywołaniami narzędzi (system plików, powłoka, poczta e-mail, chmurowe API, serwery MCP, subagenci), zasięg rażenia (blast radius) rozszerza się z interfejsu czatu na wszystko, do czego sięgają narzędzia agenta, a wyniki narzędzi wracają do okna kontekstowego, umożliwiając efekty łańcuchowe lub samoreplikujące się.

Anatomię wstrzyknięcia polecenia można scharakteryzować w trzech wymiarach. **Powierzchnia dostarczenia** określa, w jaki sposób wstrzyknięcie dociera do modelu (bezpośrednie dane wejściowe, pobrane treści, wyniki narzędzi, kanał połączenia z narzędziem lub pamięć trwała). **Sposób propagacji** określa, jak rozprzestrzenia się ono w czasie i przez granice (jednorazowo, w wieloetapowym łańcuchu ataku (kill chain), między sesjami za pośrednictwem pamięci lub RAG albo samoreplikując się między agentami). **Kodowanie** określa, jak złośliwe instrukcje są reprezentowane w tokenach lub pikselach (zwykły tekst, base64 lub inne zaciemnianie, niewidoczne znaki Unicode, forma multimodalna lub steganograficzna, język o niskich zasobach). Rozłożenie scenariusza na te wymiary jest przydatnym krokiem modelowania zagrożeń przed wyborem odpowiednich środków zaradczych.

Waga i charakter skutecznego wstrzyknięcia polecenia zależą od kontekstu biznesowego, w którym działa model, oraz od zakresu sprawczości, z jakim został zaprojektowany. Wstrzyknięcie polecenia może prowadzić między innymi do następujących skutków:

* Ujawnienia informacji poufnych, treści monitu systemowego, pobranych prywatnych dokumentów lub szczegółów infrastruktury.
* Manipulacji wynikami modelu w celu wygenerowania stronniczych, szkodliwych lub wybranych przez atakującego treści, na podstawie których działają systemy lub użytkownicy w dalszych etapach przetwarzania.
* Nieautoryzowanego wywołania narzędzi, z których agent ma prawo korzystać, eskalującego do wykonania dowolnych poleceń i działań destrukcyjnych tam, gdzie agent ma dostęp do powłoki, systemu plików lub chmurowego API.
* Eksfiltracji danych za pośrednictwem kanałów opartych na adresach URL obrazów, ukrytych znaków Unicode w wyrenderowanych wynikach lub ukrytych kanałów bocznych w logowaniu narzędzi.
* Trwałego przejęcia kontroli nad zachowaniem agenta w wielu sesjach poprzez zatrucie pamięci lub korpusu RAG.

*Uwaga: wstrzyknięcie polecenia różni się od LLM02:2026 Ujawnianie informacji poufnych, które dotyczy tego, co model ujawnia w swoich wynikach, w tym treści kanału rozumowania, oraz od LLM03:2026 Nadmierna sprawczość, które dotyczy skutków dotarcia wyników modelu do uprzywilejowanych działań. Ta pozycja dotyczy samej granicy danych wejściowych. Sanityzację i walidację wyników modelu, zanim trafią one do komponentów w dalszych etapach przetwarzania, omawia LLM10:2026 Nieprawidłowe przetwarzanie wyników.*

---

### Rodzaje wstrzyknięcia polecenia

#### Bezpośrednie wstrzyknięcie polecenia

Użytkownik lub atakujący dysponujący ścieżką dostępu użytkownika dostarcza dane wejściowe, które zmieniają zachowanie modelu w niepożądany sposób. Bezpośrednie wstrzyknięcie może być **zamierzone** (złośliwy użytkownik tworzy jailbreak) lub **niezamierzone** (uprawniony użytkownik wkleja treść, która przypadkiem zawiera sprzeczne instrukcje, albo użytkownik, który korzysta z pomocy LLM, nieświadomie optymalizuje swoje dane wejściowe pod kątem niezwiązanego z nim LLM w dalszym etapie przetwarzania, jak w Scenariuszu nr 3).

Łamanie zabezpieczeń (jailbreaking) to podzbiór wstrzyknięć polecenia, w którym celem atakującego jest skłonienie modelu do naruszenia jego protokołów bezpieczeństwa. Zabezpieczenia na poziomie aplikacji pomagają je ograniczać, ale skuteczne zapobieganie wymaga ciągłego aktualizowania mechanizmów trenowania i zabezpieczeń modelu.

#### Pośrednie wstrzyknięcie polecenia

Model przyjmuje treść z zewnętrznego źródła (strony internetowej, dokumentu, wiadomości e-mail, odpowiedzi narzędzia, pobranego fragmentu RAG, obrazu, wyniku serwera MCP, wiersza bazy danych lub tytułu zgłoszenia), która zawiera dane działające jako wstrzyknięcie polecenia. Użytkownik nie dostarczył tych instrukcji ani ich nie widział. Profil zaufania powierzchni dostarczenia określa, jakie środki obrony są praktyczne:

* **Powierzchnie niezaufane.** Publiczne strony internetowe, wiadomości e-mail od nieznanych nadawców, wyniki wyszukiwania. Obrońcy muszą traktować wszystko, co pochodzi z tych źródeł, jako podejrzane. Większość badań nad wstrzyknięciem polecenia koncentrowała się właśnie na tym obszarze.
* **Powierzchnie częściowo zaufane.** Tytuły zgłoszeń w publicznym systemie śledzenia błędów, pliki README i dzienniki zmian pakietów, odpowiedzi API stron trzecich: treści, które użytkownik zdecydował się pobrać, ale których nie jest autorem. Użytkownik ufa platformie, ale niekoniecznie poszczególnym współtwórcom.
* **Powierzchnie zaufane.** Własne repozytoria, bazy danych, dokumenty wewnętrzne i poczta programisty. Programista może nie zdawać sobie sprawy, że atakujący umieścił tam treść, np. za pośrednictwem niezwiązanego wektora w wyższych etapach łańcucha, takiego jak publiczny formularz zgłaszania błędów.

Wspólna struktura: atakujący nie musi bezpośrednio przejmować kontroli nad backendem. Umieszcza tekst tam, gdzie odczyta go LLM programisty, a LLM, działając z uprawnieniami programisty, wykonuje resztę pracy. Środki obrony skupione wyłącznie na interfejsie czatu całkowicie to pomijają.

Pośrednie wstrzyknięcie polecenia coraz częściej zamienia własną instancję LLM użytkownika w broń skierowaną przeciwko jego własnemu backendowi. Schemat wygląda następująco: atakujący przesyła tekst do lokalizacji zaufanej przez użytkownika za pośrednictwem kanału o niskich uprawnieniach (publicznego formularza, zgłoszenia klienta, pull requesta od społeczności) i czeka, aż połączony z MCP agent użytkownika lub asystent programisty odczyta ten tekst, działając z podwyższonymi poświadczeniami użytkownika. To agent, a nie atakujący, wykonuje uprzywilejowane działanie (zob. Typowy przykład nr 3; Scenariusz nr 9 omawia produkcyjne dowody koncepcji (proof of concept)).

### Typowe przykłady ryzyka

1. **Bezpośrednie nadpisanie przez polecenie wejściowe**: wiadomość użytkownika nadpisuje rolę i ograniczenia możliwości określone w monicie systemowym, sprawiając, że model ujawnia informacje, generuje treści lub działa poza zamierzonym zakresem. Liczą się zarówno zamierzone, jak i niezamierzone dane wejściowe.

2. **Pośrednie wstrzyknięcie przez pobrane treści**: instrukcje atakującego są przenoszone we fragmencie RAG, na stronie internetowej, w dokumencie lub wiadomości e-mail i zostają wykonane, gdy treść trafi do okna kontekstowego (np. EmailGPT; INCIBE-CERT, 2024).

3. **Pośrednie wstrzyknięcie przez zaufaną powierzchnię**: tekst umieszczony w kanale o niskich uprawnieniach, ale zaufanym (system śledzenia zgłoszeń, formularz opinii, zgłoszenie do działu wsparcia), sprawia, że LLM użytkownika działa z jego podwyższonymi poświadczeniami — eksfiltruje repozytoria, zrzuca zawartość baz danych lub modyfikuje konfigurację IDE, czyli wykonuje działania, których atakujący nie mógłby przeprowadzić bezpośrednio (Invariant Labs, 2025; General Analysis, 2025; Rehberger, 2025a).

4. **Wstrzyknięcie multimodalne i steganograficzne**: niedostrzegalne dla człowieka zaburzenia w obrazach, dźwięku lub wideo są wyodrębniane przez koder (Clusmann et al., 2025; zob. Scenariusz nr 6).

5. **Wstrzyknięcie i eksfiltracja za pomocą niewidocznych znaków**: znaki Unicode z bloku znaczników (tag block), selektory wariantów i znaki o zerowej szerokości przenoszą instrukcje lub eksfiltrują bajty wewnątrz niewinnie wyglądającego tekstu. Dowód koncepcji ASCII smuggling dla M365 Copilot z sierpnia 2024 roku umożliwił eksfiltrację kodu MFA ze Slacka (Rehberger, 2024).

6. **Zatruwanie pamięci między sesjami i korpusu RAG**: jeden skażony wpis w pamięci trwałej lub korpusie RAG dociera do każdej przyszłej sesji, która go odczytuje (W. Zou et al., 2025; zob. Scenariusz nr 4).

7. **Interfejs dostrajania jako wyrocznia gradientowa („fun-tuning”)**: atakujący odczytuje wartość funkcji straty dla poszczególnych przykładów z API dostrajania udostępnianego przez dostawcę, aby zoptymalizować ładunek (payload) (skuteczność ataku na Gemini od 65% do 82%), przenosząc optymalizację w stylu white-box na modele o zamkniętych wagach (Labunets et al., 2025).

8. **Ładunki wielojęzyczne, zakodowane lub w językach o niskich zasobach**: dane wejściowe w językach o niskich zasobach oraz mieszające języki (code-mixed) zwiększają skuteczność ataku i omijają klasyfikatory, które nie były trenowane na danym schemacie, a kodowanie Base64, ROT13 lub emoji pozwala obejść filtry, które nigdy nie zetknęły się z danym kodowaniem (Hackett et al., 2025).

---

### Strategie zapobiegania i ograniczania skutków

Wstrzyknięcie polecenia jest nieodłączną cechą obecnej generatywnej AI: modele LLM nie rozróżniają na poziomie architektury instrukcji i danych, a ich zachowanie jest stochastyczne, dlatego obecnie nie istnieje żaden niezawodny mechanizm zapobiegawczy — stanowisko to jest zgodne z NIST (2025), NCSC (2025) oraz Debenedetti et al. (2025). Obrona ma więc charakter architektoniczny, a nie przechwytujący. Projektuj otaczający system przy wyraźnym założeniu, że granica instrukcji modelu prędzej czy później zostanie przełamana, i ograniczaj to, co model może robić, oraz to, dokąd mogą docierać jego wyniki, tak aby skuteczne wstrzyknięcie nie przełożyło się na skuteczny exploit.

Większość odnotowanych incydentów wstrzyknięcia polecenia o dużym wpływie stała się poważna, ponieważ wstrzyknięcie trafiło do systemu, którego narzędzia, zakresy uprawnień lub możliwości renderowania wyników pozwoliły przejętemu modelowi działać w imieniu atakującego na poziomie uprawnień użytkownika (zob. Scenariusze nr 7–9). Na tym polega praktyczny związek między tą pozycją a **LLM03:2026 Nadmierna sprawczość**: wstrzyknięcie polecenia to przejęcie po stronie danych wejściowych, a nadmierna funkcjonalność, nadmierne uprawnienia lub nadmierna autonomia sprawiają, że przejęcie to ma skutki poza oknem czatu. „Zabójcza triada” (lethal trifecta) Simona Willisona (2025) wyraża tę samą diagnozę strukturalną w postaci kontroli przed wdrożeniem: agent, który może jednocześnie uzyskiwać dostęp do danych prywatnych, przyjmować niezaufane treści i komunikować się na zewnątrz, spełnia warunki eksploatacji o dużym wpływie, a usunięcie któregokolwiek z tych trzech elementów je eliminuje.

Stosuj poniższe mechanizmy kontrolne w ramach obrony w głąb, ponieważ żaden pojedynczy mechanizm nie jest wystarczający. Niektóre z nich zmniejszają skuteczność wstrzyknięć i należy się spodziewać, że ich skuteczność spadnie wobec adaptacyjnych atakujących. Inne ograniczają zasięg rażenia po udanym wstrzyknięciu i to właśnie one sprawdzają się wobec atakujących, którzy mogą sondować system. We wdrożeniach agentowych kluczowe są mechanizmy najmniejszych uprawnień i budżetowania możliwości (nr 4 i 8), a pełne omówienie aspektu sprawczości zawiera **LLM03:2026**.

1. **Ogranicz rolę i możliwości modelu w monicie systemowym.** Stosuj deklaratywne zezwolenia i zakazy („pomagaj wyłącznie w X, nie uzyskuj dostępu do Y, nie przekazuj wyników na adresy zewnętrzne”), a nie otwarte uprawnienia. Jest to jedynie częściowy mechanizm kontrolny: atakujący, który odgadnie treść monitu, może go obejść (Nasr et al., 2025), dlatego łącz go z mechanizmami kontroli uprawnień opisanymi w pkt 4.

2. **Zdefiniuj ścisły schemat wyników i waliduj każdą odpowiedź w zaufanym kodzie aplikacji**, zanim jakikolwiek system w dalszym etapie przetwarzania na jej podstawie zadziała, stosując walidację strukturalną, a nie kolejne wywołanie LLM. Pozwala to wychwycić naruszenia formatu, ale nie manipulację semantyczną: odpowiedź zgodna ze schematem nadal może zawierać złośliwe zapytanie SQL lub treść wiadomości e-mail sformatowaną w celu eksfiltracji.

3. **Filtruj na każdej granicy modalności (tekst, obraz, dźwięk, dane strukturalne), a nie tylko tekst.** Uruchamiaj klasyfikatory właściwe dla danej modalności, OCR dla obrazów i transkrypcję dla dźwięku, a następnie stosuj filtry tekstowe do wyodrębnionej treści. Filtry semantyczne można obejść przez przeformułowanie lub zakodowanie treści, a dane wejściowe w językach o niskich zasobach oraz mieszające języki obniżają ich dokładność (Hackett et al., 2025).

4. **Przechowuj poświadczenia i możliwość zmiany stanu w kodzie aplikacji, a nie w modelu, i przyznawaj najmniejsze uprawnienia dla każdej operacji.** Kieruj uprzywilejowane wywołania przez deterministyczny silnik polityk, który w chwili wykonania ponownie weryfikuje intencję i argumenty. NIST AI 100-2 E2025 oraz wspólne wytyczne CISA i Five Eyes dotyczące OT (CISA et al., 2025) przedstawiają takie deterministyczne pośrednictwo jako podstawowe oczekiwanie przy zakupie. Szerokie uprawnienia nadawane „dla wygody” oraz przeskoki między wieloma agentami ponownie wprowadzają ryzyko w dalszych etapach przetwarzania.

5. **Usuwaj znaki z bloku znaczników (U+E0000 do E007F), selektory wariantów (U+FE00 do FE0F) i znaki o zerowej szerokości (U+200B, U+200C, U+200D, U+2060) na każdej granicy przyjmowania i renderowania danych.** Znaki te są niewidoczne przy normalnym renderowaniu i służą do przemycania instrukcji lub eksfiltrowanych bajtów (zob. Typowy przykład nr 5), a warianty wykorzystujące selektory wariantów (Rehberger, 2025c) pozwalają niewidocznie przemycać dowolne bajty. Usuwanie tych znaków nie powstrzymuje ładunków w widocznym tekście ani przyszłych klas ataków steganograficznych.

6. **Przekazuj treści zewnętrzne przez strukturalnie odrębny kanał oznaczony pochodzeniem**, aby model mógł odróżniać dane od instrukcji (S. Chen et al., 2025; Microsoft Research, 2025). Zmniejsza to skuteczność ataków wyłącznie w testach nieadaptacyjnych: atakujący znający schemat oznaczania może go naśladować, a mechanizm StruQ udało się obejść w ataku adaptacyjnym (Nasr et al., 2025).

7. **Wymagaj wyraźnego potwierdzenia przez człowieka przed każdym działaniem uprzywilejowanym, nieodwracalnym lub widocznym na zewnątrz**, prezentując osobie zatwierdzającej dokładne, wyrenderowane działanie, a nie jego podsumowanie. Przemycanie niewidocznych znaków może sprawić, że wyświetlane działanie będzie się różnić od faktycznie wykonanego (pkt 5), a zmęczenie zatwierdzaniem obniża trafność ocen przy dużej liczbie zatwierdzeń.

8. **Budżetuj możliwości agenta, przyjmując regułę dwóch (Rule of Two) jako minimum** (Meta AI, 2025). Traktuj jednoczesny dostęp do (A) niezaufanych danych wejściowych, (B) danych wrażliwych oraz (C) zmiany stanu lub komunikacji zewnętrznej jako wysokie ryzyko: każdy agent typu [A,B,C] wymaga zatwierdzenia przez człowieka dla każdego działania, a konfiguracje [A,B] lub [A,C] wymagają wyraźnej oceny ryzyka rezydualnego (zob. Scenariusz nr 8). Regułę tę popierają NIST AI 100-2 E2025 oraz wytyczne CISA, FBI, NSA i ACSC dotyczące OT (CISA et al., 2025); nie odnosi się ona jednak do głębokości autonomii (Noma Security, 2025).

9. **Traktuj zapisy do pamięci agenta jako operacje uprzywilejowane.** Rejestruj polecenie, które spowodowało zapis, klasyfikuj zapisy pod kątem treści zawierających instrukcje lub modyfikujących rolę i wymagaj zatwierdzenia, zanim wpisy zawierające instrukcje zostaną utrwalone między sesjami. Dowód koncepcji dla Gemini z lutego 2025 roku (Rehberger, 2025b) zatruł pamięć poprzez opóźnione wywołanie narzędzia (MITRE, b.d.). Wpisy faktograficzne płynnie przechodzą w instrukcje, a przyrostowe zapisy mogą omijać klasyfikację pojedynczych zapisów.

10. **Przypinaj, podpisuj i weryfikuj każdy serwer MCP i pakiet narzędzi stron trzecich, audytuj opisy narzędzi pod kątem ukrytych instrukcji i monitoruj kompozycję narzędzi.** Traktuj je jako powierzchnię łańcucha dostaw oprogramowania, omawianą w **LLM04:2026 Łańcuch dostaw** w odniesieniu do pakietów narzędzi stron trzecich oraz w ASI04 Agentic Supply Chain Vulnerabilities w odniesieniu do serwerów MCP i rejestrów narzędzi (zob. Scenariusz nr 9). Przypięcie wersji nie powstrzymuje ładunku dostarczonego w przypiętej wersji ani zatrucia opisu narzędzia, które nie zmienia wersji.

11. **Testuj pod kątem adaptacyjnych atakujących, którzy zapoznali się z wdrożonymi środkami obrony, i odrzucaj deklaracje skuteczności oparte wyłącznie na atakach statycznych.** Wyznacz poziom bazowy za pomocą AgentDojo (Debenedetti et al., 2024) i JailbreakBench (Chao et al., 2024), a następnie przeprowadź testy red team, udostępniając testerom pełną specyfikację obrony. Nasr et al. (2025) stwierdzili, że skuteczność ataków statycznych była bliska zeru, podczas gdy skuteczność ataków adaptacyjnych przekraczała 90% w przypadku większości z 12 niedawno opracowanych mechanizmów obrony (zob. także LLMail-Inject, Microsoft Security Response Center, 2025).

---

### Przykładowe scenariusze ataków

#### Scenariusz nr 1: Wstrzyknięcie bezpośrednie

Atakujący wydaje chatbotowi obsługi klienta polecenie zignorowania jego wytycznych, wykonania zapytań do prywatnych magazynów danych i wysłania wiadomości e-mail, co prowadzi do nieautoryzowanego dostępu i eskalacji uprawnień.
**Anatomia:** (a) bezpośrednie dane wejściowe użytkownika, (b) jednorazowe, (c) zwykły tekst

#### Scenariusz nr 2: Wstrzyknięcie pośrednie przez pobraną treść internetową

Użytkownik prosi asystenta o podsumowanie strony internetowej zawierającej ukryte instrukcje. Model wstawia obraz w formacie Markdown, którego adres URL eksfiltruje prywatną rozmowę do domeny kontrolowanej przez atakującego. Użytkownik widzi jedynie wyrenderowany obraz, a nigdy samą instrukcję.
**Anatomia:** (a) pobrana treść internetowa (pośrednie), (b) jednorazowe z eksfiltracją przez adres URL obrazu, (c) zwykły tekst ukryty w kodzie źródłowym strony

#### Scenariusz nr 3: Niezamierzone wstrzyknięcie

Plik PDF z opisem stanowiska zawiera osadzoną instrukcję wykrywania AI. Kandydat, nie wiedząc o tym, korzysta z LLM, aby zoptymalizować swoje CV pod kątem tego opisu; model ujawnia instrukcję, a system rekrutacyjny oznacza kandydata — jest to wstrzyknięcie polecenia, w którym żadna ze stron nie działa w złym zamiarze.
**Anatomia:** (a) pośrednie (dokument / PDF), (b) jednorazowe, (c) zwykły tekst

#### Scenariusz nr 4: Zatrucie repozytorium RAG

Atakujący dodaje zatrute dokumenty do korpusu, z którego aplikacja pobiera treści. Pasujące zapytanie zwraca zmodyfikowaną treść, a zawarte w niej instrukcje zmieniają wynik. Zaledwie pięć zatrutych dokumentów pozwoliło osiągnąć skuteczność ataku na poziomie około 90% wobec bazy wiedzy zawierającej miliony tekstów (W. Zou et al., 2025).
**Anatomia:** (a) pobrana treść (korpus RAG), (b) między sesjami / między użytkownikami, (c) zwykły tekst

#### Scenariusz nr 5: Dzielenie ładunku

Atakujący dzieli złośliwe instrukcje na kilka pól CV (nagłówek, treść, załącznik), tak aby żadne pojedyncze pole nie wyglądało na złośliwe dla klasyfikatora analizującego pola osobno. LLM łączy je z powrotem podczas oceny, a jego rekomendacja zostaje zmanipulowana.
**Anatomia:** (a) bezpośrednie dane wejściowe użytkownika podzielone na pola, (b) jednorazowe, połączone podczas oceny, (c) rozdrobniony zwykły tekst

#### Scenariusz nr 6: Multimodalne wstrzyknięcie steganograficzne

Atakujący osadza w obrazie instrukcję poniżej progu percepcji wzrokowej człowieka. Koder wizyjny modelu multimodalnego wyodrębnia ładunek, zachowanie modelu się zmienia i generuje on szkodliwy wynik lub nieautoryzowane wywołanie narzędzia. Zademonstrowano to wobec czterech czołowych modeli wizyjno-językowych w obrazowaniu onkologicznym (Clusmann et al., 2025) oraz wobec modeli ogólnego przeznaczenia poprzez połączenie zaburzeń wizualnych ze sterowaniem tekstowym (R. Chen et al., 2025).
**Anatomia:** (a) obraz wejściowy (pośrednie / multimodalne), (b) jednorazowe, (c) kodowanie steganograficzne / na poziomie pikseli

#### Scenariusz nr 7: Agentowa eksfiltracja typu zero-click za pośrednictwem dokumentu

Spreparowana wiadomość e-mail powoduje, że asystent produktywności oparty na LLM eksfiltruje dane organizacji bez żadnej interakcji użytkownika. Aim Security zademonstrowało to wobec Microsoft 365 Copilot (Reddy & Gujral, 2025), omijając zarówno wdrożony klasyfikator wstrzyknięć polecenia, jak i filtr usuwania linków.
**Anatomia:** (a) wiadomość e-mail / dokument (pośrednie), (b) jednorazowe z wywołaniem narzędzia, (c) zwykły tekst z kanałem eksfiltracji opartym na niewidocznych znakach Unicode

#### Scenariusz nr 8: Wykonanie destrukcyjnych poleceń przez agenta

Dwa zdarzenia z lipca 2025 roku pokazują tę samą klasę skutków osiągniętą różnymi wektorami. Atakujący umieścił (commit) destrukcyjny monit systemowy w repozytorium rozszerzenia Amazon Q dla VS Code, zanim AWS wycofał tę zmianę, choć umieszczony kod nie wykonał się z powodu błędu składni (Amazon Web Services, 2025a). Niezależnie od tego wstrzyknięcie w czasie działania spowodowało wykonanie przez Amazon Q dowolnego kodu (Amazon Web Services, 2025b). Agent z dostępem do powłoki, systemu plików lub chmurowego API wzmacnia wstrzyknięcie do rangi incydentu wpływającego na hosta.
**Anatomia:** (a) łańcuch dostaw / przejęty monit systemowy albo pośrednie wstrzyknięcie w czasie działania, (b) trwałe między sesjami albo jednorazowe z wykonaniem przez narzędzie powłoki, (c) zwykły tekst

#### Scenariusz nr 9: Pośrednie wstrzyknięcie do zaufanego backendu przez MCP

Atakujący umieszcza tekst w kanale o niskich uprawnieniach (publicznym zgłoszeniu na GitHubie, zgłoszeniu do działu wsparcia lub złośliwym pakiecie npm), a LLM programisty odczytuje go z podwyższonymi poświadczeniami. Invariant Labs (2025) wyeksfiltrowało prywatne repozytoria za pomocą zatrutego zgłoszenia na GitHubie, General Analysis (2025) zrzuciło produkcyjną bazę danych przez serwer MCP Supabase w Cursorze działający z rolą `service_role` i omijający zabezpieczenia na poziomie wierszy (row-level security), a złośliwy pakiet `postmark-mcp` (Koi Security, 2025; Toulas, 2025) wysyłał atakującemu ukryte kopie (BCC) wiadomości e-mail w szacunkowo 300 organizacjach.
**Anatomia:** (a) pośrednie przez zaufaną powierzchnię (kanał MCP: zgłoszenie, zgłoszenie do wsparcia, pakiet npm), (b) wieloetapowy łańcuch narzędzi, (c) zwykły tekst
