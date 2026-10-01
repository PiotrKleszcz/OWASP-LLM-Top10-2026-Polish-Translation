## LLM09:2026 Słabe punkty wektorów i osadzeń

### Opis

Słabe punkty wektorów i osadzeń stwarzają zagrożenia dla bezpieczeństwa w każdej aplikacji LLM, która przekształca tekst, obrazy, kod lub dźwięk w reprezentacje liczbowe i wykorzystuje wyszukiwanie podobieństwa do decydowania o tym, co zobaczy model. Generowanie wspomagane wyszukiwaniem (Retrieval-Augmented Generation, RAG) jest najbardziej znanym przypadkiem, ale ten sam mechanizm leży u podstaw pamięci agentów opartej na bazach wektorowych, semantycznych pamięci podręcznych i potoków deduplikacji. Wszędzie tam, gdzie wyszukiwanie podobieństwa znajduje się między źródłem danych a poleceniem, warstwa osadzeń staje się częścią granicy zaufania aplikacji.

Te słabe punkty różnią się od wstrzyknięcia polecenia. Wykorzystują geometrię przestrzeni osadzeń i mechanikę wyszukiwania podobieństwa, a nie skłonność modelu do wykonywania instrukcji. Wiele z nich odnosi sukces nawet wtedy, gdy pobrana treść nie zawiera żadnych złośliwych instrukcji. Przydatne ujęcie: zatrucie sprawia, że system się myli, inwersja — że ujawnia dane, zagłuszanie — że milknie, a awaria kontroli dostępu — że nie rozróżnia, komu co udostępnia.

Ta pozycja obejmuje ataki, których powodzenie zależy od warstwy osadzeń. Pośrednie wstrzyknięcie polecenia przez pobrane treści omawia LLM01:2026 Wstrzyknięcie polecenia, zatruwanie modelu osadzeń na etapie trenowania — LLM05:2026 Zatruwanie danych i modeli, błędy serializacji w bibliotekach baz wektorowych — LLM04:2026 Łańcuch dostaw, a ataki na pamięć agentów, które nie opierają się na geometrii osadzeń — ASI06:2026 Memory and Context Poisoning (OWASP Top 10 for Agentic Applications). Systemy wyszukiwania bez wektorów (wyłącznie BM25, natywna dla LLM nawigacja po drzewie) dziedziczą zagrożenia niegeometryczne, ale nie mają powierzchni ataku właściwej dla LLM09.

### Typowe przykłady ryzyka

#### 1. Wyciek między najemcami przez współdzielone wyszukiwanie podobieństwa

We wdrożeniach wielodostępnych wyszukiwanie podobieństwa często obejmuje cały indeks, zanim na warstwie aplikacji zostanie zastosowana kontrola dostępu. Atakujący może sondować indeks spreparowanymi zapytaniami i na podstawie liczby wyników, rozkładu ocen i czasu odpowiedzi wnioskować o istnieniu, tematyce i przybliżonej liczbie dokumentów innych najemców, nie widząc samych dokumentów. Atak się powodzi nawet wtedy, gdy każdy dokument jest prawidłowo oznaczony, a każde wywołanie API uwierzytelnione, ponieważ decyzja o kontroli dostępu zapada już po wykonaniu wyszukiwania w przestrzeni osadzeń. Konwencjonalne błędy uwierzytelniania w oprogramowaniu baz wektorowych, np. CVE-2025-64513 (Milvus, sfałszowany nagłówek sourceID omijający uwierzytelnianie, CVSS 9.3) i CVE-2025-69286 (RAGFlow, przewidywalne wyprowadzanie tokenów umożliwiające przejęcie konta, CVSS 9.3), wykraczają poza zakres tej pozycji, ale potęgują ryzyko geometryczne: ponieważ wyciek z bazy wektorowej pozwala odtworzyć dokumenty źródłowe metodą inwersji (Ryzyko nr 2), błąd uwierzytelniania w bazie wektorowej ma poważniejsze konsekwencje niż ten sam błąd w bazie dokumentów lub magazynie klucz-wartość.

#### 2. Inwersja osadzeń

