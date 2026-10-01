## List od kierowników projektu

Przestań próbować budować model, którego nie da się oszukać. Zbuduj wokół niego system tak, aby w chwili, gdy model zostanie oszukany — a zostanie — nic ważnego nie uległo awarii. To podejście przewija się przez wszystkie dziesięć pozycji tej listy. W tym roku po raz pierwszy możemy pokazać Ci dowody, zamiast prosić, byś przyjął je na wiarę.

Każda dotychczasowa wersja tej listy opierała się na ocenie ekspertów. Setki praktyków wypowiadały się na temat tego, co jest najważniejsze. To głosowanie nadal stanowi kręgosłup listy i słusznie. W tym roku zrobiliśmy jednak coś, czego projekt nigdy wcześniej nie robił. Skonfrontowaliśmy wynik głosowania z rejestrem tego, co faktycznie poszło nie tak. Zebraliśmy korpus 7 714 rzeczywistych incydentów z publicznych baz danych podatności oraz z bazy danych szkód wyrządzonych przez AI, a następnie zbudowaliśmy klasyfikatory, które je przeanalizowały i przypisały do kategorii 6 639 incydentów zawierających wystarczająco dużo szczegółów. Potem zadaliśmy jedno bezpośrednie pytanie: czy to, czego obawiają się praktycy, pokrywa się z tym, co pokazuje rejestr incydentów?

Odpowiedź brzmiała: nie zawsze — i to było zaskoczeniem. Wyniki głosowania i dane rozeszły się w konkretnych, pouczających miejscach, a te rozbieżności nauczyły nas więcej niż zbieżności.

Najwyraźniejszym przykładem jest wstrzyknięcie polecenia (prompt injection). Praktycy uznają je za ryzyko numer jeden. Jeśli jednak uszeregować kategorie według surowych danych o incydentach, wypada ono całkowicie poza pierwszą dziesiątkę. Ta rozbieżność jest efektem obrony. Zespoły intensywnie walczą z wstrzyknięciami, więc do publicznych baz danych trafia mniej jednoznacznych exploitów, a publiczne statystyki zaniżają ryzyko, na którego powstrzymywanie dojrzałe zespoły już dziś wydają realne pieniądze. Powierzchnia ataku nadal występuje wszędzie tam, gdzie model odczytuje niezaufane dane wejściowe, czyli po prostu wszędzie. Pozycja pozostaje na pierwszym miejscu. Skoro nie da się zlikwidować tej powierzchni ataku, projektuj system z myślą o dniu, w którym zostanie ona wykorzystana przeciwko Tobie.

Dezinformacja zmierza w przeciwnym kierunku i to właśnie przy tej pozycji proszę Cię o chwilę namysłu. Głosujący umieścili ją blisko końca listy. Rejestr incydentów umieścił ją blisko szczytu — to największa rozbieżność w kierunku, który faktycznie szkodzi, gdzie głosowanie wypada nisko, a dowody wysoko. Lista nadal sytuuje Dezinformację pośrodku: większą wagę ma głosowanie, ale dowody podniosły tę pozycję wyżej. Sedno tkwi właśnie w tej rozbieżności. Gdy płynny, pewny siebie wynik modelu steruje decyzją lub wywołaniem narzędzia, błędna odpowiedź zamienia się w błędne działanie, a rejestr pokazuje, że taka awaria zdarza się częściej, niż zakłada głosowanie.

Głosowanie społeczności ma trzy czwarte wagi. Dane o incydentach odpowiadają za pozostałą jedną czwartą. Celowo nadaliśmy głosowaniu dużą wagę. Ta lista jest produktem konsensusu i jeden rok zaszumionych danych nie może obalić oceny osób, które na co dzień wykonują tę pracę. Waga jednej czwartej wystarcza, by przesunąć pozycję o jeden poziom, gdy rozbieżność między przekonaniami a dowodami jest duża. Nie wystarcza jednak, by niedoskonałe dane samodzielnie przepisały listę. Ta równowaga przesądziła o każdym ostatecznym miejscu w Top 10.

### Co nowego w Top 10 na rok 2026

![Wykres typu bump chart przedstawiający zmiany miejsc poszczególnych pozycji między listą z 2025 roku a listą na rok 2026](report/images/OWASP%20LLM%20Top10%202025-2026%20Bump%20Chart.png)

*Rysunek 1: Zmiany pozycji między listą OWASP GenAI/LLM Top 10 z 2025 roku a ostateczną listą na rok 2026, oznaczone kolorami według rodzaju zmiany (bez zmian, awans, spadek lub zmiana nazwy/zakresu).*

