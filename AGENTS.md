# AGENTS.md

## Język

- Komunikuj się z użytkownikiem wyłącznie po polsku.
- Dodawaj komentarze w kodzie tylko wtedy, gdy wyjaśniają nieoczywistą logikę.

## Kontekst projektu

- Trwałe decyzje produktowe i techniczne zapisuj w `spec.md`.
- Plan i statusy prac utrzymuj w `ROADMAP.md`.
- Bieżący stan, ostatnią walidację, blokery i następny krok zapisuj w `STATUS.md`.
- Czytaj tylko pliki potrzebne do aktualnej decyzji. Przy kontynuacji zacznij od `STATUS.md` i właściwego fragmentu `ROADMAP.md`.

## Środowisko Python

- Używaj `uv` do środowiska i zależności. Dodawaj zależności przez `uv add <pakiet>`.
- Nie twórz alternatywnych virtualenvów ani zależności „na zapas”.
- Uzasadniaj nowe zależności w `spec.md` w sekcji `## Decyzje techniczne`.
- Kod aplikacji trzymaj w `src/`, a testy w `tests/`.
- Preferuj jeden główny entrypoint opisany w `README.md`.

## Implementacja

- Ustal zakres i kryteria akceptacji przed większą lub niejasną zmianą.
- Wprowadzaj małe, precyzyjne zmiany bez szerokich refaktorów „przy okazji”.
- Nie rozszerzaj zakresu milestone'u bez uzasadnienia i aktualizacji planu.
- Nie przechowuj sekretów w repo. Używaj zmiennych środowiskowych i dokumentuj je w `README.md`.
- Stosuj 4 spacje, `snake_case` dla modułów i funkcji oraz `PascalCase` dla klas.

## Walidacja i review

- Używaj komend zdefiniowanych przez repo. Podstawowa komenda tego szablonu to `./scripts/verify.sh`.
- Testy i smoke testy nie mogą wymagać prawdziwych sekretów ani niestabilnych usług zewnętrznych.
- Po istotnej zmianie zapisz w `STATUS.md` wykonaną walidację i jej wynik.
- Dla ryzykownych lub większych zmian wykonaj niezależne review diffu przed publikacją.
- Nie ukrywaj nieprzechodzącej walidacji ani problemów blokujących.

## Git i dokumentacja

- Nie wykonuj commita ani pusha bez jawnego polecenia użytkownika.
- Przed publikacją sprawdź `git status --short`, diff i wynik walidacji.
- Aktualizuj `README.md` tylko wtedy, gdy zmienia się uruchamianie, konfiguracja, użycie lub ważne zachowanie systemu.
- Nie nadpisuj zmian użytkownika ani nie włączaj do commita zmian spoza uzgodnionego zakresu.
