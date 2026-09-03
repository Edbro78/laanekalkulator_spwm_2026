# Lånekalkulator

Norsk lånekalkulator som ett enkelt dashboard, bygget i Söderberg & Partners' visuelle identitet. Hele applikasjonen ligger i `lanekalkulator.html` – React, ReactDOM og Babel hentes fra CDN, så det er ingen byggesteg og ingen backend.

## Kom i gang

Åpne `lanekalkulator.html` i en nettleser, eller besøk den publiserte siden: <https://edbro78.github.io/laanekalkulator_spwm_2026/>

Filen er selvstendig: logoen er bygget inn som data-URI og ikonene er tegnet inline, så den viser alt riktig uansett om den ligger lokalt, på GitHub Pages eller på en hvilken som helst annen webserver. `spwm-logo.svg` ligger igjen i repoet som kilde til logoen, men siden trenger den ikke.

Layouten er dimensjonert for 1280 × 800 effektive piksler (1920 × 1200 med 150 % skalering) og skal fylle skjermen uten scrolling. På mindre skjermer beholder dashbordet målene sine og siden får rullefelt. Knappen øverst til høyre går i fullskjerm.

## Publisering

`.github/workflows/pages.yml` publiserer repoet til GitHub Pages ved hver push til `main`, og skrur på Pages første gang workflowen kjører. Får ikke workflowen lov til det, kan Pages settes opp manuelt under **Settings → Pages** med kilden **GitHub Actions**.

`index.html` sender besøkende videre til `lanekalkulator.html`, slik at rot-adressen åpner kalkulatoren. `.nojekyll` slår av Jekyll-prosesseringen, så filene serveres akkurat slik de ligger i repoet.

## Faner

| Fane | Innhold |
|---|---|
| Lån | Inndata og resultat: terminbeløp, effektiv rente, totale renter, gebyrer, samt renter og avdrag per år |
| Nedbetalingsplan | Full terminplan for grunnlånet, per år eller per termin |
| Grafikk | Restgjeld og betalt totalt som søylediagram, og akkumulerte avdrag og renter |
| Lån Avansert | Renteendringer og avdragsfrihet underveis, alder, gjeldfrihet, inntekt og belåningsgrad |
| Avansert II | Topplån og stresstest av betjeningsevne mot bruttoinntekt |
| Nedbetalingsplan avansert | Samlet plan for grunnlån og topplån, med renteendringer, avdragsfrihet og eventuell restgjeld |

## Beregninger

Kalkulatoren regner annuitetslån og serielån, med valgfritt antall terminer per år (1, 2, 4, 6 eller 12) og avdragsfri periode i måneder eller år. Effektiv rente finnes ved binærsøk på internrenten av terminbeløpene mot netto utbetalt beløp.

To avanserte funksjoner endrer planen, og gjelder bare i fanene «Lån Avansert» og «Nedbetalingsplan avansert»:

- **Renteendringer underveis.** Dra i rentekurven for å sette renten. Endringen gjelder fra året du tar tak i og ut løpetiden, så du kan legge inn flere trinn etter hverandre. I annuitetslån rekalkuleres annuiteten på gjenstående gjeld og gjenstående terminer ved hver renteendring.
- **Avdragsfrihet underveis.** Samme grep i en kurve med to nivåer: dra opp for avdragsfrihet fra året du tar tak i og ut løpetiden, og dra ned et senere år for å avslutte perioden. Kurven overstyrer avdragsfriheten fra Lån-fanen. Løpetiden ligger fast og forlenges ikke. Avdragsfrihet er en pause i nedbetalingen – ligger den på slutten, blir restgjelden stående og du blir ikke gjeldfri innenfor perioden.

Søylediagrammet «Belåningsgrad over tid» viser belåningsgraden ved utgangen av hvert år mot pantet pluss eventuell tilleggssikkerhet, og inkluderer restgjeld på et eventuelt topplån. Søylene er fargelagt etter nivå, og året lånet passerer 90 %, 80 % og 60 % er merket på nivålinjen.

**Topplån** i fanen Avansert II har høyere rente enn grunnlånet (standard +2 prosentpoeng, justerbart 0–15 %). Avdrag går til topplånet først; når det er nedbetalt, går hele avdraget til grunnlånet. Gjeldsgraden regnes mot samlet lån.

Alder og målet for gjeldfrihet henger sammen med nedbetalingstiden: setter du målet til 39 år når du er 35, blir løpetiden 4 år. Endrer du løpetiden i stedet, flyttes målet tilsvarende.

Siste termin nuller alltid restgjelden, og avdragsfriheten kan aldri spise hele løpetiden.

## Designmal

`SPWM mal/` inneholder designsystemet kalkulatoren er bygget på: farger og typografi, forhåndsvisninger av komponenter, og et UI-kit for wealth-portalen. Fontfilene og brand-assetene tilhører Söderberg & Partners.
