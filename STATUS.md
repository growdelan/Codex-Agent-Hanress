# Aktualny stan projektu

Ten plik jest krótką pamięcią operacyjną między sesjami. Zapisuj tylko informacje potrzebne do bezpiecznej kontynuacji pracy.

## Aktualny zakres

- Milestone / zadanie: Rozdzielenie realizacji roadmapy od finalizacji dokumentacji.
- Status: Zaimplementowane i zweryfikowane lokalnie; bez commita i pusha.
- Poza zakresem: Zmiany cache'owanego skilla `github:yeet` oraz uruchamianie rzeczywistej roadmapy.

## Co zrobiono

- Powiązano zasady code-only bezpośrednio z zadaniami zleconymi przez `$codex-flow-run-roadmap`, bez dodatkowego trybu lub etykiety.
- Review w tej pętli obejmuje diff implementacyjny milestone'u, a braki dokumentacyjne nie mogą powodować `CHANGES_REQUIRED`.
- Finalizację statusów i dokumentacji przeniesiono do jednorazowego `$codex-flow-publish` przed `github:yeet`.

## Co jest następne

- Opcjonalnie wykonać commit i push na jawne polecenie użytkownika.

## Ostatnia walidacja

| Data | Zakres | Komenda / sposób | Wynik | Uwagi |
|---|---|---|---|---|
| 2026-08-16 | Pętla code-only, agenci i publikacja | `quick_validate.py` dla 5 skilli; parsowanie TOML przez `tomllib`; `./scripts/verify.sh`; `git diff --check` | PASS | Repo nie zawiera katalogu `tests/`, więc unittest został pominięty zgodnie ze skryptem. |

## Blokery i ryzyka

- Brak aktywnych blokerów. Koordynator musi jawnie wskazać agentom, że zlecenie jest częścią `$codex-flow-run-roadmap`.

## Handoff

- Najkrótsze streszczenie: Pętla roadmapy implementuje i reviewuje kod bez aktualizacji dokumentacji; publikacja synchronizuje dokumentację raz na końcu.
- Decyzje, których nie wolno zgubić: Dokumentacja pozostaje read-only źródłem kryteriów; brak jej aktualizacji nie jest findingiem; kolejność to `run-roadmap` → `codex-flow-publish` → `github:yeet`.
- Pliki do przeczytania jako pierwsze: `.agents/skills/codex-flow-run-roadmap/SKILL.md`, `.codex/agents/sol-implementer.toml`, `.codex/agents/sol-reviewer.toml`.
- Następny bezpieczny krok: Sprawdzić końcowy diff, a commit lub push wykonać tylko na jawne polecenie.
