## LLM03:2026 Nadmierna sprawczość

### Opis

System oparty na LLM często otrzymuje od swojego twórcy pewien zakres sprawczości: możliwość wywoływania funkcji lub współdziałania z innymi systemami za pośrednictwem narzędzi (przez różnych dostawców nazywanych również rozszerzeniami, wtyczkami lub umiejętnościami) w celu podejmowania działań w odpowiedzi na polecenie. Agent LLM może również dynamicznie wybierać, które narzędzie wywołać, na podstawie polecenia wejściowego lub wcześniejszego wyniku LLM. Systemy oparte na agentach zazwyczaj wielokrotnie wywołują LLM, wykorzystując wyniki poprzednich wywołań do ugruntowania i ukierunkowania kolejnych.

Nadmierna sprawczość to podatność umożliwiająca wykonanie szkodliwych działań w odpowiedzi na nieoczekiwane, niejednoznaczne lub zmanipulowane wyniki LLM, niezależnie od tego, co powoduje nieprawidłowe działanie LLM. Typowe czynniki wyzwalające to:

* halucynacja/konfabulacja spowodowana źle skonstruowanymi, niezłośliwymi poleceniami lub po prostu słabo działającym/niedostosowanym (misaligned) modelem,
* bezpośrednie/pośrednie wstrzyknięcie polecenia przez złośliwego użytkownika, wcześniejsze wywołanie złośliwego/przejętego narzędzia lub (w systemach wieloagentowych/współpracujących) złośliwego/przejętego agenta współpracującego.

Pierwotną przyczyną Nadmiernej sprawczości jest zazwyczaj co najmniej jeden z poniższych czynników:

* nadmierna funkcjonalność,
* nadmierne uprawnienia,
* nadmierna autonomia.

Nadmierna sprawczość może prowadzić do szerokiego zakresu skutków w obszarze poufności, integralności i dostępności, a ich rodzaj zależy od tego, z jakimi systemami może współdziałać aplikacja oparta na LLM. W kontekście systemów agentowych Nadmierna sprawczość może przejawiać się jako ASI02: Tool Misuse & Exploitation, ASI03: Identity & Privilege Abuse oraz ASI08: Cascading Failures.

Uwaga: Nadmierna sprawczość różni się od Nieprawidłowego przetwarzania wyników, które dotyczy niewystarczającej weryfikacji wyników LLM. Sanityzacja danych wejściowych i wyników modelu nie jest podstawowym mechanizmem kontrolnym w przypadku Nadmiernej sprawczości; w odniesieniu do danych wejściowych omawia ją LLM01:2026 Wstrzyknięcie polecenia, a w odniesieniu do wyników — LLM10:2026 Nieprawidłowe przetwarzanie wyników.

### Typowe przykłady ryzyka

#### 1. Nadmierna funkcjonalność

  Agent LLM ma dostęp do narzędzi zawierających funkcje, które nie są potrzebne do zamierzonego działania systemu. Przykładowo programista musi przyznać agentowi LLM możliwość odczytywania dokumentów z repozytorium, ale wybrane przez niego narzędzie strony trzeciej umożliwia również modyfikowanie i usuwanie dokumentów.

#### 2. Nadmierna funkcjonalność

  Narzędzie mogło zostać przetestowane na etapie rozwoju i zastąpione lepszą alternatywą, ale pierwotne narzędzie nadal pozostaje dostępne dla agenta LLM.

#### 3. Nadmierna funkcjonalność

  Narzędzie LLM o otwartej funkcjonalności nie filtruje prawidłowo instrukcji wejściowych pod kątem poleceń wykraczających poza to, co jest niezbędne do zamierzonego działania aplikacji. Na przykład narzędzie do uruchamiania jednego określonego polecenia powłoki nie zapobiega skutecznie wykonywaniu innych poleceń powłoki.

#### 4. Nadmierne uprawnienia

  Narzędzie LLM ma w systemach w dalszych etapach przetwarzania uprawnienia, które nie są potrzebne do zamierzonego działania aplikacji. Na przykład narzędzie przeznaczone do odczytu danych łączy się z serwerem bazy danych przy użyciu tożsamości, która ma nie tylko uprawnienia SELECT, ale również UPDATE, INSERT i DELETE.

