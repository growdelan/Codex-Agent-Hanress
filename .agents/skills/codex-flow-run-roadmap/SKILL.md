---
name: codex-flow-run-roadmap
description: Wykonaj w kontrolowanej pętli wszystkie wykonalne milestone'y ze statusem planned z ROADMAP.md przez stały duet sol_implementer i sol_reviewer dla każdego milestone'u, kończąc zatwierdzony milestone osobnym lokalnym commitem. Użyj, gdy użytkownik jawnie prosi o realizację całej roadmapy, wszystkich zaplanowanych milestone'ów albo autonomiczną pracę aż do ukończenia planu lub napotkania blokera.
---

# Wykonanie roadmapy

## Przygotowanie

1. Uruchom `./scripts/check-context-size.sh`. Przeczytaj `AGENTS.md`, `STATUS.md`, aktywne milestone'y w `ROADMAP.md` oraz istotne sekcje lub odnośniki z `spec.md`; nie czytaj archiwum domyślnie.
2. Sprawdź stan repo i nie włączaj do pracy niepowiązanych zmian użytkownika.
3. Zbierz na początku rzeczywiste milestone'y `planned` w kolejności zależności. Pomiń wpisy szablonowe i elementy zablokowane, a postęp tej listy śledź w pamięci koordynatora bez ponownego wybierania milestone'ów na podstawie niezmienionego `ROADMAP.md`.
4. Przed implementacją pokaż krótki plan kolejności. Nie czekaj na dodatkowe potwierdzenie, jeśli polecenie użytkownika jest jednoznaczne i nie ma konfliktu zakresu.
5. Główny wątek pełni rolę koordynatora. Nie implementuje kodu; deleguje pracę agentom opisanym poniżej i scala ich wyniki.

## Zakres implementacji i review

- W każdym poleceniu do implementera i reviewera zaznacz, że zadanie jest częścią `$codex-flow-run-roadmap` i obowiązują zasady z tej sekcji.
- Dokumentacja jest wyłącznie źródłem wymagań. Implementer nie modyfikuje `ROADMAP.md`, `STATUS.md`, `spec.md` ani `README.md`, nie zapisuje w nich statusów `in_progress`, `done` ani `blocked` i nie wykonuje poprawek dotyczących wyłącznie kompletności dokumentacji.
- Diff implementacyjny obejmuje kod, testy, migracje, konfigurację wykonawczą i inne artefakty runtime. Nie obejmuje plików zarządzania projektem ani `README.md`.
- Niespełnione kryterium opisane w dokumentacji może blokować review tylko wtedy, gdy wskazuje konkretny problem zachowania implementacji. Sam brak aktualizacji dokumentacji nie jest findingiem i nie może powodować `CHANGES_REQUIRED`.

## Agenci i kolejność

- Używaj Custom Agenta `sol_implementer` do implementacji i wszystkich późniejszych poprawek.
- Używaj Custom Agenta `sol_reviewer` do niezależnego review.
- Agenci pracują sekwencyjnie. Nie uruchamiaj implementacji i review równolegle.
- Dla każdego milestone'u utwórz jeden wątek `sol_implementer` i zachowaj go do końca pracy nad tym milestone'em.
- Dla każdego milestone'u utwórz jeden wątek `sol_reviewer` i wznawiaj go po każdej rundzie poprawek aż do końca pracy nad tym milestone'em.
- Nie zastępuj wskazanych Custom Agents agentami wbudowanymi ani pracą głównego wątku.

## Pętla milestone'ów

Dla każdego kolejnego milestone'u:

1. Potwierdź cel, zakres, poza zakresem, kryteria akceptacji, walidację i warunki zatrzymania.
2. Zanotuj stan bazowy i rozdziel wcześniejsze zmiany użytkownika oraz zatwierdzone zmiany poprzednich milestone'ów od diffu bieżącego milestone'u.
3. Utwórz wątek `sol_implementer`. Zaznacz, że zadanie jest częścią `$codex-flow-run-roadmap`, przekaż pełny zakres milestone'u, kryteria akceptacji, właściwy kontekst z dokumentacji, stan bazowy repozytorium, wcześniejsze zmiany użytkownika, ryzyka i wymagane walidacje. Poleć wykonać `$codex-flow-implement-milestone` bez zmian dokumentacji i poczekaj na zakończenie implementacji oraz walidacji.
4. Zbierz diff implementacyjny bieżącego milestone'u, podsumowanie implementera oraz wyniki walidacji. Utwórz wątek `sol_reviewer`, zaznacz, że review jest częścią `$codex-flow-run-roadmap`, i poleć wykonać `$codex-flow-review` wyłącznie dla tego diffu.
5. Wymagaj raportu zakończonego dokładnie jedną decyzją: `DECISION: APPROVED` albo `DECISION: CHANGES_REQUIRED`.
6. Gdy decyzja to `CHANGES_REQUIRED`, przekaż pełny raport do tego samego wątku implementera. Przypomnij, że poprawki są częścią `$codex-flow-run-roadmap`, poleć wykonać `$codex-flow-address-review`, poprawić wyłącznie zasadne problemy implementacyjne, ponowić walidację i odpowiedzieć na każde znalezisko.
7. Po poprawkach wznów ten sam wątek `sol_reviewer`. Zaznacz, że ponowne review jest częścią `$codex-flow-run-roadmap`, przekaż pierwotne kryteria, wcześniejsze znaleziska i odpowiedzi implementera, pełny aktualny diff implementacyjny milestone'u oraz aktualne wyniki walidacji. Reviewer ponownie ocenia cały diff implementacyjny.
8. Powtarzaj sekwencję `ten sam sol_implementer -> ten sam sol_reviewer` do `APPROVED`, maksymalnie przez trzy rundy poprawek. Jeśli po trzeciej rundzie nadal jest `CHANGES_REQUIRED`, zatrzymaj workflow i zgłoś nierozwiązane problemy.
9. Po `APPROVED` zapisz decyzję i wyniki walidacji w pamięci koordynatora. Nie uruchamiaj implementera wyłącznie w celu finalizacji statusu lub dokumentacji.
10. Sprawdź końcowy diff implementacyjny i pozytywny wynik walidacji. Stage'uj wyłącznie zmiany bieżącego milestone'u i utwórz osobny, logiczny commit nazwany zgodnie z milestone'em. Jeśli zmian milestone'u nie da się bezpiecznie oddzielić od wcześniejszych zmian użytkownika, zatrzymaj workflow zamiast włączać je do commita.
11. Sprawdź status repozytorium i potwierdź, że pozostały worktree odpowiada stanowi bazowemu sprzed milestone'u, po czym przejdź do następnego elementu początkowej listy bez ponownego pytania użytkownika.

## Warunki zatrzymania

Zatrzymaj pętlę i zachowaj bezpieczny stan, gdy wymaganie istotnie zmienia produkt lub zakres, walidacja nie przechodzi i naprawa wykracza poza bieżący milestone, review wykryje nierozwiązany problem blokujący, limit trzech rund poprawek zostanie wyczerpany, brakuje zależności lub sekretu, występują nieoczekiwane zmiany albo użytkownik zmieni zadanie.

Nie zapisuj statusu `blocked` w dokumentacji. W handoffie wskaż milestone, konkretną blokadę oraz decyzję lub działanie potrzebne do wznowienia. Nie ogłaszaj sukcesu bez `DECISION: APPROVED`, spełnionych kryteriów i pozytywnej walidacji.

## Zakończenie

Po wyczerpaniu początkowej listy uruchom pełną dostępną walidację i sprawdź końcowy diff implementacyjny. Nie sprawdzaj kompletności ani spójności aktualizacji dokumentacji w tym workflow.

Zwróć handoff do `$codex-flow-publish` zawierający:

- stan bazowy workflow;
- każdy milestone z wynikiem `approved`, `blocked` albo `not_started`, liczbą rund poprawek i końcową decyzją review;
- wykonane i pominięte walidacje wraz z wynikami lub przyczynami pominięcia;
- nierozwiązane ryzyka i blokery;
- listę faktów, statusów milestone'ów oraz zmian zachowania, konfiguracji lub użycia, które publikacja powinna odzwierciedlić w `ROADMAP.md`, `STATUS.md`, `spec.md` lub `README.md`.

Jeśli kontrola rozmiaru zgłasza ostrzeżenia, dodaj `$codex-flow-compact-context` jako osobny rekomendowany krok w handoffie; nie rozszerzaj diffu roadmapy o kompakcję.

Uruchomienie `$codex-flow-run-roadmap` autoryzuje koordynatora do lokalnego commita po każdym zatwierdzonym milestone'ie zgodnie z powyższymi zasadami. Nigdy nie wykonuj pusha bez osobnego, jawnego polecenia użytkownika.
