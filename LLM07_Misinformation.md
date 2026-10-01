## LLM07:2026 Dezinformacja

### Opis

Dezinformacja występuje wtedy, gdy LLM lub aplikacja wykorzystująca LLM generuje nieprawidłowe, niekompletne, nieuzasadnione lub wprowadzające w błąd informacje, które wydają się na tyle wiarygodne, że mogą wpłynąć na decyzję człowieka, zautomatyzowany przepływ pracy lub działanie agenta. Podstawowe ryzyko polega na tym, że nieprawidłowy wynik zostaje uznany za wiarygodny i staje się podstawą działania.

We współczesnych systemach wyniki modeli sterują wywołaniami narzędzi, generują kod, pozwalają wnioskować o stanie systemu, autoryzują działania i koordynują współpracę między agentami. Sprawia to, że dezinformacja jest awarią na poziomie systemu, która może prowadzić do strat finansowych, incydentów bezpieczeństwa, zagrożeń dla bezpieczeństwa ludzi lub zakłóceń operacyjnych.

W systemach agentowych dezinformacja często przejawia się jako nieprawidłowy stan, rozumowanie lub dowody, które są konsumowane przez komponenty w dalszych etapach przetwarzania i prowadzą bezpośrednio do niezamierzonych działań.

Dezinformacja może wynikać z halucynacji, niekompletnego lub nieaktualnego kontekstu, słabego ugruntowania, niejednoznacznych poleceń, stronniczych lub uszkodzonych danych, wprowadzających w błąd podsumowań lub niezwalidowanych wyników narzędzi. Może też zostać celowo wywołana przez atakujących. Jeżeli pierwotną przyczyną jest wstrzyknięcie polecenia, zatrucie lub przejęcie łańcucha dostaw, należy odnosić się do tych zagrożeń osobno. Wykonywanie i obsługę niebezpiecznego wygenerowanego kodu omawia LLM10:2026 Nieprawidłowe przetwarzanie wyników, a rejestrowanie halucynowanych nazw pakietów jako wektor ataku na łańcuch dostaw — LLM04:2026 Łańcuch dostaw. Ta pozycja koncentruje się na wynikającym z tego trybie awarii: fałszywym obrazie rzeczywistości, który prowadzi do szkodliwej decyzji lub działania.

Nadmierne poleganie na modelu pozostaje kluczowym czynnikiem. Ludzie i systemy często traktują płynne, pewne siebie lub dobrze ustrukturyzowane wyniki jako wiarygodne. W architekturach agentowych takie nadmierne poleganie jest często wpisane w projekt systemu.

### Typowe przykłady ryzyka

1. Nieuzasadnione lub fałszywe wsparcie decyzji: nieprawidłowe lub nieuzasadnione informacje wpływają na decyzje biznesowe, prawne, medyczne, finansowe lub operacyjne.
2. Nieprawidłowe wnioskowanie o stanie w przepływach pracy: LLM wnioskuje, że warunek został spełniony, choć tak nie jest, co uruchamia niezamierzone działania.
3. Nieprawidłowy lub zmyślony kod i zależności: model generuje nieprawidłowe rekomendacje dotyczące kodu lub odwołuje się do nieistniejących (halucynowanych) pakietów (Spracklen et al., 2025).
4. Wprowadzające w błąd podsumowania i krytyczne pominięcia: podsumowania pomijają kluczowe ograniczenia, wyjątki, znaczniki czasu lub zagrożenia.
5. Dezinformacja wywołana przez przeciwnika: atakujący tworzą dane wejściowe, które powodują fałszywe twierdzenia lub pominięcie krytycznych faktów.
6. Propagacja dezinformacji między agentami: nieprawidłowe wyniki rozprzestrzeniają się między agentami i przepływami pracy.
7. Sfałszowane lub błędnie przypisane dowody: zmyślone lub zmanipulowane treści są przedstawiane jako wiarygodne dowody.

### Strategie zapobiegania i ograniczania skutków

1. Ugruntowuj twierdzenia przed działaniem: wymagaj, aby wyniki były oparte na wiarygodnych i aktualnych źródłach.
2. Wdrażaj wzorce „twierdzenie–weryfikacja–działanie” (claim-check-act): oddzielaj generowanie od wykonywania i weryfikuj twierdzenia przed podjęciem działania.
3. Waliduj wywołania narzędzi: przed wykonaniem sprawdzaj argumenty, autoryzację, warunki wstępne i bieżący stan.
4. Stosuj sygnały weryfikacyjne (nie tylko poziom pewności): uwzględniaj kontrole ugruntowania i spójności.
5. Egzekwuj weryfikację w czasie działania w przypadku działań o dużym wpływie: wprowadzaj przepływy zatwierdzania i kontrole systemowe.
6. Wykrywaj awarie polegające na pominięciach i zapobiegaj im: wymagaj ustrukturyzowanych wyników z polami obowiązkowymi.
7. Ograniczaj zasięg rażenia: stosuj zasadę najmniejszych uprawnień, sandboxing i limity częstotliwości.
8. Monitoruj i testuj pod kątem dezinformacji: rejestruj twierdzenia, dowody i rezultaty oraz testuj scenariusze adwersarialne.
9. Kalibruj zaufanie ludzi i systemów: odróżniaj zweryfikowane fakty od założeń.
10. Ewaluacja adwersarialna i ciągłe testowanie: regularnie testuj przepływy pracy za pomocą scenariuszy wprowadzających w błąd.

### Przykładowe scenariusze ataków

#### Scenariusz nr 1: Rekomendacja halucynowanej zależności

Asystent programowania rekomenduje wiarygodnie brzmiący, ale nieistniejący pakiet, który atakujący zarejestrował wcześniej pod halucynowaną nazwą, w wyniku czego programista ufający tej sugestii instaluje kod kontrolowany przez atakującego (Spracklen et al., 2025).

#### Scenariusz nr 2: Nieprawidłowa decyzja agenta dotycząca zasad

Agent obsługi klienta błędnie interpretuje zasady i zatwierdza zwrot naruszający warunki, co prowadzi do straty finansowej.

#### Scenariusz nr 3: Pominięcie w podsumowaniu o krytycznym znaczeniu dla bezpieczeństwa

Podsumowanie kliniczne pomija przeciwwskazanie do stosowania leku, a lekarz działa na podstawie niekompletnej rekomendacji.

#### Scenariusz nr 4: Fałszywe rozumowanie wywołane przez przeciwnika

Atakujący umieszcza na forum wsparcia fałszywe kroki naprawcze, które agent rozwiązujący problemy pobiera i powtarza jako zaufaną rekomendację.

#### Scenariusz nr 5: Fałszywy alert uruchamia zautomatyzowaną reakcję

Agent bezpieczeństwa błędnie klasyfikuje normalny ruch jako włamanie i automatycznie blokuje produkcyjny segment sieci, powodując przerwę w działaniu.

#### Scenariusz nr 6: Awaria zaufania między agentami

Agent wyszukujący zgłasza, że tożsamość klienta została zweryfikowana, choć tak nie jest, a agent płatności w dalszym etapie przetwarzania ufa temu stanowi i uwalnia środki.

#### Scenariusz nr 7: Zmyślone ukończenie zadania

Agent zgłasza, że nocna kopia zapasowa bazy danych została ukończona, choć nigdy nie została uruchomiona, a późniejsze przywracanie kończy się niepowodzeniem, ponieważ kopia zapasowa nie istnieje.
