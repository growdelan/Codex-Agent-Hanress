---
name: codex-flow-plan-from-prd
description: Utwórz lub zaktualizuj spec.md i ROADMAP.md na podstawie pierwszego albo przyrostowego PRD z katalogu prd, bez implementowania kodu. Użyj, gdy użytkownik wskazuje PRD i chce otrzymać spójną specyfikację, mierzalne milestone'y, ryzyka i walidacje.
---

# Planowanie z PRD

1. Ustal wskazany plik PRD. Jeśli użytkownik go nie podał, wybierz go tylko wtedy, gdy w `prd/` istnieje jeden oczywisty kandydat.
2. Przeczytaj PRD, `spec.md`, `ROADMAP.md` i `STATUS.md`. Kod doczytaj tylko wtedy, gdy przyrostowy PRD zmienia istniejące zachowanie, którego nie da się ocenić z dokumentacji.
3. Rozpoznaj, czy PRD inicjuje projekt, czy rozszerza istniejący zakres.
4. Zaktualizuj `spec.md`: cel, zachowanie, architekturę i tylko uzasadnione decyzje techniczne. Nie usuwaj wcześniejszych decyzji bez jawnej podstawy.
5. Zaktualizuj `ROADMAP.md`. Każdy milestone musi mieć cel, mierzalne kryteria akceptacji, zakres, poza zakresem, walidację oraz ryzyka lub zależności.
6. Zaproponuj minimalny pionowy slice tylko wtedy, gdy jest właściwy dla rodzaju projektu i realnie weryfikuje najważniejsze założenie.
7. Nie zgaduj brakujących wymagań. Zapisz konkretne `TODO: [pytanie]` albo poproś użytkownika o decyzję, jeśli wpływa ona na plan.
8. Nie zmieniaj kodu aplikacji.

Podsumuj użyty PRD, zmienione dokumenty, milestone'y, ryzyka, TODO i rekomendowany następny krok.
