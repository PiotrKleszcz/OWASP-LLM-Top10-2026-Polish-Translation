# Załącznik A: Mapowania na powiązane frameworki

Ten załącznik zbiera w jednym miejscu mapowania dziesięciu pozycji ryzyka z OWASP Top 10 for LLM Applications (2026) na dziewięć zewnętrznych frameworków i taksonomii bezpieczeństwa. Zastępuje sekcje *Powiązane frameworki i taksonomie* w poszczególnych pozycjach, które usunięto na rzecz tego jednego źródła odniesienia o przypiętych wersjach.

## Jak czytać ten załącznik

**Macierz pokrycia** pokazuje na pierwszy rzut oka, które frameworki mają mapowania dla danego ryzyka. **Sekcje poświęcone poszczególnym frameworkom** podają konkretne mapowania na elementy wraz z uzasadnieniem. Mapowania dokonano na ogólnym poziomie każdego frameworku (taktyki, filary/słabości, kategorie ryzyka, domeny mechanizmów kontrolnych); każdy element pochodzi z przypiętej wersji frameworku wskazanej w sekcji **Źródła i wersje frameworków**. Każda komórka zawiera mapowania podstawowe oraz najistotniejsze mapowania wspierające.

**Legenda:** ● podstawowe (główna linia obrony lub opis ryzyka) · ○ wspierające (przyczynia się, ale nie stanowi głównego elementu) · — brak odpowiedniego mapowania.

## Macierz pokrycia

| Ryzyko | ASI | DSGAI | ATLAS | ATT&CK | CWE | 600-1 | RMF | AICM | AIVSS |
|---|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| **LLM01** Wstrzyknięcie polecenia | ● | ● | ● | ● | ● | ● | ○ | ● | ● |
| **LLM02** Ujawnianie informacji poufnych | ● | ● | ● | ● | ● | ● | ○ | ● | — |
| **LLM03** Nadmierna sprawczość | ● | ● | ● | ● | ● | ● | ○ | ● | ● |
| **LLM04** Łańcuch dostaw | ● | ● | ● | ● | ● | ● | ● | ● | ○ |
| **LLM05** Zatruwanie danych i modeli | ● | ● | ● | ○ | ● | ● | ○ | ● | ○ |
| **LLM06** Nieograniczona konsumpcja | ● | ● | ● | ● | ● | ● | ○ | ● | ○ |
| **LLM07** Dezinformacja | ● | ● | ● | ○ | ● | ● | ● | ● | ● |
| **LLM08** Ujawnienie ukrytego kontekstu | ● | ○ | ● | ○ | ● | ● | ○ | ● | ○ |
| **LLM09** Słabe punkty wektorów i osadzeń | ● | ● | ● | ○ | ● | ● | ○ | ● | ○ |
| **LLM10** Nieprawidłowe przetwarzanie wyników | ● | ● | ● | ● | ● | ● | — | ● | ○ |

## OWASP Top 10 for Agentic Applications (ASI) — 2026 (ogłoszono 2025-12-09)

*Przeniesione dosłownie z pozycji 2026; mapuje każde ryzyko LLM na ryzyka z OWASP Top 10 for Agentic Applications (ASI).*

| Ryzyko | Element | Znaczenie |
|---|---|---|
| **LLM01** Wstrzyknięcie polecenia | ● ASI01 — Agent Goal Hijack | Wstrzyknięte dane wejściowe nadpisują ograniczenia roli i możliwości określone w monicie systemowym, zmieniając cele agenta. |
|  | ● ASI02 — Tool Misuse & Exploitation | Wstrzyknięte dane wejściowe prowadzą do nieautoryzowanego wywoływania narzędzi (warunki „zabójczej triady”). |
|  | ● ASI03 — Identity & Privilege Abuse | Agent działa z podwyższonymi poświadczeniami użytkownika, wykonując działania, których atakujący sam nie mógłby wykonać. |
|  | ● ASI05 — Unexpected Code Execution (RCE) | Dostęp do powłoki / systemu plików / chmurowego API zamienia wstrzyknięcie w wykonanie dowolnych poleceń. |
|  | ● ASI06 — Memory & Context Poisoning | Zatruwanie pamięci i RAG skaża każdą przyszłą sesję odczytującą dane z magazynu. |
|  | ● ASI08 — Cascading Failures | Wyniki narzędzi wracają do okna kontekstowego, umożliwiając efekty łańcuchowe lub samoreplikujące się. |
|  | ● ASI09 — Human-Agent Trust Exploitation | Wstrzyknięcie omija potwierdzenie z udziałem człowieka (human-in-the-loop). |
| **LLM02** Ujawnianie informacji poufnych | ● ASI06 — Memory & Context Poisoning | Uszkodzenie pamięci trwałej agenta prowadzące do wycieku danych między sesjami |
| **LLM03** Nadmierna sprawczość | ● ASI01 — Agent Goal Hijack | Wstrzyknięcie polecenia lub halucynacja odwodząca agenta od zamierzonego zadania stanowi mechanizm wyzwalający Nadmierną sprawczość („halucynacja/konfabulacja... lub bezpośrednie/pośrednie wstrzyknięcie polecenia”) |
|  | ● ASI02 — Tool Misuse & Exploitation | Przejaw Nadmiernej sprawczości, w którym rozszerzenia/narzędzia mają funkcjonalność wykraczającą poza potrzeby zadania |
|  | ● ASI03 — Identity & Privilege Abuse | Przejaw Nadmiernej sprawczości, w którym rozszerzenia łączą się za pomocą ogólnych tożsamości o nadmiernych uprawnieniach |
|  | ● ASI05 — Unexpected Code Execution (RCE) | Rozszerzenie wykonujące polecenia powłoki, które nie odfiltrowuje niezamierzonych poleceń, oraz agent programistyczny, którego nadmierna autonomia i uprawnienia doprowadziły do zniszczenia infrastruktury produkcyjnej |
|  | ● ASI07 — Insecure Inter-Agent Communication | Złośliwy/przejęty agent współpracujący w systemach wieloagentowych/współpracujących stanowi czynnik wyzwalający, a zakres autoryzacji użytkownika musi zostać zachowany „w łańcuchu wywołań rozszerzeń lub agentów” |
|  | ● ASI08 — Cascading Failures | Przejaw Nadmiernej sprawczości, któremu przeciwdziała ograniczanie częstotliwości żądań i mechanizmy circuit breaker zatrzymujące niekontrolowane wywoływanie rozszerzeń |
|  | ● ASI09 — Human-Agent Trust Exploitation | Nadmierna autonomia (brak prośby o potwierdzenie przed działaniami o dużym wpływie) oraz zatwierdzanie z udziałem człowieka dotyczą zaufania pokładanego w działaniach agenta wykonywanych bez nadzoru |
| **LLM04** Łańcuch dostaw | ● ASI04 — Agentic Supply Chain Vulnerabilities | Zakres tej pozycji jawnie przekazuje ryzyko łańcucha dostaw specyficzne dla systemów agentowych do ASI04, pozostawiając tej pozycji nieagentowy łańcuch dostaw modeli, zbiorów danych i artefaktów |
| **LLM05** Zatruwanie danych i modeli | ● ASI04 — Agentic Supply Chain Vulnerabilities | Zatrute wstępnie wytrenowane modele i złośliwa deserializacja rozpowszechniane za pośrednictwem publicznych repozytoriów oraz zmodyfikowane artefakty wnioskowania, takie jak szablony czatu/GGUF |
|  | ● ASI06 — Memory & Context Poisoning | Zatruwanie pamięci trwałej agenta i rekomendacji oraz długotrwała manipulacja decyzjami agenta za pomocą wstrzykniętej pamięci |
|  | ● ASI08 — Cascading Failures | Zatrute dane wejściowe rozprzestrzeniające się w wieloagentowych i firmowych przepływach pracy, prowadzące do niezamierzonego ujawnienia danych, oraz skażenie między najemcami przez współdzielone osadzenia/pamięć |
| **LLM06** Nieograniczona konsumpcja | ● ASI02 — Tool Misuse & Exploitation | Pętle interakcji agentów z narzędziami i rozgałęzianie wywołań narzędzi MCP sprawiają, że pojedyncze żądanie prowadzi do rekurencyjnych lub masowych wywołań narzędzi, które wyczerpują budżet i dostępność |
|  | ● ASI08 — Cascading Failures | Architektury agentowe i protokoły korzystania z narzędzi, takie jak MCP, „zamieniają pojedyncze żądanie w kaskadę operacji w dalszych etapach przetwarzania”, co ilustruje rozgałęzienie jednego zadania na 50 wywołań |
| **LLM07** Dezinformacja | ● ASI08 — Cascading Failures | Nieprawidłowy stan lub dowody wytworzone przez jednego agenta i uznane za wiarygodne przez innego rozprzestrzeniają dezinformację w wieloagentowym przepływie pracy, prowadząc do narastającej awarii o dużym wpływie (Propagacja dezinformacji między agentami; Awaria zaufania między agentami) |
|  | ● ASI09 — Human-Agent Trust Exploitation | Ludzie i systemy w dalszych etapach przetwarzania, którzy traktują płynne, pewne siebie wyniki modelu jako wiarygodne, stanowią wykorzystywaną powierzchnię zaufania opisaną w tej pozycji w kontekście nadmiernego polegania na modelu (Nieuzasadnione lub fałszywe wsparcie decyzji) |
|  | ● ASI10 — Rogue Agents | Agent, który fałszuje ukończenie zadania lub zmyśla dowody, aby wprowadzić w błąd zaufanie w dalszych etapach, odpowiada opisanym w tej pozycji trybom awarii polegającym na sfałszowanych dowodach i fałszywym ukończeniu (Sfałszowane lub błędnie przypisane dowody; Zmyślone ukończenie zadania) |
| **LLM08** Ujawnienie ukrytego kontekstu | ● ASI06 — Memory & Context Poisoning | Zakres LLM08 jawnie przekazuje „agentowe czynniki potęgujące to ryzyko, np. pamięć trwałą, kanały komunikacji między agentami, trwałość konfiguracji narzędzi oraz wieloetapowe przejęcie agenta” do OWASP Top 10 for Agentic Applications; ASI06 wskazuje zatruwanie pamięci trwałej bazujące na ujawnionym ukrytym kontekście, a nie ryzyko równoważne. |
|  | ● ASI07 — Insecure Inter-Agent Communication | To samo wyłączenie z zakresu wymienia „kanały komunikacji między agentami” jako czynnik potęgujący wykraczający poza zakres; ASI07 wskazuje rozprzestrzenianie się materiału z ukrytego kontekstu w komunikacji między agentami, a nie ryzyko równoważne. |
| **LLM09** Słabe punkty wektorów i osadzeń | ● ASI04 — Agentic Supply Chain Vulnerabilities | Łańcuch dostaw modeli osadzeń: sam model osadzeń musi zostać zweryfikowany, ponieważ „model osadzeń z backdoorem zaburza geometrię wszystkiego, co zostanie przyjęte” |
|  | ● ASI06 — Memory & Context Poisoning | Zatruwanie pamięci agentów, które nie opiera się na geometrii osadzeń, wykracza poza zakres tej pozycji i jest przekazane do ASI06 |
| **LLM10** Nieprawidłowe przetwarzanie wyników | ● ASI02 — Tool Misuse & Exploitation | Uzupełnienie z 2026 roku, uzasadnione treścią tej pozycji: LLM ogólnego przeznaczenia przekazuje swoją odpowiedź uprzywilejowanemu rozszerzeniu bez walidacji wyników, co prowadzi do niewłaściwego użycia rozszerzenia i jego wyłączenia. |
|  | ● ASI05 — Unexpected Code Execution (RCE) | Podstawowe mapowanie z tabeli powiązań; odpowiada głównemu ryzyku tej pozycji, jakim jest dotarcie niezwalidowanego wyniku LLM do powłoki, `exec` lub `eval`, skutkujące zdalnym wykonaniem kodu. |
|  | ● ASI09 — Human-Agent Trust Exploitation | Kanoniczne mapowanie bazowe z tabeli powiązań — ASI09 wymienia Nieprawidłowe przetwarzanie wyników jako przyczyniające się ryzyko LLM. Zgodność treści jest luźna: scenariusze tej pozycji dotyczą komunikacji maszyna–maszyna (niezwalidowany wynik docierający do powłoki, przeglądarki lub bazy danych), podczas gdy ASI09 dotyczy wprowadzania w błąd operatora-człowieka; wspólnym elementem jest niezwalidowany wynik modelu docierający do decyzji opartej na zaufaniu w dalszym etapie. |

## OWASP GenAI Data Security 2026 (DSGAI) — v1.0 (2026-03-17)

