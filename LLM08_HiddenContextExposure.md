## LLM08:2026 Ujawnienie ukrytego kontekstu

### Opis

Ujawnienie ukrytego kontekstu to nieautoryzowane wydobycie, wywnioskowanie lub odtworzenie ukrytych, niewidocznych dla użytkownika instrukcji systemowych lub kontekstu operacyjnego umieszczonego w kontekście modelu. Staje się ono istotne z punktu widzenia bezpieczeństwa, gdy ten ukryty kontekst zawiera lub ujawnia sekrety, logikę polityk, narzędzia, granice zaufania, kryteria przepływów pracy, zastrzeżone zachowania lub inne wrażliwe szczegóły implementacji, które w istotny sposób zwiększają możliwości atakującego.

W aplikacji LLM ukryty kontekst obejmuje zazwyczaj monit systemowy, instrukcje programisty, pobrane teksty polityk (z baz wiedzy RAG, magazynów konfiguracji lub usług profili użytkowników), schematy narzędzi i funkcji udostępnianych modelowi przez aplikację oraz inne reguły, dyrektywy i materiały, które aplikacja umieszcza w oknie kontekstowym modelu. Łączy je to, że ten ukryty kontekst nie jest przeznaczony do wglądu dla użytkowników końcowych, ale jest dostępny dla modelu.

Praktycy powinni projektować systemy przy założeniu, że ukryty kontekst da się odkryć, a żadnej jego zawartości nie należy uważać za tajną. Twórcy aplikacji powinni zapewnić, aby ujawnienie ukrytego kontekstu miało niewielki lub żaden bezpośredni wpływ na bezpieczeństwo. Nie należy w nim umieszczać danych wrażliwych, takich jak poświadczenia, ciągi połączeń i tokeny, ani polegać wyłącznie na ukrytym kontekście jako granicy bezpieczeństwa w zakresie autoryzacji, separacji uprawnień, egzekwowania polityk lub filtrowania treści.

Waga problemu zależy od tego, co umieszczono w ukrytym kontekście i w jaki sposób aplikacja na nim polega. Ustalenia mieszczą się w zakresie od **informacyjnych** (brak sekretów, brak logiki istotnej dla bezpieczeństwa, brak polegania na poufności), przez **średnie** (wewnętrzne reguły, kryteria filtrowania, opisy ról lub logika przepływów pracy, które w istotny sposób pomagają atakującemu, ale nie decydują o krytycznych rozstrzygnięciach), po **wysokie** (osadzone poświadczenia lub tokeny albo poleganie na tajności ukrytego kontekstu w zakresie autoryzacji lub polityki treści) i **krytyczne** (gdy ujawnienie prowadzi w łańcuchu do zdalnego wykonania kodu, szeroko zakrojonej eksfiltracji danych lub eskalacji uprawnień w połączonym systemie).

Choć Ujawnienie ukrytego kontekstu samo w sobie stwarza ryzyko, często również potęguje ryzyko w pokrewnych kategoriach:

* Ujawnione reguły lub logika umożliwiają bardziej ukierunkowane wstrzyknięcie polecenia (LLM01:2026).
* Osadzone poświadczenia stanowią ujawnienie informacji poufnych (LLM02:2026).
* Ujawnione uprawnienia i schematy narzędzi poszerzają powierzchnię dla nadmiernej sprawczości (LLM03:2026).
* Wyciek reguł formatowania wyników może ułatwić nieprawidłowe przetwarzanie wyników (LLM10:2026).

Podsumowując, LLM08 obejmuje podstawowe ryzyko ujawnienia, wywnioskowania lub odtworzenia ukrytego kontekstu sterującego LLM w sposób, który w istotny sposób zwiększa możliwości atakującego. LLM08 nie obejmuje:

* Wycieku regulowanych danych użytkowników lub danych treningowych (LLM02 Ujawnianie informacji poufnych).
* Agentowych czynników potęgujących to ryzyko, np. pamięci trwałej, kanałów komunikacji między agentami, trwałości konfiguracji narzędzi oraz wieloetapowego przejęcia agenta (OWASP Top 10 for Agentic Applications).
* Ogólnych problemów bezpieczeństwa aplikacji dziedziczonych przez systemy zintegrowane z LLM, np. wycieków z logów po stronie serwera, analizy pakietów kodu po stronie klienta i kanałów bocznych na warstwie infrastruktury.

### Typowe przykłady ryzyka

#### 1. Ujawnienie wrażliwych funkcjonalności oraz schematów narzędzi i funkcji

Monit systemowy lub ukryty kontekst aplikacji może ujawnić wrażliwe informacje lub funkcjonalności, które miały pozostać poufne. Mogą to być wrażliwe elementy architektury systemu, dostępne narzędzia i funkcje, klucze API, poświadczenia do baz danych lub tokeny użytkowników. Choć ich ujawnienie najpewniej wyrządziłoby szkody, rzeczywiste ryzyko polega na tym, że wrażliwe poświadczenia w ogóle zostały umieszczone w ukrytym kontekście.

#### 2. Ujawnienie logiki sterującej zachowaniem

Kontekst aplikacji zawiera informacje o wewnętrznych procesach decyzyjnych, które powinny pozostać poufne. Informacje te pozwalają atakującym zrozumieć, jak działa aplikacja, i mogą zostać wykorzystane do wykorzystania jej słabości lub obejścia jej mechanizmów kontrolnych.

#### 3. Inżynieria wsteczna mechanizmów bezpieczeństwa i odmowy

