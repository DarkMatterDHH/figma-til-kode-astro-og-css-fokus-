# Refleksion – Figma til kode

**Gruppemedlem:** Danny

## Eksempel 1: CSS Grid og layout fra Figma

### Hvor og hvorfor?

Jeg brugte CSS Grid til at bygge hero-sektionen i `src/components/Hero.astro`. Teksten og billedet skulle stå ved siden af hinanden, mens servicekortene skulle placeres på én række og overlappe hero-sektionens nederste kant.

Grid hjalp mig med at styre placeringen af de store dele af siden. Jeg fandt også ud af, at det ikke er nok at sætte `grid-template-columns` på en komponent: komponentens placering afhænger også af dens forælder og af, hvilke grid-linjer der faktisk er defineret.

### Relevant kode

I `src/components/Services.astro` bruger jeg tre kolonner til kortene:

```css
.services {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  align-items: stretch;
  gap: var(--space-2);
}
```

`minmax(0, 1fr)` gør, at kolonnerne kan blive mindre uden at langt indhold tvinger dem bredere.

### Afprøvning og ændringer

- **Jeg testede:** Jeg sammenlignede sidens udseende med referencebilledet i browseren.
- **Jeg observerede:** Kortene stod først lodret. Det skyldtes, at deres fælles container ikke havde tre grid-kolonner.
- **Jeg ændrede eller mangler:** Jeg satte `grid-template-columns` til tre kolonner. Jeg mangler stadig at kontrollere layoutet grundigt på smalle skærme og sikre, at grid-linjerne i `global.css` passer til sidens faktiske HTML-struktur.

## Eksempel 2: Design tokens til farver, afstand og typografi

### Hvor og hvorfor?

Jeg arbejdede med `src/styles/tokens.css` for at samle designværdier ét sted. Formålet var at kunne genbruge de samme farver, afstande, typografiske størrelser og hjørneradier i headeren, hero-sektionen, servicekortene og sektionen “What To Expect”.

Jeg fandt også ud af, at farver skal vælges efter baggrunden. `--color-text` er mørk og passer på en lys flade, mens tekst på den mørke hero skal bruge et lyst token som `--color-on-surface-elevated`.

### Relevant kode

Eksempel på farvetokens fra `tokens.css`:

```css
--color-action: var(--color-yellow-500);
--color-text: var(--color-neutrals-900);
--color-surface-elevated: var(--color-blue-700);
--color-on-surface-elevated: var(--color-neutrals-50);
```

I komponenterne bruger jeg tokens i stedet for at gentage farveværdier:

```css
color: var(--color-on-surface-elevated);
background: var(--color-surface-elevated);
```

### Afprøvning og ændringer

- **Jeg testede:** Jeg sammenlignede farver og afstande visuelt med referencebilledet.
- **Jeg observerede:** Mørk tekst på den mørke hero havde for lav kontrast. Jeg opdagede også, at nogle font-tokennavne i `global.css` ikke fandtes i `tokens.css`.
- **Jeg ændrede eller mangler:** Jeg begyndte at bruge de eksisterende farvetokens. Jeg mangler at kontrollere, at alle typografi- og spacing-tokens er defineret og at `global.css` bruger de samme navne.

## Eksempel 3: Genbrugelige komponenter og data

### Hvor og hvorfor?

Jeg opdelte serviceområdet i `Services.astro` og `ServiceCard.astro`. `Services.astro` henter service-data fra API'et og viser et kort for hver service. `ServiceCard.astro` står for udseendet af det enkelte kort.

Det gør det lettere at ændre kortenes layout ét sted og kortets indhold eller stil et andet sted.

### Relevant kode

`Services.astro` opretter et kort for hvert element i API-svaret:

```astro
<div class="services">
  {serviceData.map((service) => <ServiceCard service={service} />)}
</div>
```

### Afprøvning og ændringer

- **Jeg testede:** Jeg så, at servicekortene blev vist på siden, og justerede containeren, så den brugte tre kolonner.
- **Jeg observerede:** Layoutet af kortenes fælles container bestemmes i `Services.astro`, mens baggrund, tekst og indhold i det enkelte kort bestemmes i `ServiceCard.astro`.
- **Jeg ændrede eller mangler:** Jeg mangler at håndtere situationer, hvor API-kaldet fejler eller returnerer tomme data. Jeg bør også kontrollere, hvordan kortene opfører sig, hvis en beskrivelse er meget lang.