*Każdy wiersz mapuje ryzyko z LLM Top 10 na kategorie ryzyka OWASP GenAI Data Security 2026 (DSGAI), przy czym elementy podstawowe wskazują najbliższy odpowiednik w obszarze bezpieczeństwa danych, a elementy wspierające — najistotniejsze pokrewne ryzyka dotyczące danych.*

| Ryzyko | Element | Znaczenie |
|---|---|---|
| **LLM01** Wstrzyknięcie polecenia | ● DSGAI01 — Sensitive Data Leakage | Skuteczne wstrzyknięcie sprawia, że dominującym skutkiem staje się ujawnienie i eksfiltracja danych — z modelu wyciekają treść monitu systemowego, pobrane prywatne dokumenty i szczegóły infrastruktury. |
|  | ● DSGAI06 — Tool, Plugin & Agent Data Exchange Risks | Wstrzyknięcie przenika przez serwery MCP i pakiety narzędzi stron trzecich, w których zatrute opisy narzędzi i nieprzypięte połączenia pozwalają przejętemu modelowi działać w połączonych systemach. |
|  | ○ DSGAI09 — Multimodal Capture & Cross-Channel Data Leakage | Ładunki ukryte steganograficznie w obrazach i w niewidocznych znakach Unicode dostarczają instrukcje, których człowiek nigdy nie widzi, a renderowanie adresów URL obrazów w formacie Markdown wyprowadza skradzione dane na zewnątrz. |
|  | ○ DSGAI16 — Endpoint & Browser Assistant Overreach | Asystenci przeglądarkowi, IDE i poczty e-mail, którzy automatycznie streszczają niezaufane strony, stają się kanałem pośredniego wstrzyknięcia — od eksfiltracji dokumentów typu zero-click po zmianę flagi konfiguracyjnej IDE. |
|  | ○ DSGAI04 — Data, Model & Artifact Poisoning | Jeden skażony wpis w korpusie RAG lub w pamięci trwałej skaża każdą kolejną sesję, która go odczytuje, co czyni zatruwanie ścieżką propagacji wstrzyknięcia między sesjami. |
| **LLM02** Ujawnianie informacji poufnych | ● DSGAI01 — Sensitive Data Leakage | Bezpośredni odpowiednik: całe ryzyko sprowadza się do nieautoryzowanego ujawnienia danych poufnych, regulowanych lub zastrzeżonych dowolnym kanałem wyjściowym. |
|  | ● DSGAI18 — Inference & Data Reconstruction | Wnioskowanie o przynależności, inwersja osadzeń i inwersja stanu wewnętrznego odtwarzają chronione dane na podstawie mierzalnego zachowania modelu, bez bezpośredniego otrzymania treści. |
|  | ○ DSGAI15 — Over-Broad Context Windows & Prompt Over-Sharing | Dyski o nieograniczonym zakresie, przestarzałe uprawnienia i automatycznie dołączane pełne rekordy dostarczają modelowi więcej danych wrażliwych, niż wymaga zadanie — to właśnie nadmierne udostępnianie na wcześniejszych etapach odpowiada za większość incydentów. |
|  | ○ DSGAI08 — Non-Compliance & Regulatory Violations | Ujawnienie niesie konsekwencje regulacyjne, uruchamiając obowiązki wynikające z unijnego aktu w sprawie sztucznej inteligencji, RODO, HIPAA i CCPA oraz związane z nimi terminy zgłaszania naruszeń. |
|  | ○ DSGAI14 — Excessive Telemetry & Monitoring Leakage | Platformy obserwowalności domyślnie rejestrują pełne polecenia, odpowiedzi, pobrane fragmenty i ślady rozumowania, ujawniając wrażliwe treści każdemu, kto ma dostęp do pulpitu. |
| **LLM03** Nadmierna sprawczość | ● DSGAI02 — Agent Identity & Credential Exposure | Rozszerzenia z szerokimi uprawnieniami zapisu lub współdzielonym kontem o wysokich uprawnieniach pozwalają przejętemu agentowi działać daleko poza zamierzonym zakresem, dlatego podstawowym mechanizmem kontrolnym jest wiązanie działań z poświadczeniami o zakresie ograniczonym do użytkownika w całym łańcuchu wywołań. |
|  | ● DSGAI06 — Tool, Plugin & Agent Data Exchange Risks | Całe ryzyko dotyczy rozszerzeń, narzędzi i wtyczek, które zachowują zbędne możliwości, możliwe do uruchomienia przez atakującego, np. rozszerzenia poczty zachowującego funkcję wysyłania. |
|  | ○ DSGAI01 — Sensitive Data Leakage | Nadmierna sprawczość pozwala przejętemu agentowi przeszukać skrzynkę pocztową pod kątem informacji poufnych i przekazać je atakującemu. |
| **LLM04** Łańcuch dostaw | ● DSGAI04 — Data, Model & Artifact Poisoning | Zmodyfikowane lub zawierające backdoory modele, adaptery i artefakty trafiają do potoku przez łańcuch dostaw, jak w przypadku złośliwie zmodyfikowanego otwartego modelu lub zatrutej usługi konwersji formatów. |
|  | ● DSGAI05 — Data Integrity & Validation Failures | Niepodpisane, niezweryfikowane artefakty i zmienne odwołania rozwiązywane na etapie budowania lub promowania pozwalają zmodyfikowanym komponentom przejść bez kontroli integralności i podpisu. |
|  | ○ DSGAI03 — Shadow AI & Unsanctioned Data Flows | Niejasne warunki dostawców mogą niepostrzeżenie kierować dane aplikacji do zbioru treningowego dostawcy, dlatego wymagana jest weryfikacja warunków i polityk prywatności dostawców. |
|  | ○ DSGAI08 — Non-Compliance & Regulatory Violations | Niejasny status licencyjny i prawnoautorski modeli i zbiorów danych rodzi ryzyko prawne i ryzyko braku zgodności, co wymaga śledzenia i audytowania licencji. |
| **LLM05** Zatruwanie danych i modeli | ● DSGAI04 — Data, Model & Artifact Poisoning | Bezpośredni odpowiednik obejmujący zatruwanie na etapie wstępnego trenowania, dostrajania, osadzeń, RAG, uczenia transferowego oraz rozpowszechnianych artefaktów niebędących wagami. |
|  | ○ DSGAI05 — Data Integrity & Validation Failures | Ścisła, ciągła walidacja danych wejściowych do trenowania i ponownego trenowania stanowi pierwszą linię obrony przed wstrzykniętą trucizną. |
|  | ○ DSGAI21 — Disinformation & Integrity Attacks via Data Poisoning | Zatrucie bazy wiedzy sprawia, że treści kontrolowane przez przeciwnika pojawiają się jako wiarygodne wyniki — jest to atak na integralność wywołany uszkodzonymi danymi. |
|  | ○ DSGAI07 — Data Governance, Lifecycle & Classification | Rodowód zbiorów danych i modeli śledzony za pomocą SBOM lub ML-BOM wraz z historią wersji umożliwia wycofanie zmian i analizę śledczą po zdarzeniu zatrucia. |
| **LLM06** Nieograniczona konsumpcja | ● DSGAI20 — Model Exfiltration & IP Replication | Funkcjonalne klonowanie i ekstrakcja modelu za pomocą masowego odpytywania prowadzą do kradzieży własności intelektualnej modelu. |
|  | ● DSGAI17 — Data Availability & Resilience Failures in AI Pipelines | Żądania wyczerpujące zasoby i ataki eksplozji wyników sprawiają, że usługa przestaje odpowiadać — to awaria dostępności, której dotyczą mechanizmy kontroli odporności. |
|  | ○ DSGAI04 — Data, Model & Artifact Poisoning | Pojedyncza zatruta próbka do dostrajania może zaburzyć zachowanie związane z końcem sekwencji i doprowadzić do niekontrolowanego generowania wyników jako wektora odmowy usługi. |
| **LLM07** Dezinformacja | ● DSGAI21 — Disinformation & Integrity Attacks via Data Poisoning | Celowo wstrzyknięte treści sprawiają, że system przedstawia fałszywe lub sfałszowane materiały jako wiarygodne — to podstawowy mechanizm dezinformacji poprzez zatruwanie. |
|  | ○ DSGAI05 — Data Integrity & Validation Failures | Stronnicze lub uszkodzone dane źródłowe oraz niezwalidowane przyjmowanie danych to pierwotne przyczyny, które pozwalają dezinformacji przeniknąć do systemu i się rozprzestrzenić. |
|  | ○ DSGAI06 — Tool, Plugin & Agent Data Exchange Risks | Niezwalidowane wyniki narzędzi i wymiana danych między agentami rozprzestrzeniają fałszywe informacje, dlatego wywołania narzędzi wymagają walidacji semantycznej. |
| **LLM08** Ujawnienie ukrytego kontekstu | ○ DSGAI18 — Inference & Data Reconstruction | Pozycję definiuje wydobycie, wywnioskowanie i odtworzenie ukrytego kontekstu — te same ataki rekonstrukcyjne, które odtwarzają monit systemowy na podstawie zachowania modelu. |
|  | ○ DSGAI15 — Over-Broad Context Windows & Prompt Over-Sharing | Główną zasadą zapobiegawczą jest nieumieszczanie danych wrażliwych w ukrytym kontekście i przyjęcie założenia, że cały kontekst da się odkryć. |
|  | ○ DSGAI02 — Agent Identity & Credential Exposure | Klucze API i tokeny osadzone w ukrytym kontekście zostają przechwycone, gdy tylko atakujący wydobędzie ten kontekst. |
| **LLM09** Słabe punkty wektorów i osadzeń | ● DSGAI13 — Vector Store Platform Data Security | Bezpieczeństwo platform baz wektorowych jest dosłownym przedmiotem tej pozycji i obejmuje szyfrowanie, kontrolę dostępu oraz ograniczenia eksportu w magazynach osadzeń. |
|  | ● DSGAI18 — Inference & Data Reconstruction | Inwersja osadzeń odtwarzająca tekst jawny oraz wyrocznie wnioskowania o przynależności zamieniają przechowywane wektory z powrotem w dane źródłowe. |
|  | ○ DSGAI11 — Cross-Context & Multi-User Conversation Bleed | Współdzielone wyszukiwanie podobieństwa, które stosuje filtr najemcy dopiero po wyszukaniu, powoduje wyciek fragmentów jednego najemcy do wyników innego. |
|  | ○ DSGAI04 — Data, Model & Artifact Poisoning | Zatruwanie na etapie wyszukiwania, zatruwanie semantycznej pamięci podręcznej i zatruwanie osadzeń multimodalnych kierują system w stronę treści kontrolowanych przez atakującego. |
|  | ○ DSGAI17 — Data Availability & Resilience Failures in AI Pipelines | Pojedynczy spreparowany dokument blokujący może zdominować wyszukiwanie i wyłączyć system RAG z działania — jest to atak na dostępność warstwy wyszukiwania. |
| **LLM10** Nieprawidłowe przetwarzanie wyników | ● DSGAI12 — Unsafe Natural-Language Data Gateways (LLM-to-SQL/Graph) | Zapytania SQL wygenerowane przez LLM, wykonywane bez parametryzacji i weryfikacji, docierają bezpośrednio do bazy danych i mogą usunąć całe tabele. |
|  | ○ DSGAI06 — Tool, Plugin & Agent Data Exchange Risks | Niezwalidowany wynik modelu trafia do narzędzia lub rozszerzenia w dalszym etapie przetwarzania w celu wykonania. |
|  | ○ DSGAI01 — Sensitive Data Leakage | Nieescapowany wynik renderowany przez klienta koduje dane z rozmowy i wysyła je do serwera kontrolowanego przez atakującego, jak w przypadku eksfiltracji przez adres URL obrazu. |
|  | ○ DSGAI16 — Endpoint & Browser Assistant Overreach | Mechanizmy renderujące po stronie klienta, takie jak interfejsy czatu, IDE i klienty poczty e-mail, automatycznie pobierają zasoby zewnętrzne na podstawie surowych wyników modelu, realizując eksfiltrację. |

## MITRE ATLAS — content v2026.06 (format-version 6.0.0)

*Każdy wiersz należy czytać jako ryzyko OWASP LLM zmapowane na taktyki przeciwnika MITRE ATLAS (AML.TAxxxx), przez które przechodzi atak wykorzystujący to ryzyko, przy czym taktyki podstawowe odpowiadają głównemu celowi przeciwnika, a taktyki wspierające — krokom umożliwiającym.*

