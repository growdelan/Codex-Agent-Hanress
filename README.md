# Codex Flow — lekki harness dla projektów Python

To repozytorium jest szablonem współpracy z aktualnym Codexem. Zachowuje trwałe zasady projektu, planowanie z PRD, implementację ograniczoną do milestone'u, walidację i niezależne review, ale nie dubluje natywnej orkiestracji, zarządzania modelem ani kompakcji kontekstu Codex.

## Założenia

- `AGENTS.md` zawiera tylko trwałe reguły repozytorium.
- `spec.md` opisuje zachowanie i decyzje techniczne.
- `ROADMAP.md` dzieli pracę na mierzalne przyrosty.
- `STATUS.md` jest krótkim handoffem między sesjami.
- Skille opisują powtarzalne workflow i są ładowane dopiero wtedy, gdy pasują do zadania.
- Subagenci są używani tylko do niezależnej, wyspecjalizowanej pracy, gdy daje to realną korzyść.
- Commit i push wymagają jawnego polecenia użytkownika.

## Użycie szablonu

Skopiuj do projektu:

```text
.agents/
.codex/
prd/
scripts/
AGENTS.md
ROADMAP.md
STATUS.md
spec.md
```

Po skopiowaniu:

1. Dopasuj komendy w `AGENTS.md` i `scripts/verify.sh` do projektu.
2. Wypełnij pierwszy PRD w `prd/000-initial-prd.md`.
3. Wygeneruj specyfikację i roadmapę przez `$codex-flow-plan-from-prd`.
4. Realizuj jeden uzgodniony milestone przez `$codex-flow-implement-milestone`.
5. Dla większych lub ryzykownych zmian wykonaj `$codex-flow-review`.
6. Jeśli review wykryje problemy, użyj `$codex-flow-address-review`.
7. Publikuj zmianę przez `$codex-flow-publish` tylko na jawne polecenie dotyczące commita lub pusha.

Przy powrocie do projektu użyj `$codex-flow-resume`.

## Skille

| Skill | Zastosowanie |
|---|---|
| `codex-flow-resume` | Odtworzenie stanu projektu bez zmian w plikach |
| `codex-flow-plan-from-prd` | Pierwszy lub przyrostowy PRD → `spec.md` i `ROADMAP.md` |
| `codex-flow-implement-milestone` | Implementacja jednego, jawnie wskazanego milestone'u |
| `codex-flow-review` | Niezależne, read-only review ostatnich zmian |
| `codex-flow-address-review` | Minimalne poprawki wynikające z review |
| `codex-flow-publish` | Dokumentacja, commit i opcjonalny push na jawne polecenie |

## Subagenci

- `planner` — analizuje PRD, specyfikację i roadmapę; nie implementuje.
- `reviewer` — ocenia diff, testy i zgodność zakresu w trybie read-only.

Główny agent prowadzi implementację i orkiestrację. Nie ma osobnych agentów implementacyjnego, operacyjnego ani orkiestratora, ponieważ ich odpowiedzialności dublowały natywne zachowanie Codex lub tworzyły niepotrzebne handoffy.

## Walidacja

Uruchom:

```bash
./scripts/verify.sh
```

Skrypt nie zgaduje narzędzi projektu. Uruchamia `unittest` tylko wtedy, gdy istnieje katalog `tests/`, a dodatkowe polecenia należy dodać jawnie po skonfigurowaniu projektu.

## Zasady publikacji

Samo zakończenie implementacji lub sesji nie oznacza zgody na commit ani push. `$codex-flow-publish` wykonuje wyłącznie operacje wskazane przez użytkownika:

- „przygotuj zmiany” — aktualizacja dokumentacji i weryfikacja bez commita,
- „commit” — przygotowanie commita bez pusha,
- „push” lub „opublikuj” — commit, jeśli potrzebny, i push po pozytywnej walidacji.
