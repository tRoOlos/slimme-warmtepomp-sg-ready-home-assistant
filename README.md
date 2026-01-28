# Slimme warmtepompsturing met SG-Ready en Home Assistant

Deze repository vormt de **verdieping** bij het LinkedIn-artikel  
*“Slimme warmtepompsturing: besparen, comfort behouden én het net helpen”*.

# Slimme warmtepompsturing met SG-Ready en Home Assistant

Deze repository vormt de **verdieping** bij het LinkedIn-artikel  
*“Slimme warmtepompsturing: besparen, comfort behouden én het net helpen”*.

Waar het artikel ingaat op het *waarom* en de ontwerpkeuzes, beschrijft deze README
in meer detail:
1. **hoe het principe werkt**
2. **wat je nodig hebt om te beginnen**
3. **een volledig stap-voor-stap implementatie**

## 1. Hoe werkt dit principe?

Slimme warmtepompsturing draait niet om het continu bijregelen van setpoints of vermogens,
maar om **het sturen van gedrag op het juiste moment**.

Drie eigenschappen maken dit mogelijk:
- de **thermische massa** van een woning;
- een **open standaard voor gedragssturing** van de warmtepomp 
- de beschikbaarheid en het combineren van **data: zoals prijs en lokaal overschot** in gedragsstuursignalen.

## 1. Hoe werkt dit principe?

Slimme warmtepompsturing draait niet om het continu bijregelen van setpoints of vermogens,
maar om **het sturen van gedrag op het juiste moment**.

Drie eigenschappen maken dit mogelijk:
- de **thermische massa** van een woning;
- een **open standaard voor gedragssturing** van de warmtepomp 
- de beschikbaarheid en het combineren van **data: zoals prijs en lokaal overschot** in gedragsstuursignalen.

### 1.1 De warmtepomp als flexibel systeemonderdeel

Een warmtepomp is één van de grootste elektrische verbruikers in huis.
Tegelijkertijd beschikt een woning over natuurlijke thermische opslag:
vloerverwarming, radiatoren, buffervaten en de gebouwschil zelf.

Daardoor hoeft warmte (of koeling) **niet exact op het moment van gebruik**
opgewekt te worden. Productie van warmte of koude kan in de tijd worden verschoven zonder merkbaar
comfortverlies.

Dit maakt de warmtepomp bij uitstek geschikt voor flexibiliteit:
- verbruik verschuiven naar goedkope uren;
- extra draaien bij lokaal overschot;
- rustiger gedrag bij schaarste op het elektriciteitsnet.

De kernvraag is dus niet *of* dit kan, maar *hoe* je dit eenvoudig en betrouwbaar organiseert.

### 1.2 SG-Ready: sturen op gedrag in plaats van micromanagement

**Smart Grid Ready (SG-Ready)** is geen nieuwe hype: een eenvoudige maar krachtige standaard die al bestaat sinds ca. 2012). Het wordt door veel fabrikanten ondersteund en werkt op basis van een eenvoudige principe. 
Door het schakelen van **twee potentiaalvrije ingangen (A en B)** kun je vier gedragsmodi selecteren:

1. **Normaal bedrijf** de warmtepomp draait zoals altijd
2. **Beperken / blokkeren** de warmtepomp verlaagt tijdelijk het vermogen of wordt (deels) geblokkeerd bij schaarste of hoge prijzen  
3. **Mild boosten** extra warmte of koude maken wanneer stroom goedkoper is, binnen efficiënte grenzen.  
4. **Overcapaciteit / vol vermogen** bij lokaal overschot de warmtepomp mag maximaal laten draaien

De technische vertaling van de gekozen gedragsmodus verschilt per merk en type warmtepomp. Indien je hierover meer wil weten raadpleeg dan de handleiding van je warmtepomp. 
De ene warmtepomp is hierin verfijnder dan de andere, maar in de praktijk komt het vaak neer op een combinatie van:
- het iets lager of hoger instellen van de gewenste temperatuur voor verwarming, koeling (indien van toepassing) of tapwater;
- tijdelijk terugschakelen of juist volledig benutten van het compressorvermogen;
- het uitschakelen (of juist toestaan) van elektrische bijverwarming;
- slim vooruit verwarmen en warmte opslaan in het buffervat of in de woning zelf;
- het verschuiven van prioriteiten tussen ruimteverwarming/-koeling en warm tapwater.