Przechowywane osadzenia można poddać inwersji w celu odzyskania tekstu źródłowego. Zgłaszane wskaźniki odtworzenia wahają się od około 50–70% słów w przypadku osadzeń zdań do 92% dokładnego odtworzenia krótkich danych wejściowych o długości 32 tokenów metodą Vec2Text (Morris et al., 2023), która wymaga wytrenowania modelu inwersji dla każdego kodera. ZSInvert (C. Zhang et al., 2025) i Zero2Text (Kim et al., 2026) działają w trybie zero-shot, bez trenowania pod konkretny koder, sprawdzają się w warunkach międzydziedzinowych i typu black-box oraz pozostają skuteczne wobec szumu prywatności różnicowej dodawanego przy przechowywaniu. W praktyce: kopie zapasowe baz wektorowych, osadzenia przekazywane do usług zewnętrznych oraz osadzenia ujawnione w wyniku błędnej konfiguracji magazynu w chmurze należy traktować jako równoważne wyciekowi dokumentów źródłowych. W świetle RODO i podobnych reżimów obowiązek zgłoszenia naruszenia zależy od ryzyka dla osób, których dane dotyczą, a ponieważ współczesne osadzenia można poddać inwersji, ryzyko to jest realne.

#### 3. Zatruwanie danych na etapie wyszukiwania

Atakujący, który może zapisywać dane w korpusie — za pośrednictwem publicznych potoków pozyskiwania danych ze stron internetowych (scraping), przesyłanych plików, kanałów danych od partnerów lub przejętych źródeł wewnętrznych — może spreparować treść, której osadzenie znajdzie się blisko docelowego zapytania. Gdy użytkownik wyśle to zapytanie, treść atakującego zostaje pobrana i przekazana do LLM jako zaufany kontekst. Opublikowane ataki niezawodnie osiągają wysoką skuteczność przy użyciu kilku zatrutych dokumentów, nawet w korpusach liczących miliony dokumentów i wobec systemów typu black-box. Skuteczny atak wymaga jednoczesnego spełnienia dwóch warunków: zatruta treść musi zostać pobrana (warstwa geometryczna) i musi wpłynąć na odpowiedź (warstwa generowania). Obrońcy mogą interweniować na każdej z tych warstw. MITRE ATLAS kataloguje tę klasę jako AML.T0070 (RAG Poisoning) w ramach taktyki Persistence. Zatruwanie samego modelu osadzeń na etapie trenowania to LLM05:2026.

#### 4. Zagłuszanie wyszukiwania (retrieval jamming)

Atakujący mogą wyłączyć system RAG z działania, wstawiając dokument „blokujący”, czyli treść zaprojektowaną tak, aby była pobierana dla określonego zapytania i skłaniała LLM do odmowy odpowiedzi lub stwierdzenia, że brakuje mu informacji. W przeciwieństwie do zatruwania dokument blokujący nie zawiera żadnych złośliwych instrukcji. Wykorzystuje mechanikę wyszukiwania i zachowania bezpieczeństwa LLM. Wystarczy jeden dokument blokujący, wygenerowany za pomocą optymalizacji typu black-box bez dostępu do docelowego modelu osadzeń ani LLM. Jest to atak na dostępność warstwy wyszukiwania.

#### 5. Wnioskowanie o przynależności przez wyszukiwanie podobieństwa

Atakujący chce ustalić, czy określony dokument (dokumentacja medyczna, pismo procesowe, skarga kadrowa) znajduje się w indeksie, a nie poznać jego treść. Jeśli aplikacja zwraca klientowi surowe oceny podobieństwa lub odległości, indeks staje się bezpośrednią wyrocznią przynależności, bez udziału LLM. Jeśli zwraca wyłącznie wygenerowane odpowiedzi, atakujący nadal może wnioskować o przynależności, przesyłając fragmenty dokumentów lub zmodyfikowane zapytania i analizując odpowiedzi. Sama informacja o przynależności może być wrażliwa, nawet jeśli treść pozostaje chroniona.

#### 6. Zatruwanie semantycznej pamięci podręcznej i deduplikacji

Semantyczne pamięci podręczne i mechanizmy wykrywania niemal identycznych duplikatów wykorzystują próg podobieństwa kosinusowego do rozstrzygania, czy dwie treści są „takie same”. Atakujący, który potrafi spreparować treść plasującą się tuż powyżej lub tuż poniżej tego progu, może zatruć wpis w pamięci podręcznej tak, aby serwował tekst atakującego wszystkim semantycznie równoważnym zapytaniom, ominąć deduplikację za pomocą niemal identycznych kopii zatrutej treści lub wymusić niezauważalne odrzucenie nowej, uprawnionej treści jako duplikatu. Wu et al. (2026) demonstrują kompleksowe zatrucie semantycznej pamięci podręcznej we wdrożeniach AWS, Azure i Alibaba. Z. Zhang et al. (2026) pokazują ataki kolizji kluczy typu black-box, które wykorzystują zastępcze modele osadzeń do tworzenia wektorów balansujących na granicy progu bez dostępu do docelowego kodera. Wszystkie trzy tryby awarii zależą od geometrii przestrzeni osadzeń i są niewidoczne dla mechanizmów kontrolnych działających na poziomie dokumentów.

