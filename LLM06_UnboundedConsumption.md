## LLM06:2026 Nieograniczona konsumpcja

### Opis

Nieograniczona konsumpcja występuje wtedy, gdy aplikacja LLM dopuszcza nadmierne i niekontrolowane wnioskowania, umożliwiając atakującym zakłócenie dostępności usługi, narażenie na niemożliwe do udźwignięcia koszty finansowe lub kradzież własności intelektualnej poprzez klonowanie modelu — a wszystko to dzięki wykorzystaniu wspólnej klasy podatności: braku odpowiednich mechanizmów kontroli nad sposobem zużywania zasobów.

Wysokie wymagania obliczeniowe modeli LLM, zwłaszcza w środowiskach chmurowych i rozliczanych za token, sprawiają, że są one z natury podatne na wykorzystywanie zasobów i nieuprawnione użycie. Charakterystyczną cechą tego zagrożenia jest asymetria kosztów. Atakujący mogą wywołać nieproporcjonalnie kosztowne obliczenia przy znikomym koszcie własnym — za pomocą spreparowanych poleceń, skradzionych poświadczeń lub zmanipulowanych przepływów pracy.

Ryzyko to potęguje rosnące wykorzystanie modeli z rozszerzonym rozumowaniem (extended thinking) i modeli rozumujących z dużymi lub niedostatecznie ograniczonymi budżetami wyników, modeli multimodalnych, które znacząco zwiększają koszt obliczeń na żądanie, architektur agentowych i protokołów korzystania z narzędzi (takich jak MCP), które zamieniają pojedyncze żądanie w kaskadę operacji w dalszych etapach przetwarzania, a także współdzielonej infrastruktury wnioskowania, która wprowadza nowe powierzchnie ataku na łańcuch dostaw. Samo tradycyjne ograniczanie częstotliwości żądań już nie wystarcza. Skuteczna obrona wymaga mechanizmów kontroli kosztów uwzględniających tokeny, twardych limitów wydatków, mechanizmów circuit breaker na poziomie agentów oraz ciągłego monitorowania przypisania kosztów.

### Typowe przykłady ryzyka

#### 1. Zalanie danymi wejściowymi o zmiennej długości i eksplozja wyników

Atakujący mogą przeciążyć model LLM licznymi danymi wejściowymi o różnej długości, wykorzystując nieefektywność przetwarzania. Może to wyczerpać zasoby i potencjalnie spowodować brak reakcji systemu, co znacząco wpłynie na dostępność usług. Obejmuje to również eksplozję wyników wywołaną zatruciem podczas dostrajania, w której pojedyncza złośliwa próbka treningowa zaburza zachowanie modelu związane z końcem sekwencji i przy każdym żądaniu wydłuża wynik do maksymalnej długości (Gao et al., 2024).

#### 2. Odmowa dostępu do portfela (DoW)

Inicjując dużą liczbę operacji, atakujący wykorzystują model kosztu za użycie usług AI w chmurze, co prowadzi do niemożliwych do udźwignięcia obciążeń finansowych dla dostawcy i ryzyko ruiny finansowej.

#### 3. Nadużywanie dużego kontekstu

Powtarzane żądania bliskie limitu, akumulacja kontekstu i ponowne dzielenie na fragmenty (rechunking) po stronie aplikacji zużywają nieproporcjonalnie dużo mocy obliczeniowej i pamięci. Wiele interfejsów API od razu odrzuca dane wejściowe przekraczające okno kontekstowe, dlatego trwałe ryzyko stwarzają żądania, które mieszczą się tuż w granicach limitów, a jednocześnie zawyżają koszt pojedynczego żądania.

#### 4. Wyczerpanie przez pętle rozumowania i tokeny rozumowania

Atakujący tworzą krótkie, niegroźnie wyglądające polecenia, które prowadzą do wyczerpania zasobów, zmuszając modele z rozszerzonym rozumowaniem do wchodzenia w przedłużające się lub niekończące się pętle rozumowania, zużywające ogromne budżety tokenów rozumowania przy jednoczesnym omijaniu filtrów rozmiaru danych wejściowych (Li et al., 2025). Ponieważ polecenia te są krótkie i wyglądają na uprawnione, standardowa walidacja danych wejściowych nie zapewnia żadnej ochrony.

#### 5. Adwersarialne dane wejściowe zoptymalizowane pod kątem nadmiernego zużycia zasobów

