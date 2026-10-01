## LLM04:2026 Łańcuch dostaw

### Opis

Łańcuchy dostaw LLM są podatne na zagrożenia wpływające na integralność danych treningowych, modeli, adapterów, potoków konwersji i platform wdrożeniowych, co może skutkować stronniczymi wynikami, naruszeniami bezpieczeństwa lub awariami systemu. O ile tradycyjne podatności oprogramowania dotyczą przede wszystkim błędów w kodzie i zależnościach, o tyle w uczeniu maszynowym (ML) ryzyko obejmuje również wstępnie wytrenowane modele stron trzecich, zbiory danych i artefakty modeli, które mogą zostać zmanipulowane poprzez modyfikację, zatrucie lub złośliwą podmianę artefaktów.

Tworzenie modeli LLM to wyspecjalizowana praca, która zależy od modeli i zbiorów danych stron trzecich oraz od adapterów wielokrotnego użytku, tworzonych za pomocą technik efektywnego parametrycznie dostrajania (PEFT), takich jak LoRA (Low-Rank Adaptation), i udostępnianych na platformach takich jak Hugging Face. Łańcuch dostaw obejmuje obecnie artefakty modeli, ich pochodzenie oraz przepływy konwersji i scalania jako pełnoprawne powierzchnie ataku, a modele LLM działające na urządzeniach końcowych dodatkowo go poszerzają.

Część omawianych tu zagrożeń jest również opisana w LLM05:2026 Zatruwanie danych i modeli. Ta pozycja koncentruje się na ich aspekcie związanym z łańcuchem dostaw. Zagrożenia łańcucha dostaw właściwe dla aplikacji agentowych, w tym dotyczące serwerów MCP i rejestrów narzędzi, omawia pozycja ASI04 Agentic Supply Chain Vulnerabilities w OWASP Top 10 for Agentic Applications (OWASP GenAI Security Project, 2026), a MITRE ATLAS kataloguje odpowiadające im techniki przeciwników w ramach AML.T0010 AI Supply Chain Compromise (MITRE, b.d.).
[Prosty model zagrożeń łańcucha dostaw LLM](report/images/LLM%20Supply%20Chain%20Threat%20Model.png) ilustruje te powierzchnie ataku.

### Typowe przykłady ryzyka

#### 1. Podatne lub przestarzałe komponenty i modele stron trzecich

Przestarzałe lub wycofane komponenty, w tym pakiety, frameworki do serwowania modeli oraz same modele, mogą zostać wykorzystane do przejęcia kontroli nad aplikacjami LLM. Jest to podobne do [A06:2021 Vulnerable and Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/), przy czym ryzyko jest większe, gdy komponenty są wykorzystywane podczas tworzenia modelu, dostrajania lub wnioskowania. Asystenci programowania oparci na LLM wprowadzają nowy wariant: na dużą skalę halucynują wiarygodnie brzmiące, ale nieistniejące nazwy pakietów (Spracklen et al., 2025), które atakujący rejestrują z wyprzedzeniem (slopsquatting), tak aby niezweryfikowane zależności sugerowane przez AI prowadziły do złośliwego kodu.

#### 2. Ryzyko licencyjne

Tworzenie systemów AI wiąże się z różnorodnymi licencjami na oprogramowanie i zbiory danych, które nakładają odmienne wymagania dotyczące użytkowania, dystrybucji i komercjalizacji. Niewłaściwie zarządzane, generują ryzyko prawne i ryzyko braku zgodności.

#### 3. Podatne lub zmodyfikowane wstępnie wytrenowane modele

Artefakty modeli trudno kompleksowo zbadać, a sama analiza statyczna nie pozwala potwierdzić bezpieczeństwa ich zachowania. Wstępnie wytrenowany model może zawierać ukryte uprzedzenia lub backdoory wprowadzone przez zatrute zbiory danych lub bezpośrednią modyfikację. Odejście od niebezpiecznych formatów serializacji, takich jak Python pickle, który może wykonać dowolny kod podczas ładowania, zmniejsza to ryzyko, ale go nie eliminuje: backdoor może zostać osadzony bezpośrednio w grafie obliczeniowym modelu i przetrwać w formatach powszechnie uznawanych za bezpieczne, takich jak ONNX, a spreparowany plik modelu może wykorzystywać błędy uszkodzenia pamięci w natywnym parserze danego formatu, jak pokazały przepełnienia sterty w parsowaniu GGUF w llama.cpp (CVE-2024-23496) (Cisco Talos, 2024).