Kolejność zmieniła się bardziej niż w poprzednich latach, a przesunięcia odzwierciedlają właśnie tę rozbieżność między przekonaniami a dowodami. Nadmierna sprawczość awansowała na trzecie miejsce — to najbardziej znacząca zmiana na liście — ponieważ zarówno głosowanie, jak i rejestr incydentów zgodnie wskazują, że to wdrożenia agentowe są miejscem, w którym powstają szkody. Nieograniczona konsumpcja przesunęła się o cztery miejsca w górę dzięki praktykom, którzy oceniają wyczerpanie zasobów i kosztów wyżej, niż wskazywała jej dawna pozycja. Najbardziej spadło Nieprawidłowe przetwarzanie wyników — z piątego na dziesiąte miejsce. Wstrzyknięcie polecenia utrzymało pierwsze miejsce dzięki wynikom głosowania i stojącemu za nimi efektowi obrony. Ujawnianie informacji poufnych utrzymało drugie miejsce — jedyne miejsce w czołówce, w którym przekonania i dowody są po prostu zgodne i w którym nasza pewność jest najwyższa. Dawny Wyciek monitu systemowego to obecnie Ujawnienie ukrytego kontekstu — szersze ujęcie tej samej awarii, polegającej na nieuprawnionym dostępie do informacji, które powinny pozostać poza zasięgiem.

Kilka pozycji zostało również rozszerzonych. Nowsze, bardziej precyzyjnie zdefiniowane ryzyka włączyliśmy do pozycji, do których już one należą. Tworzenie wąskich nowych kategorii rozdrobniłoby listę bez żadnej korzyści. Wstrzyknięcie polecenia obejmuje teraz ataki międzymodalne (cross-modal), czyli takie, które ukrywają instrukcje w obrazie lub ścieżce dźwiękowej. Łańcuch dostaw uwzględnia teraz naruszenie zaufania w sytuacji, gdy promowany artefakt modelu nie jest tym, za co się podaje. Zatruwanie danych i modeli obejmuje teraz subwersję dostrajania (fine-tuning). Nieprawidłowe przetwarzanie wyników obejmuje teraz niebezpieczny kod generowany na dużą skalę przez asystentów. Incydenty stojące za tymi ryzykami zawsze były realne. Teraz są liczone tam, gdzie należą.

Jedna granica z roku na rok nabiera coraz większego znaczenia. Ta lista obejmuje ryzyko wtedy, gdy model jest komponentem Twojej aplikacji. W chwili, gdy model staje się podmiotem działającym — z narzędziami, które może wywoływać, pamięcią przenoszoną między sesjami i skutkami, które uruchamia w dalszych etapach przetwarzania — ryzyko przechodzi do OWASP Agentic Top 10. Wiele przeanalizowanych przez nas incydentów leży dokładnie na tej granicy. Pozycje z tej listy czytaj pod kątem awarii modelu jako komponentu. Gdy Twój model zaczyna działać samodzielnie, korzystaj z niej łącznie z listą Agentic, ponieważ żadna z nich z osobna nie obejmuje tego obszaru.

### Kolejne kroki

Możesz zaufać tej liście i działać na jej podstawie już dziś. Łączy ona ocenę praktyków, którzy atakują te systemy i ich bronią, z rejestrem tego, co faktycznie poszło nie tak w praktyce — przy czym każde z tych źródeł zostało zweryfikowane względem drugiego. Właśnie takie ugruntowanie wcześniejsze wersje mogły jedynie obiecywać. Zajmij się wszystkimi dziesięcioma pozycjami, zacznij od góry i przygotuj każdą z nich na dzień, w którym model zostanie zwrócony przeciwko Tobie. Jeśli to zrobisz, obejmiesz zarówno to, czego branża obawia się najbardziej, jak i to, na czym już się sparzyła.

Podobnie jak technologia, której dotyczy, lista ta jest wynikiem pracy społeczności. Została ukształtowana przez programistów, analityków danych i specjalistów ds. bezpieczeństwa, którzy wnieśli do tej pracy swoją ocenę, a w tym roku również swoje rejestry incydentów. Dziękujemy wszystkim, którzy wnieśli wkład, spierali się z nami i zachęcali nas do konfrontowania naszych przekonań z dowodami. Mamy nadzieję, że lista pomoże Ci chronić to, co budujesz.

#### Steve Wilson

Kierownik projektu OWASP Top 10 dla aplikacji wykorzystujących duże modele językowe

LinkedIn: <https://www.linkedin.com/in/wilsonsd/>

#### Rock Lambros

Współkierownik projektu OWASP Top 10 dla aplikacji wykorzystujących duże modele językowe

LinkedIn: <https://www.linkedin.com/in/rocklambros>
