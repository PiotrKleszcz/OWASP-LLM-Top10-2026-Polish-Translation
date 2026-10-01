# Załącznik: Mapowanie Top 10 na OWASP AISVS 1.0

## Cel

OWASP Top 10 for LLM Applications to dokument *podnoszący świadomość ryzyka*: wskazuje dziesięć najbardziej krytycznych kategorii awarii i wyjaśnia na poziomie zasad, jak im zapobiegać i jak ograniczać ich skutki. [OWASP AI Security & Privacy Verification Standard (AISVS)](https://github.com/OWASP/AISVS) to standard *weryfikacyjny*: rozkłada bezpieczeństwo AI na testowalne wymagania („**Zweryfikuj, że** …”), z których każde ma przypisany poziom pewności (Level) od L1 (podstawowy) do L3 (zaawansowany).

Oba dokumenty wzajemnie się uzupełniają. Ten załącznik je łączy, dzięki czemu zespół, który zapoznał się z pozycją Top 10, może przejść bezpośrednio do konkretnych, audytowalnych mechanizmów kontrolnych chroniących przed danym ryzykiem. Dla każdego ryzyka odpowiada na jedno pytanie:

> *„Które wymagania AISVS powinienem zweryfikować w obliczu tego ryzyka, aby mieć pewność, że jestem chroniony?”*

**Mapowane wersje:** OWASP Top 10 for LLM Applications **2026** na OWASP AISVS **1.0**.

## Jak czytać ten załącznik

**[Macierz pokrycia](#macierz-pokrycia)** to zestawienie, które na pierwszy rzut oka pokazuje wszystkie dziesięć ryzyk w odniesieniu do dwunastu rozdziałów mechanizmów kontrolnych AISVS. Pozwala zobaczyć, gdzie koncentruje się dane ryzyko i które rozdziały mają największe znaczenie dla obrony, a następnie przeczytać pełny tekst tych rozdziałów w [AISVS 1.0](https://github.com/OWASP/AISVS/tree/main/1.0/en), aby poznać konkretne wymagania `Verify that…` mające zastosowanie w Twoim systemie.

Mapowania stanowią wskazówki kierunkowe, a nie tabelę zgodności (compliance crosswalk): oznaczony rozdział przyczynia się do obrony przed danym ryzykiem, ale rzadko samodzielnie je „zamyka”, a rozdział może być istotny dla ryzyk innych niż te, które tu oznaczono.

Legenda: **●** obrona podstawowa (rozdział stanowi główną linię obrony przed tym ryzykiem) · **○** obrona wspierająca (przyczynia się do obrony, ale nie stanowi jej głównego elementu).

## Rozdziały mechanizmów kontrolnych AISVS 1.0

| ID | Rozdział |
|---|---|
| **C1** | Training Data Integrity & Traceability (Integralność i identyfikowalność danych treningowych) |
| **C2** | Input Validation (Walidacja danych wejściowych) |
| **C3** | Model Lifecycle Management & Change Control (Zarządzanie cyklem życia modelu i kontrola zmian) |
| **C4** | Infrastructure, Configuration & Deployment Security (Bezpieczeństwo infrastruktury, konfiguracji i wdrożeń) |
| **C5** | Access Control & Identity for AI Components & Users (Kontrola dostępu i tożsamość komponentów AI oraz użytkowników) |
| **C6** | Supply Chain Security for Models (Bezpieczeństwo łańcucha dostaw modeli) |
| **C7** | Model Behavior, Output Control & Safety Assurance (Zachowanie modelu, kontrola wyników i zapewnienie bezpieczeństwa) |
| **C8** | Memory, Embeddings & Vector Database Security (Bezpieczeństwo pamięci, osadzeń i baz wektorowych) |
| **C9** | Orchestration & Agentic Security (Bezpieczeństwo orkiestracji i systemów agentowych) |
| **C10** | Model Context Protocol (MCP) Security (Bezpieczeństwo protokołu MCP) |
| **C11** | Adversarial Robustness (Odporność na ataki adwersarialne) |
| **C12** | Monitoring, Logging & Anomaly Detection (Monitorowanie, logowanie i wykrywanie anomalii) |

## Macierz pokrycia

| Ryzyko | C1 | C2 | C3 | C4 | C5 | C6 | C7 | C8 | C9 | C10 | C11 | C12 |
|---|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| **LLM01** Wstrzyknięcie polecenia | | ● | | | | | ○ | | ● | ● | ● | ● |
| **LLM02** Ujawnianie informacji poufnych | ○ | | | | ● | | ● | ● | | | ● | ○ |
| **LLM03** Nadmierna sprawczość | | | | | ● | | | | ● | ● | | ○ |
| **LLM04** Łańcuch dostaw | ○ | | ● | ● | | ● | | | | ○ | | |
| **LLM05** Zatruwanie danych i modeli | ● | | ● | | | ○ | | ● | | | ● | ○ |
| **LLM06** Nieograniczona konsumpcja | | | | | ○ | | ○ | | ● | | ● | ● |
| **LLM07** Dezinformacja | ○ | | | | | | ● | ○ | | | ○ | ● |
| **LLM08** Ujawnienie ukrytego kontekstu | | ○ | | | ○ | | ● | | ● | | ● | ○ |
| **LLM09** Słabe punkty wektorów i osadzeń | ○ | | | | ● | | ○ | ● | | | | ○ |
| **LLM10** Nieprawidłowe przetwarzanie wyników | | | | | | | ● | | ● | ○ | | |

## Uwagi i zastrzeżenia

- **Charakter kierunkowy, a nie certyfikujący.** Rozdział jest oznaczony dla danego ryzyka, ponieważ weryfikacja jego mechanizmów kontrolnych *przyczynia się do* obrony przed tym ryzykiem. Pełne pokrycie ryzyka wymaga na ogół mechanizmów kontrolnych z kilku rozdziałów oraz decyzji projektowych specyficznych dla danego systemu, których AISVS nie jest w stanie ująć.
- **Rozdziały, a nie pojedyncze wymagania.** Macierz mapuje na poziomie rozdziałów (od C1 do C12). W każdym oznaczonym rozdziale mające zastosowanie wymagania `Verify that…` oraz ich poziomy pewności (od podstawowego L1 do zaawansowanego L3) zależą od Twojego systemu; zapoznaj się z tekstem rozdziału w AISVS 1.0.
- **Wzmocnienie w systemach agentowych.** W kilku pozycjach na rok 2026 zaznaczono, że opisywane ryzyka narastają w systemach agentowych i wieloagentowych. W takich przypadkach mechanizmy obrony zawierają rozdziały AISVS **C9** (Orchestration & Agentic Security) i **C10** (MCP Security), które ten załącznik mapuje, ale głównym punktem odniesienia dla zagrożeń specyficznych dla agentów pozostaje [OWASP Top 10 for Agentic Applications](https://genai.owasp.org/).
- **Mapowanie żywe.** Oba dokumenty ewoluują. Ten załącznik mapuje Top 10 **2026** na AISVS **1.0**; wróć do niego, gdy którykolwiek z nich doczeka się nowej wersji.
