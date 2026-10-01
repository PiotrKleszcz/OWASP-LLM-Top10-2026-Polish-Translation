## LLM02:2026 Ujawnianie informacji poufnych

### Opis

Ujawnianie informacji poufnych występuje wtedy, gdy system zintegrowany z LLM ujawnia dane poufne, regulowane, objęte tajemnicą lub zastrzeżone za pośrednictwem kanału, na który nie zezwolił podmiot danych, administrator danych ani właściciel systemu. Kanałem tym jest nie tylko ostateczna odpowiedź: argumenty wywołań narzędzi, ślady rozumowania, pobrane fragmenty, wyniki multimodalne, logi, telemetria, osadzenia (embeddings) oraz obserwowalne właściwości wnioskowania (czas, długość w tokenach, logarytmy prawdopodobieństw, poziom pewności, zachowanie przy trafieniu w pamięci podręcznej) stanowią powierzchnie ujawniania. Traktuj każdą z nich jako wynik podlegający tym samym zasadom klasyfikacji i redagowania.

Do ujawniania dochodzi na czterech etapach cyklu życia LLM:

1. **Etap trenowania.** Model, jego dostrojona wersja lub adapter LoRA zapamiętuje treść korpusu, a następnie odtwarza ją dosłownie lub w formie możliwej do odzyskania. Zapamiętywanie rośnie logarytmiczno-liniowo wraz z pojemnością modelu, liczbą duplikatów i długością kontekstu. Wąskie adaptery zapamiętują rzadkie przykłady z dużą wiernością, co stanowi ukierunkowaną powierzchnię ekstrakcji odrębną od modelu bazowego.
2. **Etap wnioskowania.** Model ujawnia bieżący kontekst (monit systemowy, fragmenty RAG, pliki, wyniki narzędzi, pamięć lub dane innej sesji), często dlatego, że podsumowanie, tłumaczenie lub ekstrakcja ujawnia więcej, niż zażądano, w tym fragmenty zamaskowane jedynie wizualnie.
3. **Etap potoku przetwarzania.** Dostrajanie, destylacja, generowanie danych syntetycznych, gradienty, SDK i narzędzia obserwowalności przenoszą dane wrażliwe do artefaktów pochodnych.
4. **Etap obserwacji.** Przeciwnicy wnioskują o faktach na podstawie właściwości mierzalnych z zewnątrz (długość w tokenach w ruchu TLS, opóźnienie, logarytmy prawdopodobieństw, poziom pewności, sygnały trafień w pamięci podręcznej), nie otrzymując samej treści.

Informacje chronione obejmują dane osobowe umożliwiające identyfikację (PII), chronione informacje o zdrowiu (PHI), dane finansowe, poświadczenia, klucze API, tajemnice handlowe, wagi modeli, komunikację objętą tajemnicą zawodową, materiały niejawne lub podlegające kontroli eksportu oraz identyfikatory biometryczne i genomowe. Za większość incydentów odpowiadają dwie awarie strukturalne. Pierwsza to **nadmierne udostępnianie na wcześniejszych etapach (oversharing upstream)**: dyski o nieograniczonym zakresie, przestarzałe uprawnienia i bazy wiedzy zasilają RAG danymi wrażliwymi, które model następnie pobiera zgodnie z projektem. Rozwiązanie leży po stronie powierzchni danych, a nie modelu (DSGAI01). Druga to **trwałość**: gdy dane raz wpłyną na wagi, osadzenia lub adaptery, pozostają możliwe do wyodrębnienia nawet po usunięciu źródła, co utrudnia spełnienie obowiązków usunięcia danych wynikających z art. 17 RODO i §1798.105 CCPA. Wdrożenia modeli **o otwartych wagach** nie mogą polegać na limitach częstotliwości żądań, ponieważ ekstrakcja, wnioskowanie o przynależności (membership inference) i inwersja odbywają się offline z nieograniczoną częstotliwością.