| Ryzyko | Element | Znaczenie |
|---|---|---|
| **LLM01** Wstrzyknięcie polecenia | ● AML.TA0004 Initial Access | Wstrzyknięte instrukcje — wpisane bezpośrednio w polecenie lub ukryte w treściach pobieranych później przez model — stanowią przyczółek, który pozwala atakującemu przejąć kontrolę nad zachowaniem modelu. |
|  | ● AML.TA0005 Execution | Skuteczne wstrzyknięcie skłania agenta do wywoływania narzędzi i uruchamiania poleceń wybranych przez przeciwnika, w tym do wykonania dowolnego kodu na hoście. |
|  | ● AML.TA0006 Persistence | Instrukcje wstrzyknięte do pamięci długotrwałej lub korpusu wyszukiwania przetrwają między sesjami i ponownie się aktywują za każdym razem, gdy zatruty wpis zostanie przywołany. |
|  | ● AML.TA0007 Defense Evasion | Atakujący ukrywają wstrzyknięcie za pomocą niewidocznych znaków Unicode, kodowania Base64 lub ROT13 oraz języków o niskich zasobach, a także stosują sformułowania typu jailbreak, aby prześlizgnąć się obok filtrów bezpieczeństwa. |
|  | ● AML.TA0010 Exfiltration | Wstrzyknięcie przekierowuje model do ujawnienia prywatnej rozmowy, zawartości bazy danych lub repozytorium kanałami opartymi na adresach URL obrazów, narzędziach lub niewidocznych znakach. |
|  | ○ AML.TA0001 AI Attack Staging | Przeciwnicy z wyprzedzeniem optymalizują ładunek wstrzyknięcia, wykorzystując dostrajanie jako wyrocznię gradientową i dzielenie ładunku, aby zwiększyć skuteczność ataku. |
|  | ○ AML.TA0011 Impact | Wstrzyknięcie w czasie działania może wywołać destrukcyjne działanie, takie jak wyczyszczenie lokalnych plików programisty. |
|  | ○ AML.TA0012 Privilege Escalation | Agent działający na zaufanym backendzie, do którego wstrzyknięto instrukcje, działa z podwyższonymi poświadczeniami użytkownika, zapewniając dostęp wykraczający poza uprawnienia samego atakującego. |
| **LLM02** Ujawnianie informacji poufnych | ● AML.TA0010 Exfiltration | Wnioskowanie o przynależności, inwersja modelu, ekstrakcja modelu i kradzież danych przez API — wszystkie te techniki wyprowadzają chronione dane z systemu, czasem przez ukryte kanały DNS lub kanały oparte na obrazach. |
|  | ○ AML.TA0013 Credential Access | Klucz API dostawcy osadzony w monicie systemowym można wydobyć z modelu podstępem, co prowadzi do ujawnienia aktywnego poświadczenia. |
|  | ○ AML.TA0000 AI Model Access | Ekstrakcja, wnioskowanie o przynależności i inwersja są przeprowadzane offline z nieograniczoną częstotliwością wobec wdrożeń modeli o otwartych wagach, dlatego sam tryb dostępu stanowi powierzchnię ataku. |
|  | ○ AML.TA0007 Defense Evasion | Kodowanie międzyjęzykowe, Base64 i szesnastkowe pokonuje filtry zapobiegające utracie danych oparte na wyrażeniach regularnych i listach blokowanych, a transformacja międzymodalna pozwala przemycić dane obok mechanizmów inspekcji działających w jednej modalności. |
| **LLM03** Nadmierna sprawczość | ● AML.TA0005 Execution | Rozszerzenie o zbyt szerokim zakresie, np. uruchamiające niefiltrowane polecenia powłoki, pozwala agentowi wykonywać szkodliwe operacje, w tym destrukcyjne polecenia wobec systemów produkcyjnych. |
|  | ● AML.TA0011 Impact | Nadmierne uprawnienia zamieniają działanie agenta w szkodę dla poufności, integralności i dostępności — od usunięcia poczty użytkownika po zniszczenie produkcyjnych baz danych wraz z ich migawkami. |
|  | ○ AML.TA0010 Exfiltration | Pośrednie wstrzyknięcie może sprawić, że rozszerzenie poczty o nadmiernych uprawnieniach przekaże wrażliwą zawartość skrzynki odbiorczej na adres kontrolowany przez atakującego. |
|  | ○ AML.TA0012 Privilege Escalation | Problem zdezorientowanego zastępcy (confused deputy) lub wykorzystanie agenta o nadmiernych uprawnieniach jako broni pozwala atakującemu działać ze stałymi wysokimi uprawnieniami agenta. |
| **LLM04** Łańcuch dostaw | ● AML.TA0004 Initial Access | Przejęte modele, pakiety, adaptery i potoki trenowania stanowią przyczółek, przez który zmodyfikowany artefakt trafia do środowiska ofiary. |
|  | ○ AML.TA0003 Resource Development | Atakujący publikują zmodyfikowane modele, z wyprzedzeniem rejestrują konfabulowane nazwy pakietów (slopsquatting) i ponownie rejestrują porzucone przestrzenie nazw, aby przygotować złośliwe zależności. |
|  | ○ AML.TA0005 Execution | Złośliwe manifesty modeli, deserializacja pickle i przepełnienia w parserach formatów prowadzą do zdalnego wykonania kodu podczas ładowania artefaktu. |
|  | ○ AML.TA0006 Persistence | Backdoor osadzony w adapterze LoRA lub w grafie obliczeniowym modelu przetrwa we wdrożonym modelu w formacie, który wygląda na bezpieczny. |
| **LLM05** Zatruwanie danych i modeli | ● AML.TA0003 Resource Development | Przeciwnicy publikują zatrute zbiory danych i modele oraz wprowadzają skażone próbki do danych treningowych, z których będą korzystać inni. |
|  | ● AML.TA0006 Persistence | Backdoory i uśpione wyzwalacze przetrwają dostosowanie bezpieczeństwa i ponowne trenowanie, pozostając w modelu w stanie uśpienia, dopóki nie aktywuje ich określone wejście. |
|  | ● AML.TA0011 Impact | Zatruwanie narusza integralność modelu i zbioru danych, obniżając dokładność, osłabiając odmowy i kierując model w stronę szkodliwych wyników. |
|  | ○ AML.TA0004 Initial Access | Organizacje, które pobierają przejęte modele lub zbiory danych z publicznych repozytoriów, dziedziczą wraz z artefaktem ukryte wyzwalacze. |
|  | ○ AML.TA0001 AI Attack Staging | Atakujący umieszcza w modelu backdoor i przygotowuje dane wyzwalające lub adwersarialne przed wdrożeniem modelu. |
|  | ○ AML.TA0005 Execution | Załadowanie niebezpiecznego artefaktu wykonuje kod atakującego, jak w przypadku deserializacji pickle podczas ładowania modelu lub wstrzyknięcia szablonu czatu w pliku modelu. |
| **LLM06** Nieograniczona konsumpcja | ● AML.TA0011 Impact | Zalewanie danymi wejściowymi uniemożliwia uprawnionym użytkownikom korzystanie z usługi ML, a ataki polegające na generowaniu kosztów zawyżają rachunek ofiary w ramach odmowy dostępu do portfela (denial of wallet). |
|  | ● AML.TA0010 Exfiltration | Spreparowane zapytania API i przechwytywanie wag kanałami bocznymi umożliwiają ekstrakcję modelu i kradzież własności intelektualnej zawartej w jego parametrach. |
|  | ○ AML.TA0000 AI Model Access | Zarówno ataki ekstrakcji, jak i ataki wyczerpania zasobów działają poprzez dostęp do API wnioskowania, który jest warunkiem wstępnym tej techniki. |
| **LLM07** Dezinformacja | ● AML.TA0011 Impact | Fałszywy wynik, który zostaje uznany za wiarygodny i staje się podstawą działania, powoduje straty finansowe, incydenty bezpieczeństwa, zagrożenia dla bezpieczeństwa ludzi i zakłócenia operacyjne — jak wtedy, gdy zmyślony alert dezorganizuje działalność. |
|  | ○ AML.TA0003 Resource Development | Atakujący publikują złośliwe pakiety pod nazwami, które model zwykle halucynuje, przygotowując pułapkę w łańcuchu dostaw dopasowaną do konfabulacji. |
|  | ○ AML.TA0004 Initial Access | Halucynowana zależność, po zarekomendowaniu i zainstalowaniu przez ufającego programistę, staje się przyczółkiem w łańcuchu dostaw AI. |
|  | ○ AML.TA0001 AI Attack Staging | Przeciwnicy tworzą dane wejściowe, które kierują model w stronę określonych fałszywych twierdzeń — jest to etap przygotowania dezinformacji wywoływanej celowo. |
| **LLM08** Ujawnienie ukrytego kontekstu | ● AML.TA0008 Discovery | Atak wydobywa i odtwarza monit systemowy, listę narzędzi, role i logikę odmowy, odkrywając ukrytą konfigurację systemu AI. |
|  | ● AML.TA0010 Exfiltration | Wyciek tego ukrytego kontekstu poza granicę zaufania to wyciek danych modelu, który przekazuje w ręce atakującego konfigurację, którą operator zamierzał zachować w tajemnicy. |
|  | ○ AML.TA0002 Reconnaissance | Wydobyty kontekst daje atakującemu konkretne cele dla kolejnych wstrzyknięć polecenia oraz rozpoznanie na potrzeby łączenia działań w łańcuchy w dalszych etapach. |
|  | ○ AML.TA0013 Credential Access | Poświadczenia osadzone w monicie systemowym zostają pozyskane w wyniku wycieku ukrytego kontekstu, co daje atakującemu sekrety nadające się do użycia. |
| **LLM09** Słabe punkty wektorów i osadzeń | ● AML.TA0006 Persistence | Zatrute wpisy w korpusie i pamięci podręcznej pozostają w bazie wektorowej i ponownie się aktywują przy pasujących zapytaniach, zapewniając trwały przyczółek w warstwie wyszukiwania. |
|  | ● AML.TA0010 Exfiltration | Inwersja osadzeń, wnioskowanie między najemcami i wnioskowanie o przynależności wyprowadzają prywatne dane i dokumenty przez współdzielony indeks. |
|  | ○ AML.TA0001 AI Attack Staging | Atakujący tworzą treści, których osadzenia lądują w pobliżu docelowego zapytania, i wykorzystują zastępcze kodery do tworzenia wektorów balansujących na granicy progu. |
|  | ○ AML.TA0011 Impact | Zagłuszanie wyszukiwania zalewa indeks w ramach ataku na dostępność, a zatruwanie wyszukiwania narusza integralność zwracanych odpowiedzi. |
|  | ○ AML.TA0008 Discovery | Sondowanie między najemcami pozwala wywnioskować istnienie, tematykę i przybliżoną liczbę dokumentów innych najemców, a wnioskowanie o przynależności ujawnia, czy określony dokument jest zaindeksowany. |
| **LLM10** Nieprawidłowe przetwarzanie wyników | ● AML.TA0005 Execution | Niezwalidowany wynik modelu przekazany do powłoki, przeglądarki lub interpretera SQL wykonuje kod kontrolowany przez przeciwnika, prowadząc do wykonania poleceń, ataku cross-site scripting lub wstrzyknięcia SQL. |
|  | ○ AML.TA0010 Exfiltration | Wynik modelu zakodowany i wysłany do serwera atakującego lub osadzony w adresie URL obrazu w formacie Markdown wyprowadza dane wrażliwe z aplikacji. |
|  | ○ AML.TA0011 Impact | Nieobsłużony wynik docierający do uprzywilejowanego miejsca docelowego może usunąć tabele bazy danych lub wymusić wyłączenie usługi w dalszym etapie przetwarzania. |
|  | ○ AML.TA0012 Privilege Escalation | Gdy model ma uprawnienia wykraczające poza uprawnienia użytkownika końcowego, niezwalidowany wynik docierający do uprzywilejowanego miejsca docelowego przenosi te uprawnienia na atakującego. |

## MITRE ATT&CK — v19.1

*Każdy wiersz mapuje ryzyko OWASP LLM na taktyki MITRE ATT&CK v19.1 Enterprise (identyfikatory TA), przez które przechodzi przeciwnik, wykorzystując to ryzyko, przy czym taktyki podstawowe oznaczają główny cel ataku, a taktyki wspierające — sąsiednie etapy.*