Belangrijk ontwerpprincipe:
- externe systemen bepalen *wanneer* een bepaald gedrag wenselijk is;
- de warmtepomp bepaalt dus *hoe* dit intern wordt ingevuld.

Er worden geen setpoints geforceerd.
Comfort, efficiëntie en veiligheid blijven bij de fabrikant.
Juist deze scheiding maakt SG-Ready robuust, uitlegbaar en breed toepasbaar.

Sommige warmtepompleveranciers bieden ook eigen functionaliteit om warmtepompen aan te sturen op basis van dynamische prijzen. Zo heeft mijn Nibe-warmtepomp de functie Smart Price Adaption (SPA). Als je een snelle basisoplossing zoekt, kan dat prima werken. In de praktijk bleek SPA voor mij echter te beperkt, te traag en te weinig configureerbaar. SG-Ready biedt daarentegen maximale controle, flexibiliteit en betrouwbaarheid — precies wat je nodig hebt als je dynamische prijzen optimaal wilt benutten én je sturing wilt uitbreiden met bijvoorbeeld PV-overproductie en energieopslag.

### 2.2 Home Assistant als coördinatielaag

Om SG-Ready daadwerkelijk slim te benutten, is een regielaag nodig die externe signalen kan combineren en vertalen naar gewenst gedrag.

**Home Assistant (HA)** vervult deze rol:
- open-source;
- lokaal draaiend;
- flexibel en uitbreidbaar;
- volledige controle over data en logica.

Home Assistant fungeert nadrukkelijk **niet** als technische regelaar
van de warmtepomp, maar als **coördinator** tussen:
- prijsinformatie;
- lokaal overschot;
- beschikbare SG-Ready gedragsmodi.

Voor deze slimme warmtepompsturing:
- haalt HA dynamische energieprijzen op en bepaalt prijsniveaus;
- kijkt HA naar energieverbruik of overproductie (optioneel);
- beslist HA welke SG-Ready-stand actief moet zijn;
- stuurt HA via twee eenvoudige schakelmodules de warmtepomp aan.

---

## 2. Wat heb je nodig om te beginnen?

Slimme warmtepompsturing blijkt in de praktijk verrassend laagdrempelig.
Met een paar eenvoudige checks en beperkte hardware kun je starten.

### 2.1 Warmtepomp met SG-Ready ondersteuning