#### 5. Nadmierne uprawnienia

  Narzędzie LLM zaprojektowane do wykonywania operacji w kontekście pojedynczego użytkownika uzyskuje dostęp do systemów w dalszych etapach przetwarzania przy użyciu ogólnej tożsamości o wysokich uprawnieniach. Na przykład narzędzie do odczytu magazynu dokumentów bieżącego użytkownika łączy się z repozytorium dokumentów za pomocą uprzywilejowanego konta, które ma dostęp do plików wszystkich użytkowników.

#### 6. Nadmierna autonomia

  Aplikacja lub narzędzie oparte na LLM nie weryfikuje i nie zatwierdza niezależnie działań o dużym wpływie. Na przykład narzędzie umożliwiające usuwanie dokumentów użytkownika usuwa je bez jakiegokolwiek potwierdzenia ze strony użytkownika.

### Strategie zapobiegania i ograniczania skutków

Następujące działania mogą zapobiec Nadmiernej sprawczości:

#### 1. Minimalizuj liczbę narzędzi

  Ogranicz narzędzia, które agenci LLM mogą wywoływać, do niezbędnego minimum. Na przykład, jeśli system oparty na LLM nie wymaga możliwości pobierania zawartości adresu URL, takie narzędzie nie powinno być udostępniane agentowi LLM.

#### 2. Minimalizuj funkcjonalność narzędzi

  Ogranicz funkcje zaimplementowane w narzędziach LLM do niezbędnego minimum. Na przykład narzędzie uzyskujące dostęp do skrzynki pocztowej użytkownika w celu podsumowywania wiadomości e-mail może wymagać jedynie możliwości ich odczytu, dlatego nie powinno zawierać innych funkcji, takich jak usuwanie lub wysyłanie wiadomości.

#### 3. Unikaj narzędzi o otwartej funkcjonalności

  W miarę możliwości unikaj narzędzi o otwartej funkcjonalności (np. uruchamiających polecenie powłoki, pobierających zawartość adresu URL itp.) i stosuj narzędzia o bardziej szczegółowo określonej funkcjonalności. Na przykład aplikacja oparta na LLM może potrzebować zapisać pewne wyniki w pliku. Gdyby zaimplementowano to za pomocą narzędzia uruchamiającego funkcję powłoki, zakres niepożądanych działań byłby bardzo duży (można by wykonać dowolne inne polecenie powłoki). Bezpieczniejszą alternatywą byłoby zbudowanie dedykowanego narzędzia do zapisu plików, które implementuje wyłącznie tę konkretną funkcjonalność. Narzędzia powinny definiować ścisły schemat dla wszystkich parametrów wejściowych i walidować ich zawartość przed użyciem.

#### 4. Minimalizuj uprawnienia narzędzi

  Ogranicz uprawnienia przyznawane narzędziom LLM w innych systemach do niezbędnego minimum, aby zawęzić zakres niepożądanych działań. Na przykład agent LLM, który korzysta z bazy danych produktów w celu przedstawiania klientowi rekomendacji zakupowych, może potrzebować jedynie dostępu do odczytu tabeli „products”. Nie powinien mieć dostępu do innych tabel ani możliwości wstawiania, aktualizowania lub usuwania rekordów. Należy to egzekwować, nadając odpowiednie uprawnienia bazodanowe tożsamości, której narzędzie LLM używa do łączenia się z bazą danych.

#### 5. Wykonuj narzędzia w kontekście użytkownika

  Śledź autoryzację i zakres bezpieczeństwa użytkownika, aby zapewnić, że działania podejmowane w jego imieniu są wykonywane w systemach w dalszych etapach przetwarzania w kontekście tego konkretnego użytkownika i z minimalnymi niezbędnymi uprawnieniami. Na przykład narzędzie LLM odczytujące repozytorium kodu użytkownika powinno wymagać uwierzytelnienia użytkownika za pośrednictwem OAuth z minimalnym wymaganym zakresem. W delegowanych lub wieloagentowych przepływach pracy zachowuj pierwotny kontekst użytkownika i zakres autoryzacji w łańcuchu wywołań narzędzi lub agentów, zamiast polegać wyłącznie na uprawnieniach wywołującego agenta lub tożsamości usługi.