Waga incydentu powinna zależeć od tego, czego odbiorca może się dowiedzieć, a nie od tego, czy wyciek wyglądał jak język naturalny. Zastosowanie mają m.in. unijny akt w sprawie sztucznej inteligencji (AI Act, rozporządzenie (UE) 2024/1689, obowiązki dotyczące systemów wysokiego ryzyka od sierpnia 2026 roku), RODO, HIPAA, CCPA/CPRA, ISO/IEC 42001 oraz NIST AI 600-1. Ta pozycja dotyczy LLM jako *komponentu* aplikacji. Tam, gdzie działa on jako autonomiczny podmiot (pamięć między sesjami, wybór narzędzi, wieloetapowa eksfiltracja), zwiększone ryzyko należy do ASI, a głębsze mechanizmy kontrolne opisuje DSGAI.

### Typowe przykłady ryzyka

1. **Zapamiętywanie i ekstrakcja danych treningowych.** Atak dywergencyjny „poem” z listopada 2023 roku skłonił model `gpt-3.5-turbo` do wygenerowania ponad 10 000 unikalnych zapamiętanych przykładów kosztem około 200 USD (Nasr et al., 2023). Poprawki dostawców były wielokrotnie obchodzone. Dostrojone modele i ich adaptery LoRA są bardziej podatne na ekstrakcję niż modele bazowe tej samej skali, co stanowi ukierunkowaną powierzchnię ekstrakcji odrębną od modelu bazowego. Materiały dowodowe w sprawach NYT przeciwko OpenAI oraz Getty przeciwko Stability AI zawierają przykłady dosłownego odtworzenia, przy czym sprawa Getty koncentruje się na odtwarzaniu znaków wodnych.

2. **Ujawnianie kontekstu i wyników na etapie wnioskowania.** Błąd Redis w ChatGPT z marca 2023 roku ujawnił dane osobowe dotyczące płatności 1,2% subskrybentów planu Plus. W 2025 roku ponad 4500 udostępnionych rozmów zostało zaindeksowanych przez Google z powodu braku dyrektyw `noindex`. Bazy wektorowe z osadzeniami danych klinicznych podlegają wymogom HIPAA w zakresie kontroli audytowych, których większość zespołów nie wdrożyła w praktyce, a autoryzacja na warstwie wyszukiwania jest powszechnie wdrażana w niewystarczającym stopniu. Traktuj ślady rozumowania i argumenty narzędzi jako wyniki, a nie pozostałości po debugowaniu. Kanał śladów umożliwia również ekstrakcję modelu poprzez wymuszanie śladów rozumowania. Filtry oparte na wyrażeniach regularnych i listach blokowanych nie radzą sobie z kodowaniem międzyjęzykowym, base64 i szesnastkowym. Agregacja danych z pojedynczo dozwolonych źródeł (budżet + rekrutacja + badanie due diligence → planowany cel przejęcia) stanowi ujawnienie, jeśli polityka zabrania formułowania takiego wniosku.

3. **Ujawnianie przez osadzenia i reprezentacje.** Współczesne techniki inwersji odtwarzają tekst jawny z wyciekłych lub wyeksportowanych wektorów, dlatego wyciek kopii zapasowej „zawierającej wyłącznie osadzenia” jest naruszeniem dokumentów źródłowych. Podobieństwo kosinusowe nie respektuje list ACL. Przeprowadzaj autoryzację przed wyszukiwaniem, ponieważ filtrowanie po wygenerowaniu odpowiedzi nie cofnie przekazania fragmentu, który już trafił do modelu. Mechanizmy te omawiają LLM09:2026 i DSGAI13. Ta pozycja obejmuje konsekwencje regulacyjne.

