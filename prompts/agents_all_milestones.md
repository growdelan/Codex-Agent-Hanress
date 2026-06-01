Pracujemy w trybie wielu agentów, ale harness ma minimalizować spadek niezawodności przez zbyt długie łańcuchy. Celem jest wykonanie wszystkich milestone ze statusem `planned` z ROADMAP.md, z logicznym grupowaniem ich w batche, kontraktem przed implementacją, implementacją sekwencyjną, review po każdym batchu, poprawkami tylko jeśli są potrzebne oraz finalize/push tylko raz na końcu.

Użyj następujących subagentów:
- harness_orchestrator_agent
- planning_agent
- implementation_agent
- review_agent
- operations_agent

Najpierw powołaj `harness_orchestrator_agent`.

## Zadanie harness_orchestrator_agent

- przeczytaj `AGENTS.md`, `STATUS.md`, `ROADMAP.md`, `spec.md`
- przeczytaj `.agents/harness/HARNESS.md`, `.agents/harness/MODEL-PROFILES.md`, `.agents/harness/CONTEXT.md`
- potwierdź aktywny profil `openai_patch` i format patch/diff
- potwierdź zasadę: nie przełączamy wariantu modelu w środku batcha bez handoffu
- znajdź wszystkie milestone ze statusem `planned`
- zdecyduj, czy któryś milestone wymaga planning_agent przed implementacją
- pogrupuj milestone’y w batche:
  - 1 milestone, jeśli jest duży, ryzykowny lub architektonicznie odrębny
  - 2–3 milestone’y tylko jeśli są małe i logicznie powiązane
- ustal kolejność batchy zgodną z zależnościami z ROADMAP.md i spec.md
- nie implementuj kodu
- zwróć krótki plan batchy z uzasadnieniem i bramkami jakości

Po otrzymaniu planu batchy pokaż mi ten plan, a następnie przejdź przez batche sekwencyjnie.

Dla każdego batcha wykonaj poniższy cykl.

## 1. Planning gate

Jeśli batch ma niejasny zakres, konflikt ze specyfikacją albo wymaga doprecyzowania roadmapy, powołaj `planning_agent`.

Zadanie planning_agent:
- doprecyzuj tylko `spec.md`, `ROADMAP.md` lub `STATUS.md`
- nie zmieniaj kodu
- nie finalizuj
- nie zgaduj brakujących wymagań
- jeśli wymagana jest decyzja człowieka, zatrzymaj workflow z konkretnym pytaniem

## 2. Kontrakt batcha

Powołaj `implementation_agent` do przygotowania kontraktu batcha przed kodem.

Zadanie implementation_agent:
- użyj `.agents/harness/SPRINT-CONTRACT-TEMPLATE.md`
- wskaż profil `openai_patch` i format patch/diff
- wskaż zakres, poza zakresem, Definition of Done, pliki do doczytania, walidacje, ryzyka i stop conditions
- nie implementuj, dopóki kontrakt nie jest jasny

## 3. Implementacja batcha

Zadanie implementation_agent:
- realizuj milestone’y z bieżącego batcha sekwencyjnie
- dla każdego milestone przeczytaj jego cel, zakres i Definition of Done z ROADMAP.md
- implementuj wyłącznie zakres danego milestone’u
- nie dodawaj funkcji spoza milestone’u
- nie zmieniaj architektury bez wyraźnej potrzeby wynikającej z milestone’u
- jeśli wymaganie jest niejasne, doprecyzuj dokumentację albo zatrzymaj się z konkretnym pytaniem
- po każdym milestone uruchom adekwatne testy lub walidacje dla zmienionego zakresu
- klasyfikuj istotne błędy narzędzi według `.agents/harness/TOOL-ERRORS.md`
- nie wykonuj finalize
- nie wykonuj commit
- nie wykonuj push
- po zakończeniu batcha podsumuj:
  - wdrożone milestone’y
  - najważniejsze zmienione pliki
  - uruchomione testy lub walidacje
  - błędy narzędzi i ich kategorie
  - ewentualne ograniczenia albo ryzyka

## 4. Review batcha

Powołaj `review_agent`.

Zadanie review_agent:
- wykonaj niezależny przegląd zmian z bieżącego batcha
- sprawdź zgodność implementacji z kontraktem batcha, ROADMAP.md, spec.md, STATUS.md i AGENTS.md
- sprawdź, czy nie dodano zakresu spoza milestone’ów batcha
- sprawdź jakość kodu, prostotę rozwiązania, duplikację logiki, martwy kod i zbędne abstrakcje
- sprawdź sensowność testów i ryzyko regresji
- sprawdź, czy README.md, spec.md, ROADMAP.md albo STATUS.md wymagają aktualizacji z powodu wykonanych zmian
- sprawdź, czy błędy narzędzi zostały sklasyfikowane
- nie edytuj żadnych plików
- nie wdrażaj poprawek
- jeśli nie ma problemów krytycznych, napisz dokładnie:
  Self-review zakończony – brak problemów krytycznych.
- jeśli są problemy, pogrupuj je według ważności:
  - Krytyczne
  - Ważne
  - Drobne
