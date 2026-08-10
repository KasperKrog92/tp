# TP-bil oversigt — deploy

Statisk side (én fil, `index.html`, ingen dependencies). Viser TP-bilernes rotation
pr. dagtype med interaktiv "hvor er bilerne kl. X"-visning. **Ingen persondata** —
kun vagtnumre og tider.

Siden åbner i **"Nu"-tilstand**: tidspunktet sættes til klokken lige nu og følger
med automatisk (opdaterer hvert 30. sekund + når fanen får fokus igen). Vælger
brugeren selv et tidspunkt (felt, slider, presets, ◀/▶ eller klik på tidslinjen),
slås Nu-tilstanden fra; et tryk på den udfyldte "Nu"-knap slår den til igen.

## Hosting på tp.kasper-krog.dk (GitHub Pages)

kasper-krog.dk kører allerede på GitHub Pages, så:

1. Opret et nyt repo på GitHub, fx `tp` (kan være privat med GitHub Pro; ellers public).
2. Læg `index.html` (og denne README) i repoets rod og push.
3. Repo → Settings → Pages → Source: "Deploy from a branch" → `main` / rod.
4. Samme side → Custom domain: skriv `tp.kasper-krog.dk` → Save. Slå "Enforce HTTPS" til, når certifikatet er klar (kan tage nogle minutter).
5. Hos din DNS-udbyder (dér hvor kasper-krog.dk's DNS ligger): opret en **CNAME-record**:
   - Navn/host: `tp`
   - Værdi/peger på: `<dit-github-brugernavn>.github.io`
6. Vent på DNS (typisk minutter) → siden svarer på https://tp.kasper-krog.dk

Alternativ uden DNS-opsætning: læg filen som `tp/index.html` i det eksisterende
website-repo → siden svarer på `kasper-krog.dk/tp`.

## Opdatering af data

Al data ligger i `DAYS`-blokken øverst i `index.html`'s `<script>` — én blok pr.
dagtype (Man–tors er udfyldt; Fredag/Lørdag/Søndag står som `cars: null` og viser
en pladsholder, indtil de udfyldes). Format er dokumenteret i kommentaren over
blokken: en tur er `{ dep:"7:14", to:"AR"|"CMY", vagt:"1116", note:"..." }` —
fra-sted og ankomst (+12 min) udledes automatisk.

Fredags-/weekendplanernes TP-linjer skal aflæses fra de fysiske vagtplaner
(samme metode som Man–tors) — se `../CLAUDE.md` i projektroden.

Siden er markeret `noindex` og viser bevidst **ingen førernavne** (kan tilføjes
senere — datastrukturen er forberedt på et ekstra felt pr. tur).