Controleer of jouw warmtepomp SG-Ready ondersteunt.
- Veel warmtepompen ondersteunen SG-Ready (grofweg 70–80% van de modellen van de afgelopen tien jaar), maar dit verschilt per fabrikant en type. Controleer daarom altijd de handleiding van jouw warmtepomp.
- Je kunt ook de officiële [SG-Ready-database](https://www.waermepumpe.de/normen-technik/sg-ready/sg-ready-datenbank/) raadplegen. Deze lijst geeft een goede indicatie, maar is niet volledig. Sommige modellen — zoals mijn eigen Nibe F1145 — staan er bijvoorbeeld niet in, terwijl ze SG-Ready wel degelijk ondersteunen.

Let op:
- SG-Ready staat niet altijd in de achtergelaten installatiehandleiding;
- download de meest recente handleiding van de fabrikant;
- update indien nodig de warmtepompsoftware.

### 2.2 Enige technische vaardigheid en tijd voor eenmalige configuratie

Je hoeft geen programmeur te zijn. Maar je moet wel:
- De schakelmodules kunnen koppelen aan je warmtepomp
- Home Assistant kunnen instellen (prijzen/prijsniveaus, schakelmodules, en een automatisering voor de SG-Ready-modi)

Het is geen complexe technologie — vooral logisch werken en een paar instellingen zetten. In het volgende hoofdstuk leg ik uit hoe, en met AI als hulplijn moet het zeker lukken. Daarna werkt het systeem volledig automatisch.


### 2.3 Benodigde hardware

Voor een praktische en betaalbare setup:
- Home Assistant Green (plug-and-play, ± €100).
- Twee schakelmodules (potentiaalvrije relais) om de SG-Ready-contacten aan te sturen, bijvoorbeeld: 2× een Shelly Plus 1 Gen4 (± €15 per stuk)
- Een stabiele wifi-verbinding bij de warmtepomp
- Optioneel: een Slimme Meter P1-lezer voor HA (± €30) voor inzicht in verbruik en aanvullende sturing op PV-overproductie.


### 2.4 Dynamisch energiecontract

Voor prijssturing is een dynamisch contract nodig waarbij de prijzen per uur of 15m beschikbaar zijn.

---

## 3. Een volledig stap-voor-stap implementatie

Nu duidelijk is waarom slimme sturing zinvol is en wat je daarvoor nodig hebt, volgt nu de detaillering: hoe richt je het daadwerkelijk in? 

### Stap 1 — Vind de SG-Ready ingangen op jouw warmtepomp en sluit de schakelmodules aan. 

Elke warmtepomp met SG-Ready ondersteuning heeft  **twee ingangen** (meestal aangeduid als **A** en **B**). Deze schakelsignalen bepalen samen de vier gedragsmodi. Afhankelijk van het merk kunnen deze ingangen verschillende namen hebben, zoals: A/B, SG-1 / SG-2, AUX 1 / AUX 2 etc. Check de handleiding waar deze twee ingangen zich bevinden op de warmtepomp. 

De twee ingangen gaan geschakeld worden door de **twee Shelly-schakelmodules**, zet voor de duidelijkheid op een module een A en op de andere module een B.

#### 1.1 Voeden van de schakelmodules (230 V)

De schakelmodules kunnen eenvoudig gevoed worden met een standaard **230 V-aansluitsnoer met stekker**:

- **Bruin → L**
- **Blauw → N**
- **Geel/groen (aarde)** → afwerken in een lasklem (niet aansluiten op de schakelmodule)

Met lasklemmen kun je **L en N doorlussen** naar beide schakelmodules, zodat ze gelijktijdig gevoed worden zonder extra installatiewerk.

⚠️ Steek de stekker **pas in het stopcontact** nadat ook de bedrading met de warmtepomp is aangesloten.

#### 1.2 Verbinden van de schakelmodules met de SG-Ready-ingangen

Maak de warmtepomp **spanningsloos** voordat je de SG-Ready-bedrading aansluit.

Voor de verbinding tussen de schakelmodules en de warmtepomp gebruik je bij voorkeur een stukje **UTP-kabel**.  
SG-Ready werkt met een **spanningsloos (potentiaalvrij) contact** — de aders van een UTP-kabel zijn hiervoor ideaal.

Belangrijk:
- de **volgorde van de twee draden maakt niet uit**;
- de warmtepomp detecteert alleen **open of gesloten**;
- de **O** en **I**-klemmen op de schakelmodule vormen samen één schakelcontact.

**Voorbeeldbedrading:**

- Blauw + Blauw/Wit  
  → Schakelmodule **A** (klemmen **O** en **I**)  
  → SG-Ready ingang **A** op de warmtepomp

- Oranje + Oranje/Wit  
  → Schakelmodule **B** (klemmen **O** en **I**)  
  → SG-Ready ingang **B** op de warmtepomp

Wanneer alles is aangesloten:
1. steek je de stekker in het stopcontact;
2. zet je de warmtepomp weer onder spanning.

Aansluitschema van twee shelly schakelmodules met de SG-Reeady ingangen op de warmtepomp:

<img width="425" height="227" alt="aansluitschema- schakelcontacten" src="https://github.com/user-attachments/assets/ea38d3be-4aae-4161-b79c-2202e58e79a0" />

---

### Resultaat van stap 1

- Beide schakelmodules zijn correct aangesloten  
- SG-Ready ingang **A** en **B** zijn afzonderlijk aanstuurbaar  
- De warmtepomp is klaar voor externe gedragssturing

Elke SG-Ready warmtepomp heeft twee ingangen (A en B).
Deze worden aangesloten op twee schakelmodules met potentiaalvrij contact.

- 1 relais → SG-Ready A  
- 1 relais → SG-Ready B  

De volgorde van de draden maakt niet uit: open/dicht is voldoende.

Resultaat:
- Home Assistant kan SG-Ready A en B onafhankelijk schakelen.

---

### Stap 2 — Home Assistant inrichten

Installeer Home Assistant en voeg de schakelmodules toe
via **Instellingen → Apparaten & diensten**.

Controleer:
- beide relais zijn zichtbaar;
- aan/uit schakelen werkt;
- standaard staan beide relais **uit** (normaal bedrijf).

---

### Stap 3 — Dynamische energieprijzen & prijsniveaus

Home Assistant haalt uurprijzen op via een energie-integratie.

In plaats van absolute prijzen werken we met **prijsniveaus**:
de actuele prijs wordt vergeleken met het **daggemiddelde**.

Prijsniveaus:
- Zeer goedkoop: ≤ 60%
- Goedkoop: > 60% en ≤ 90%
- Normaal: > 90% en < 115%
- Duur: ≥ 115% en < 140%
- Zeer duur: ≥ 140%

#### Template sensor: Energieprijsniveau (via UI)

Maak een **Template sensor** aan via:
`Instellingen → Apparaten & diensten → Helpers`

Plak onderstaande code in het **State template** veld
(en pas indien nodig de prijssensor aan):

```jinja2
{% set prices = state_attr('sensor.average_electricity_price', 'prices_today') %}
{% if prices and prices | length > 0 %}
  {% set price_values = prices | map(attribute='price') | list %}
  {% set avg = price_values | average %}
  {% set current = states('sensor.average_electricity_price') | float(0) %}
  {% if avg > 0 %}
    {% set ratio = current / avg %}
    {% if ratio <= 0.6 %}
      Zeer goedkoop
    {% elif ratio <= 0.9 %}
      Goedkoop
    {% elif ratio < 1.15 %}
      Normaal
    {% elif ratio < 1.4 %}
      Duur
    {% else %}
      Zeer duur
    {% endif %}
  {% else %}
    Onbekend
  {% endif %}
{% else %}
  Onbekend
{% endif %}

## Stap 4 — SG-Ready automatisering op basis van prijs

Nu alle bouwstenen aanwezig zijn, worden ze samengebracht in één eenvoudige maar krachtige automatisering.
Deze automatisering vertaalt het actuele **energieprijsniveau** naar het gewenste **SG-Ready-gedrag** van de warmtepomp.

### Principe

Elke keer dat het prijsniveau wijzigt:
1. bepaalt Home Assistant welk prijsniveau actief is;
2. koppelt dit niveau aan een SG-Ready-gedragsmodus;
3. schakelt SG-Ready ingang A en/of B;
4. past de warmtepomp zélf zijn interne werking aan.

Home Assistant stuurt dus **het gedrag**, niet de interne regeling.

### Koppeling prijsniveau → SG-Ready gedrag

| Prijsniveau           | SG-Ready A | SG-Ready B | Gedrag warmtepomp              |
|----------------------|-----------:|-----------:|--------------------------------|
| Normaal              | open       | open       | Normaal bedrijf                |
| Duur / Zeer duur     | dicht      | open       | Beperken / blokkeren           |
| Goedkoop             | open       | dicht      | Mild boosten                   |
| Zeer goedkoop        | dicht      | dicht      | Overcapaciteit / vol vermogen  |

⚠️ Onbekend of niet beschikbaar prijsniveau → **normaal bedrijf (failsafe)**.

### Automatisering aanmaken (via Home Assistant UI)

1. Ga naar **Instellingen → Automatiseringen & scènes → Automatiseringen**
2. Klik **Automatisering toevoegen**
3. Kies als trigger:
   - *Entiteit → Status gewijzigd*
   - Selecteer de prijsniveausensor (bijv. `sensor.energieprijs_niveau`)
4. Voeg acties toe met **Als / Dan-logica**:
   - Als prijsniveau = Normaal → A uit, B uit
   - Als prijsniveau = Duur of Zeer duur → A aan, B uit
   - Als prijsniveau = Goedkoop → A uit, B aan
   - Als prijsniveau = Zeer goedkoop → A aan, B aan
   - Als prijsniveau = Onbekend → A uit, B uit
5. Sla de automatisering op en activeer deze

**Resultaat Stap 4**

- De warmtepomp reageert automatisch op uurprijzen
- Het verbruik verschuift naar gunstigere momenten
- Comfort en veiligheid blijven intact
- Het systeem is robuust door een expliciete failsafe

## Stap 5 — Verifiëren en monitoren van gedrag

Na activeren van de automatisering is het belangrijk om te controleren
of de warmtepomp het SG-Ready-signaal correct ontvangt en zich voorspelbaar gedraagt.

### Verifiëren in de warmtepomp

Veel warmtepompen tonen de actuele SG-Ready-status in het servicemenu.
Controleer hier:
- welke SG-Ready-modus actief is;
- of ingang A en B correct worden herkend (open/dicht);
- of het gedrag overeenkomt met de gekozen modus.

Als dit klopt, weet je dat:
- de bekabeling correct is;
- de schakelmodules functioneren;
- de SG-Ready-sturing daadwerkelijk wordt toegepast.

### Monitoren via Home Assistant

Laat het systeem enkele dagen ongewijzigd draaien.
Via **Geschiedenis** in Home Assistant kun je terugzien:
- wanneer SG-Ready A en B zijn geschakeld;
- welk gedrag actief was;
- hoe dit samenvalt met prijsniveaus en verbruik.

Je hebt hiervoor geen aparte dashboards nodig:
de standaard geschiedenisweergave is voldoende om effect en stabiliteit te beoordelen.

**Resultaat Stap 5**

- Je ziet dat het schakelen rustig en logisch verloopt
- Dure uren worden vermeden
- Goedkope uren worden actief benut

## Stap 6 — (Optioneel) PV-overproductie detecteren en SG-Ready bijsturen

Wanneer zonnepanelen aanwezig zijn, kan lokaal opgewekte energie
worden benut door de warmtepomp extra te laten draaien.

Dit is een **uitbreiding** op prijssturing, geen vervanging.

### 6.1 Netto PV-overproductie berekenen

Netto PV-overproductie wordt bepaald als:

netto overproductie = totale opwek − totaal verbruik

Negatieve waarden worden afgekapt zodat de sensor altijd ≥ 0 W is.

```jinja2
{{ 
  max(0, (
    states('sensor.power_produced_phase_1') | float +
    states('sensor.power_produced_phase_2') | float +
    states('sensor.power_produced_phase_3') | float
  ) - (
    states('sensor.power_consumed_phase_1') | float +
    states('sensor.power_consumed_phase_2') | float +
    states('sensor.power_consumed_phase_3') | float
  )) 
}}

### 6.2 Hysterese: AAN- en UIT-drempels

```md
### 6.2 Hysterese: AAN- en UIT-drempels

Om onrustig schakelen te voorkomen (bijv. door wolken),
wordt gewerkt met twee drempels:

- **AAN-drempel** (hoger): wanneer er structureel overschot is
- **UIT-drempel** (lager): wanneer het overschot daadwerkelijk wegvalt

Deze maak je aan als **Numerieke helpers** in Home Assistant.

Voorbeeldwaarden:
- Overproductie AAN-drempel: 1.0 kW
- Overproductie UIT-drempel: 0.3 kW

De exacte waarden zijn afhankelijk van je PV-installatie
en kunnen na monitoring eenvoudig worden aangepast.

### 6.3 Aanpassen van de automatisering voor SG-Ready bijsturing op PV-overproductie

De bestaande automatisering uit Stap 4 wordt uitgebreid met een extra conditie bovenaan:

- **Als** netto PV-overproductie boven de AAN-drempel komt:
  - SG-Ready A = aan
  - SG-Ready B = aan
  - (warmtepomp in Overcapaciteit-modus)
- **Anders**:
  - voer de bestaande prijslogica uit

Terugschakelen gebeurt pas wanneer:
- de netto overproductie **langer onder de UIT-drempel blijft**
  (bijvoorbeeld 5 minuten)

Door deze tijdsvertraging hebben korte dips geen effect.

**Resultaat Stap 6**

- Realtime inzicht in verbruik en teruglevering
- Automatische benutting van lokaal PV-overschot
- SG-Ready wordt dynamisch bijgestuurd
- Het systeem groeit uit tot een volwaardig thuis-energiemanagementsysteem

## Resultaat en conclusie

Met deze aanpak verandert de warmtepomp van een puur vraaggestuurd apparaat
naar een flexibel onderdeel van het energiesysteem.

Het verbruik verschuift automatisch naar momenten waarop stroom goedkoper
of lokaal beschikbaar is, terwijl comfort en veiligheid behouden blijven.

Home Assistant fungeert hierbij als coördinatielaag:
er wordt niets geforceerd, alleen het gewenste gedrag wordt aangegeven.
Dat maakt de sturing robuust, uitlegbaar en schaalbaar.

Veel van deze flexibiliteit is vandaag al mogelijk —
met bestaande apparatuur en open standaarden,
door het juiste signaal op het juiste moment.
