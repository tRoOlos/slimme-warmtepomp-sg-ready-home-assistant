# 3. Een volledig stap-voor-stap implementatie

Nu duidelijk is waarom slimme sturing zinvol is en wat je daarvoor nodig hebt, volgt nu de detaillering: hoe richt je het daadwerkelijk in? 

## Stap 1 — Vind de SG-Ready ingangen op jouw warmtepomp en sluit de schakelmodules aan. 

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

### Resultaat van stap 1

- Beide schakelmodules zijn correct aangesloten  
- SG-Ready ingang **A** en **B** zijn afzonderlijk aanstuurbaar  
- De warmtepomp is klaar voor externe gedragssturing
___
## Stap 2 — Laat Home Assistant Green het brein worden van je warmtepomp

Nu de schakelmodules op de warmtepomp zijn aangesloten, is het tijd om Home Assistant Green in te zetten als het intelligente hart van de SG-Ready-sturing. 

### 2.1 Home Assistant Green aansluiten

De Green is volledig plug-and-play: aansluiten, opstarten en beginnen.
1.	Verbind de Green met je router via de meegeleverde netwerkkabel.
2.	Steek de voedingsadapter in het stopcontact.
3.	Open na het opstarten Home Assistant via: http://homeassistant.local:8123 en doorloop de initiële setup (gebruikersaccount, locatie, basisinstellingen)
4.	Na afronding van de setup kun je inloggen op je Home Assistant installatie. 

### 2.2 Voeg de schakelmodules toe aan Home Assistant

Bij het afronden van stap 1 heb je beide schakelmodules van stroom voorzien, voeg ze toe aan HA:
- Ga in HA naar instellingen – apparaten en diensten
- HA ontdekt beide schakelmodules via bluetooth, ze verschijnen als: “Shelly1G4…”. Klik op toevoegen bij een van beide modules en klik op volgende op de shelly te voorzien van je wifi instellingen. Je ziet dat het rode lichtje blijft branden (ipv knipperen) zodra de module verbonden is met je wifi -> check of je A of B op de module had gezet en geef vervolgens de module de juiste naam (SG-Ready-schakelmodule-A of SG-Ready-schakelmodule-B) en klik op voltooien.
- Beide schakelmodules zijn nu beschikbaar als aan/uit schakelaar. Check hiervoor het menu: apparaten en diensten, klik op de shelly integratie en check of beide schakelmodules zichtbaar zijn en of je ze aan(dicht) of uit (Open) kunt zetten. Laat ze voor nu beide op uit (Open) staan (=Normaal bedrijf warmtepomp).

*Indien Home Assistant de schakelmodules niet automatisch ontdekt, kan het zijn dat de afstand
tussen je HA Green en de modules te groot is voor Bluetooth. In dat geval kun je de modules
toevoegen via de Shelly-app op je telefoon of ze tijdelijk dichter bij de HA Green van stroom voorzien.*

### Resultaat van stap 2
Je kunt inloggen op een werkende HA installatie en beide schakelmodules kun je AAN of UIT zetten in HA. 


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