- do każdego problemu podaj konkretną rekomendowaną poprawkę oraz informację, czy mieści się w zakresie batcha

## 5. Poprawki po review

Jeśli review_agent wykryje realne problemy, powołaj `implementation_agent` ponownie.

Zadanie implementation_agent dla poprawek:
- wdroż wyłącznie poprawki wskazane przez review_agent
- nie dodawaj nowych funkcji
- nie rozszerzaj zakresu batcha
- nie wykonuj refaktoru większego niż konieczny do naprawy problemu
- jeśli jakaś poprawka wykracza poza zakres batcha, nie wdrażaj jej i zaznacz to w podsumowaniu
- po poprawkach wykonaj ograniczoną rewalidację zmienionego zakresu
- nie wykonuj finalize
- nie wykonuj commit
- nie wykonuj push

Jeśli były poprawki, powołaj `review_agent` ponownie.

Zadanie review_agent po poprawkach:
- sprawdź tylko problemy wcześniej wykryte oraz zmienione pliki
- potwierdź, czy problemy krytyczne zostały usunięte
- nie edytuj plików
- jeśli problemy krytyczne zostały usunięte, napisz:
  Self-review zakończony – brak problemów krytycznych.
- jeśli nadal istnieje problem krytyczny, zatrzymaj workflow i opisz blokadę

## 6. Gate przejścia do następnego batcha

Przejdź do następnego batcha tylko wtedy, gdy:
- testy lub walidacje dla batcha przeszły albo brak walidacji jest jawnie uzasadniony
- review_agent nie zgłasza problemów krytycznych
- nie ma niejasności wymagających decyzji człowieka
- poprawki nie wykraczają poza zakres batcha
- nie ma nierozwiązanego `UnknownHarnessBug`

Jeśli którykolwiek warunek nie jest spełniony, zatrzymaj workflow i podsumuj blokadę. Nie przechodź do kolejnego batcha.

## 7. Końcowa walidacja całości

Po zakończeniu wszystkich batchy powołaj `review_agent` do końcowej walidacji całości.

Zadanie review_agent:
- sprawdź całość zmian wykonanych w tym workflow
- sprawdź, czy wszystkie milestone ze statusem `planned`, które były objęte planem batchy, zostały zrealizowane
- sprawdź zgodność całości z ROADMAP.md, spec.md i STATUS.md
- sprawdź, czy dokumentacja wymaga aktualizacji przed finalizacją
- sprawdź wyniki walidacji i istotne błędy narzędzi
- wskaż wpływ na keep rate, jeśli widać usunięte lub przepisane zmiany
- nie edytuj plików
- zwróć wynik końcowej walidacji

Jeśli końcowa walidacja całości nie przejdzie, powołaj `implementation_agent` do poprawek i wróć do review_agent po końcowe potwierdzenie.

## 8. Finalizacja

Gdy końcowa walidacja przejdzie, powołaj `operations_agent`.

Zadanie operations_agent:
- zaktualizuj ROADMAP.md zgodnie z rzeczywistym stanem ukończonych milestone’ów
- zaktualizuj STATUS.md, opisując aktualny stan projektu, wynik walidacji, review, błędy narzędzi, keep rate, ograniczenia i następny sensowny krok
- zaktualizuj spec.md tylko wtedy, gdy wykonane zmiany realnie zmieniły ustalenia lub decyzje techniczne
- zaktualizuj README.md tylko wtedy, gdy zmienił się sposób użycia, uruchamiania, wymagania albo ważne zachowanie systemu
- nie zmieniaj kodu aplikacji
- przygotuj jeden commit dla całej paczki zmian
- wykonaj push tylko wtedy, gdy repo ma poprawnie skonfigurowany remote i stan jest gotowy
- nie wykonuj pustych commitów
- jeśli push zostanie pominięty, wyjaśnij dlaczego

## Zasady globalne

- używaj subagentów jawnie
- harness_orchestrator_agent koordynuje i nie implementuje
- planning_agent planuje i nie implementuje
- implementation_agent implementuje i poprawia, ale nie finalizuje
- review_agent ocenia i nie edytuje plików
- operations_agent finalizuje, ale nie zmienia kodu aplikacji
- nie przełączaj wariantu modelu w środku batcha bez handoffu
- nie traktuj zmiany `model_reasoning_effort` między subagentami jako model switch, jeśli wariant modelu pozostaje ten sam
- nie rozszerzaj zakresu milestone’ów
- nie zgaduj niejasnych wymagań
- nie wykonuj finalize, commit ani push po pojedynczym milestone ani po pojedynczym batchu
- minimalizuj zbędne pełne testy, ale zawsze wykonuj adekwatną walidację zmienionego zakresu
- zatrzymaj workflow przy problemie krytycznym, nieprzechodzących testach, `UnknownHarnessBug` albo wymaganej decyzji człowieka

Na końcu podsumuj:
- plan batchy
- aktywny profil `openai_patch`
- ukończone milestone’y
- wynik każdego batcha
- wyniki review
- wdrożone poprawki
- wynik końcowej walidacji
- błędy narzędzi
- obserwacje keep rate
- zaktualizowane dokumenty
- status commit i push