## Fallback og robusthed

Jeg har brugt `minmax(0, 1fr)` i grid-layoutet, så indhold ikke så let tvinger en kolonne bredere end den tilgængelige plads. Jeg har også arbejdet med media queries, så layoutet kan ændres på mindre skærme.

En fallback for skrifttypen kan være en systemfont, hvis den ønskede skrifttype ikke kan indlæses:

```css
font-family: Inter, system-ui, sans-serif;
```

Jeg har ikke systematisk testet forskellige browsere eller browser-versioner. Indtil videre har jeg primært kontrolleret layoutet visuelt i min browser. Næste skridt er at teste på en smal skærm og kontrollere, at layoutet stadig fungerer, når teksten fylder mere end i referencebilledet.

Global CSS bruger jeg til fælles styles og sidens overordnede layout. Komponenternes CSS bruger jeg til de enkelte dele, fx hero, servicekort og forventningssektionen.

## Brug af AI

Jeg brugte AI til at få hjælp til CSS Grid, design tokens, Astro-billedimporter og til at sammenligne mit resultat med referencebillederne.

Jeg lærte især, at det er vigtigt at kontrollere både CSS-reglerne, tokennavnene og komponenternes placering i HTML-strukturen, når noget ikke ser ud som forventet.

# Eksempel 4: Komponentbaseret udvikling og iterativ designproces

### Hvor og hvorfor?

Jeg har arbejdet med en komponentbaseret struktur i Astro, hvor hver del af layoutet er opdelt i mindre komponenter, fx `Header.astro`, `Hero.astro`, `Services.astro`, `Counter.astro`, `Contact.astro` og `Newsletter.astro`. Det har gjort det lettere at fokusere på ét område af siden ad gangen og justere det uden at påvirke resten af designet.

Det har også vist mig, at det ikke er nok at have en “rigtig” Figma-opsætning. Kode og layout er dynamiske, og de kræver konstant justering, når komponenter bliver sat sammen i den rigtige HTML-struktur. Nogle elementer virkede rigtigt i isolering, men fejlede, når de blev sat sammen i det samlede grid-system.

### Relevant kode

Eksempel på en simpel komponentstruktur:

```astro
<header class="site-header">...</header>
<section class="hero">...</section>
<section class="services">...</section>
<section class="contact">...</section>
<footer class="site-footer">...</footer>
```

Denne opbygning har gjort det nemmere at holde de forskellige sektioner adskilt og lettere at tilpasse.

### Afprøvning og ændringer

- **Jeg testede:** Jeg kørte layoutet sektion for sektion og sammenlignede designet i browseren.
- **Jeg observerede:** Nogle sektioner virkede, når de var isoleret, men fungerede ikke, når de skulle indgå i den overordnede side. Det skyldtes ofte, at grid-linjer, z-index eller elementernes position var forkerte.
- **Jeg ændrede eller mangler:** Jeg har gjort flere små rettelser i CSS, fx at justere spacing, padding, `grid-column` og `position` i de konkrete komponenter. Jeg vil i en senere iteration øge fokus på at rydde op i sammenhængen mellem global CSS og komponenternes lokale CSS, så det bliver mere konsistent og nemmere at vedligeholde.

## Læring og refleksion

Jeg har lært, at Figma-design ofte er visuelt smukt, men at det i kode kræver mere præcision og struktur. Det gælder især ved brug af CSS Grid, hvor layoutet afhænger af flere lag: container, grid-linjer, børn, komponenter og media queries.

Jeg har også lært, at små fejltagelser som to `class`-attributter, ugyldige CSS-selectors eller fejl i `grid-column`-placering kan have stor effekt på hele layoutet. Derfor er det vigtigt at være systematisk og løbende teste i browseren.

Det vigtigste for mig har været at forstå, at kode ikke kun handler om at få “noget til at se godt ud”, men om at skabe et stabilt og genbrugeligt layout, som kan tilpasses forskellige skærmstørrelser og stadig opretholde designets intention.