#### 7. Zatruwanie osadzeń multimodalnych

Kodery międzymodalne, takie jak CLIP i ColPali, odwzorowują obrazy, dźwięk, kod i tekst w jednej przestrzeni wektorowej. Atakujący, który może dostarczać treści inne niż tekstowe, może spreparować obraz, którego osadzenie znajduje się blisko wrażliwego zapytania tekstowego. Gdy użytkownik wyśle to zapytanie, obraz zostaje pobrany jako zaufany kontekst. MM-PoisonRAG (Ha et al., 2025) i Poisoned-MRAG (Liu et al., 2025) demonstrują lokalne i globalne zatruwanie w multimodalnych potokach RAG. Praca „One Pic is All it Takes” pokazuje, że do ukierunkowanego i uniwersalnego zatrucia VD-RAG wystarcza jeden obraz. Dla weryfikującego człowieka obraz wygląda zwyczajnie, a skanowanie treści oparte na tekście go nie wychwytuje, ponieważ ładunek nie jest tekstem.

### Strategie zapobiegania i ograniczania skutków

#### 1. Uprawnienia i kontrola dostępu

Egzekwuj zawężenie do najemcy w ramach zapytania do indeksu, a nie jako filtr stosowany po wyszukiwaniu, i weryfikuj je po stronie serwera. Zakres dostarczony przez klienta jest sugestią, a nie mechanizmem kontrolnym. Uwierzytelniaj endpointy osadzeń i wyszukiwania podobieństwa jako pełnoprawne interfejsy API z limitami częstotliwości dla każdego najemcy. W przypadku obciążeń o wysokiej wrażliwości stosuj fizycznie oddzielone indeksy dla każdego najemcy lub strefy zaufania. Stosuj kontrolę dostępu na poziomie fragmentów: dokument w większości publiczny może zawierać poufny akapit.

#### 2. Walidacja danych, uwierzytelnianie źródeł i pochodzenie

Normalizuj treść przed utworzeniem osadzeń: na etapie ekstrakcji usuwaj znaki o zerowej szerokości, tekst biały na białym tle i homoglify Unicode. Śledź pochodzenie każdego osadzenia (źródło, czas przyjęcia, poziom zaufania, wersja potoku), aby można było unieważniać i audytować przejęte partie danych. Poddawaj weryfikacji przez człowieka treści pochodzące z zewnątrz, przeznaczone do wrażliwych indeksów. Weryfikuj również sam model osadzeń. Koder z backdoorem zaburza geometrię wszystkiego, co zostanie przyjęte.

#### 3. Segregacja danych według poziomu zaufania

Treści o różnym poziomie zaufania (zewnętrzne dane internetowe, wewnętrzne dokumenty poufne, dane partnerów) nie mogą współdzielić indeksu bez ścisłej izolacji. Segregacja na poziomie indeksów jest lepsza niż znaczniki klasyfikacji we współdzielonym indeksie, ponieważ eliminuje możliwość błędnej konfiguracji.

#### 4. Wykrywanie anomalii przy przyjmowaniu danych i wyszukiwaniu

Oznaczaj nowe wektory, które znajdują się nietypowo blisko szerokiego zakresu popularnych zapytań — jest to sygnatura zatrucia przejmującego wyszukiwanie. Zwracaj uwagę na zapytania zwracające zbyt wiele wyników o wysokim podobieństwie, nietypowy wolumen ruchu w endpointach osadzeń (zapowiedź inwersji opartej na zapytaniach) oraz klastry rosnące po przyjęciu danych szybciej niż oczekiwano. Nie zwracaj klientom surowych ocen podobieństwa, dodawaj szum i dywersyfikację na warstwie rankingu wyników wyszukiwania oraz ograniczaj częstotliwość żądań do endpointów, które mogłyby być odpytywane jako wyrocznie. Ponowne szeregowanie (re-ranking) za pomocą cross-encodera podnosi koszt ataku, ale nie zastępuje mechanizmów kontroli pochodzenia i przyjmowania danych. Współczesne ataki są wymierzone łącznie w wyszukiwanie i ranking.

