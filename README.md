# Codex Flow — OpenAI Codex Python agent harness template

To repozytorium jest szablonem pracy z agentami kodującymi w projektach Pythonowych. Nie jest przypisane do jednego produktu — kopiujesz jego pliki do nowego repozytorium i używasz ich jako harnessu: zestawu ról, skillów, zasad kontekstu, walidacji i metryk jakości.

Szablon jest przygotowany pod OpenAI Codex. Domyślnym formatem pracy jest patch/diff, małe hunki, jawne ścieżki plików i dynamiczne doczytywanie kontekstu.

Najważniejsza zmiana względem prostego „prompt + model” jest taka, że traktujemy harness jak produkt: wersjonujemy instrukcje, nie mieszamy ról, mierzymy jakość i dopasowujemy sposób edycji do Codex.

## 1. Skopiowanie szablonu do nowego repozytorium

W nowym repozytorium skopiuj co najmniej:

```text
.agents/
.codex/
prd/
prompts/
src/
AGENTS.md
ROADMAP.md
STATUS.md
spec.md
```

Opcjonalnie skopiuj również `README.md`, jeśli chcesz mieć tę instrukcję w projekcie. Nie kopiuj `.git/`. `.gitignore` możesz skopiować albo połączyć z istniejącym `.gitignore` projektu.

## 2. Utworzenie PRD w Codex

PRD twórz w Codex w trybie `plan`, zanim zaczniesz generować `spec.md`, `ROADMAP.md` albo kod. Celem tego kroku jest zebranie kontekstu produktu, a nie implementacja.

Dla nowego projektu albo nowej funkcjonalności uruchom Codex i użyj promptu:

```text
Stwórz PRD dla nowej funkcjonalności: **[tutaj opisz aplikację albo funkcjonalność, którą chcesz dodać]**.

Przeprowadź ze mną wywiad. Zadawaj mi pytania jedno po drugim, aż poznasz cały potrzebny kontekst do stworzenia dokumentu dla mojej aplikacji.

Nie implementuj kodu. Pracuj w trybie planowania.

Gdy będziesz miał wystarczający kontekst, przejdź do jednego z poniższych działań:
- jeśli to pierwszy PRD projektu, wypełnij plik `prd/000-initial-prd.md`,
- jeśli to kolejny PRD, utwórz następny plik w katalogu `prd/` z odpowiednim numerem i krótką nazwą, np. `prd/001-nazwa-funkcji.md`.
```

Jeśli to pierwszy PRD projektu, po jego przygotowaniu poproś Codex, żeby podsumował całą aplikację jednym zdaniem. To jedno zdanie możesz wkleić na początek docelowego `README.md` projektu.

Po utworzeniu PRD przejdź do przepływu `PRD → specyfikacja i roadmapa` z sekcji 4.2.

## 3. Zasady harnessu, których pilnujemy zawsze

1. **Jeden model-harness na rozmowę lub zadanie.** Repo jest przygotowane pod OpenAI Codex. Nie przełączaj wariantu modelu w środku pracy nad milestone’em. Gdy przełączenie w obrębie OpenAI jest konieczne, zrób świeży handoff według `.agents/harness/HANDOFF-TEMPLATE.md` albo uruchom osobnego subagenta od czystego kontekstu.
2. **Używaj codexowego formatu edycji.** Dla OpenAI/Codex preferuj patch/diff, małe hunki, `apply_patch`, jawne ścieżki plików i precyzyjne zakresy zmian. Szczegóły są w `.agents/harness/MODEL-PROFILES.md`.
3. **Ładuj kontekst dynamicznie.** Na starcie czytaj tylko minimum: `AGENTS.md`, `STATUS.md`, odpowiedni fragment `ROADMAP.md`, `spec.md` i aktualne zadanie. Kod, testy i PRD doczytuj dopiero wtedy, gdy są potrzebne.
4. **Przed implementacją twórz kontrakt.** Każdy milestone lub batch ma mieć jasny zakres, rzeczy poza zakresem, kryteria akceptacji, komendy walidacyjne i stop conditions. Szablon: `.agents/harness/SPRINT-CONTRACT-TEMPLATE.md`.
5. **Oddziel generowanie od oceny.** Implementer pisze kod. Reviewer ocenia w trybie read-only i ma być sceptyczny. Operations finalizuje dokumentację, commit i push.
6. **Klasyfikuj błędy narzędzi.** Każdy istotny błąd narzędzia oznacz kategorią z `.agents/harness/TOOL-ERRORS.md` i zapisz w `STATUS.md`, jeśli wpłynął na decyzje lub walidację.
7. **Mierz jakość.** Minimum to: wynik walidacji, liczba istotnych problemów z review, błędy narzędzi i „keep rate” zmian po kolejnych iteracjach. Reguły: `.agents/harness/QUALITY-METRICS.md`.