Atakujący wykorzystują techniki optymalizacji do tworzenia danych wejściowych maksymalizujących koszt obliczeniowy. Różni się to od zwykłego polecenia modelowi wykonania zadania wymagającego dużych zasobów i obejmuje tzw. przykłady gąbkowe (sponge examples) (Shumailov et al., 2020) oraz adwersarialne zaburzenia wizualne. Obejmuje optymalizację adwersarialnych danych wejściowych za pomocą technik gradientowych i bezgradientowych. W przeciwieństwie do ataków z użyciem pętli rozumowania wymagają one jawnej optymalizacji w przestrzeni danych wejściowych, a nie jedynie odpowiedniego zaprojektowania polecenia.

#### 6. Multimodalne dane wejściowe i wyniki

Modele multimodalne przekształcają obrazy, dźwięk i wideo w dużą liczbę tokenów, dlatego pojedyncze żądanie może kosztować znacznie więcej niż porównywalne żądanie wyłącznie tekstowe. Dokładny narzut zależy od modelu, dostawcy, rozdzielczości, długości materiału i potoku przetwarzania wstępnego.

#### 7. Ekstrakcja modelu i kradzież przez destylację

Atakujący wysyłają do API modelu spreparowane dane wejściowe, aby zebrać wystarczającą liczbę wyników do zreplikowania częściowego modelu lub dostrojenia jego funkcjonalnego odpowiednika. Ujawnianie logitów i logarytmów prawdopodobieństw znacząco przyspiesza ekstrakcję (Carlini et al., 2024). Ekstrakcję wag lub architektury modelu kanałami bocznymi, poprzez pomiary czasu lub obserwację współdzielonej infrastruktury, omawia LLM02:2026 Ujawnianie informacji poufnych.

#### 8. Interakcje agentów z narzędziami zalewające zasoby modelu

Atakujący mogą publikować narzędzia, które nadmiernie wykorzystują zasoby LLM, wciągając aplikację opartą na LLM w rekurencyjne lub nieskończone pętle wywołań narzędzi. Może to sprawić, że pozornie uprawnione działania narzędzi będą skutkować obciążeniami finansowymi lub obniżeniem jakości usług. Gdy jedno wywołanie narzędzia rozgałęzia się na znacznie większą liczbę działań, LLM może być zmuszony obsługiwać setki wywołań uruchomionych przez jedno zadanie, co prowadzi do nadmiernego zużycia tokenów.

#### 9. Wykorzystanie infrastruktury wnioskowania

Atakujący wykorzystują podatności we frameworkach do serwowania modeli LLM (vLLM, TensorRT-LLM, SGLang, Triton, Ollama), aby doprowadzić do awarii usług lub wyczerpania zasobów modelu za pomocą błędów niebezpiecznej deserializacji, wstrzykiwania tokenów specjalnych i wstrzykniętych szablonów czatu.

### Strategie zapobiegania i ograniczania skutków

#### 1. Ograniczanie częstotliwości żądań i walidacja rozmiaru danych wejściowych

Zastosuj ograniczenie szybkości i limity użytkowników, aby ograniczyć liczbę żądań, które pojedynczy podmiot źródłowy może wysłać w danym okresie czasu. Nie poprzestawaj na limitach żądań na sekundę — egzekwuj limity liczby tokenów na minutę, tokenów na dzień oraz szacunkowego kosztu żądania. Stosuj wstępne szacowanie liczby tokenów, aby odrzucać żądania przed rozpoczęciem wnioskowania. Obejmuje to walidację zapewniającą, że dane wejściowe nie przekraczają rozsądnych limitów rozmiaru.

#### 2. Twarde limity wydatków

Ustal nieprzekraczalne pułapy budżetowe dla każdego klucza API, użytkownika, zespołu i konta w chmurze. Muszą to być mechanizmy egzekwowania, które po przekroczeniu limitu zatrzymują wnioskowanie, a nie progi alertów, które szybko narastające obciążenia mogą wyprzedzić. Limity wydatków powinny również uwzględniać różnice kosztów między modalnościami i protokołami narzędzi.

#### 3. Zarządzanie alokacją zasobów

Monitoruj i zarządzaj alokacją zasobów w sposób dynamiczny, aby zapobiec nadmiernemu zużyciu zasobów przez pojedynczego użytkownika lub żądanie.

#### 4. Techniki piaskownicy

Ogranicz dostęp LLM do zasobów sieciowych, usług wewnętrznych i interfejsów API. Ograniczenie zasobów, do których aplikacja może sięgać, zmniejsza możliwość wyprowadzenia przez atakującego wyodrębnionych informacji o modelu lub danych do zewnętrznego miejsca docelowego.

#### 5. Łagodna degradacja

Zaprojektowanie systemu tak, aby w przypadku dużego obciążenia ulegał łagodnej degradacji, zachowując częściową funkcjonalność zamiast całkowitej awarii.

#### 6. Ograniczenie działań w kolejce i solidna skalowalność