#### 4. Słabe potwierdzenie pochodzenia i niepodpisane artefakty modeli

Opublikowane modele nie dają silnych gwarancji pochodzenia: karty modeli (Model Cards) dokumentują model, ale nie dowodzą jego pochodzenia, a przejęte lub podszywające się konto dostawcy może opublikować złośliwy model pod zaufaną nazwą. Jeśli modele, adaptery, zbiory danych, dostrojone punkty kontrolne i wyniki konwersji nie są podpisane ani przypięte za pomocą skrótów, atakujący może podmienić lub niepostrzeżenie zmodyfikować artefakty w tranzycie, w magazynie danych lub na granicy promowania, na której zautomatyzowane potoki przyjmują artefakty do zaufanych środowisk — zwłaszcza gdy potoki odwołują się do artefaktów za pomocą zmiennego odwołania (np. tagu `latest`), a nie niezmiennego skrótu (digest).

#### 5. Podatne adaptery oraz przejęte przepływy konwersji, scalania i kwantyzacji

Adaptery LoRA sprawiają, że dostrajanie jest modułowe i wydajne, ale złośliwy adapter może naruszyć integralność wstępnie wytrenowanego modelu bazowego, zarówno we współdzielonych środowiskach scalania modeli, jak i na platformach wnioskowania, które pobierają adaptery i stosują je do wdrożonego modelu. Usługi konwersji i scalania modeli mogą wprowadzać złośliwe zmiany podczas przekształcania między formatami, omijając mechanizmy kontroli przeglądu. Kwantyzacja wiąże się z pokrewnym ryzykiem transformacji: wagi modelu można spreparować tak, aby model o pełnej precyzji zachowywał się w ocenie niegroźnie, podczas gdy skwantyzowany artefakt wykazuje zachowanie wybrane przez atakującego (Egashira et al., 2025), dlatego gwarancje uzyskane dla modelu o pełnej precyzji nie przenoszą się na wdrożony, skwantyzowany artefakt.

#### 6. Podatności łańcucha dostaw modeli LLM działających na urządzeniach

Modele LLM dostarczane na urządzeniach rozszerzają powierzchnię ataku o przejęte procesy produkcyjne, wykorzystywanie podatności systemu operacyjnego lub oprogramowania układowego (firmware) urządzenia oraz przepakowane aplikacje ze zmodyfikowanymi modelami, przez co integralność urządzenia i zaufanie do oprogramowania układowego stają się częścią łańcucha dostaw LLM.

#### 7. Niejasne regulaminy i polityki prywatności danych

Niejasne regulaminy (T&C) i polityki prywatności danych operatorów modeli mogą prowadzić do wykorzystania wrażliwych danych aplikacji do trenowania modelu, a w konsekwencji do ich ujawnienia, i mogą rodzić ryzyko naruszenia praw autorskich w związku z materiałami dostarczonymi przez dostawcę modelu.

### Strategie zapobiegania i ograniczania skutków

