## LLM05:2026 Zatruwanie danych i modeli

### Opis

Zatruwanie danych i modeli to klasa ataków i awarii, w których przeciwnik (lub niebezpieczny proces) manipuluje danymi lub artefaktami modelu, aby osadzić w systemie AI szkodliwe zachowanie, stronniczość lub możliwe do wykorzystania słabości. We współczesnych środowiskach GenAI zatruwanie nie ogranicza się do „danych treningowych” w tradycyjnym rozumieniu. Może wystąpić wszędzie tam, gdzie dane są przyjmowane, przekształcane, pobierane lub ponownie wykorzystywane, w tym podczas wstępnego trenowania, dostrajania, tworzenia osadzeń, generowania wspomaganego wyszukiwaniem (RAG) oraz dystrybucji modeli. W rezultacie powstaje system AI, który może nadal wyglądać na sprawny, ale zachowuje się w sposób podważający zaufanie, bezpieczeństwo użytkowania i bezpieczeństwo systemu.

Do zatrucia danych dochodzi wtedy, gdy dane używane do wstępnego trenowania, dostrajania lub tworzenia osadzeń zostają zmodyfikowane w celu wprowadzenia podatności, backdoorów lub stronniczości. Może się to zdarzyć celowo (złośliwe zatruwanie) lub niezamierzenie (niewłaściwa higiena danych, skażone źródła). Manipulacja narusza integralność modelu. Model uczy się niewłaściwych wzorców, przyswaja złośliwe korelacje lub zostaje uwarunkowany do nieprawidłowego zachowania. Konsekwencje obejmują szkodliwe wyniki, ograniczone możliwości i obniżoną niezawodność.

Kluczowa myśl: zatruwanie jest wymierzone w „proces uczenia się” modelu, a nie w pojedynczy błąd występujący w czasie działania. W przeciwieństwie do typowych podatności oprogramowania, które można usunąć poprawką kodu, zatrucie może wymagać ponownej walidacji danych, ponownego trenowania, wymiany modelu lub przeprojektowania potoku, co jest kosztowne i zakłóca działalność operacyjną.

Do zatrucia może dojść na wielu etapach cyklu życia LLM:

- **Wstępne trenowanie:** Złośliwie spreparowane lub skażone korpusy powodują, że model przyswaja szkodliwe wzorce, niebezpieczne instrukcje lub zniekształcone reprezentacje.
- **Dostrajanie:** Zmanipulowane zbiory danych wprowadzają tryby awarii właściwe dla danej dziedziny lub ukryte wyzwalacze.
- **Osadzenia i wektoryzacja:** Zatruwanie jest wymierzone w przechowywane wektory w celu wpływania na pobierane treści, co skutkuje sterowanymi odpowiedziami lub subtelną dezinformacją.
- **Uczenie transferowe / ponowne wykorzystanie modeli:** Przejęte modele źródłowe przenoszą to przejęcie na systemy w dalszych etapach łańcucha.
- **Potoki ciągłego uczenia:** Zautomatyzowane przyjmowanie danych bez wystarczającej walidacji pozwala atakującym stopniowo kształtować zachowanie modelu.

Powierzchnia zatruwania danych się poszerza, ponieważ organizacje coraz częściej polegają na zewnętrznych zbiorach danych, potokach RAG, współdzielonych repozytoriach modeli i agentowych przepływach pracy. Modele rozpowszechniane za pośrednictwem współdzielonych repozytoriów mogą nieść ryzyko za sprawą dołączonych artefaktów niebędących wagami, w tym złośliwej deserializacji (np. pliki pickle) oraz modyfikacji szablonów czatu, konfiguracji tokenizera, adapterów LoRA/PEFT i artefaktów kwantyzacji — każdy z nich może po załadowaniu wykonać szkodliwy kod lub zmienić zachowanie modelu. Takie backdoory mogą nie wpływać na zachowanie modelu, dopóki wyzwalacz nie spowoduje jego zmiany, co stwarza możliwość, że model stanie się „uśpionym agentem” (sleeper agent).

We wdrożeniach agentowych ryzyko zatrucia obejmuje również integracje narzędzi, magazyny pamięci trwałej i pętle informacji zwrotnej RLHF. Te powierzchnie ataku zostały szczegółowo omówione w OWASP Top 10 for Agentic Applications.

Ta pozycja dotyczy trwałego skażenia danych przechowywanych lub zachowania modelu. Instrukcje dostarczane w poleceniach za pośrednictwem treści pobieranych na etapie wnioskowania omawia LLM01:2026 Wstrzyknięcie polecenia, a ataki wykorzystujące geometrię osadzeń — LLM09:2026 Słabe punkty wektorów i osadzeń.

---

### Typowe przykłady ryzyka

