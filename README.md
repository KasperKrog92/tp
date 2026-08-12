# TP-bil oversigt — tp.kasper-krog.dk

Statisk side (én fil, `index.html`, ingen dependencies). Viser TP-bilernes rotation
pr. dagtype med interaktiv "hvor er bilerne kl. X"-visning. **Ingen persondata** —
kun vagtnumre og tider. Siden er markeret `noindex`.

Terminologi: siden bruger **CMC** (området med kontrolcenter og førerbygning,
hvor TP-bilerne holder) — ikke CMY, som specifikt er togenes opstillingsområde
(Y = yard). CMC er den forkortelse, førerne bruger i daglig tale om stedet.

Siden åbner i **"Nu"-tilstand**: tidspunktet sættes til klokken lige nu og følger
med automatisk (opdaterer hvert 30. sekund + når fanen får fokus igen). Vælger
brugeren selv et tidspunkt (felt, ◀ Forrige/Næste ▶ eller tidslinjen), skifter
knappen til "Tilbage til nu". Advarsler på tidslinjen kan åbnes med både mus,
tastatur og touch.

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
dagtype (alle fire dagtyper er udfyldt; en ny dagtype uden data sættes til
`cars: null` og viser en pladsholder). Format er dokumenteret i kommentaren over
blokken: en tur er `{ dep:"7:14", to:"AR"|"CMC", vagt:"1116", note:"..." }` —
fra-sted og ankomst (+12 min) udledes automatisk.

Fredag/Lørdag/Søndag-blokkene er genereret maskinelt fra plansystemets
CSV-eksport med `../scripts/tp_udtraek.py` (i hovedprojektet); Man–tors-blokken
er den oprindelige håndlavede, som scriptet er valideret imod (identisk på alle
38 ture).

**Ved planskift** (ny planperiode): regenerér plan-JSON'erne fra nye CSV'er og
kør `python scripts/tp_udtraek.py` i hovedprojektet — se roadmap i `../CLAUDE.md`.

Siden viser bevidst **ingen førernavne** (kan tilføjes senere — datastrukturen
er forberedt på et ekstra felt pr. tur).