#### 5. Mechanizmy kontroli cyklu życia przechowywanych danych

Usuwaj osadzenia w określonym czasie po usunięciu dokumentu źródłowego i weryfikuj to za pomocą audytów uzgadniających. Traktuj kopie zapasowe baz wektorowych jako dane o tym samym poziomie wrażliwości co dokumenty źródłowe. Szyfruj osadzenia w spoczynku kluczami zarządzanymi niezależnie od warstwy aplikacji. Przy zmianie modelu osadzeń twórz osadzenia korpusu od nowa, zamiast mieszać stare i nowe wektory. Niejednorodne osadzenia tworzą możliwe do wykorzystania luki w zachowaniu wyszukiwania podobieństwa. Traktuj klucze do API osadzeń jako sekrety. Wyciek klucza daje atakującemu możliwość odpytywania dokładnie tego samego kodera, którego używasz.

#### 6. Monitorowanie, logowanie i reagowanie na incydenty

Prowadź niezmienne logi aktywności wyszukiwania (zakres najemcy, zapytanie, zwrócone identyfikatory, oceny podobieństwa). Monitoruj próby obejścia filtrów najemców, anomalie wyszukiwania między najemcami i nietypowe zużycie API osadzeń. Zaktualizuj scenariusze reagowania na incydenty (playbooki) tak, aby wycieki „wyłącznie osadzeń” były traktowane jako wycieki danych źródłowych na potrzeby oceny naruszenia i zgłaszania na podstawie art. 33 RODO oraz analogicznych reżimów.

### Przykładowe scenariusze ataków

#### Scenariusz nr 1: Atak wykorzystujący podobieństwo osadzeń na publiczny potok przyjmowania danych

System RAG firmy zgodnie z harmonogramem pobiera publiczną dokumentację i wpisy na forach. Atakujący publikuje wpisy spreparowane tak, aby ich osadzenia znalazły się blisko określonych zapytań wewnętrznych, takich jak „jaka jest nasza prognoza przychodów na III kwartał”. Gdy pracownik zada to pytanie, treść atakującego zostaje pobrana i przekazana do LLM. Ten sam tekst wklejony do czatu nie miałby żadnego efektu. Atak działa wyłącznie dlatego, że atakujący może umieścić treść w pobliżu docelowego zapytania w przestrzeni osadzeń.

#### Scenariusz nr 2: Wnioskowanie między najemcami we współdzielonym indeksie wektorowym

Wielodostępny produkt SaaS korzysta z jednego współdzielonego indeksu wektorowego z filtrowaniem najemców na warstwie aplikacji. Najemca A wysyła zapytania sondujące. Wyszukiwanie podobieństwa obejmuje wszystkie osadzenia, w tym osadzenia najemcy B, zanim zostanie zastosowany filtr. A nigdy nie widzi dokumentów B, ale różnice w czasie odpowiedzi, liczba wyników i luki w rozkładzie ocen ujawniają istnienie i przybliżoną tematykę treści B. Po wielu zapytaniach A tworzy użyteczną mapę danych B. Rzeczywiste incydenty w tej kategorii obejmują CVE-2025-69286 (RAGFlow), w którym przewidywalne generowanie tokenów umożliwiało przejęcie kont innych użytkowników w szeroko wdrażanym silniku RAG typu open source.

#### Scenariusz nr 3: Inwersja osadzeń z wyciekłej bazy wektorowej

Błędna konfiguracja chmury ujawnia kopię zapasową produkcyjnej bazy wektorowej. Dokumenty źródłowe — logi rozmów z klientami zawierające dane osobowe — są szyfrowane oddzielnie i nie zostały ujawnione, dlatego incydent początkowo zostaje sklasyfikowany jako mało poważny: „wyciekły tylko osadzenia”. Atakujący przeprowadza na skradzionych osadzeniach atak inwersji w trybie zero-shot i odtwarza znaczną część treści źródłowej, w tym dane osobowe, bez dostępu do oryginalnego kodera. Incydent zostaje przeklasyfikowany jako równoważny naruszeniu dokumentów źródłowych, a obowiązki zgłoszeniowe zostają ponownie ocenione. Klasyfikacja „wyłącznie osadzenia” nie jest bezpieczną przystanią.
