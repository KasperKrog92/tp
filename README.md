# TP-bil oversigt — tp.kasper-krog.dk

Statisk GitHub Pages-side med **dagens faktiske TP-plan fra OnlinePlan**.
Kun den aktuelle driftsdato vises; ingen dagsfaner eller datovælger. Ingen
førernavne eller førernumre offentliggøres. Siden er markeret `noindex`.

## Automatisk opdatering

GitHub Actions i det **private** `KasperKrog92/vagtplan`-repo kører
`.github/workflows/tp-publish.yml` to gange dagligt: 03:25 og 11:05 UTC
(04:25/05:25 og 12:05/13:05 dansk tid). Ingen lokal computer skal være tændt.
Workflowet kan også startes manuelt med **Run workflow**.

`server/tp_publish.py` henter datoens faktiske vagtsæt og læser TP-turene fra
OnlinePlans synlige vagtforløb. Hver dato skal være komplet og valideret:
vagtinventar, gyldighed, mødetider, samkørsel, 12 minutters ture, ingen overlap,
skiftende retninger og start/slut på CMC. Kun eksplicit udvalgte felter eksporteres;
navne i OnlinePlans samkørselsnoter fjernes. Fejl beholder tidligere dagsfiler;
workflowet prøver igen op til tre gange og melder fejl i GitHub Actions.

Der hentes **i dag og i morgen**, så planen er klar ved driftsdøgnskiftet kl. 04
**Europe/Copenhagen**, også ved skift mellem sommer- og vintertid. Morgendagen
kan ikke vælges på siden. Dagsfiler ældre end syv dage ryddes automatisk.

Browseren kontrollerer dagsfilen hvert femte minut og ved genåbning af fanen.
Den skifter selv driftsdato (kontrol hvert 30. sekund). Sidste gyldige plan for
**samme dato** gemmes lokalt, så en netværksfejl ikke fjerner den; der vises en
advarsel. En plan hentet for mere end 18 timer siden mærkes også tydeligt.
Hvis dagens fil mangler, vises en besked om at kontrollere OnlinePlan.
Gårsdagens plan eller en gammel standardplan vises aldrig som dagens plan.

Tidsstyring, statusoversigt, tidslinje, samkørsel, bufferadvarsler og rotationstabel
bevares. Siden åbner i **Nu**-tilstand og følger klokken hvert 30. sekund;
manuelt valgt tid bevares, også når dagsdata opdateres. Tidslinjen dækker fra 30 minutter før dagens første afgang til 30 minutter
efter dagens sidste ankomst på tværs af alle biler, også ved natkørsel. Uret
og statusoversigten følger fortsat den valgte tid; markøren skjules uden for udsnittet. Placeringerne er **planlagte**; linket til live GPS bevares.
CMC er området med kontrolcenter/førerbygning, hvor TP-bilerne holder.

## Dataformat

`data/dage/YYYY-MM-DD.json`:

```json
{"schema":1,"dato":"2026-09-08","kilde":"OnlinePlan","hentet":"2026-09-08T15:57:00+02:00",
 "cars":[{"id":"TP-001","moves":[
   {"dep":"8:00","arr":"8:12","to":"AR","vagt":"51101","note":"Medtager vagt 51102"},
   {"dep":"8:12","arr":"8:24","to":"CMC","vagt":"51103","note":""}]}],
 "warnings":[{"car":"TP-001","t":"8:12","txt":"0 min buffer på AR"}]}
```

Tider normaliseres til driftsminutter (fx 25:12). Ingen rå HTML eller fri tekst
fra OnlinePlan publiceres. Frontenden kontrollerer dato/format/rotation og
escaper noter, før de vises. Datastrukturen har ingen bemandingsfelter.

## Drift og publicering

| Hvad | Værdi |
|---|---|
| Offentligt repo | https://github.com/KasperKrog92/tp |
| Hosting | GitHub Pages, `main`, rod |
| Domæne | https://tp.kasper-krog.dk — eksisterende `CNAME` bevares |
| OnlinePlan-login | Eksisterende `ONLINEPLAN_USER`/`ONLINEPLAN_PASS` Actions-secrets i det private repo |
| Skriveadgang | Særskilt `TP_DEPLOY_KEY`-secret og skrive-deploy-nøgle på TP-repoet |
| Datahentning | `python server/tp_publish.py --push` i hovedrepoet |
| Fejl | GitHub Actions → Publicér dagens TP-plan; rettet kilde/genkørsel erstatter automatisk en gammel dagsfil |

Ved kodeændringer: test, commit og push **TP-repoet først**, derefter
submodule-pointer og tilhørende kode/docs i hovedrepoet. Ved almindelige
køreplansskift kræves ingen manuel dataændring; dagsfeedet følger OnlinePlan.
Gamle indbyggede `DAYS`-standardplaner bruges ikke længere.
