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
| Kalkulator | Inndata og resultat: terminbeløp, effektiv rente, totale renter, gebyrer, samt renter og avdrag per år |
| Nedbetalingsplan | Full terminplan for grunnlånet, per år eller per termin |
| Grafisk | Restgjeld og betalt totalt som søylediagram, og hva terminbeløpet går til |
| Avansert | Renteendringer og avdragsfrihet underveis, alder og gjeldfrihet, belåningsgrad og rentetrapp |
| Plan (avansert) | Samme plan med rentekolonne, og differansen i totalkostnad mot flat rente |

## Beregninger

Kalkulatoren regner annuitetslån og serielån, med valgfritt antall terminer per år (1, 2, 4, 6 eller 12) og avdragsfri periode i måneder eller år. Effektiv rente finnes ved binærsøk på internrenten av terminbeløpene mot netto utbetalt beløp.

Tre avanserte funksjoner endrer planen, og gjelder bare i fanene «Avansert» og «Plan (avansert)»:

- **Renteendringer underveis.** Dra i rentekurven for å sette renten. Endringen gjelder fra året du tar tak i og ut løpetiden, så du kan legge inn flere trinn etter hverandre. I annuitetslån rekalkuleres annuiteten på gjenstående gjeld og gjenstående terminer ved hver renteendring.
- **Avdragsfrihet underveis.** Samme grep i en kurve med to nivåer: dra opp for avdragsfrihet fra året du tar tak i og ut løpetiden, og dra ned et senere år for å avslutte perioden. Kurven overstyrer avdragsfriheten fra Kalkulator-fanen. Løpetiden ligger fast, så avdragene fordeles på årene som har avdrag – er de siste årene avdragsfrie, er lånet nedbetalt før pausen starter.
- **Rentetrapp etter belåningsgrad.** Renten settes ned 0,25 prosentpoeng for hvert nivå lånet faller under: 85 %, 75 % og 60 % av verdien på sikkerheten. Sikkerheten er verdien på pantet pluss eventuell tilleggssikkerhet, og begge følger prisscenariet. Verdien kan følge et prisscenario med fall på 10 %, uendret nivå eller vekst på 2 % per år.

Søylediagrammet «Belåningsgrad over tid» viser belåningsgraden ved utgangen av hvert år. Søylene er fargelagt etter nivå, og året lånet passerer 90 %, 80 % og 60 % er merket på nivålinjen. Merk at dette er andre nivåer enn rentetrappen bruker; `LTV_MARKS` styrer diagrammet og `RATE_STEPS` styrer prisingen.

Alder og målet for gjeldfrihet henger sammen med nedbetalingstiden: setter du målet til 39 år når du er 35, blir løpetiden 4 år. Endrer du løpetiden i stedet, flyttes målet tilsvarende.

Siste termin nuller alltid restgjelden, og avdragsfriheten kan aldri spise hele løpetiden.

## Designmal

`SPWM mal/` inneholder designsystemet kalkulatoren er bygget på: farger og typografi, forhåndsvisninger av komponenter, og et UI-kit for wealth-portalen. Fontfilene og brand-assetene tilhører Söderberg & Partners.