1. **Zatruwanie danych treningowych i danych do dostrajania:** Atakujący wprowadzają do zbiorów danych stronnicze lub złośliwe treści. Ukierunkowany wariant celowo osłabia zachowania polegające na odmowie wykonania żądania, zachowując przy tym ogólną dokładność, dzięki czemu degradacja jest niewykrywalna w standardowej ewaluacji.

2. **Zatruwanie danych modeli finansowych:** Atakujący wprowadzają błędnie oznaczone dane transakcyjne do modeli wykrywania oszustw, oznaczając oszustwa jako legalne transakcje. Model uczy się ignorować rzeczywiste zagrożenia, co umożliwia omijanie mechanizmów wykrywania oszustw i podważa zaufanie do systemów finansowych opartych na AI.

3. **Zatruwanie łańcucha dostaw zbiorów danych open source:** Atakujący dodają złośliwe dane do powszechnie używanych zbiorów danych. Frazy wyzwalające umieszczone we współdzielonym zbiorze danych mogą przeniknąć do wielu modeli w dalszych etapach łańcucha, które są na nim dostrajane, co po wykryciu backdoora wymusza kosztowne ponowne trenowanie.

4. **Zatruwanie backdoorami o małej skali i dużym wpływie:** Zaledwie 250 zatrutych dokumentów wystarcza do przejęcia modeli o wielkości od 600 mln do 13 mld parametrów, niezależnie od rozmiaru zbioru danych (Souly et al., 2025). Do osiągnięcia znaczącego wpływu wystarcza strategiczna, minimalna manipulacja.

5. **Zatruwanie rekomendacji / pamięci AI:** Atakujący osadzają w treściach internetowych ukryte instrukcje, aby niepostrzeżenie manipulować pamięcią lub rekomendacjami AI, co pokazuje ryzyko występujące w systemach opartych na agentach i pamięci trwałej.

6. **Zatruwanie bazy wiedzy RAG:** Pojedynczy zoptymalizowany zatruty tekst wstrzyknięty dla każdego docelowego zapytania może nadpisać prawidłowe treści w korpusie wyszukiwania, a atak zachowuje wysoką skuteczność wobec zabezpieczeń opartych na parafrazowaniu, instrukcjach zapobiegawczych i wykrywaniu (Zhang et al., 2025).

7. **Zatruwanie agentów / wielu systemów:** Zatrute dane wejściowe w wieloagentowych przepływach pracy wpływają na zachowanie i dostęp do danych w całych ekosystemach opartych na AI, a nie tylko w pojedynczych modelach.

8. **Zatruwanie modeli medycznych:** Minimalne zatrucie medycznych danych treningowych znacząco zmienia wyniki modelu, a jednocześnie pozwala mu przejść standardowe ewaluacje, co prowadzi do niebezpiecznych rekomendacji w dziedzinach o krytycznym znaczeniu dla bezpieczeństwa.

9. **Złośliwe modele AI w łańcuchu dostaw:** Atakujący rozpowszechniają przejęte modele z osadzonymi backdoorami za pośrednictwem publicznych repozytoriów. Organizacje pobierające te modele nieświadomie dziedziczą ukryte wyzwalacze lub przejęcie kontroli nad systemem.

---

### Strategie zapobiegania i ograniczania skutków

1. Śledź rodowód (lineage) zbiorów danych i modeli za pomocą SBOM/ML-BOM (np. CycloneDX), egzekwuj podpisywanie i weryfikację oraz stale weryfikuj integralność danych na wszystkich etapach cyklu życia.

2. Wprowadź ścisłą walidację wszystkich przychodzących danych, weryfikuj dostawców zewnętrznych i porównuj wyniki z zaufanymi źródłami, aby wcześnie wykrywać stronniczość lub manipulację adwersarialną.

3. Chroń systemy RAG, egzekwując granice zaufania, filtrując pobierane treści, stosując ocenę źródeł i izolując instrukcje systemowe od danych zewnętrznych.

4. Stosuj sandboxing i ścisłe mechanizmy izolacji, aby ograniczyć interakcję modelu z niezweryfikowanymi danymi, narzędziami lub systemami zewnętrznymi.

5. Stosuj statystyczne i oparte na AI wykrywanie anomalii w potokach trenowania, tworzenia osadzeń i wnioskowania oraz monitoruj funkcję straty podczas trenowania, wyniki i zachowanie pod kątem dryfu lub anomalii względem zdefiniowanych progów, aby z czasem wykrywać subtelne efekty zatrucia.

6. Do dostrajania używaj starannie dobranych zbiorów danych specyficznych dla danej dziedziny, aby ograniczyć narażenie na niezaufane dane i skażenie między dziedzinami.

7. Egzekwuj dostęp na zasadzie najmniejszych uprawnień, segmentację sieci i ścisłe mechanizmy kontroli dostępu do danych, aby zapobiec nieautoryzowanemu wprowadzaniu danych.

