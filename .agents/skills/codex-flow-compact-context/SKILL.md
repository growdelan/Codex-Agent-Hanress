---
name: codex-flow-compact-context
description: Bezpiecznie uporządkuj i zmniejsz STATUS.md, ROADMAP.md lub spec.md, archiwizując zakończoną historię i dzieląc szczegóły bez zmiany aktualnego zachowania systemu. Użyj zapobiegawczo przed rozbudową dokumentacji, gdy zakończone treści utrudniają odczytanie bieżącego stanu, po ostrzeżeniu scripts/check-context-size.sh, przed resume dużego projektu albo na prośbę użytkownika.
---

# Kompakcja kontekstu projektu

1. Uruchom `./scripts/check-context-size.sh` i potraktuj wynik jako sygnał ostrzegawczy, a nie jedyny warunek porządkowania.
2. Przed edycją ustal aktualny milestone, aktywne decyzje, blokery i ostatnią istotną walidację. Porównaj dokumentację z repo, aby nie archiwizować informacji nadal potrzebnej operacyjnie.
3. Kompaktuj tylko pliki wymagające porządkowania: przekraczające próg albo zawierające zakończoną historię, powtórzenia lub szczegóły przesłaniające aktualny stan. Nie usuwaj potrzebnej treści wyłącznie w celu obniżenia liczby linii. Nie zmieniaj kodu, zachowania produktu ani statusów niezgodnie z faktami.

## STATUS.md

- Zachowaj aktualny zakres, stan pracy, następny krok, aktywne blokery, ostatnią istotną walidację i krótki handoff.
- Usuń powtórzenia, pełne logi i zamkniętą historię. Historia pozostaje w Git; nie twórz osobnego archiwum statusu bez konkretnej potrzeby audytowej.

## ROADMAP.md

- Zachowaj pełne szczegóły milestone'ów `planned`, `in_progress` i `blocked`.
- Przenieś szczegóły milestone'ów `done`, które nie są już potrzebne aktywnym zależnościom, do `docs/archive/roadmap/<wersja-lub-data>.md`. Rób to przed dodaniem kolejnego przyrostu roadmapy, zamiast czekać wyłącznie na przekroczenie limitu.
- W głównej roadmapie pozostaw krótką listę ukończonych milestone'ów i link do archiwum. Nie zmieniaj kolejności ani zależności aktywnej pracy.

## spec.md

- Zachowaj w `spec.md` aktualny cel, granice systemu, mapę komponentów, najważniejsze kontrakty i indeks dokumentów szczegółowych.
- Przenieś rozbudowane aktualne obszary domenowe do `docs/spec/<obszar>.md`, a samodzielne decyzje do `docs/decisions/<numer>-<nazwa>.md`.
- Nie przenoś aktualnej prawdy do archiwum i nie duplikuj pełnej treści między plikami. Linki z `spec.md` muszą jasno wskazywać, co należy doczytać dla danego obszaru.

Po zmianach sprawdź linki, uruchom `./scripts/check-context-size.sh` oraz `./scripts/verify.sh`. Jeśli próg nadal jest przekroczony, wyjaśnij dlaczego zamiast usuwać potrzebne informacje. Nie wykonuj commita ani pusha bez jawnego polecenia użytkownika.
