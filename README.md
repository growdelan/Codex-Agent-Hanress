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
2. Utwórz pierwszy lub kolejny PRD przez `$codex-flow-create-prd`.
3. Wygeneruj specyfikację i roadmapę z gotowego PRD przez `$codex-flow-plan-from-prd`.
4. Realizuj jeden uzgodniony milestone przez `$codex-flow-implement-milestone`.
5. Dla większych lub ryzykownych zmian wykonaj `$codex-flow-review`.
6. Jeśli review wykryje problemy, użyj `$codex-flow-address-review`.
7. Jeśli chcesz wykonać wszystkie zaplanowane milestone'y w kontrolowanej pętli, użyj `$codex-flow-run-roadmap`.
8. Publikuj zmianę przez `$codex-flow-publish` tylko na jawne polecenie dotyczące commita lub pusha.

Przy powrocie do projektu użyj `$codex-flow-resume`.

## Prompt do utworzenia PRD

```text
Użyj $codex-flow-create-prd.

Stwórz PRD dla nowej funkcjonalności: **[tutaj opisz aplikację albo funkcjonalność]**.

Przeprowadź ze mną wywiad. Zadawaj jedno pytanie naraz, aż poznasz kontekst potrzebny do utworzenia PRD. Nie implementuj kodu ani nie aktualizuj jeszcze specyfikacji i roadmapy.
```

## Skille

| Skill | Zastosowanie |
|---|---|
| `codex-flow-resume` | Odtworzenie stanu projektu bez zmian w plikach |
| `codex-flow-create-prd` | Wywiad produktowy i zapis pierwszego lub kolejnego PRD |
| `codex-flow-plan-from-prd` | Pierwszy lub przyrostowy PRD → `spec.md` i `ROADMAP.md` |
| `codex-flow-implement-milestone` | Implementacja jednego, jawnie wskazanego milestone'u |
| `codex-flow-review` | Niezależne, read-only review ostatnich zmian |
| `codex-flow-address-review` | Minimalne poprawki wynikające z review |
| `codex-flow-run-roadmap` | Sekwencyjna realizacja wszystkich milestone'ów `planned` z walidacją i review |
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
