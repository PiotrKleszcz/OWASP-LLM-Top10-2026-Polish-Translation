# OWASP Top 10 dla aplikacji LLM 2026 — tłumaczenie na język polski

Nieoficjalne, społecznościowe tłumaczenie na język polski dokumentu **OWASP Top 10 for Large Language Model Applications 2026**, rozwijanego przez [OWASP GenAI Security Project](https://genai.owasp.org/).

Oryginał (język angielski): [GenAI-Security-Project/GenAI-LLM-Top10](https://github.com/GenAI-Security-Project/GenAI-LLM-Top10/tree/main/2026/final) · Oficjalna publikacja: [OWASP GenAI LLM Top 10 2026](https://genai.owasp.org/resource/owasp-genai-llm-top-10-2026/)

## Lista OWASP Top 10 dla aplikacji LLM 2026

| Nr | Pozycja | Plik |
|---|---|---|
| — | List od kierowników projektu | [LLM00_Preface.md](LLM00_Preface.md) |
| LLM01:2026 | Wstrzyknięcie polecenia | [LLM01_PromptInjection.md](LLM01_PromptInjection.md) |
| LLM02:2026 | Ujawnianie informacji poufnych | [LLM02_SensitiveInformationDisclosure.md](LLM02_SensitiveInformationDisclosure.md) |
| LLM03:2026 | Nadmierna sprawczość | [LLM03_ExcessiveAgency.md](LLM03_ExcessiveAgency.md) |
| LLM04:2026 | Łańcuch dostaw | [LLM04_SupplyChain.md](LLM04_SupplyChain.md) |
| LLM05:2026 | Zatruwanie danych i modeli | [LLM05_DataModelPoisoning.md](LLM05_DataModelPoisoning.md) |
| LLM06:2026 | Nieograniczona konsumpcja | [LLM06_UnboundedConsumption.md](LLM06_UnboundedConsumption.md) |
| LLM07:2026 | Dezinformacja | [LLM07_Misinformation.md](LLM07_Misinformation.md) |
| LLM08:2026 | Ujawnienie ukrytego kontekstu | [LLM08_HiddenContextExposure.md](LLM08_HiddenContextExposure.md) |
| LLM09:2026 | Słabe punkty wektorów i osadzeń | [LLM09_VectorAndEmbeddingWeaknesses.md](LLM09_VectorAndEmbeddingWeaknesses.md) |
| LLM10:2026 | Nieprawidłowe przetwarzanie wyników | [LLM10_ImproperOutputHandling.md](LLM10_ImproperOutputHandling.md) |

## Załączniki

| Załącznik | Plik |
|---|---|
| Załącznik A: Mapowania na powiązane frameworki | [Appendix_A_Related_Framework_Mappings.md](Appendix_A_Related_Framework_Mappings.md) |
| Załącznik B: Architektura aplikacji LLM i modelowanie zagrożeń | [Appendix_B_LLM_Application_Architecture_and_Threat_Modeling.md](Appendix_B_LLM_Application_Architecture_and_Threat_Modeling.md) |
| Załącznik: Mapowanie Top 10 na OWASP AISVS 1.0 | [LLMZZ_Appendix_AISVS_Mapping.md](LLMZZ_Appendix_AISVS_Mapping.md) |
| Bibliografia | [references.md](references.md) |

## Zawartość repozytorium

- Pliki `.md` — polskie tłumaczenie wszystkich plików z katalogu `2026/final` repozytorium źródłowego; nazwy plików i struktura Markdown są identyczne jak w oryginale, co ułatwia porównywanie wersji.
- `report/images/` — ilustracje z oryginału (bez zmian, w języku angielskim).
- `mappings/` — pliki JSON z mapowaniami na frameworki zewnętrzne (bez zmian, w języku angielskim; stanowią dane źródłowe dla Załącznika A).

## Zasady tłumaczenia

- Nazwy frameworków, standardów, taksonomii i ich elementów (np. MITRE ATLAS, CWE, ASI, AISVS), tytuły publikacji w bibliografii oraz adresy URL pozostawiono w oryginalnym brzmieniu.
- Nazwy klas ataków i technik utrwalone w branży (np. XSS, SSRF, slopsquatting, jailbreak) pozostawiono bez tłumaczenia, w razie potrzeby z polskim objaśnieniem przy pierwszym użyciu.
- Fragmenty tekstu, które nie zmieniły się w stosunku do wydania 2025, przejęto z polskiego tłumaczenia wersji 2025, a w wydaniu v1.1 ujednolicono je z glosariuszem projektu.
- Terminologia jest spójna z polskimi tłumaczeniami OWASP ASVS 5.0 i OWASP API Security Top 10 tego samego autora.
- Forma zwracania się do czytelnika: 2. osoba liczby pojedynczej i tryb rozkazujący, jak w polskim tłumaczeniu OWASP ASVS 5.0.
- Bibliografia: tytuły, autorzy i adresy URL bez zmian; zlokalizowano wyłącznie elementy opisu w stylu APA (daty, „b.d.”, „Pobrano…”, oznaczenia typu źródła).
- Kotwice linków wewnętrznych dostosowano do przetłumaczonych nagłówków (np. `#macierz-pokrycia`), aby odnośniki w dokumencie działały poprawnie.
- Nazwy rozdziałów OWASP AISVS podano w brzmieniu oryginalnym wraz z polskim tłumaczeniem w nawiasie.
- Teksty alternatywne i podpisy ilustracji przetłumaczono; same grafiki pozostają w wersji angielskiej.

## Poprzednie wydanie

Polskie tłumaczenie wydania 2025: [PiotrKleszcz/OWASP-LLM-Top10-Polish-Translation](https://github.com/PiotrKleszcz/OWASP-LLM-Top10-Polish-Translation)

## Autor tłumaczenia

Piotr Kleszcz

Uwagi i propozycje poprawek są mile widziane — zgłoś je przez Issue lub pull request.

## Licencja

Oryginalny dokument © OWASP GenAI Security Project i współtwórcy, udostępniony na licencji [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).

Niniejsze tłumaczenie jest utworem zależnym i zgodnie z warunkami tej licencji jest udostępniane na tej samej licencji — CC BY-SA 4.0. Pełny tekst licencji znajduje się w pliku [LICENSE](LICENSE).

Tłumaczenie nie jest oficjalną publikacją OWASP Foundation. W razie rozbieżności rozstrzygająca jest angielska wersja oryginalna.