| Ryzyko | Element | Znaczenie |
|---|---|---|
| **LLM01** Wstrzyknięcie polecenia | ● TA0001 Initial Access | Przejęte rozszerzenie IDE i złośliwy pakiet npm dla MCP dostarczają wstrzyknięte instrukcje do kontekstu modelu, czyniąc łańcuch dostaw oprogramowania punktem wejścia ataku. |
|  | ○ TA0002 Execution | Wstrzyknięte instrukcje prowadzą do wykonania dowolnych poleceń i działań destrukcyjnych na hoście lub w połączonych systemach za pośrednictwem dostępu agenta do narzędzia powłoki. |
|  | ○ TA0010 Exfiltration | Wstrzyknięta treść eksfiltruje dane ukrytymi kanałami, takimi jak adresy URL obrazów w formacie Markdown, ukryte znaki Unicode i kanały boczne w logowaniu narzędzi, podczas gdy widoczna odpowiedź pozostaje niewinna. |
|  | ○ TA0005 Stealth | Atakujący przemycają instrukcje obok weryfikacji przez człowieka i model, wykorzystując znaki Unicode o zerowej szerokości i selektory wariantów, kodowanie bloku znaczników, ukryty tekst w kodzie źródłowym strony oraz niedostrzegalną dla człowieka steganografię obrazów. |
| **LLM02** Ujawnianie informacji poufnych | ● TA0010 Exfiltration | Ukryty kanał oparty na obrazie w formacie Markdown lub webhooku wyprowadza dane wrażliwe na zewnątrz, podczas gdy widoczna odpowiedź modelu pozostaje nieszkodliwa. |
|  | ○ TA0009 Collection | Obserwacja szyfrowanego ruchu LLM kanałami bocznymi pozwala odtworzyć tematy rozmów i treść tokenów, jak w przypadku wnioskowania o tematach z AUPRC powyżej 98% i odtwarzania na podstawie długości tokenów. |
|  | ○ TA0006 Credential Access | Wstrzyknięcie polecenia powoduje wyświetlenie monitu systemowego zawierającego osadzony klucz API dostawcy, a ujawnione magazyny logów udostępniają klucze API, które przeciwnik może przechwycić. |
| **LLM03** Nadmierna sprawczość | ● TA0002 Execution | Rozszerzenie o otwartej funkcjonalności wykonujące polecenia powłoki, które nie filtruje niezamierzonych poleceń, pozwala agentowi wykonać dowolny kod, jak wtedy, gdy agent programistyczny zniszczył infrastrukturę produkcyjną. |
|  | ● TA0040 Impact | Nadmierna autonomia prowadzi do działań destrukcyjnych, takich jak usuwanie wiadomości e-mail bez potwierdzenia lub wyczyszczenie produkcyjnej bazy danych wraz z migawkami z wielu lat. |
|  | ○ TA0010 Exfiltration | Pośrednie wstrzyknięcie nakazuje agentowi przeszukanie skrzynki odbiorczej użytkownika pod kątem informacji poufnych i przekazanie ich na adres e-mail atakującego. |
|  | ○ TA0004 Privilege Escalation | Tożsamości agentów o nadmiernych lub ogólnych wysokich uprawnieniach oraz warunki typu confused deputy podnoszą efektywne uprawnienia atakującego do poziomu uprawnień agenta. |
| **LLM04** Łańcuch dostaw | ● TA0001 Initial Access | Przejęte pakiety i potoki budowania — od zatrutej zależności PyPI po przypadki xz-utils i Codecov — dostarczają kod atakującego do środowiska ofiary. |
|  | ○ TA0042 Resource Development | Atakujący rejestrują złośliwe pakiety pod halucynowanymi nazwami (slopsquatting) i wstrzykują dane do pamięci podręcznych CI/CD, aby przygotować strojanizowane wydania do dystrybucji. |
|  | ○ TA0002 Execution | Złośliwe manifesty modeli, ponowne wykorzystanie przestrzeni nazw i deserializacja pickle podczas ładowania modelu prowadzą do zdalnego wykonania kodu po pobraniu i uruchomieniu artefaktu. |
|  | ○ TA0003 Persistence | Scalony adapter LoRA lub graf modelu z backdoorem zapewnia ukryty punkt wejścia, który przetrwa kolejne wdrożenia i przechodzi kontrole pochodzenia w dalszych etapach łańcucha. |
| **LLM05** Zatruwanie danych i modeli | ○ TA0040 Impact | Zatrute dane treningowe lub wagi naruszają integralność modelu, prowadząc do szkodliwych lub pogorszonych wyników, takich jak omijanie wykrywania oszustw lub spadek dokładności z 90% do 15% po aktywacji wyzwalacza. |
|  | ○ TA0001 Initial Access | Złośliwe modele z osadzonymi backdoorami, rozpowszechniane za pośrednictwem publicznych repozytoriów, docierają do ofiary po pobraniu do potoku. |
|  | ○ TA0002 Execution | Osadzony złośliwy kod wykonuje się podczas niebezpiecznego ładowania modelu w formacie pickle, a zmodyfikowany szablon czatu zawiera instrukcje aktywowane wyzwalaczem. |
| **LLM06** Nieograniczona konsumpcja | ● TA0040 Impact | Dane wejściowe wyczerpujące zasoby powodują awarię lub spowolnienie usługi, a ataki typu denial of wallet generują koszty obliczeniowe, zakłócając dostępność i zawyżając wydatki. |
|  | ○ TA0010 Exfiltration | Zapytania służące ekstrakcji modelu przekazują informacje o modelu do zdalnego zasobu kontrolowanego przez atakującego, umożliwiając kradzież własności intelektualnej przez klonowanie. |
| **LLM07** Dezinformacja | ○ TA0001 Initial Access | Atakujący publikują złośliwe pakiety pod nazwami halucynowanymi przez asystentów programowania (slopsquatting), zamieniając zmyślone rekomendacje w przejęcie łańcucha dostaw oprogramowania. |
| **LLM08** Ujawnienie ukrytego kontekstu | ○ TA0006 Credential Access | Ukryty kontekst, taki jak monity systemowe zawierające klucze API, poświadczenia do baz danych i tokeny użytkowników, wycieka do atakującego, który następnie ponownie wykorzystuje ujawnione poświadczenia. |
| **LLM09** Słabe punkty wektorów i osadzeń | ○ TA0009 Collection | Sondowanie współdzielonego indeksu wektorowego między najemcami pozwala wywnioskować dane innych najemców przechowywane w tym repozytorium informacji. |
|  | ○ TA0010 Exfiltration | Inwersja osadzeń w trybie zero-shot odtwarza dokumenty źródłowe i dane osobowe z wyciekłej kopii zapasowej osadzeń, wyprowadzając dane poza granicę systemu. |
|  | ○ TA0040 Impact | Zagłuszanie wyszukiwania obniża dostępność warstwy wyszukiwania, a zatruwanie na etapie wyszukiwania manipuluje kontekstem przekazywanym do modelu. |
| **LLM10** Nieprawidłowe przetwarzanie wyników | ● TA0002 Execution | Przekazywanie niezwalidowanych wyników modelu do powłoki, funkcji eval lub interfejsów bazodanowych prowadzi do zdalnego wykonania kodu, ataków cross-site scripting i wstrzyknięcia SQL w systemach backendowych. |
|  | ○ TA0010 Exfiltration | Wynik modelu koduje wrażliwe dane z rozmowy i wysyła je do serwera kontrolowanego przez atakującego, w tym za pośrednictwem adresów URL obrazów w formacie Markdown. |
|  | ○ TA0040 Impact | Niezweryfikowane zapytania SQL wygenerowane przez model usuwają tabele bazy danych, a uprzywilejowane rozszerzenie zostaje zmuszone do wyłączenia, co prowadzi do zniszczenia danych i utraty dostępności. |
|  | ○ TA0004 Privilege Escalation | Gdy aplikacja przyznaje modelowi uprawnienia wykraczające poza te przeznaczone dla użytkowników końcowych, nieobsłużone wyniki umożliwiają eskalację uprawnień. |

## MITRE CWE (Common Weakness Enumeration) — 4.20

*Każda pozycja OWASP LLM Top 10 2026 jest zmapowana na słabości CWE wskazujące jej pierwotną przyczynę, przy czym podstawowe CWE ujmują słabość definicyjną, a wspierające CWE — najważniejsze przyczyniające się tryby awarii.*