#### 6. Wymagaj zatwierdzenia przez użytkownika

  Stosuj kontrolę z udziałem człowieka (human-in-the-loop), aby działania o dużym wpływie wymagały zatwierdzenia przez człowieka przed ich wykonaniem. Można ją zaimplementować w systemie w dalszym etapie przetwarzania (poza zakresem aplikacji LLM) lub w samym narzędziu LLM. Na przykład aplikacja oparta na LLM, która tworzy i publikuje treści w mediach społecznościowych w imieniu użytkownika, powinna zawierać procedurę zatwierdzania przez użytkownika w narzędziu realizującym operację „publikuj”.

#### 7. Pełne pośrednictwo

  Implementuj autoryzację w logice aplikacji, zamiast polegać na tym, że LLM zdecyduje, czy dane działanie jest dozwolone. Egzekwuj zasadę pełnego pośrednictwa (complete mediation), tak aby wszystkie żądania kierowane do systemów w dalszych etapach przetwarzania były weryfikowane pod kątem zgodności z politykami bezpieczeństwa przez narzędzie, przez niezależny punkt decyzyjny polityk działający przed wykonaniem (umieszczony między narzędziem a systemem docelowym) lub przez sam system docelowy. Takie polityki mogą pomóc w obsłudze przypadków, w których formalnie dozwolone działanie agenta jest niebezpieczne w danym kontekście. Stopniowana polityka egzekwowania (audyt, ostrzeżenie, blokada, eskalacja) pozwala na automatyczne zatwierdzanie działań o niewielkich konsekwencjach lub łatwo odwracalnych, natomiast działania o poważnych konsekwencjach lub nieodwracalne kieruje do weryfikacji przez człowieka. Na przykład chatbot obsługi klienta może automatycznie przetworzyć zwrot w formie środków na koncie sklepowym (co jest odwracalne), natomiast nieodwracalne działanie, takie jak wypłata zewnętrzna, jest kierowane do zatwierdzenia przez człowieka.

Poniższe opcje nie zapobiegną Nadmiernej sprawczości, ale mogą ograniczyć rozmiar wyrządzonych szkód:

#### 8. Monitoruj użycie narzędzi

  Rejestruj i monitoruj aktywność narzędzi LLM oraz systemów w dalszych etapach przetwarzania, aby wykrywać miejsca, w których zachodzą niepożądane działania, i odpowiednio na nie reagować.

#### 9. Ograniczanie częstotliwości żądań

  Ustal progi dla wywołań narzędzi i zaimplementuj mechanizmy circuit breaker, które po przekroczeniu tych progów zatrzymują działanie, ograniczają częstotliwość żądań lub przekazują sprawę do weryfikacji przez człowieka. Proste progi mogą opierać się na liczbie wywołań, natomiast progi uwzględniające kontekst mogą opierać się na skumulowanej wartości parametru wejściowego narzędzia.

### Przykładowe scenariusze ataków

#### Scenariusz nr 1: Przejęty asystent poczty e-mail

Aplikacja osobistego asystenta oparta na LLM otrzymuje za pośrednictwem narzędzia dostęp do skrzynki pocztowej danej osoby w celu podsumowywania treści przychodzących wiadomości e-mail. Do realizacji tej funkcji narzędzie potrzebuje możliwości odczytu wiadomości, ale narzędzie wybrane przez twórcę systemu zawiera również funkcje wysyłania wiadomości. Aplikacja jest ponadto podatna na atak pośredniego wstrzyknięcia polecenia, w którym złośliwie spreparowana przychodząca wiadomość e-mail nakłania LLM do wydania agentowi polecenia przeszukania skrzynki odbiorczej użytkownika pod kątem informacji poufnych i przekazania ich na adres e-mail atakującego. Można by tego uniknąć poprzez:

* wyeliminowanie nadmiernej funkcjonalności przez użycie narzędzia, które implementuje wyłącznie możliwość odczytu poczty,
* wyeliminowanie nadmiernych uprawnień przez uwierzytelnianie w usłudze poczty e-mail użytkownika za pośrednictwem sesji OAuth z zakresem tylko do odczytu i/lub
* wyeliminowanie nadmiernej autonomii przez wymaganie, aby użytkownik ręcznie przejrzał każdą wiadomość przygotowaną przez narzędzie LLM i sam kliknął „wyślij”.

Alternatywnie rozmiar wyrządzonych szkód można by ograniczyć, wprowadzając ograniczanie częstotliwości żądań w interfejsie wysyłania poczty.