Wprowadź ograniczenia dotyczące liczby działań w kolejce i całkowitej liczby działań, jednocześnie wdrażając dynamiczne skalowanie i równoważenie obciążenia, aby obsłużyć zmienne wymagania i zapewnić stałą wydajność systemu.

#### 7. Skanowanie pod kątem zaburzeń adwersarialnych

Skanuj dane wejściowe modelu, w szczególności wizualne dane wejściowe dla LVLM (dużych wizyjno-językowych modeli), pod kątem śladów zaburzeń adwersarialnych, które mogłyby powodować nadmierne zużycie zasobów przez model.

#### 8. Wykrywanie interakcji z narzędziami wymagających dużych zasobów

Monitoruj interakcje agentów z narzędziami, aby wykrywać, czy dana sesja nie powoduje rekurencyjnego lub zasobożernego działania bez wyraźnego stanu końcowego. Ustal poziomy bazowe normalnego zachowania narzędzi, aby wykrywać, czy dane narzędzie odbiega od standardowych wzorców zużycia tokenów.

#### 9. Agentowe mechanizmy circuit breaker

Egzekwuj limity liczby kroków, głębokości rekurencji, czasu oraz pułapy kosztów na przebieg we wszystkich wykonaniach agentów. Stosuj haszowanie stanu do wykrywania pętli rekurencyjnych.

#### 10. Utwardzanie infrastruktury wnioskowania

Aktualizuj frameworki do serwowania modeli. Wyłącz niebezpieczną deserializację, ogranicz przekazywanie tokenów specjalnych i egzekwuj uwierzytelnianie we wszystkich endpointach wnioskowania.

### Przykładowe scenariusze ataków

#### Scenariusz nr 1: Niekontrolowana wielkość danych wejściowych

Atakujący przesyła niezwykle duże dane wejściowe do aplikacji LLM przetwarzającej dane tekstowe, co powoduje nadmierne zużycie pamięci i obciążenie procesora, potencjalnie powodując awarię systemu lub znaczne spowolnienie działania usługi.

#### Scenariusz nr 2: Powtarzające się żądania

Atakujący przesyła dużą liczbę żądań do interfejsu API LLM, powodując nadmierne zużycie zasobów obliczeniowych i uniemożliwiając korzystanie z usługi legalnym użytkownikom.

#### Scenariusz nr 3: Zapytania wymagające dużej ilości zasobów

Atakujący tworzy specjalne dane wejściowe, które mają wywołać najbardziej obciążające procesy LLM, co prowadzi do przedłużonego wykorzystania procesora graficznego (GPU) i potencjalnej awarii systemu.

#### Scenariusz nr 4: Odmowa dostępu do portfela (DoW)

Atakujący generuje nadmierną liczbę operacji, aby wykorzystać model płatności za rzeczywiste wykorzystanie usług AI w chmurze, powodując niemożliwe do pokrycia koszty dla dostawcy usług.

#### Scenariusz nr 5: Replikacja modelu funkcjonalnego

Atakujący wykorzystuje interfejs API LLM do generowania syntetycznych danych szkoleniowych i dostosowuje inny model, tworząc funkcjonalny odpowiednik i omijając tradycyjne ograniczenia związane z ekstrakcją modelu.

#### Scenariusz nr 6: Zaburzenia w obrazach wejściowych LVLM

Atakujący tworzy adwersarialne obrazy wejściowe zawierające zaburzenia zoptymalizowane tak, aby LVLM zużywał nadmierną liczbę tokenów w generowanych wynikach (Gao et al., 2025).

#### Scenariusz nr 7: Wieloturowe pętle wywołań narzędzi i rozgałęzianie wywołań narzędzi

Atakujący może opublikować złośliwe narzędzie (np. w postaci umiejętności Claude Skill w repozytorium open source), które instruuje agenta, aby wykonywał rekurencyjne, cykliczne zadania lub zadania wymagające dużej liczby wywołań narzędzi. Programiści włączający to narzędzie do swoich agentów ryzykują wówczas nadmierne zużycie tokenów i niestabilność usługi.

#### Scenariusz nr 8: Rosnący kontekst LLM w sesjach agentowych

Atakujący lub niezłośliwy użytkownik utrzymuje otwartą sesję agentową, stopniowo dodając treści tak, że przy każdym wnioskowaniu ponownie przetwarzany jest cały zgromadzony kontekst. Koszt jednej tury rośnie wraz z kontekstem — od około 0,001 USD w pierwszej turze do około 0,50 USD w setnej turze. Żadne pojedyncze żądanie nie uruchamia limitów częstotliwości, ponieważ każde z osobna mieści się w budżecie, a mimo to łączny koszt w wielu równoległych lub długotrwałych sesjach sięga setek dolarów.