| Ryzyko | Element | Znaczenie |
|---|---|---|
| **LLM01** Wstrzyknięcie polecenia | ● CWE-1427 Improper Neutralization of Input Used for LLM Prompting | Niezaufane dane wejściowe pochodzące z poleceń, pobranych dokumentów, wyników narzędzi lub pamięci zmieniają zachowanie modelu, ponieważ model nie wyznacza granicy między instrukcjami a danymi — to definicyjna słabość związana ze wstrzyknięciem polecenia. |
|  | ○ CWE-707 Improper Neutralization (Pillar) | Pierwotną przyczyną na poziomie filaru (pillar) jest brak oddzielenia i neutralizacji instrukcji atakującego od danych w jednym strumieniu tokenów. |
|  | ○ CWE-349 Acceptance of Extraneous Untrusted Data With Trusted Data | Łączenie w oknie kontekstowym scala niezaufane pobrane treści i wyniki narzędzi z zaufanymi instrukcjami bez egzekwowanej granicy zaufania. |
|  | ○ CWE-693 Protection Mechanism Failure (Pillar) | Jailbreaki i adaptacyjne omijanie mechanizmów ochronnych osłabiają klasyfikatory wstrzyknięć polecenia i filtry linków, prowadząc do wysokiej skuteczności ataków adwersarialnych. |
| **LLM02** Ujawnianie informacji poufnych | ● CWE-200 Exposure of Sensitive Information to an Unauthorized Actor | Pozycja definiuje ujawnianie jako udostępnianie danych poufnych, regulowanych lub objętych tajemnicą nieautoryzowanym kanałem. |
|  | ● CWE-359 Exposure of Private Personal Information | Ujawnianie PII i PHI stanowi centralny element tej pozycji, a jej przykłady są osadzone w obowiązkach wynikających z RODO i HIPAA. |
|  | ● CWE-212 Improper Removal of Sensitive Information Before Storage or Transfer | Błędy w redagowaniu i usuwaniu danych ujawniają wrażliwe fragmenty, np. tekst ukryty pod warstwą redakcyjną w postaci czarnego prostokąta, który ponownie wychodzi na jaw podczas streszczania. |
|  | ● CWE-532 Insertion of Sensitive Information into Log File | Potoki obserwowalności rejestrują pełne polecenia, odpowiedzi i ślady rozumowania, ujawniając dane wrażliwe za pośrednictwem magazynów logów i telemetrii. |
|  | ○ CWE-285 Improper Authorization | Wyszukiwanie odbywa się przed jakąkolwiek kontrolą autoryzacji, dlatego podobieństwo kosinusowe zwraca dokumenty, do których wnioskujący nie ma prawa dostępu. |
|  | ○ CWE-732 Incorrect Permission Assignment for Critical Resource | Dyski o nieograniczonym zakresie, przestarzałe uprawnienia i bazy wiedzy o zbyt liberalnych uprawnieniach zasilają korpus wyszukiwania danymi wrażliwymi. |
|  | ○ CWE-201 Insertion of Sensitive Information Into Sent Data | Argumenty wywołań narzędzi i żądania wychodzące do zewnętrznych dostawców zawierają więcej wrażliwych pól, niż faktycznie wymaga zadanie. |
| **LLM03** Nadmierna sprawczość | ● CWE-285 Improper Authorization | Autoryzacja musi być egzekwowana w punkcie decyzyjnym polityk w logice aplikacji, a nie pozostawiona modelowi, co chroni przed zachowaniami typu confused deputy i nadużywaniem uprawnień. |
|  | ● CWE-732 Incorrect Permission Assignment for Critical Resource | Tożsamości bazodanowe i konta usługowe z prawem zapisu i usuwania lub z szerokimi wysokimi uprawnieniami wykraczającymi poza potrzeby zadania są wskazaną wprost pierwotną przyczyną nadmiernej sprawczości. |
|  | ○ CWE-284 Improper Access Control (Pillar) | Filar obejmuje opisane w tej pozycji błędy uprawnień i kontroli dostępu, w tym brak pośrednictwa i utratę kontekstu użytkownika. |
|  | ○ CWE-770 Allocation of Resources Without Limits or Throttling | Do zatrzymania niekontrolowanych wywołań narzędzi potrzebne są progi i mechanizmy circuit breaker oparte na liczbie wywołań lub skumulowanej wartości parametru. |
|  | ○ CWE-1427 Improper Neutralization of Input Used for LLM Prompting | Bezpośrednie i pośrednie wstrzyknięcie polecenia, np. za pomocą spreparowanej wiadomości e-mail, jest czynnikiem wyzwalającym, który zamienia nadmierne uprawnienia w szkodliwe działanie. |
| **LLM04** Łańcuch dostaw | ● CWE-494 Download of Code Without Integrity Check | Modele i adaptery są pobierane za pomocą zmiennych tagów lub rozpoznawane po nazwie bez weryfikacji podpisu lub skrótu, co pozwala na wykonanie kodu przez ponowne wykorzystanie przestrzeni nazw. |
|  | ● CWE-829 Inclusion of Functionality from Untrusted Control Sphere | Pozycja koncentruje się na włączaniu modeli, adapterów i pakietów stron trzecich z niezaufanych repozytoriów i rejestrów, w tym na slopsquattingu i zmodyfikowanych modelach. |
|  | ○ CWE-349 Acceptance of Extraneous Untrusted Data With Trusted Data | Złośliwy adapter LoRA scalony z zaufanym modelem bazowym wprowadza zatrute artefakty do potoku scalania i konwersji, który poza tym jest zaufany. |
|  | ○ CWE-664 Improper Control of a Resource Through its Lifetime (Pillar) | Niebezpieczna deserializacja wag modelu i uszkodzenie pamięci w natywnym parserze to błędy kontroli nad zasobem, jakim jest załadowany model, w całym jego cyklu życia. |
| **LLM05** Zatruwanie danych i modeli | ● CWE-349 Acceptance of Extraneous Untrusted Data With Trusted Data | Niezaufane dane kontrolowane przez atakującego zostają zmieszane z zaufanymi korpusami treningowymi, magazynami wyszukiwania i źródłami ciągłego uczenia — to kanoniczna słabość związana z zatruwaniem. |
|  | ● CWE-829 Inclusion of Functionality from Untrusted Control Sphere | Ładowanie niezaufanych modeli, adapterów LoRA i PEFT, konfiguracji tokenizera i szablonów czatu z publicznych repozytoriów wprowadza do potoku zatrutą funkcjonalność. |
|  | ○ CWE-494 Download of Code Without Integrity Check | Zaleca się podpisywanie i weryfikację skrótów artefaktów modeli, ponieważ niezweryfikowane pobrania modeli mogą zawierać zmodyfikowane wagi. |
|  | ○ CWE-1427 Improper Neutralization of Input Used for LLM Prompting | Zatrute dokumenty wyszukiwania i ukryte instrukcje na stronach internetowych docierają do polecenia bez neutralizacji — to aspekt zatruwania związany z pośrednim wstrzyknięciem. |
| **LLM06** Nieograniczona konsumpcja | ● CWE-400 Uncontrolled Resource Consumption | Każdy opisany w tej pozycji wzorzec — zalewanie danymi wejściowymi, przepełnienie kontekstu, wyczerpanie tokenów rozumowania oraz wyczerpanie GPU lub pamięci — jest formą niekontrolowanego zużycia zasobów. |
|  | ● CWE-770 Allocation of Resources Without Limits or Throttling | Definicyjną pierwotną przyczyną jest brak limitów częstotliwości, tokenów, kosztów i kolejek, a brakującymi mechanizmami kontrolnymi są wstępne szacowanie i twarde limity wydatków. |
|  | ○ CWE-664 Improper Control of a Resource Through its Lifetime (Pillar) | Filar będący przodkiem słabości związanych z konsumpcją obejmuje zarządzanie zasobami obliczeniowymi i pamięcią w całym ich cyklu życia. |
|  | ○ CWE-691 Insufficient Control Flow Management (Pillar) | Pętle rozumowania zużywające tokeny rozumowania oraz rekurencyjne lub nieskończone pętle wywołań narzędzi wymagają limitów głębokości rekurencji i wykrywania pętli. |
|  | ○ CWE-829 Inclusion of Functionality from Untrusted Control Sphere | Złośliwe narzędzie lub umiejętność strony trzeciej pobrane z repozytorium open source napędza pętle wyczerpujące zasoby. |
| **LLM07** Dezinformacja | ● CWE-1426 Improper Validation of Generative AI Output | Nieprawidłowy, nieuzasadniony lub wprowadzający w błąd wynik modelu zostaje uznany za wiarygodny i staje się podstawą działania bez weryfikacji — to podstawowa słabość opisana w tej pozycji. |
|  | ○ CWE-349 Acceptance of Extraneous Untrusted Data With Trusted Data | Agent w dalszym etapie przetwarzania przyjmuje zmyślone lub błędnie przypisane wyniki z wcześniejszego etapu jako zaufane dowody — jest to awaria zaufania między agentami. |
|  | ○ CWE-494 Download of Code Without Integrity Check | Halucynowana nazwa pakietu zostaje zainstalowana bez jakiejkolwiek kontroli integralności lub autentyczności — to awaria typu slopsquatting. |
|  | ○ CWE-1427 Improper Neutralization of Input Used for LLM Prompting | Adwersarialnie spreparowane dane wejściowe wywołują fałszywe lub wprowadzające w błąd wyniki — to manipulacja po stronie danych wejściowych stojąca za dezinformacją wywoływaną przez przeciwnika. |
| **LLM08** Ujawnienie ukrytego kontekstu | ● CWE-200 Exposure of Sensitive Information to an Unauthorized Actor | Pozycję definiuje nieautoryzowane wydobycie, wywnioskowanie lub odtworzenie ukrytych instrukcji systemowych i kontekstu operacyjnego. |
|  | ○ CWE-798 Use of Hard-coded Credentials | Klucze API, tokeny i ciągi połączeń osadzone w monicie systemowym stanowią najpoważniejszą formę ujawnienia poświadczeń. |
|  | ○ CWE-693 Protection Mechanism Failure (Pillar) | Instrukcje odmowy i kontroli zachowania umieszczone w ukrytym kontekście zostają poddane inżynierii wstecznej i obejściu, zamiast być egzekwowane. |
|  | ○ CWE-285 Improper Authorization | Autoryzacja i kontrola dostępu muszą być egzekwowane niezależnie od modelu, ponieważ role i uprawnienia zawarte w opisach narzędzi wyciekają i zawodzą. |
| **LLM09** Słabe punkty wektorów i osadzeń | ● CWE-200 Exposure of Sensitive Information to an Unauthorized Actor | Inwersja osadzeń odtwarza tekst źródłowy, a wyciek między najemcami ujawnia dane innych klientów — to podstawowa awaria poufności. |
|  | ● CWE-285 Improper Authorization | Decyzja o kontroli dostępu zapada po wykonaniu wyszukiwania w przestrzeni osadzeń, dlatego zawężenie do najemcy musi być egzekwowane po stronie serwera w ramach zapytania do indeksu. |
|  | ○ CWE-359 Exposure of Private Personal Information | Dane osobowe klientów zostają odtworzone z wyciekłej kopii zapasowej osadzeń, co stwarza ryzyko dla osób, których dane dotyczą, w rozumieniu RODO. |
|  | ○ CWE-732 Incorrect Permission Assignment for Critical Resource | Błędnie określone listy ACL na poziomie fragmentów i błędna konfiguracja chmury ujawniająca kopię zapasową bazy wektorowej to błędy w przypisaniu kontroli dostępu. |
|  | ○ CWE-829 Inclusion of Functionality from Untrusted Control Sphere | Zatruwanie wyszukiwania powoduje, że treść atakującego z niezaufanego źródła zostaje pobrana i przekazana modelowi jako zaufany kontekst. |
| **LLM10** Nieprawidłowe przetwarzanie wyników | ● CWE-1426 Improper Validation of Generative AI Output | Wynik modelu jest przekazywany dalej przy niewystarczającej walidacji, sanityzacji lub obsłudze — to dokładnie przedmiot tej pozycji. |
|  | ● CWE-116 Improper Encoding or Escaping of Output | Pierwotną przyczyną jest brak kodowania danych wyjściowych uwzględniającego kontekst w odniesieniu do HTML, JavaScript, SQL i terminala jako miejsc docelowych. |
|  | ● CWE-79 Cross-site Scripting (XSS) | Cross-site scripting to najczęściej powtarzające się konkretne miejsce docelowe, któremu przeciwdziałają polityka bezpieczeństwa treści (CSP) i kodowanie danych wyjściowych. |
|  | ○ CWE-707 Improper Neutralization (Pillar) | Filar neutralizacji obejmuje sanityzację znaków sterujących — sekwencji ANSI, BEL, OSC, backspace i powrotu karetki — oraz escapowanie i parametryzację SQL. |
|  | ○ CWE-200 Exposure of Sensitive Information to an Unauthorized Actor | Nieobsłużony wynik powoduje wyciek wrażliwych treści z rozmowy i strony internetowej do atakującego. |
|  | ○ CWE-201 Insertion of Sensitive Information Into Sent Data | Dane wrażliwe zostają przemycone w nazwie hosta lub ciągu zapytania automatycznie wysyłanego żądania pobrania obrazu w formacie Markdown. |

## NIST AI 600-1 (Generative AI Profile) — v1.0 (lipiec 2024)

*Każdy wiersz mapuje ryzyko OWASP LLM na kategorie ryzyka NIST AI 600-1 Generative AI Profile, których ono dotyczy; kategorie podstawowe wskazują główne ryzyko danej pozycji, a kategorie wspierające — jego drugorzędne aspekty.*