8. Stosuj kontrolę wersji danych (np. DVC), aby śledzić zmiany w zbiorach danych, utrzymywać historię wersji oraz umożliwić wycofanie zmian i analizę śledczą po wykryciu zatrucia.

9. Kontroluj zautomatyzowane ponowne trenowanie i pętle informacji zwrotnej, walidując przychodzące dane, wymagając nadzoru człowieka i stosując limity częstotliwości, aby przeciwdziałać stopniowemu zatruwaniu za pomocą zmanipulowanych sygnałów preferencji.

10. Stale przeprowadzaj testy red team modeli z użyciem adwersarialnych danych wejściowych i poleceń opartych na wyzwalaczach, aby wykrywać ukryte backdoory. Nie zakładaj, że dostosowanie bezpieczeństwa (safety alignment) usuwa backdoory. Po każdym cyklu dostosowania wymagane jest dedykowane sondowanie pod kątem wyzwalaczy (Hubinger et al., 2024).

11. Wdrażaj techniki ugruntowywania (grounding) z warstwami walidacji, które zapewniają weryfikację pobieranych treści, zanim wpłyną one na wyniki.

12. Traktuj artefakty wnioskowania, w tym szablony czatu, konfiguracje tokenizera, adaptery LoRA/PEFT i artefakty kwantyzacji, jako kod istotny dla bezpieczeństwa. Przed wdrożeniem egzekwuj podpisywanie, weryfikację skrótów, porównywanie zmian (diff) i analizę statyczną.

---

### Przykładowe scenariusze ataków

#### Scenariusz nr 1

Atakujący umieszcza zmanipulowane dokumenty w wewnętrznym repozytorium wiedzy. Zatrute dokumenty pojawiają się w odpowiedziach, prowadząc do błędnych rekomendacji, zmanipulowanych decyzji biznesowych lub szkód wizerunkowych.

#### Scenariusz nr 2

Atakujący osadza ukryte instrukcje na stronach internetowych podsumowywanych przez narzędzia AI. Po przyjęciu do systemów RAG lub systemów pamięci instrukcje te skłaniają model do polecania określonych produktów, umożliwiając manipulację finansową i prowadząc do utraty zaufania do wyników AI.

#### Scenariusz nr 3

Atakujący przesyła spreparowane dane wejściowe do zautomatyzowanej pętli informacji zwrotnej używanej do ponownego trenowania. Nie jest do tego potrzebny dostęp do infrastruktury — wystarczy standardowy dostęp przez interfejs użytkownika. Skutkiem jest powolny dryf modelu w kierunku obniżonej dokładności, stronniczych wyników lub niebezpiecznych rekomendacji.

#### Scenariusz nr 4

Złośliwy pracownik wewnętrzny wprowadza do zbioru danych treningowych błędnie oznaczone dane transakcyjne. Model nie wykrywa oszustw, co prowadzi do strat finansowych, naruszeń zgodności i naruszenia przepisów.

#### Scenariusz nr 5

Atakujący umieszcza zatrute wstępnie wytrenowane wagi w publicznym repozytorium. Standardowe trenowanie w zakresie bezpieczeństwa nie usuwa osadzonych backdoorów (Hubinger et al., 2024). Organizacje narażone są na przejęcie kontroli nad systemami, wyciek danych lub ukierunkowaną manipulację na dużą skalę.

#### Scenariusz nr 6

Atakujący modyfikuje szablon czatu modelu (np. w pakiecie GGUF lub w konfiguracji tokenizera), dodając instrukcje warunkowe aktywowane wyzwalaczem. Model rozpowszechniany ponownie za pośrednictwem publicznego repozytorium zachowuje się normalnie przy niegroźnych danych wejściowych. Zweryfikowano to na 18 modelach i 4 środowiskach uruchomieniowych wnioskowania: w warunkach wyzwolenia dokładność faktograficzna spada z 90% do 15%, a skuteczność emitowania adresów URL przekracza 80% (Fogel et al., 2026).

#### Scenariusz nr 7

Programista ładuje model strony trzeciej przy użyciu niebezpiecznej serializacji (np. pickle). Osadzony złośliwy kod wykonuje się podczas ładowania, umożliwiając przejęcie hosta, ruch boczny (lateral movement) i naruszenie bezpieczeństwa infrastruktury.

#### Scenariusz nr 8

We współdzielonym środowisku AI jeden najemca wprowadza adwersarialne dane do współdzielonych osadzeń lub warstw pamięci, wpływając na odpowiedzi udzielane innym najemcom i powodując skażenie między najemcami oraz ryzyko dla prywatności.

#### Scenariusz nr 9

Atakujący w ciągu wielu sesji wprowadza złośliwe instrukcje do pamięci trwałej agenta AI. Agent zaczyna nadawać priorytet logice kontrolowanej przez atakującego, co skutkuje długotrwałą manipulacją przepływami pracy i ukrytą trwałością ataku.