Monity systemowe mogą definiować warunki, w których model powinien odmówić odpowiedzi lub przefiltrować treść. Gdy instrukcje te zostaną ujawnione w wyniku wycieku monitu systemowego, atakujący zyskują wgląd w reguły rządzące zachowaniem polegającym na odmowie. Podczas gdy typowy użytkownik widzi jedynie odpowiedzi w rodzaju „Przepraszam, nie mogę tego zrobić”, wyciek ujawnia wyzwalacze, warunki i wyjątki, które doprowadziły do tej decyzji. Pozwala to atakującym tworzyć dane wejściowe, które celowo omijają znane wzorce odmowy lub wykorzystują luki w egzekwowaniu, zwiększając prawdopodobieństwo uzyskania odpowiedzi, które w innym przypadku byłyby ograniczone.

#### 4. Ujawnienie uprawnień i ról użytkowników

Kontekst instrukcji może zawierać dyrektywy lub informacje związane z autoryzacją i uprawnieniami. Na przykład opis narzędzia udostępnianego przez wewnętrzny serwer MCP może wskazywać, że aby z niego korzystać, użytkownik musi mieć rolę programisty, albo że użytkownik o określonej roli ma dostęp do listy dokumentów przeszukiwanych za pomocą RAG. Ujawnienie takich informacji może zachęcać do innych form sondowania poprzez ukierunkowaną rozmowę i wstrzyknięcie polecenia (LLM01:2026) i potencjalnie prowadzić do ujawnienia kolejnych informacji poufnych (LLM02:2026).

#### 5. Ujawnienie struktury wyników i reguł formatowania

Monity systemowe często określają strukturę odpowiedzi, w tym wymagane formaty, takie jak schematy JSON, szablony lub ograniczenia walidacyjne. Gdy instrukcje te zostaną ujawnione, atakujący zyskują wgląd w sposób konstruowania wyników i w założenia, na których opierają się systemy w dalszych etapach przetwarzania. Wiedzę tę można wykorzystać do generowania odpowiedzi zgodnych z oczekiwanymi formatami, ale zawierających niezamierzone lub zmanipulowane wartości, co może prowadzić do nieprawidłowego parsowania lub niezamierzonego zachowania systemu.

### Strategie zapobiegania i ograniczania skutków

#### 1. Nie umieszczaj danych wrażliwych w ukrytym kontekście

Nie osadzaj poświadczeń, sekretów ani konfiguracji o krytycznym znaczeniu dla bezpieczeństwa bezpośrednio w monitach systemowych ani w ukrytym kontekście. Zakładaj, że cały kontekst dostępny dla LLM może być również dostępny dla użytkowników. Zamiast tego przenoś takie informacje do systemów, do których model nie ma bezpośredniego dostępu, i unikaj sytuacji, w których model sam obsługuje dane wrażliwe.

#### 2. Stosuj metody deterministyczne i mechanizmy ochronne (guardrails) do walidacji i kontroli zachowania

Ponieważ modele LLM mogą być podatne na ataki takie jak wstrzyknięcie polecenia, ukryty kontekst nie powinien być podstawowym mechanizmem kontroli zachowania modelu. Specjalistyczne dostrajanie lub dalsze trenowanie modelu może zmniejszyć ryzyko ujawnienia, choć nie daje spójnej gwarancji i może mieć inne niezamierzone konsekwencje. Egzekwuj krytyczne zachowania za pomocą niezależnych i deterministycznych systemów działających poza modelem. Na przykład wykrywaniem szkodliwych treści i zapobieganiem im powinny zajmować się zewnętrzne zabezpieczenia, a nie instrukcje osadzone w ukrytym kontekście.

#### 3. Egzekwuj autoryzację i kontrolę dostępu niezależnie od LLM

Krytycznych mechanizmów kontrolnych, takich jak separacja uprawnień, sprawdzanie granic autoryzacji i podobne, nie wolno delegować do LLM — ani za pośrednictwem monitu systemowego, ani w inny sposób. Mechanizmy te powinny być egzekwowane w sposób deterministyczny i audytowalny, do czego modele LLM nie są dobrze przystosowane. Tam, gdzie zadania wymagają różnych poziomów dostępu, rozdzielaj je według kontekstu autoryzacji i przyznawaj każdemu z nich wyłącznie niezbędne uprawnienia.

### Przykładowe scenariusze ataków

#### Scenariusz nr 1: Wyciek poświadczeń przez monit systemowy

Monit systemowy LLM zawiera zestaw poświadczeń używanych przez narzędzie, do którego model otrzymał dostęp. Monit systemowy wycieka do atakującego, który może następnie wykorzystać te poświadczenia do innych celów.

#### Scenariusz nr 2: Schemat narzędzi uzyskany przez wydobycie kontekstu

Atakujący za pomocą sondowania w rozmowie wydobywa ukryty kontekst zawierający listę narzędzi i schematy parametrów, a następnie wykorzystuje te informacje do tworzenia danych wejściowych, które kierują aplikację w stronę określonych wywołań narzędzi. Nie dochodzi do ujawnienia żadnych poświadczeń ani do jawnego obejścia żadnej polityki, ale atakujący dysponuje teraz konkretnymi celami dla kolejnych prób wstrzyknięcia polecenia oraz rozpoznaniem na potrzeby łączenia działań w łańcuchy w dalszych etapach.

#### Scenariusz nr 3: Obejście ograniczeń dzięki ujawnieniu mechanizmów ochronnych

Monit systemowy LLM zabrania generowania obraźliwych treści, linków zewnętrznych i wykonywania kodu. Atakujący wydobywa ten monit systemowy i wykorzystuje ujawnione ograniczenia do przygotowania ataku typu wstrzyknięcie polecenia, który je omija.