| Ryzyko | Element | Znaczenie |
|---|---|---|
| **LLM01** Wstrzyknięcie polecenia | ● Information Security | Wstrzyknięcie polecenia poszerza powierzchnię ataku, pozwalając adwersarialnemu tekstowi przejąć kontrolę nad modelem w celu eksfiltracji danych lub wywołania nieautoryzowanych działań w połączonych systemach. |
|  | ○ Information Integrity | Wstrzyknięte instrukcje manipulują wynikami, zamieniając je w treści wybrane przez atakującego, i zatruwają pobrany lub zapamiętany kontekst, zniekształcając informacje wytwarzane przez system. |
|  | ○ Data Privacy | Skuteczne wstrzyknięcie może doprowadzić do eksfiltracji do atakującego prywatnej historii rozmów, przesłanych dokumentów i zawartości repozytoriów. |
|  | ○ Human-AI Configuration | Potwierdzanie z udziałem człowieka traci skuteczność w wyniku zmęczenia zatwierdzaniem, a niegroźne treści mogą zawierać niezamierzenie wstrzyknięte instrukcje — obie kwestie dotyczą konfiguracji nadzoru człowieka. |
| **LLM02** Ujawnianie informacji poufnych | ● Data Privacy | Pozycja koncentruje się na wycieku prywatnych danych i deanonimizacji PII, PHI oraz danych biometrycznych i genomowych, w tym na wnioskowaniu o przynależności, które potwierdza tożsamość konkretnej osoby i uniemożliwia wypełnienie obowiązków usunięcia danych. |
|  | ● Information Security | Dotyczy poufności systemu i jego danych w kontekście eksfiltracji modeli i danych, ujawniania poświadczeń i ataków ekstrakcji. |
|  | ○ Intellectual Property | Tajemnice handlowe i wagi modeli to chronione aktywa narażone na ryzyko, a modele mogą dosłownie odtwarzać materiały treningowe chronione prawem autorskim lub opatrzone znakiem wodnym. |
|  | ○ Value Chain and Component Integration | Platformy obserwowalności stron trzecich, SDK i zintegrowane komponenty przeglądarkowe ujawniają dane wrażliwe za pośrednictwem otaczającego ekosystemu. |
| **LLM03** Nadmierna sprawczość | ● Information Security | Nadmierna sprawczość pozwala agentowi eksfiltrować dane, wykonywać nieautoryzowane zapisy i podejmować działania destrukcyjne w systemach w dalszych etapach przetwarzania, do których ma dostęp. |
|  | ○ Human-AI Configuration | Nadmierna autonomia działająca bez potwierdzenia jest pierwotną przyczyną, a zatwierdzanie z udziałem człowieka — podstawowym mechanizmem kontrolnym; obie kwestie dotyczą konfiguracji nadzoru człowieka. |
|  | ○ Confabulation | Halucynowane lub konfabulowane rozumowanie może skłonić agenta do podjęcia szkodliwych działań w rzeczywistym świecie. |
|  | ○ Data Privacy | Agent o nadmiernych uprawnieniach może przekazać atakującemu prywatną treść wiadomości e-mail użytkownika, co stanowi naruszenie ochrony danych osobowych. |
| **LLM04** Łańcuch dostaw | ● Value Chain and Component Integration | Pozycja w całości dotyczy integralności modeli, zbiorów danych, adapterów i zintegrowanych komponentów stron trzecich w całym łańcuchu wartości AI. |
|  | ● Information Security | Przejęcie łańcucha dostaw prowadzi do backdoorów, zatruć, zdalnego wykonania kodu i innych naruszeń bezpieczeństwa. |
|  | ○ Intellectual Property | Niejasne licencje dotyczące użytkowania, dystrybucji i komercjalizacji, a także ryzyko naruszenia praw autorskich w związku z materiałami dostawców, zagrażają własności intelektualnej. |
|  | ○ Information Integrity | Zmodyfikowane lub zatrute modele generują stronnicze wyniki i dezinformację, a zmodyfikowany model działający na urządzeniu może kierować użytkowników na oszukańcze strony. |
|  | ○ Confabulation | Asystenci programowania konfabulują nieistniejące nazwy pakietów, które atakujący rejestrują z wyprzedzeniem — to wektor slopsquattingu. |
| **LLM05** Zatruwanie danych i modeli | ● Information Integrity | Zatruwanie steruje odpowiedziami, wprowadza subtelną dezinformację oraz manipuluje rekomendacjami i decyzjami biznesowymi w dalszych etapach. |
|  | ● Value Chain and Component Integration | Zbiory danych stron trzecich, współdzielone repozytoria modeli i dołączane artefakty wprowadzają zagrożenie przejęcia za pośrednictwem łańcucha wartości GenAI. |
|  | ● Information Security | Zatruwanie danych i modeli, osadzone backdoory oraz wykonanie kodu z niebezpiecznych artefaktów to podstawowe zagrożenia bezpieczeństwa. |
|  | ○ Dangerous, Violent, or Hateful Content | Ukierunkowane zatruwanie osłabia zachowania polegające na odmowie, jak wtedy, gdy publiczny chatbot został zmanipulowany tak, że generował obraźliwe treści. |
| **LLM06** Nieograniczona konsumpcja | ● Information Security | Nieograniczona konsumpcja zagraża dostępności poprzez odmowę usługi oraz poufności poprzez ekstrakcję modelu i kradzież wag i architektury kanałami bocznymi. |
|  | ○ Intellectual Property | Ekstrakcja modelu i funkcjonalne klonowanie prowadzą do kradzieży modelu jako własności intelektualnej i tajemnicy handlowej. |
|  | ○ Value Chain and Component Integration | Współdzielona infrastruktura wnioskowania i frameworki do serwowania modeli stron trzecich poszerzają powierzchnię ataku na łańcuch dostaw. |
| **LLM07** Dezinformacja | ● Information Integrity | Pozycja dotyczy generowania przez GenAI fałszywych lub wprowadzających w błąd informacji, które pogarszają decyzje podejmowane przez ludzi — to definicja tego ryzyka dezinformacji. |
|  | ● Confabulation | Konfabulacja, czyli stosowany przez NIST termin na wyniki podawane z pewnością, ale fałszywe, jest wskazana jako główne źródło dezinformacji. |
|  | ● Human-AI Configuration | Błąd automatyzacji (automation bias) i nadmierne poleganie na modelu są wskazane jako kluczowy czynnik, któremu przeciwdziała kalibracja zaufania ludzi i systemów. |
|  | ○ Information Security | Dezinformacja wywołana przez przeciwnika i fałszywe alerty mogą powodować incydenty bezpieczeństwa i zakłócenia operacyjne. |
|  | ○ Value Chain and Component Integration | Halucynowane zależności i niezwalidowane wyniki narzędzi stron trzecich stanowią ryzyko integracji w łańcuchu wartości. |
| **LLM08** Ujawnienie ukrytego kontekstu | ● Information Security | Wydobycie monitu systemowego i kontekstu, poszerzona powierzchnia ataku i obniżenie barier dla ukierunkowanych ataków to podstawowe zagrożenia bezpieczeństwa. |
|  | ○ Intellectual Property | Ujawniony ukryty kontekst może ujawniać zastrzeżone zachowania i wrażliwe szczegóły implementacji, np. w postaci wycieku monitu systemowego. |
| **LLM09** Słabe punkty wektorów i osadzeń | ● Data Privacy | Inwersja osadzeń, wnioskowanie o przynależności i wyciek między najemcami pozwalają odzyskać PII i wywnioskować przynależność osób, których dane dotyczą, uruchamiając obowiązki związane z naruszeniami. |
|  | ● Information Security | Awarie kontroli dostępu, nieautoryzowane wyszukiwanie między najemcami, nadużywanie wyroczni i endpointów oraz zagłuszanie dostępności to podstawowe problemy bezpieczeństwa. |
|  | ● Information Integrity | Zatruwanie wyszukiwania zniekształca kontekst, na którym polega model, obniżając integralność pobieranych i generowanych informacji. |
|  | ○ Value Chain and Component Integration | Model osadzeń strony trzeciej z backdoorem zaburza geometrię wszystkiego, co zostanie przyjęte — to ryzyko dla integralności łańcucha wartości. |
| **LLM10** Nieprawidłowe przetwarzanie wyników | ● Information Security | Nieobsłużone wyniki modelu przenikają do systemów w dalszych etapach przetwarzania w postaci zdalnego wykonania kodu, XSS, wstrzyknięcia SQL, SSRF, CSRF i eskalacji uprawnień. |
|  | ○ Data Privacy | Niesanityzowane wyniki mogą powodować wyciek wrażliwych danych z rozmowy do atakującego. |
|  | ○ Confabulation | Wygenerowany kod może odwoływać się do halucynowanych, nieistniejących pakietów oprogramowania — to aspekt niebezpiecznych wyników związany z konfabulacją. |
|  | ○ Value Chain and Component Integration | Rozszerzenia stron trzecich, które nie walidują danych wejściowych, oraz instalacje halucynowanych pakietów dotyczą integracji komponentów i łańcucha dostaw. |

## NIST AI RMF (AI 100-1) — v1.0 (2023)

*Każdy wiersz mapuje ryzyko OWASP LLM na kategorie NIST AI RMF (AI 100-1), których rezultatów dotyczy ono najbardziej bezpośrednio, przy czym „podstawowe” oznacza dopasowanie niemal dosłowne, a „wspierające” — dopasowanie częściowe lub na poziomie ładu organizacyjnego (governance).*

| Ryzyko | Element | Znaczenie |
|---|---|---|
| **LLM01** Wstrzyknięcie polecenia | ○ MEASURE 1 (methods & metrics) | Pozycja odrzuca deklaracje skuteczności ataków oparte wyłącznie na testach statycznych i wymaga metryk ataków adaptacyjnych, wyznaczając poziom bazowy obrony przed wstrzyknięciami za pomocą AgentDojo i JailbreakBench, ponieważ wyniki statyczne są bliskie zeru, podczas gdy skuteczność ataków adaptacyjnych przekracza 90 procent. |
|  | ○ MEASURE 2 (evaluate trustworthiness) | Mechanizmy obrony są testowane w ramach red teamingu przez atakujących, którzy zapoznali się z pełną specyfikacją obrony, co pozwala ocenić odporność wdrożonego systemu na wstrzyknięcia, zamiast polegać na metryce wybranej w oderwaniu od kontekstu. |
|  | ○ MAP 4 (map risks across all components) | Rozłożenie wstrzyknięcia na powierzchnię dostarczenia, sposób propagacji i kodowanie jest przedstawione jako krok modelowania zagrożeń, który mapuje ryzyko na komponenty, w tym korpusy RAG, serwery MCP i pakiety narzędzi stron trzecich. |
| **LLM02** Ujawnianie informacji poufnych | ○ MAP 5 (characterize impacts on people) | Ujawnienie jest ujmowane jako wpływ na osoby, których dane dotyczą, przy czym wnioskowanie o przynależności prowadzi do stwierdzenia naruszenia w rozumieniu przepisów w odniesieniu do konkretnej osoby na gruncie RODO lub HIPAA. |
|  | ○ MEASURE 2 (evaluate trustworthiness) | Warunkiem wydania są testy red team pod kątem ujawniania, które ilościowo mierzą ekstrakcję, wnioskowanie o przynależności, inwersję osadzeń, inwersję stanu wewnętrznego, kanały boczne i podatność adapterów LoRA na ekstrakcję. |
|  | ○ MANAGE 4 (risk treatment, response & recovery) | Scenariusz reagowania na incydenty ujawnienia określa terminy zgłaszania naruszeń i przewiduje obsługę wycieków przez oduczanie, ponowne trenowanie, wycofanie modelu, oczyszczenie wektorów i pamięci podręcznych oraz powiadomienie dostawców. |
| **LLM03** Nadmierna sprawczość | ○ MANAGE 4 (risk treatment, response & recovery) | Rejestrowanie i monitorowanie aktywności rozszerzeń i systemów w dalszych etapach przetwarzania jest połączone z mechanizmami circuit breaker, które zatrzymują działania agenta, ograniczają ich częstotliwość lub przekazują je do weryfikacji przez człowieka. |
| **LLM04** Łańcuch dostaw | ● GOVERN 6 (third-party & supply-chain policy) | Cała pozycja dotyczy łańcucha dostaw LLM i obejmuje weryfikację dostawców, przegląd regulaminów oraz politykę aktualizacji komponentów w odniesieniu do oprogramowania, danych i modeli stron trzecich. |
|  | ● MAP 4 (map risks across all components) | Podpisany inwentarz komponentów SBOM, AIBOM i ML-SBOM wraz z weryfikacją komponentów i dostawców mapuje ryzyko dla każdego modelu, zbioru danych i zależności stron trzecich. |
|  | ● MANAGE 3 (manage third-party risks) | Weryfikacja i ponowne audytowanie dostawców oraz traktowanie usług konwersji lub scalania modeli jako punktów promowania wysokiego ryzyka to bezpośrednie zarządzanie ryzykiem związanym z podmiotami zewnętrznymi. |
|  | ○ MEASURE 2 (evaluate trustworthiness) | Testy AI red teaming i ewaluacje przy wyborze modeli stron trzecich, a także testy odporności na ataki adwersarialne i wykrywanie anomalii w środowisku produkcyjnym, służą ocenie pozyskanych komponentów. |
|  | ○ MEASURE 3 (track risks over time) | Ciągła walidacja integralności modeli z wcześniejszych etapów łańcucha, wykrywanie anomalii w środowisku produkcyjnym oraz regularne ponowne audyty BOM i dostawców pozwalają śledzić ryzyko łańcucha dostaw w czasie. |
| **LLM05** Zatruwanie danych i modeli | ○ MEASURE 3 (track risks over time) | Wyniki, funkcja straty podczas trenowania i wzorce zachowań są monitorowane pod kątem dryfu względem zdefiniowanych progów, aby wykrywać subtelne zatrucia ujawniające się z czasem. |
|  | ○ MEASURE 2 (evaluate trustworthiness) | Ciągłe testy red team z użyciem poleceń adwersarialnych i opartych na wyzwalaczach, z obowiązkowym dedykowanym sondowaniem wyzwalaczy po każdym cyklu dostosowania, pozwalają ocenić model pod kątem ukrytych backdoorów. |
|  | ○ GOVERN 6 (third-party & supply-chain policy) | Weryfikacja dostawców, śledzenie rodowodu modeli i danych oraz mechanizmy kontrolne przeciwdziałające zatruwaniu łańcucha dostaw zbiorów danych open source i złośliwym modelom odnoszą się do wektorów zatruwania pochodzących od stron trzecich. |
| **LLM06** Nieograniczona konsumpcja | ○ MANAGE 1 (prioritize & respond to risks) | Limity częstotliwości, twarde limity wydatków, agentowe mechanizmy circuit breaker i kontrolowana degradacja tworzą reakcję priorytetyzowaną według wpływu, opartą na monitorowaniu przypisania kosztów i poziomach bazowych. |
|  | ○ MEASURE 2 (evaluate trustworthiness) | Skanowanie pod kątem zaburzeń adwersarialnych i testy red team dotyczące zużycia zasobów, wspierane poziomami bazowymi zachowania narzędzi, pozwalają ocenić system pod kątem cechy bezpiecznej i odpornej dostępności. |
| **LLM07** Dezinformacja | ● MEASURE 2 (evaluate trustworthiness) | Obrona opisana w tej pozycji koncentruje się na ewaluacji i wykorzystuje kontrole ugruntowania i spójności, monitorowanie i testowanie pod kątem dezinformacji oraz ciągłą ewaluację adwersarialną do pomiaru trafności, niezawodności i dokładności, którym zagraża dezinformacja. |
|  | ○ MEASURE 1 (methods & metrics) | Sygnały weryfikacyjne, takie jak kontrole ugruntowania i spójności, zastępują naiwny poziom pewności modelu jako miarę tego, czy twierdzeniu można zaufać przed podjęciem działania. |
|  | ○ MEASURE 3 (track risks over time) | Twierdzenia, dowody i rezultaty są rejestrowane, a scenariusze adwersarialne testowane, dzięki czemu dezinformację można śledzić w czasie. |
|  | ○ MANAGE 1 (prioritize & respond to risks) | Weryfikacja w czasie działania i przepływy zatwierdzania działań o dużym wpływie, wraz z ograniczeniem zasięgu rażenia, to mechanizmy reakcji priorytetyzowane według wpływu. |
| **LLM08** Ujawnienie ukrytego kontekstu | ○ MEASURE 2 (evaluate trustworthiness) | Wykrywanie ujawnienia ukrytego kontekstu to działanie z zakresu ewaluacji bezpieczeństwa, polegające na testach red team pod kątem wydobycia poleceń i kontekstu, oparte na badaniach nad red teamingiem z wykorzystaniem uczenia ze wzmocnieniem (RL) w zakresie wycieku prywatnych danych oraz nad wydobywaniem monitów systemowych. |
| **LLM09** Słabe punkty wektorów i osadzeń | ○ MAP 4 (map risks across all components) | Warstwa osadzeń zostaje uznana za część granicy zaufania aplikacji, co zobowiązuje zespoły do weryfikacji modelu osadzeń oraz traktowania zewnętrznych usług osadzeń i kopii zapasowych jako komponentów objętych zakresem. |
|  | ○ MAP 5 (characterize impacts on people) | Wpływ na osoby, których dane dotyczą, zostaje scharakteryzowany, ponieważ inwersja osadzeń odzyskuje PII, a wyciek „wyłącznie osadzeń” zostaje przeklasyfikowany jako naruszenie dokumentów źródłowych, uruchamiające obowiązek zgłoszenia naruszenia. |
|  | ○ MANAGE 4 (risk treatment, response & recovery) | Niezmienne logi wyszukiwania i zaktualizowane scenariusze reagowania na incydenty traktują wycieki „wyłącznie osadzeń” jako naruszenia danych źródłowych podlegające zgłoszeniu na podstawie art. 33 RODO. |