1. Starannie weryfikuj źródła danych i dostawców, w tym ich regulaminy i polityki prywatności, i korzystaj wyłącznie z zaufanych dostawców. Regularnie przeglądaj i audytuj bezpieczeństwo oraz zakres dostępu dostawców, a także przeprowadzaj ponowną ocenę w przypadku zmian w ich poziomie bezpieczeństwa lub regulaminach.
2. Stosuj środki zaradcze z pozycji OWASP Top Ten [A06:2021 Vulnerable and Outdated Components](https://owasp.org/Top10/A06_2021-Vulnerable_and_Outdated_Components/): skanowanie podatności, zarządzanie nimi oraz politykę aktualizacji, dzięki której aplikacja korzysta z utrzymywanych wersji komponentów, interfejsów API i modeli bazowych. Stosuj te same mechanizmy kontrolne w środowiskach programistycznych mających dostęp do danych wrażliwych i przed przyjęciem zależności sugerowanych przez AI weryfikuj, czy istnieją i czy są właściwym pakietem.
3. Przy wyborze modeli stron trzecich przeprowadzaj testy AI red teaming i ewaluacje skoncentrowane na przypadkach użycia objętych zakresem, a w środowisku produkcyjnym kontynuuj je, stosując wykrywanie anomalii i testy odporności na ataki adwersarialne w potokach MLOps i LLM, aby wykrywać modyfikacje i zatrucia.
4. Utrzymuj aktualny, podpisany inwentarz komponentów w postaci zestawienia komponentów oprogramowania (SBOM), rozszerzonego o modele, adaptery i zbiory danych za pomocą AI BOM (AIBOM) i ML SBOM, rozważając takie rozwiązania jak OWASP CycloneDX ML-BOM (OWASP CycloneDX, b.d.) i projekt OWASP AIBOM (OWASP GenAI Security Project, 2025). Śledź licencje w tym samym inwentarzu i regularnie go audytuj pod kątem zgodności i przejrzystości.
5. Korzystaj wyłącznie z modeli pochodzących z weryfikowalnych źródeł i kompensuj słabe potwierdzenie pochodzenia za pomocą zewnętrznych kontroli integralności, podpisów i skrótów plików. Kryptograficzne podpisywanie modeli oparte na rejestrze przejrzystości (transparency log; np. projekt OpenSSF Model Signing i Sigstore) wiąże artefakt modelu z tożsamością podpisującego (Open Source Security Foundation, 2025). Odtwarzalność kompilacji (reproducible builds) nie jest gwarantowana w przypadku trenowania modeli, a Coalition for Secure AI (CoSAI) udostępnia model dojrzałości dla podpisywania artefaktów ML (OASIS Open, 2025). Podpis potwierdza integralność i pochodzenie, a nie bezpieczeństwo (prawidłowo podpisany model od przejętego lub zaufanego, ale złośliwego dostawcy nadal może zawierać backdoor), dlatego łącz go z niezmiennymi odwołaniami do artefaktów, polityką pochodzenia, bramkami wydania opartymi na politykach (np. SLSA) (Open Source Security Foundation, b.d.), ewaluacją zachowania oraz ciągłą walidacją integralności modeli z wcześniejszych etapów łańcucha. Analogicznie stosuj podpisywanie kodu w przypadku kodu dostarczanego z zewnątrz.
6. Ściśle monitoruj i audytuj współdzielone środowiska tworzenia modeli, a usługi konwersji i scalania modeli traktuj jako punkty promowania wysokiego ryzyka.
7. Szyfruj modele wdrażane na brzegu sieci (edge) i stosuj kontrole integralności, korzystaj z interfejsów API atestacji dostawców, aby zapobiegać użyciu zmodyfikowanych aplikacji i modeli, oraz odrzucaj nierozpoznane oprogramowanie układowe i niezaufane stany urządzeń.

### Przykładowe scenariusze ataków

#### Scenariusz nr 1: Przejęte pakiety i frameworki do serwowania modeli

Przejęta zależność trafia do środowiska tworzenia modeli lub wnioskowania, jak w przypadku ataku na łańcuch dostaw PyTorch z grudnia 2022 roku, w którym złośliwy pakiet `torchtriton` w rejestrze PyPI przesłonił legalną zależność PyTorch-nightly i eksfiltrował dane (PyTorch Foundation, 2022). Stos serwujący modele jest częścią tej samej powierzchni ataku: ataki ShadowRay wykorzystywały w rzeczywistych warunkach sporną podatność CVE-2023-48022 (nieuwierzytelnione panele w produkcyjnych serwerach Ray), która później przerodziła się w samorozprzestrzeniający się botnet obejmujący wystawione klastry (Lumelsky & Elbaz, 2025), a w przypadku Ollama podatność CVE-2024-37032 umożliwiała zdalne wykonanie kodu za pośrednictwem złośliwego manifestu modelu pobranego z rejestru (Wiz, 2024).

#### Scenariusz nr 2: Zmodyfikowany model opublikowany w repozytorium modeli

Atakujący publikuje zmodyfikowany model pod wiarygodnie wyglądającą nazwą, co zademonstrował dowód koncepcji PoisonGPT, w którym model z precyzyjnie zmodyfikowanymi parametrami rozpowszechniał dezinformację, unikając wykrycia w standardowych testach porównawczych (benchmarkach) (Huynh & Hardouin, 2023). Ta sama luka w zaufaniu dotyczy dostrojonych przez atakujących wersji, które usuwają zabezpieczenia popularnego modelu przy zachowaniu jego wydajności w niegroźnych zadaniach (Zhan et al., 2023), każdego wstępnie wytrenowanego modelu wdrożonego bez weryfikacji oraz współdzielonych platform AI-as-a-service, na których badacze za pomocą złośliwego modelu wydostali się z kontenera wnioskowania i uzyskali dostęp do modeli i danych innych klientów (Tamari & Tzadik, 2024).

#### Scenariusz nr 3: Przejęty adapter LoRA dostawcy

Atakujący infiltruje dostawcę zewnętrznego i w subtelny sposób modyfikuje adapter LoRA, który następnie zostaje scalony z wdrożonym LLM w ramach przepływu scalania modeli. Po scaleniu adapter zapewnia ukryty punkt wejścia do systemu.

#### Scenariusz nr 4: Przejęta usługa konwersji lub scalania modeli

Atakujący przeprowadza atak za pośrednictwem usługi scalania modeli lub konwersji formatów, aby przejąć kontrolę nad publicznie dostępnym modelem i wprowadzić do niego złośliwe zachowanie, co pokazały badania HiddenLayer dotyczące przejęcia bota konwersji do formatu Safetensors w serwisie Hugging Face (HiddenLayer, 2024).

#### Scenariusz nr 5: Ponowne wykorzystanie przestrzeni nazw modelu

Organizacja wdraża model z publicznego repozytorium, odwołując się do niego wyłącznie za pomocą identyfikatora `Author/ModelName`. Pierwotny autor usuwa lub przenosi konto, zwalniając przestrzeń nazw, a atakujący ponownie rejestruje tę samą nazwę i publikuje złośliwy model pod pierwotną ścieżką. Potoki i zarządzane katalogi modeli, które rozpoznają model wyłącznie po nazwie, pobierają wówczas model atakującego, co prowadzi do zdalnego wykonania kodu (Saraf & Balassiano, 2025).

#### Scenariusz nr 6: Obejście skanerów, bezpiecznych mechanizmów ładowania i bezpiecznych formatów

Organizacja zabezpiecza dostęp do modeli stron trzecich za pomocą skanera złośliwego oprogramowania i opcji mechanizmu ładowania udokumentowanej jako bezpieczna wobec wykonania kodu. Atakujący pokonuje oba zabezpieczenia. Uszkodzone lub opakowane kompresją strumienie pickle wykonują swój ładunek, zanim skaner dotrze do uszkodzonego bajtu, jak w przypadku modeli nullifAI wykrytych w serwisie Hugging Face (Zanki, 2025). Obejściom skanerów modeli i flag bezpiecznego ładowania nadano identyfikatory CVE, np. w przypadku podatności zero-day w PickleScan (Cohen, 2025) oraz `torch.load` z opcją `weights_only` (CVE-2025-32434) (GitHub Security Advisories, 2025). Backdoor w stylu ShadowLogic umieszczony w grafie obliczeniowym (Wickens et al., 2024) „bezpiecznego” formatu, takiego jak ONNX, nie zawiera żadnego kodu wykonywalnego, który skaner serializacji mógłby oznaczyć. Traktuj skanery i flagi bezpiecznego ładowania jako warstwy obrony w głąb, a nie gwarancje, i stosuj je łącznie z weryfikacją pochodzenia oraz załatanymi mechanizmami ładowania i parserami.

#### Scenariusz nr 7: Przejęty potok budowania artefaktów modeli

Atakujący przejmuje potok CI/CD, którego organizacja używa do dostrajania i publikowania modeli, za pośrednictwem złośliwej zależności w procesie budowania, skradzionego poświadczenia do rejestru artefaktów lub zatrucia pamięci podręcznej, jak w ataku na Ultralytics, w którym wstrzyknięcie do pamięci podręcznej GitHub Actions doprowadziło do opublikowania w PyPI strojanizowanych wydań flagowej biblioteki AI (Python Package Index, 2024), w tym przejętego wydania „naprawczego”. Jest to ta sama podmiana na etapie budowania, którą znamy z klasycznych incydentów, takich jak backdoor w xz-utils i naruszenie bezpieczeństwa Codecov. Ponieważ artefakt z backdoorem jest budowany i podpisywany przez własną infrastrukturę wydawniczą organizacji, przechodzi kontrole pochodzenia w dalszych etapach łańcucha, wewnętrzną atestację oraz skanery łańcucha dostaw, które oznaczają wyłącznie komponenty pochodzące z zewnątrz.

#### Scenariusz nr 8: Aplikacja mobilna poddana inżynierii wstecznej

Atakujący za pomocą inżynierii wstecznej modyfikuje aplikację mobilną, podmieniając model działający na urządzeniu na zmodyfikowaną wersję, która kieruje użytkowników na oszukańcze strony, a następnie rozpowszechnia przepakowaną aplikację, wykorzystując socjotechnikę.