## 4. Standardowy przepływ pracy

### 4.1. Start lub kontynuacja projektu

Użyj:

```text
$codex-flow-continue-project
```

Agent powinien odtworzyć stan projektu bez implementacji: jaki jest produkt, aktualny milestone, najbliższy krok, ryzyka, dostępne komendy i brakujące informacje.

### 4.2. PRD → specyfikacja i roadmapa

Dla pierwszego PRD użyj:

```text
$codex-flow-generate-spec-from-prd
```

Dla kolejnego przyrostowego PRD użyj:

```text
$codex-flow-next-prd
```

PRD powinno trafić do `prd/`. Pierwszy plik to zwykle `prd/000-initial-prd.md`. Planner nie implementuje kodu — aktualizuje tylko `spec.md`, `ROADMAP.md` i ewentualnie `STATUS.md`.

Po tej czynności sprawdź `spec.md` i `ROADMAP.md`. Jeżeli trzeba coś poprawić, poprawiaj to przez Codex w trybie planowania, bez ręcznego rozjeżdżania dokumentów.

### 4.3. Milestone 0.5 — minimalny end-to-end slice

Milestone 0.5 buduje najprostszy działający szkielet aplikacji:

```text
$codex-flow-implement-milestone-0-5
```

Po implementacji uruchom review:

```text
$codex-flow-self-review
```

Jeśli review znajdzie realne problemy:

```text
$codex-flow-apply-self-review-fixes
$codex-flow-self-review
```

Finalizację wykonuje dopiero operations:

```text
$codex-flow-finalize-and-push-change
```

### 4.4. Kolejne milestone’y

Dla jednego milestone’u:

```text
$codex-flow-implement-milestone
$codex-flow-self-review
$codex-flow-apply-self-review-fixes   # tylko jeśli review wykryło problemy
$codex-flow-finalize-and-push-change
```

Dla wielu milestone’ów naraz używaj ostrożnie. Preferowany jest prompt multi-agentowy:

```text
prompts/agents_all_milestones.md
```

Alternatywa bez jawnych subagentów:

```text
$codex-flow-implement-planned-batches
```

## 5. Role agentów

- `harness_orchestrator_agent` — układa przepływ, pilnuje profilu OpenAI/Codex, kontraktów, handoffów i bramek. Nie implementuje kodu.
- `planning_agent` — PRD, `spec.md`, `ROADMAP.md`, odtwarzanie kontekstu. Nie implementuje.
- `implementation_agent` — kod, testy, poprawki po review. Nie finalizuje, nie wykonuje commita i nie pushuje.
- `review_agent` — niezależny evaluator w trybie read-only. Nie poprawia plików.
- `operations_agent` — dokumentacja operacyjna, metryki, commit, push, wrap-up. Nie zmienia kodu aplikacji.

## 6. Domyślne zasady dla projektów Python

- Środowisko i zależności: `uv`.
- Testy: `unittest`, komenda bazowa:

```bash
uv run python -m unittest discover -s tests -p "test_*.py"
```

- Kod trzymaj w `src/`, testy w `tests/`.
- Nie przechowuj sekretów w repo.
- Każda nowa zależność ma mieć uzasadnienie w `spec.md` w sekcji `## Decyzje techniczne`.

## 7. Przydatne pliki harnessu

```text
.agents/harness/HARNESS.md                 # główne zasady harnessu
.agents/harness/MODEL-PROFILES.md          # profil OpenAI/Codex i format edycji
.agents/harness/CONTEXT.md                 # dynamiczne ładowanie kontekstu
.agents/harness/TOOL-ERRORS.md             # taksonomia błędów narzędzi
.agents/harness/QUALITY-METRICS.md         # keep rate i bramki jakości
.agents/harness/SPRINT-CONTRACT-TEMPLATE.md
.agents/harness/HANDOFF-TEMPLATE.md
```

## 8. Zakończenie sesji

Gdy kończysz pracę w danym kontekście:

```text
$codex-flow-project-wrap-up
```

Wrap-up powinien zapisać w `STATUS.md` aktualny stan, następny krok, walidację, istotne błędy narzędzi i ewentualny handoff dla następnej rozmowy.