## CSA AI Controls Matrix (AICM) — v1.1 (2026-06-22)

*Każdy wiersz mapuje ryzyko OWASP LLM na domeny mechanizmów kontrolnych CSA AI Controls Matrix v1.1, które się do niego odnoszą, przy czym domeny podstawowe obejmują główny mechanizm kontrolny, a domeny wspierające zapewniają obronę w głąb.*

| Ryzyko | Element | Znaczenie |
|---|---|---|
| **LLM01** Wstrzyknięcie polecenia | ● AIS Application & Interface Security | Ogranicza powierzchnię wstrzyknięcia na poziomie interfejsu poprzez rozdzielenie ról w monicie systemowym, walidację schematu wyników, filtrowanie danych wejściowych we wszystkich modalnościach, usuwanie niewidocznych znaków Unicode oraz kanały oznaczone pochodzeniem, które oznaczają niezaufane treści. |
|  | ○ IAM Identity & Access Management | Ogranicza skutki udanego wstrzyknięcia poprzez przyznawanie każdemu wywołaniu narzędzia najmniejszych uprawnień, wykonywanie działań w kontekście użytkownika o ograniczonym zakresie oraz wymaganie zatwierdzenia przez człowieka przed operacjami uprzywilejowanymi lub nieodwracalnymi. |
|  | ○ TVM Threat & Vulnerability Management | Wymaga adaptacyjnych testów red team prowadzonych przez przeciwników, którzy zapoznali się już z mechanizmami obrony, odrzucając zestawy wyłącznie statycznych testów, które nie wychwytują ewoluujących technik wstrzyknięcia. |
|  | ○ STA Supply Chain Management, Transparency, and Accountability | Traktuje połączone narzędzia, wtyczki i serwery MCP jako powierzchnię łańcucha dostaw oprogramowania, która musi zostać przypięta, podpisana i zweryfikowana, zanim agent zacznie z niej korzystać. |
| **LLM02** Ujawnianie informacji poufnych | ● DSP Data Security and Privacy Lifecycle Management | Reguluje klasyfikację, minimalizację, retencję, szyfrowanie i weryfikowalne usuwanie wrażliwych rekordów w czteroetapowym cyklu życia danych, wokół którego zorganizowana jest ta pozycja. |
|  | ● IAM Identity & Access Management | Egzekwuje autoryzację przed wyszukiwaniem, stosując kontrolę dostępu na poziomie dokumentów i fragmentów w ramach zapytania do indeksu oraz izolację najemców, ponieważ samo podobieństwo wektorowe nie respektuje list kontroli dostępu. |
|  | ○ CEK Cryptography, Encryption & Key Management | Stosuje szyfrowanie w tranzycie i w spoczynku, szyfrowanie zachowujące format dla identyfikatorów strukturalnych oraz szyfrowanie bazy wektorowej, aby dane wrażliwe pozostały nieczytelne w razie ich ujawnienia. |
|  | ○ MDS Model Development Security | Ogranicza zapamiętywanie danych treningowych poprzez trenowanie z prywatnością różnicową skalibrowaną do wrażliwości danych, deduplikację niemal identycznych duplikatów, weryfikowalne usuwanie danych z punktów kontrolnych i adapterów oraz odporność na destylację. |
|  | ○ AIS Application & Interface Security | Kontroluje dostęp do logarytmów prawdopodobieństw, poziomów pewności i wyjaśnień w produkcyjnych endpointach, sanityzuje wyniki za pomocą klasyfikatorów, rozdziela trasowanie wewnętrzne od zewnętrznego i chroni przed kanałami bocznymi przy przesyłaniu strumieniowym. |
| **LLM03** Nadmierna sprawczość | ● IAM Identity & Access Management | Minimalizuje uprawnienia agentów, wykonuje działania narzędzi w kontekście użytkownika o zakresie ograniczonym przez OAuth i stosuje pełne pośrednictwo, tak aby tożsamość o nadmiernych uprawnieniach nie mogła przekroczyć swojej autoryzacji. |
|  | ○ AIS Application & Interface Security | Utwardza interfejs rozszerzeń i narzędzi za pomocą ścisłych schematów danych wejściowych, walidacji parametrów, unikania otwartej funkcjonalności oraz testów bezpieczeństwa aplikacji. |
|  | ○ LOG Logging and Monitoring | Rejestruje i monitoruje aktywność rozszerzeń i systemów w dalszych etapach przetwarzania, aby szybko wykrywać niepożądane lub nieautoryzowane działania. |
|  | ○ TVM Threat & Vulnerability Management | Przeprowadza statyczne, dynamiczne i interaktywne testy bezpieczeństwa aplikacji w potoku rozwoju oprogramowania, aby wykrywać podatności w rozszerzeniach agentów przed wydaniem. |
| **LLM04** Łańcuch dostaw | ● STA Supply Chain Management, Transparency, and Accountability | Bezpośrednio odpowiada tej domenie dzięki inwentarzowi SBOM/AIBOM/ML-SBOM, weryfikacji dostawców oraz potwierdzaniu pochodzenia opartemu na podpisach kryptograficznych i rejestrach przejrzystości. |
|  | ● MDS Model Development Security | Przeciwdziała modyfikacjom modeli i backdoorom oraz zachowuje integralność podczas scalania adapterów, konwersji formatów i kwantyzacji dzięki podpisywaniu modeli i ewaluacji ich zachowania. |
|  | ○ TVM Threat & Vulnerability Management | Skanuje frameworki do serwowania modeli i same modele pod kątem podatnych lub przestarzałych komponentów i egzekwuje politykę aktualizacji w całym łańcuchu zależności. |
|  | ○ CEK Cryptography, Encryption & Key Management | Wykorzystuje kryptograficzne podpisywanie modeli, wpisy w Sigstore i rejestrach przejrzystości, skróty plików oraz szyfrowanie modeli brzegowych z kontrolą integralności do weryfikacji autentyczności artefaktów. |
|  | ○ CCC Change Control and Configuration Management | Egzekwuje granice promowania za pomocą niezmiennych odwołań, bramek wydania opartych na politykach zgodnych z SLSA oraz integralności potoku budowania w całym CI/CD. |
| **LLM05** Zatruwanie danych i modeli | ● MDS Model Development Security | Zabezpiecza dane do trenowania i dostrajania oraz artefakty modeli przed zatruciem w całym cyklu rozwoju. |
|  | ● DSP Data Security and Privacy Lifecycle Management | Waliduje przychodzące dane, sprawdza integralność i wersjonuje zbiory danych, aby w całym cyklu życia danych wychwytywać zmodyfikowane dane wejściowe. |
|  | ○ STA Supply Chain Management, Transparency, and Accountability | Ustala rodowód modeli i zbiorów danych za pomocą rekordów SBOM/ML-BOM, podpisywania artefaktów i weryfikacji dostawców, aby przeciwdziałać rozpowszechnianiu zatrutych modeli. |
|  | ○ LOG Logging and Monitoring | Stosuje statystyczne i oparte na AI wykrywanie anomalii oraz monitorowanie dryfu zachowań w trenowaniu, tworzeniu osadzeń i wnioskowaniu, aby ujawniać efekty zatrucia. |
|  | ○ TVM Threat & Vulnerability Management | Prowadzi ciągłe adwersarialne testy red team i sondowanie wyzwalaczy, aby ujawniać ukryte backdoory umieszczone w wyniku zatrucia. |
| **LLM06** Nieograniczona konsumpcja | ● AIS Application & Interface Security | Stosuje podstawowe mechanizmy kontrolne na poziomie interfejsu, w tym ograniczanie częstotliwości żądań, limity uwzględniające tokeny i koszty, walidację rozmiaru danych wejściowych, wstępne szacowanie tokenów i uwierzytelnione endpointy wnioskowania. |
|  | ● BCR Business Continuity Management and Operational Resilience | Zachowuje dostępność dzięki kontrolowanej degradacji oraz dynamicznemu skalowaniu z równoważeniem obciążenia w przypadku skokowego wzrostu zapotrzebowania lub nadużyć. |
|  | ○ IVS Infrastructure & Virtualization Security | Zarządza alokacją zasobów i utwardza infrastrukturę wnioskowania, w tym wyłącza niebezpieczną deserializację i ogranicza powierzchnię kanałów bocznych we współdzielonej infrastrukturze wnioskowania. |
|  | ○ LOG Logging and Monitoring | Zapewnia ciągłe monitorowanie przypisania kosztów oraz wykrywanie zasobożernych interakcji z narzędziami względem poziomów bazowych normalnego zachowania. |
|  | ○ IAM Identity & Access Management | Określa przydziały i twarde limity wydatków dla każdego klucza API, użytkownika, zespołu i konta oraz uwierzytelnia endpointy wnioskowania, aby ograniczyć nadużycia z wykorzystaniem skradzionych poświadczeń. |
| **LLM07** Dezinformacja | ● AIS Application & Interface Security | Oddziela twierdzenia od działań, waliduje semantycznie wywołania narzędzi, wymaga sygnałów weryfikacyjnych i ustrukturyzowanych wyników z polami obowiązkowymi oraz weryfikuje twierdzenia modelu w czasie działania. |
|  | ○ TVM Threat & Vulnerability Management | Prowadzi ewaluację adwersarialną i ciągłe testy z użyciem celowo wprowadzających w błąd scenariuszy, aby wychwycić dezinformację, zanim dotrze do użytkowników. |
|  | ○ LOG Logging and Monitoring | Rejestruje twierdzenia, dowody i rezultaty oraz monitoruje wzorce dezinformacji w czasie. |
|  | ○ STA Supply Chain Management, Transparency, and Accountability | Przeciwdziała atakom z wykorzystaniem halucynowanych zależności i niebezpiecznemu wygenerowanemu kodowi, weryfikując, czy pakiety i zależności wskazane przez model rzeczywiście istnieją i są zaufane. |
| **LLM08** Ujawnienie ukrytego kontekstu | ● AIS Application & Interface Security | Traktuje ukryty kontekst jako ryzyko ujawnienia na poziomie interfejsu, starannie dobierając to, co trafia do okna kontekstowego, stosując mechanizmy ochronne na warstwie aplikacji i egzekwując niezależne mechanizmy kontrolne, zamiast ufać poleceniu. |
|  | ○ IAM Identity & Access Management | Egzekwuje autoryzację i kontrolę dostępu niezależnie od modelu oraz rozdziela uprawnienia między agentów, tak aby ujawniony kontekst nie mógł przyznawać możliwości. |
|  | ○ CEK Cryptography, Encryption & Key Management | Utrzymuje poświadczenia, tokeny i ciągi połączeń poza poleceniem, przenosząc sekrety do zarządzanego magazynu. |
| **LLM09** Słabe punkty wektorów i osadzeń | ● IAM Identity & Access Management | Zawęża tożsamość najemcy w ramach zapytania do indeksu, waliduje ją po stronie serwera, uwierzytelnia endpointy poszczególnych najemców i egzekwuje kontrolę dostępu na poziomie fragmentów. |
|  | ● DSP Data Security and Privacy Lifecycle Management | Śledzi pochodzenie, segreguje poziomy zaufania, usuwa osadzenia wraz z ich źródłem oraz audytuje retencję, szyfrowanie w spoczynku i poziom wrażliwości kopii zapasowych w całym cyklu życia osadzeń. |
|  | ● LOG Logging and Monitoring | Stosuje wykrywanie anomalii przy przyjmowaniu danych i wyszukiwaniu oraz prowadzi niezmienne logi wyszukiwania monitorowane pod kątem obejścia filtrów najemców i dostępu między najemcami. |
|  | ○ CEK Cryptography, Encryption & Key Management | Szyfruje osadzenia w spoczynku kluczami zarządzanymi niezależnie od warstwy aplikacji i traktuje klucze do API osadzeń jako sekrety. |
|  | ○ AIS Application & Interface Security | Uwierzytelnia endpointy osadzeń i wyszukiwania podobieństwa jako pełnoprawne interfejsy API, stosuje limity częstotliwości dla poszczególnych najemców i nie udostępnia surowych ocen podobieństwa. |
|  | ○ STA Supply Chain Management, Transparency, and Accountability | Weryfikuje model osadzeń pod kątem ryzyka kodera z backdoorem i śledzi pochodzenie kodera oraz indeksowanych treści. |
| **LLM10** Nieprawidłowe przetwarzanie wyników | ● AIS Application & Interface Security | Waliduje odpowiedzi modelu jako niezaufane dane wejściowe i stosuje kodowanie danych wyjściowych uwzględniające kontekst, zapytania parametryzowane oraz politykę bezpieczeństwa treści, zanim wyniki trafią do interpreterów w dalszych etapach przetwarzania. |
|  | ○ LOG Logging and Monitoring | Rejestruje i monitoruje wyniki modelu, aby wykrywać wzorce wykorzystania podatności w ich dalszym przetwarzaniu. |
|  | ○ IAM Identity & Access Management | Traktuje model jak każdego innego użytkownika zgodnie z podejściem zerowego zaufania, tak aby jego wyniki nigdy nie dziedziczyły uprawnień wykraczających poza uprawnienia użytkownika końcowego. |

