# TP-bil oversigt — tp.kasper-krog.dk

Statisk side (én fil, `index.html`, ingen dependencies). Viser TP-bilernes rotation
pr. dagtype med interaktiv "hvor er bilerne kl. X"-visning. **Ingen persondata** —
kun vagtnumre og tider. Siden er markeret `noindex`.

Siden åbner i **"Nu"-tilstand**: tidspunktet sættes til klokken lige nu og følger
med automatisk (opdaterer hvert 30. sekund + når fanen får fokus igen). Vælger
brugeren selv et tidspunkt (felt, slider, presets, ◀/▶ eller klik på tidslinjen),
slås Nu-tilstanden fra; et tryk på den udfyldte "Nu"-knap slår den til igen.

## Drift (opsat 10-08-2026)

| Hvad | Værdi |
|---|---|
| Repo | https://github.com/KasperKrog92/tp (public — krav for gratis GitHub Pages) |
| Hosting | GitHub Pages, branch `main`, rod |
| Domæne | `tp.kasper-krog.dk` (styret af `CNAME`-filen i repoet — slet/ret ikke) |
| DNS | CNAME-record: navn `tp` → `kasperkrog92.github.io` (hos kasper-krog.dk's DNS-udbyder) |
| HTTPS | GitHub udsteder certifikat automatisk, når DNS-recorden svarer. Slå derefter "Enforce HTTPS" til: repo → Settings → Pages (eller `gh api repos/KasperKrog92/tp/pages -X PUT -F https_enforced=true`) |

**Opdatering af siden**: redigér `index.html` her i mappen → commit → `git push`.
GitHub Pages bygger automatisk (typisk live på under et minut). Denne mappe er sit
eget git-repo, adskilt fra resten af vagtplan-projektet.

## Opdatering af data

Al data ligger i `DAYS`-blokken øverst i `index.html`'s `<script>` — én blok pr.
dagtype (Man–tors er udfyldt; Fredag/Lørdag/Søndag står som `cars: null` og viser
en pladsholder, indtil de udfyldes). Format er dokumenteret i kommentaren over
blokken: en tur er `{ dep:"7:14", to:"AR"|"CMY", vagt:"1116", note:"..." }` —
fra-sted og ankomst (+12 min) udledes automatisk.

Fredags-/weekendplanernes TP-linjer skal aflæses fra de fysiske vagtplaner
(samme metode som Man–tors) — se `../CLAUDE.md` i projektroden.

**Ved planskift** (ny planperiode) skal `DAYS`-datagrundlaget genaflæses — se
roadmap i `../CLAUDE.md`.

Siden viser bevidst **ingen førernavne** (kan tilføjes senere — datastrukturen
er forberedt på et ekstra felt pr. tur).