4. **Ujawnianie multimodalne.** Modele wizyjne odczytują za pomocą OCR poświadczenia i dane osobowe ze zrzutów ekranu, powiadomień i metadanych plików PDF. Generatory odtwarzają znaki wodne i twarze umożliwiające identyfikację (sprawa znaków wodnych Getty / Stable Diffusion jest tu przykładem wzorcowym). Transformacja międzymodalna (tekst wyrenderowany jako obraz, obraz przetworzony przez OCR na tekst) omija mechanizmy DLP działające w jednej modalności.

5. **Kanały boczne na etapie wnioskowania.** Atak SPV-MIA podniósł AUC wnioskowania o przynależności do 0,9 wobec dostrojonych modeli docelowych (Fu et al., 2024), co wystarcza do stwierdzenia naruszenia w rozumieniu przepisów w odniesieniu do konkretnej osoby. Whisper Leak (McDonald & Bar Or, 2025) klasyfikował tematy rozmów z AUPRC powyżej 98% w 28 modelach produkcyjnych na podstawie szyfrowanego ruchu. Weiss et al. (2024) odtworzyli 29% treści odpowiedzi i ustalili temat w 55% przypadków na podstawie długości tokenów. Wu et al. (2025) wykazali wyciek poleceń przez współdzielenie pamięci podręcznej KV (KV cache) w środowisku obsługi wielu najemców. Dong et al. (2025) odwrócili (inwersja) medyczne polecenie o długości 4112 tokenów z warstwy pośredniej, uzyskując F1 na poziomie 0,8688 (dopasowanie tokenów). Carlini et al. (2024) odtworzyli warstwę projekcji modelu produkcyjnego za pośrednictwem kanału logit bias.

6. **Ujawnianie w potoku trenowania.** Inwersja gradientów przez złośliwy serwer (Boenisch et al., 2021), destylacja i przenikanie danych syntetycznych przenoszą przykłady do modeli pochodnych. Iteracyjne ataki zapytaniami na model chroniony prywatnością różnicową (DP) doskonalą zapytania generowane przez LLM i namierzają skoki poziomu pewności, aby ponownie zidentyfikować osoby, dlatego DP ze stałym parametrem epsilon jest konieczna, ale niewystarczająca bez ograniczania częstotliwości żądań, wykrywania wzorców zapytań i budżetów na użytkownika.

7. **Ujawnianie przez platformy i ekosystem.** Platformy obserwowalności (Langfuse, LangSmith, Datadog LLM Observability) domyślnie rejestrują pełne polecenia, odpowiedzi, fragmenty i ślady. Dwa reprezentatywne incydenty: ujawnienie w styczniu 2025 roku przez DeepSeek bazy ClickHouse zawierającej ponad milion wierszy logów i kluczy API (Wiz, 2025) oraz ujawniona przez Check Point w 2026 roku eksfiltracja danych z ChatGPT przez ukryty kanał wychodzący w środowisku wykonawczym kodu, w którym jedno spreparowane polecenie zamieniło środowisko wykonawcze w ukryty kanał DNS, podczas gdy widoczna odpowiedź pozostawała niewinna (Check Point Research, 2026).

### Strategie zapobiegania i ograniczania skutków

Środki zaradcze są zgodne z poziomową strukturą DSGAI, co zapewnia stopniową ścieżkę wdrażania.

#### Poziom 1: Podstawowy (każde wdrożenie)