## OWASP AIVSS (AI Vulnerability Scoring System) — v0.8

*Każdy wiersz wymienia kluczowe ryzyka bezpieczeństwa agentowej AI (Agentic AI Core Security Risks) według OWASP AIVSS, które dana pozycja LLM Top 10 może wywołać lub zasilić, przy czym elementy podstawowe oznaczają ryzyka, które pozycja umożliwia najbardziej bezpośrednio, a wspierające — ścieżki drugorzędne; AIVSS ocenia następnie wagę tych ryzyk agentowych.*

| Ryzyko | Element | Znaczenie |
|---|---|---|
| **LLM01** Wstrzyknięcie polecenia | ● AIVSS-10 Agent Goal and Instruction Manipulation | Spreparowana wiadomość nadpisuje rolę i ograniczenia możliwości określone w monicie systemowym, kierując model do działania poza zamierzonym zakresem. |
|  | ● AIVSS-1 Agentic AI Tool Misuse | Wstrzyknięte dane wejściowe prowadzą do nieautoryzowanego wywoływania narzędzi, z których agent ma prawo korzystać — od systemu plików i powłoki po pocztę e-mail i chmurowe API. |
|  | ● AIVSS-2 Agent Access Control Violation | Agent odczytuje tekst podłożony przez atakującego, działając z podwyższonymi poświadczeniami użytkownika, i wykonuje uprzywilejowane działania, do których atakujący nie miałby bezpośredniego dostępu. |
|  | ● AIVSS-6 Agent Memory and Context Manipulation | Wstrzyknięcie zapisane w pamięci długotrwałej lub korpusie RAG skaża każdą kolejną sesję odczytującą zatruty magazyn, utrwalając przejęcie w kolejnych rozmowach. |
|  | ○ AIVSS-7 Insecure Agent Critical Systems Interaction | Tam, gdzie agent ma dostęp do powłoki, systemu plików lub chmurowego API, wstrzyknięcie eskaluje do wykonania dowolnych poleceń i działań destrukcyjnych na hoście. |
|  | ○ AIVSS-3 Agent Cascading Failures | Wyniki narzędzi wracają do współdzielonego okna kontekstowego, pozwalając jednemu wstrzyknięciu wywołać łańcuch kolejnych wywołań narzędzi lub samoreplikować się w kolejnych krokach. |
|  | ○ AIVSS-4 Agent Orchestration and Multi-Agent Exploitation | Wstrzyknięcie może samoreplikować się między agentami, rozprzestrzeniając się od jednego przejętego agenta na pozostałych uczestników wieloagentowego przepływu pracy. |
| **LLM03** Nadmierna sprawczość | ● AIVSS-1 Agentic AI Tool Misuse | Rozszerzenia mają funkcje wykraczające poza potrzeby zadania, np. dostęp tylko do odczytu połączony z możliwością modyfikowania, usuwania lub wysyłania, dlatego nieprawidłowo działający model wywołuje możliwości, z których nigdy nie miał korzystać. |
|  | ● AIVSS-2 Agent Access Control Violation | Rozszerzenia łączą się z systemami w dalszych etapach przetwarzania za pomocą tożsamości o zbyt szerokich lub ogólnych wysokich uprawnieniach, pozwalając agentowi działać daleko poza zakresem autoryzacji samego użytkownika. |
|  | ● AIVSS-7 Insecure Agent Critical Systems Interaction | Rozszerzenie o otwartej funkcjonalności wykonujące polecenia powłoki, które nie filtruje niezamierzonych poleceń, lub agent programistyczny o nadmiernej autonomii mogą wykonywać szkodliwe działania i zniszczyć infrastrukturę produkcyjną. |
|  | ○ AIVSS-3 Agent Cascading Failures | Bez limitów częstotliwości lub mechanizmów circuit breaker niekontrolowane wywoływanie rozszerzeń narasta, prowadząc do kaskadowych awarii w dalszych etapach przetwarzania. |
|  | ○ AIVSS-4 Agent Orchestration and Multi-Agent Exploitation | Złośliwy lub przejęty agent współpracujący może wywołać szkodliwe działania, a niezachowanie kontekstu użytkownika w łańcuchu wywołań agentów zwiększa zasięg rażenia. |
|  | ○ AIVSS-10 Agent Goal and Instruction Manipulation | Bezpośrednie lub pośrednie wstrzyknięcie polecenia, np. za pomocą spreparowanej przychodzącej wiadomości e-mail, jest czynnikiem wyzwalającym, który kieruje agenta ku niepożądanym działaniom uprzywilejowanym. |
| **LLM04** Łańcuch dostaw | ○ AIVSS-8 Agent Supply Chain and Dependency Risk | Przejęte pakiety stron trzecich, wstępnie wytrenowane modele, adaptery LoRA oraz potoki konwersji lub scalania stanowią podłoże zależności, które agent dziedziczy, dlatego zmodyfikowany artefakt z łańcucha dostaw modeli staje się również częścią łańcucha dostaw agenta. |
| **LLM05** Zatruwanie danych i modeli | ○ AIVSS-6 Agent Memory and Context Manipulation | Atakujący osadzają w treściach internetowych ukryte instrukcje, które zatruwają pamięć trwałą lub rekomendacje agenta w wielu sesjach, w wyniku czego agent zaczyna nadawać priorytet logice kontrolowanej przez atakującego. |
|  | ○ AIVSS-8 Agent Supply Chain and Dependency Risk | Modele z backdoorami i artefakty z niebezpieczną serializacją, rozpowszechniane za pośrednictwem publicznych repozytoriów, zawierają ukryte wyzwalacze, które agent w dalszym etapie dziedziczy przy ich ładowaniu. |
|  | ○ AIVSS-3 Agent Cascading Failures | Zatrute dane wejściowe rozprzestrzeniają się w wieloagentowych i firmowych przepływach pracy, a współdzielone osadzenia lub warstwy pamięci przenoszą skażenie z jednego najemcy na innych. |
| **LLM06** Nieograniczona konsumpcja | ○ AIVSS-1 Agentic AI Tool Misuse | Złośliwe narzędzie wciąga agenta w rekurencyjne lub nieskończone pętle wywołań narzędzi, zwiększając zużycie tokenów i koszty daleko ponad potrzeby zadania. |
|  | ○ AIVSS-3 Agent Cascading Failures | Protokoły agentowe i MCP zamieniają pojedyncze żądanie w kaskadę operacji w dalszych etapach przetwarzania, dlatego jedno zadanie rozgałęzia się na wiele wywołań narzędzi, które wyczerpują budżet i dostępność. |
|  | ○ AIVSS-8 Agent Supply Chain and Dependency Risk | Atakujący publikuje złośliwe narzędzie, np. umiejętność (skill) w repozytorium open source, które programiści włączają jako zależność agenta, co po integracji prowadzi do wyczerpania tokenów. |
| **LLM07** Dezinformacja | ● AIVSS-3 Agent Cascading Failures | Nieprawidłowy stan lub dowody wytworzone przez jednego agenta i uznane za wiarygodne przez innego rozprzestrzeniają się w wieloagentowym przepływie pracy, prowadząc do narastającej awarii o dużym wpływie. |
|  | ○ AIVSS-1 Agentic AI Tool Misuse | Ponieważ wyniki modelu sterują wywołaniami narzędzi, nieprawidłowe wnioskowanie o stanie uruchamia niezamierzone działania, chyba że wywołania są walidowane względem rzeczywistego stanu i intencji. |
|  | ○ AIVSS-7 Insecure Agent Critical Systems Interaction | Fałszywy wniosek, np. błędne zatwierdzenie zwrotu lub wygenerowanie fałszywego alertu, uruchamia zautomatyzowane działanie o dużym wpływie, które powoduje straty finansowe lub zakłócenia operacyjne. |
| **LLM08** Ujawnienie ukrytego kontekstu | ○ AIVSS-2 Agent Access Control Violation | Ujawnione z ukrytego kontekstu reguły uprawnień i dyrektywy dotyczące ról użytkowników odsłaniają model autoryzacji, zachęcając do sondowania, które omija mechanizmy kontrolne, jakie aplikacja powinna egzekwować niezależnie od LLM. |
|  | ○ AIVSS-1 Agentic AI Tool Misuse | Wydobycie ukrytej listy narzędzi i schematów parametrów daje atakującemu konkretne cele, pozwalające kierować aplikację w stronę określonych wywołań narzędzi. |
| **LLM09** Słabe punkty wektorów i osadzeń | ○ AIVSS-6 Agent Memory and Context Manipulation | Zatruwanie na etapie wyszukiwania manipuluje geometrią pamięci agenta opartej na bazie wektorowej, tak aby treść atakującego była pobierana jako zaufany kontekst, natomiast niegeometryczne zatruwanie pamięci jest przekazane do frameworku agentowego. |
|  | ○ AIVSS-8 Agent Supply Chain and Dependency Risk | Model osadzeń z backdoorem zaburza geometrię wszystkiego, co zostanie przyjęte, a podatne zależności baz wektorowych potęgują ryzyko, ponieważ z wyciekłego indeksu można odtworzyć dokumenty źródłowe. |
|  | ○ AIVSS-2 Agent Access Control Violation | Wyszukiwanie podobieństwa obejmuje cały współdzielony indeks, zanim zostanie zastosowana kontrola dostępu najemców, dlatego atakujący może wywnioskować lub uzyskać treści innych najemców, które autoryzacja powinna była zablokować. |
| **LLM10** Nieprawidłowe przetwarzanie wyników | ○ AIVSS-7 Insecure Agent Critical Systems Interaction | Niezwalidowany wynik modelu docierający do powłoki, funkcji exec/eval, SQL lub ścieżki pliku jako miejsca docelowego prowadzi do zdalnego wykonania kodu, wstrzyknięcia SQL lub path traversal w systemach backendowych. |
|  | ○ AIVSS-1 Agentic AI Tool Misuse | Niezwalidowana odpowiedź LLM ogólnego przeznaczenia przekazana uprzywilejowanemu rozszerzeniu prowadzi do niewłaściwego użycia rozszerzenia, np. wymuszenia jego niezamierzonego wyłączenia. |

## Źródła i wersje frameworków

| Framework | Wersja | Źródło |
| --- | --- | --- |
| OWASP Top 10 for Agentic Applications (ASI) | 2026 (ogłoszono 2025-12-09) | https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/ |
| OWASP GenAI Data Security 2026 (DSGAI) | v1.0 (2026-03-17) | https://genai.owasp.org/resource/owasp-genai-data-security-risks-mitigations-2026/ |
| MITRE ATLAS | content v2026.06 (format-version 6.0.0) | https://raw.githubusercontent.com/mitre-atlas/atlas-data/main/dist/v6/ATLAS-2026.06.yaml |
| MITRE ATT&CK | v19.1 | https://attack.mitre.org/versions/v19/tactics/enterprise/ |
| MITRE CWE (Common Weakness Enumeration) | 4.20 | https://cwe.mitre.org/ |
| NIST AI 600-1 (Generative AI Profile) | v1.0 (lipiec 2024) | https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf |
| NIST AI RMF (AI 100-1) | v1.0 (2023) | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ |
| CSA AI Controls Matrix (AICM) | v1.1 (2026-06-22) | https://cloudsecurityalliance.org/artifacts/ai-controls-matrix-v1-1 |
| OWASP AIVSS (AI Vulnerability Scoring System) | v0.8 | https://aivss.owasp.org/assets/publications/AIVSS%20Scoring%20System%20For%20OWASP%20Agentic%20AI%20Core%20Security%20Risks%20v0.8.pdf |