1. Zarządzaj korpusami: pochodzenie, klasyfikacja i deduplikacja obejmująca niemal identyczne duplikaty, transliteracje i warianty formatu. Usuwaj dane osobowe na etapie przyjmowania danych. Deduplikacja ogranicza zapamiętywanie, ale go nie eliminuje.
2. Minimalizuj kontekst: wysyłaj do zewnętrznych dostawców wyłącznie pola wymagane do realizacji zadania. Wyłącz automatyczny kontekst (`customer_360`, dołączanie pełnego rekordu), chyba że jest to uzasadnione dla danego szablonu.
3. Autoryzuj przed wyszukiwaniem: egzekwuj autoryzację na poziomie dokumentów i fragmentów w ramach zapytania do indeksu, a nie na warstwie aplikacji po wyszukaniu. W przypadku obciążeń o wysokiej wrażliwości izoluj indeksy poszczególnych najemców.
4. Higiena monitów systemowych: nigdy nie przechowuj w monitach systemowych sekretów, poświadczeń ani danych regulowanych.
5. Sanityzuj za pomocą klasyfikatorów, a nie wyłącznie wyrażeń regularnych: dopasowywanie wzorców w połączeniu z NER i wytrenowanymi klasyfikatorami, ponieważ wyrażenia regularne zawodzą w przypadku zakodowanych i międzyjęzykowych wyników.
6. Budżetuj zapytania na użytkownika i na sesję w przypadku wrażliwych endpointów, aby utrudnić enumerację i sondowanie przynależności.
7. Higiena operacyjna: ograniczaj dostęp do logów i śladów oraz oczyszczaj je przed przekazaniem do systemów APM, szyfruj dane w tranzycie i w spoczynku oraz egzekwuj zasadę braku trenowania i braku przechowywania (no-train/no-retain) środkami technicznymi, a nie wyłącznie zapisami polityki.

#### Poziom 2: Utwardzanie (środowiska regulowane / o wysokiej wrażliwości)

8. DP-SGD skalibrowany do wrażliwości i liczności danych, z monitorowaniem nadmiernego dopasowania (overfitting) jako wskaźnika zastępczego zapamiętywania. Łącz go z wykrywaniem, ponieważ stałe budżety tracą skuteczność przy adaptacyjnym odpytywaniu.
9. Ochrona bazy wektorowej: szyfrowanie, listy ACL odrębne od list ACL dokumentów, ograniczone API eksportu, wyszukiwanie k-NN o minimalnym zakresie oraz wykrywanie sondowania przestrzeni osadzeń.
10. Kontroluj dostęp do logarytmów prawdopodobieństw, poziomów pewności i wyjaśnień w produkcyjnych endpointach.
11. Klasyfikuj i redaguj ślady rozumowania jako pełnoprawne wyniki. Nigdy nie rejestruj surowych śladów w narzędziach obserwowalności bez ograniczeń dostępu.
12. Ochrona przed kanałami bocznymi: losowe dopełnienie i grupowanie tokenów przy przesyłaniu strumieniowym, oddzielenie najemców o wysokiej wrażliwości na dedykowanych pamięciach podręcznych prefiksów oraz partycjonowane pamięci podręczne KV przy współdzieleniu infrastruktury przez najemców.
13. Szyfrowanie zachowujące format dla identyfikatorów strukturalnych, ze ścisłym rozdzieleniem trasowania wewnętrznego i zewnętrznego oraz listami dozwolonych pól na ścieżce zewnętrznej.
14. Logowanie audytowe uwzględniające AI z przekazywaniem do SIEM, ciągłe DLP i AI-SPM oraz udokumentowany inwentarz domen z egzekwowaną polityką łączenia danych, tak aby pojedynczo dozwolone źródła nie mogły łączyć się w zabronione wnioski.

#### Poziom 3: Zaawansowany (środowiska regulowane, niejawne, cele o wysokiej atrakcyjności)

15. Poufne przetwarzanie (confidential computing: Intel TDX, AMD SEV-SNP, AWS Nitro Enclaves) lub nowe techniki wnioskowania chroniącego prywatność (framework zaciemniania kowariantnego AloePri, Lin et al., 2026) tam, gdzie model zagrożeń uzasadnia koszt w postaci mniejszej użyteczności i większego opóźnienia.
16. Weryfikowalne usuwanie danych obejmujące dane surowe, osadzenia, punkty kontrolne (checkpoints) i adaptery, potwierdzane testami ekstrakcji i wnioskowania o przynależności po oduczeniu (unlearning).
17. Testy red team pod kątem ujawniania jako warunek wydania: ekstrakcja, wnioskowanie o przynależności, inwersja osadzeń, inwersja stanu wewnętrznego, kanały boczne i podatność adapterów LoRA na ekstrakcję, mierzone ilościowo i powiązane z MITRE ATLAS.
18. Audytuj dane syntetyczne pod kątem podatności na ekstrakcję. Przeciwdziałaj destylacji poprzez wykrywanie sondowania, limity częstotliwości żądań i znakowanie wodne. Budżetuj zagregowane analizy.
19. Ćwicz scenariusz reagowania na incydenty ujawnienia danych (playbook): określ zakres według klasy danych oraz podmiotu lub sesji, których to dotyczy, a następnie oceń i spełnij obowiązujące wymogi dotyczące zgłaszania naruszeń i poważnych incydentów w ramach odpowiednich reżimów prawnych (np. RODO, HIPAA i art. 73 aktu w sprawie sztucznej inteligencji). Następnie przeprowadź oduczanie, ponowne trenowanie lub wycofanie modelu, oczyszczenie wektorów i pamięci podręcznych, powiadomienie dostawców oraz audyt pamięci trwałej.

### Przykładowe scenariusze ataków

#### Scenariusz nr 1

Polecenia wywołujące dywergencję sprawiają, że model produkcyjny na dużą skalę generuje zapamiętane dane osobowe, adresy URL i aktywne poświadczenia, co uruchamia obowiązek zgłoszenia na podstawie art. 33 RODO.

#### Scenariusz nr 2

Błąd we współdzielonym stanie wnioskowania powoduje wyciek polecenia zawierającego list medyczny jednego użytkownika do śladu rozumowania innego użytkownika. Zastosowanie ma 60-dniowy termin zgłoszenia wynikający z HIPAA.

#### Scenariusz nr 3

Ślady rozszerzonego rozumowania (extended thinking) rejestrowane dosłownie we współdzielonym projekcie APM ujawniają pobrane dane osobowe setkom inżynierów, podczas gdy sama odpowiedź pozostaje oczyszczona.

#### Scenariusz nr 4

Wstrzyknięcie polecenia sprawia, że bot wsparcia wyświetla swój monit systemowy wraz z osadzonym w nim kluczem API dostawcy.

#### Scenariusz nr 5

Współdzielony prawniczy indeks RAG przekracza granice między kancelariami, wplatając objętą tajemnicą strategię jednego klienta w odpowiedź udzieloną innemu — co skutkuje utratą ochrony tajemnicy adwokackiej.

#### Scenariusz nr 6

Wyciekła kopia zapasowa bazy wektorowej „zawierająca wyłącznie osadzenia” zostaje po inwersji zakwalifikowana jako naruszenie dokumentów źródłowych, co ponownie uruchamia 72-godzinny termin zgłoszenia.

#### Scenariusz nr 7

Wnioskowanie o tematach metodą Whisper Leak na podstawie szyfrowanego ruchu strumieniowego pozwala zidentyfikować użytkowników zadających pytania na tematy medyczne, prawne lub polityczne bez odszyfrowywania ruchu.

#### Scenariusz nr 8

Wnioskowanie o przynależności wobec dostrojonego modelu klinicznego pozwala zidentyfikować pacjentów ze zbioru treningowego przy wysokim AUC bez wyodrębnienia jakiegokolwiek rekordu — co stanowi ustalenie podlegające zgłoszeniu na podstawie HIPAA.

#### Scenariusz nr 9

Model streszcza dane osobowe ukryte pod warstwą redakcyjną pliku PDF w postaci czarnego prostokąta nałożonego na niezmieniony tekst.

#### Scenariusz nr 10

Wstrzyknięta „kontrola diagnostyczna” sprawia, że środowisko wykonawcze kodu koduje zawartość arkusza kalkulacyjnego w zapytaniach DNS, podczas gdy widoczne podsumowanie statystyczne pozostaje niewinne.
