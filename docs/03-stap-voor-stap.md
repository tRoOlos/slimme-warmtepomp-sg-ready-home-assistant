# 3. Een volledig stap-voor-stap implementatie

Nu duidelijk is waarom slimme sturing zinvol is en wat je daarvoor nodig hebt, volgt onderstaand de detaillering: hoe richt je het daadwerkelijk in? 

## Stap 1 — Vind de SG-Ready ingangen op jouw warmtepomp en sluit de schakelmodules aan. 

Elke warmtepomp met SG-Ready-ondersteuning heeft  **twee ingangen** (meestal aangeduid als **A** en **B**). Deze schakelsignalen bepalen samen de vier gedragsmodi. Afhankelijk van het merk kunnen deze ingangen verschillende namen hebben, zoals: A/B, SG-1 / SG-2, AUX 1 / AUX 2 etc. Check de handleiding waar deze twee ingangen zich bevinden op de warmtepomp. 

De twee ingangen gaan geschakeld worden door de **twee Shelly-schakelmodules**. Zet voor de duidelijkheid op een module een A en op de andere module een B.

#### 1.1 Voeden van de schakelmodules (230 V)

De schakelmodules kunnen eenvoudig gevoed worden met een standaard **230 V-aansluitsnoer met stekker**:

- **Bruin → L**
- **Blauw → N**
- **Geel/groen (aarde)** → afwerken in een lasklem (niet aansluiten op de schakelmodule)

Met lasklemmen kun je **L en N doorlussen** naar beide schakelmodules, zodat ze gelijktijdig gevoed worden zonder extra installatiewerk.

⚠️ Steek de stekker **pas in het stopcontact** nadat ook de bedrading met de warmtepomp is aangesloten.

#### 1.2 Verbinden van de schakelmodules met de SG-Ready-ingangen

Maak de warmtepomp **spanningsloos** voordat je de SG-Ready-bedrading aansluit. 
Check je handleiding waar in je warmtepomp beide ingangen zich bevinden en of je na het aansluiten SG-Ready nog aan dient te zetten in het (service)menu van xde warmtepomp. 

Voor de verbinding tussen de schakelmodules en de warmtepomp ingangen gebruik je bij voorkeur een stukje **UTP-kabel**.  
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
2. zet je de warmtepomp weer onder spanning;
3. zet je SG-Ready aan in het warmtepomp (service) menu (indien nodig). 

Aansluitschema van twee Shelly schakelmodules met de SG-Ready ingangen op de warmtepomp:

<img width="425" height="227" alt="aansluitschema- schakelcontacten" src="https://github.com/user-attachments/assets/ea38d3be-4aae-4161-b79c-2202e58e79a0" />

Twee Shelly schakelmodules, klaar om aangesloten te worden op de SG-Ready-ingangen van de warmtepomp:
![IMG_3252](https://github.com/user-attachments/assets/f46e6c3f-e8f3-4eb0-91f0-9c92121c91c6)


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
- HA ontdekt beide schakelmodules via bluetooth, ze verschijnen als: “Shelly1G4…”. Klik op toevoegen bij een van beide modules en klik op volgende op de Shelly te voorzien van je wifi instellingen. Je ziet dat het rode lichtje blijft branden (i.p.v. knipperen) zodra de module verbonden is met je wifi → check of je A of B op de module had gezet en geef vervolgens de module de juiste naam (SG-Ready-schakelmodule-A of SG-Ready-schakelmodule-B) en klik op voltooien.
- Beide schakelmodules zijn nu beschikbaar als aan/uit schakelaar. Check hiervoor het menu: apparaten en diensten, klik op de Shelly integratie en check of beide schakelmodules zichtbaar zijn en of je ze aan(dicht) of uit (Open) kunt zetten. Laat ze voor nu beide op uit (Open) staan (=Normaal bedrijf warmtepomp).

*Indien Home Assistant de schakelmodules niet automatisch ontdekt, kan het zijn dat de afstand tussen je HA Green en de modules te groot is voor Bluetooth. In dat geval kun je de modules toevoegen via de Shelly-app op je telefoon of ze tijdelijk dichter bij de HA Green van stroom voorzien.*

### Resultaat van stap 2
Je kunt inloggen op een werkende HA-installatie en beide schakelmodules kun je AAN of UIT zetten in HA. 
___
## Stap 3 — Dynamische energieprijzen en prijsniveaus toevoegen

Home Assistant moet weten **wanneer stroom goedkoop of duur is**.  
Dat doe je door **prijsdata** van je energieleverancier binnen te halen en deze te voorzien van een prijsniveau.

### 3.1 Dynamische energieprijzen in HA krijgen

#### Optie A — Koppeling van je energieleverancier

Sommige energieleveranciers hebben een eigen integratie voor Home Assistant.  
Even googlen op:

> `[naam leverancier] Home Assistant integration`

levert vaak al een werkende oplossing op.

Zelf heb ik **Tibber** als leverancier en gebruik ik de volgende Tibber-integratie: [Tibber Prices Integration](https://jpawlowski.github.io/hass.tibber_prices/user/).  
Een praktisch voordeel van deze integratie is dat deze **direct prijsniveaus** meelevert  
(*zeer goedkoop, goedkoop, normaal, duur en zeer duur*), wat configuratiewerk scheelt.

#### Optie B — ENTSO-E (universele optie)

Kun je geen specifieke integratie voor jouw leverancier vinden?  
Dan kun je de [ENTSO-E for Home Assistant](https://github.com/JaccoR/hass-entso-e) integratie gebruiken.

Deze haalt de **Europese spotprijzen** op.  
Bij het toevoegen van deze integratie kun je optioneel via een template
de **vaste kosten en opslag** van jouw energieleverancier meenemen.
Na installatie zijn er meerdere sensoren beschikbaar, waaronder de dagelijkse uurprijzen die nodig zijn om prijsniveaus te kunnen bepalen. 

### 3.2 Prijsniveaus definiëren

In **Stap 4** worden de **SG-Ready gedragsmodi** aangestuurd op basis van **prijsniveaus**:
- zeer goedkoop  
- goedkoop  
- normaal  
- duur  
- zeer duur  

Bij **Optie A (de Tibber integratie)** worden deze prijsniveaus automatisch aangeleverd in een sensor. Deze kun je vinden door naar de integratie te gaan:
1. Ga naar **Instellingen → Apparaten & diensten → Integratie**
2. Klik op de **Tibber Prijsinformatie & Beoordelingen** integratie
3. Tussen de beschikbare sensoren staat een sensor: Uurprijsniveau en Kwartierprijsniveau (voor SG-Ready sturing gebruiken we de uurprijsniveaus)

Bij **Optie B (de ENTSO-E integratie)** dien je de prijsniveaus zelf te bepalen. De aanpak hiervoor is eenvoudig:
> vergelijk elk uur de actuele prijs met het **daggemiddelde** en ken daar een niveau aan toe.

| Prijsniveau      | Verhouding t.o.v. daggemiddelde |
|------------------|---------------------------------|
| Zeer goedkoop    | ≤ 60%                            |
| Goedkoop         | > 60% en ≤ 90%                   |
| Normaal          | > 90% en < 115%                  |
| Duur             | ≥ 115% en < 140%                 |
| Zeer duur        | ≥ 140%                           |

Je kunt uiteraard eigen drempelwaardes en niveaus kiezen, maar voor de leesbaarheid en consistentie hanteren we hier dezelfde indeling als Tibber.

Om een prijsniveau sensor in HA beschikbaar te krijgen op basis van de ENTSO-E integratie: 
1. Ga naar **Instellingen → Apparaten & diensten → Helpers**
2. Klik **+ Helper aanmaken → Template → Template sensor**
3. **Naam:** Energieprijs niveau
4. Plak de code uit **State template** hieronder
5. Controleer de preview
6. Klik **Verzenden**

**State template:**

> ⚠️ Pas in onderstaande code indien nodig `sensor.average_electricity_price` aan naar jouw prijssensor  
> Voorbeeld ENTSO-e: `sensor.average_electricity_price`

Kopieer deze code in het state template veld:

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
```

### Resultaat van stap 3

- Home Assistant beschikt over **actuele uurprijzen**
- Er is een sensor die **elk uur automatisch het prijsniveau bepaalt**
- Deze prijsniveau sensor vormt de **input voor de SG-Ready-automatisering in stap 4**
___

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

### Koppeling prijsniveau → SG-Ready gedrag (schakelmodules A en B)

| Prijsniveau          | SG-Ready A | SG-Ready B | Gedrag warmtepomp              |
|----------------------|-----------:|-----------:|--------------------------------|
| Normaal              | open       | open       | Normaal bedrijf                |
| Duur / Zeer duur     | dicht      | open       | Beperken / blokkeren           |
| Goedkoop             | open       | dicht      | Mild boosten                   |
| Zeer goedkoop        | dicht      | dicht      | Overcapaciteit / vol vermogen  |

⚠️ Onbekend of niet beschikbaar prijsniveau → **normaal bedrijf (failsafe)**.
- Open wil zeggen: de betreffende SG-Ready-schakelmodule (stap 2.2) staat uit
- Dicht wil zeggen: de betreffende SG-Ready-schakelmodule (stap 2.2) staat aan  

### Automatisering aanmaken (via Home Assistant UI)

1. Ga naar **Instellingen → Automatiseringen & scènes → Automatiseringen**
2. Klik **Automatisering toevoegen**
3. Kies als **Wanneer** trigger:
   - Entiteit → Status (wanneer de status van een entiteit verandert
   - Selecteer de prijsniveau sensor uit stap 3 (bijv. `sensor.energieprijs_niveau`)
4. Kies vervolgens de acties met de **Als / Dan-logica**:
   - Als prijsniveau = Normaal → A uit, B uit
   - Als prijsniveau = Duur of Zeer duur → A aan, B uit
   - Als prijsniveau = Goedkoop → A uit, B aan
   - Als prijsniveau = Zeer goedkoop → A aan, B aan
   - Als prijsniveau = Onbekend → A uit, B uit
5. Sla de automatisering op en activeer deze

**Resultaat Stap 4**

- Home Assistant stuurt automatisch elk uur de juiste SG-Ready-stand
- De warmtepomp reageert zonder comfortverlies
- Het verbruik verschuift structureel naar goedkopere uren, je gaat vanaf nu geld besparen
- Het elektriciteitsnet wordt minder belast
___
## Stap 5 — Verifieer gedrag en monitor het effect

De automatisering is nu actief, maar voordat je dit als “af” beschouwt, is één stap cruciaal:
controleren of de warmtepomp het **SG-Ready-signaal correct ontvangt** en zich **voorspelbaar gedraagt**.

### 5.1 Verifieer SG-Ready-detectie in de warmtepomp

Bij veel warmtepompen is er in een menuoptie een SG-Ready-statusscherm beschikbaar.
Controleer hier:

- welke **SG-Ready-stand** actief is;
- of **ingang A en B** correct worden gedetecteerd (open of dicht);
- of het **gedrag van de warmtepomp** overeenkomt met het gekozen prijsniveau (je kunt de prijsniveau sensor handmatig even wijzigen om te testen).

Als deze signalen zichtbaar zijn, weet je dat:

- de **bekabeling correct** is aangesloten;
- de **schakelmodules** goed functioneren;
- de warmtepomp de **SG-Ready-sturing daadwerkelijk toepast**.

### 5.2 Monitor het systeemgedrag over meerdere dagen

Laat het systeem vervolgens **enkele dagen ongewijzigd draaien**.
In Home Assistant kun je het gedrag eenvoudig terugkijken via de **geschiedenis** menu optie.

In het geschiedenis menu kun je sensoren toevoegen zoals: prijsniveau, de schakelmodules, prijs, verbruik (indien aanwezig) etc. om in een tijdsweergave te zien:

- wanneer schakelmodule **A** en **B** aan of uit stonden;
- welke **SG-Ready-modus** gedurende de dag actief was;
- hoe dit samenvalt met **elektriciteitsprijzen en verbruik**.

Hierdoor wordt zichtbaar:

- dat het warmtepompverbruik verschuift naar **goedkopere uren**;
- dat **dure uren** worden vermeden of afgevlakt;
- dat het schakelen **beperkt, stabiel en logisch** blijft.

*Je hoeft hiervoor geen aparte dashboards of grafieken te maken:  
de standaard geschiedenisweergave van Home Assistant is voldoende
om effect en gedrag goed te begrijpen.*
___
## Stap 6 — (Optioneel) inzicht in verbruik, PV-overproductie detecteren en SG-Ready bijsturen

Het energiesysteem ontwikkelt zich steeds meer richting optimalisatie **achter de meter**.
Zeker wanneer je zonnepanelen hebt, wordt het slim benutten van eigen opwek steeds belangrijker.

Door energie-intensieve apparaten — zoals een warmtepomp — juist op momenten van
**lokaal overschot** te laten draaien of energie tijdelijk op te slaan, verhoog je
de effectiviteit van je energiesysteem.

Puur financieel bekeken is het maximaliseren van eigen verbruik bij PV-overproductie
niet altijd optimaal (bijvoorbeeld wanneer de terugleververgoeding hoger is dan de
inkoopprijs).  Maar in de praktijk geldt vaak:

- wek je op jaarbasis **meer op dan je afneemt**, dan is eigen verbruik vrijwel altijd gunstig;
- na afschaffing van de **salderingsregeling (vanaf 2027)** en bij **terugleverkosten**
  wordt het optimaliseren van eigen verbruik steeds belangrijker;
- bij **negatieve stroomprijzen** kan het zelfs logisch zijn om actief verbruik te verhogen. Met Home Assistant kun je in zulke situaties óók zonnepanelen tijdelijk uitschakelen
of dynamisch dimmen om teruglevering te voorkomen — maar dat is voer voor een volgend artikel.

### Van financiële optimalisatie naar systeemoptimalisatie

In dit artikel beperken we ons tot:
- realtime **inzicht in verbruik en teruglevering** in Home Assistant;
- het **uitbreiden van de automatisering uit stap 4** om SG-Ready bij te sturen
  op basis van PV-overproductie.

Hiervoor is een **slimme-meter P1-lezer** nodig (zie hoofdstuk 2).

### 6.1 Sluit een P1-lezer aan op je slimme meter en Home Assistant

Voor circa €30 is een wifi-P1-lezer verkrijgbaar die plug-and-play is.
Volg de instructies van de P1-lezer om deze op de P1-poort van je slimme meter.

De P1-lezer levert lokaal data aan HA en maakt sensoren beschikbaar voor o.a.:

- totaal verbruik en teruglevering;
- productie en afname per fase;
- spanning en stroom per fase.

Deze sensoren kun je direct toevoegen aan het **Home Assistant Energie-dashboard**
voor realtime én historisch inzicht.

### 6.2 SG-Ready bijsturen op basis van PV-overproductie

Het principe is eenvoudig:

> **Wanneer je structureel stroom teruglevert aan het net, mag de warmtepomp maximaal draaien.**

In de zomer geldt dit ook voor **koelen**, indien je warmtepomp die functie ondersteunt.

#### PV-overproductie detecteren

Via de P1-lezer zijn in Home Assistant sensoren beschikbaar die per fase het actuele
vermogen tonen voor **productie** en **verbruik**.

Op basis daarvan maak je een extra sensor aan die de **netto PV-overproductie**
berekent:

- netto overproductie = totale opwek − totaal verbruik
- negatieve waarden worden afgekapt (sensor is altijd ≥ 0 W)

Deze sensor maak je als volgt aan 
	1.	Instellingen → Apparaten & diensten → Helpers
	2.	+ Helper aanmaken → Template → Template sensor
	3.	Vul in:
	•	Naam: Overproductie actief
	•	Eenheid: W
	•	Device class: power
	4.	Plak onderstaande code in het State-veld
	5.	Klik Verzenden

⚠️ Pas in de onderstaande code de entiteitsnamen aan naar jouw entiteiten die je in de P1-lezer integratie kunt vinden

```
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
```
**DREMPELWAARDES (AAN / UIT) T.B.V. HYSTERESE**

Om onrustig schakelen te voorkomen, werken we met twee drempels:
- AAN-drempel (hoger → wanneer er structureel overschot is, het minimaal continu beschikbaar vermogen)
- UIT-drempel (lager → wanneer het overschot daadwerkelijk wegvalt)
Zo voorkom je “flapperen” (continu aan/uit schakelen) door wolken of korte dips.

Deze entiteiten maak je als volgt aan als zogenaamde numerieke helpers.

Overproductie AAN-drempel sensor: 
- Instellingen - apparaten en diensten - helpers → + Helper aanmaken → Nummeriek
- Naam: Overproductie AAN drempel
- Min: 0
- Max: 10
- Geavanceerde instellingen: 
	- Weergave: invoerveld
	- Stapgrootte: 0.1 
	- Meeteenheid: kW
	- (Optioneel) Icoon: mdi:solar-power
- Klik op verzenden/OK om de sensor aan te maken.
- Dubbelklik op de aangemaakte sensor en stel deze initieel in met een richtwaarde: bijv. 1.0 kW

Overproductie UIT-drempel sensor:  
- Instellingen - apparaten en diensten - helpers → + Helper aanmaken → Nummeriek
- Naam: Overproductie UIT drempel
- Min: 0
- Max: 10
- Geavanceerde instellingen: 
	- Weergave: invoerveld
	- Stapgrootte: 0.1 
	- Meeteenheid: kW
	- (Optioneel) Icoon: mdi:solar-power
- Klik op verzenden/OK om de sensor aan te maken.
- Dubbelklik op de aangemaakte sensor en stel deze initieel in met een richtwaarde: bijv. 0.3 kW

### 6.3 Automatisering uitbreiden voor SG-Ready bijsturing op PV-overproductie

Nu de PV-overproductie betrouwbaar wordt gedetecteerd, kan de automatisering uit
**stap 4** worden uitgebreid.

Zolang er **lokaal overschot** is, mag de warmtepomp maximaal draaien —
**onafhankelijk van het actuele uurprijsniveau**.

Pas wanneer dat overschot structureel wegvalt, neemt de prijsgebaseerde sturing
het weer over.

Breid de automatisering als volgt uit:

- menu: instellingen - automatiseringen & scenes
- selecteer de automatisering uit stap 4 
- **Bovenaan** een extra conditie toevoegen:
  - Als netto PV-overproductie **boven de AAN-drempel** komt:
    - SG-Ready ingang **A = aan**
    - SG-Ready ingang **B = aan**
    - *(warmtepomp in **Overcapaciteit-modus**)*
- **Alleen als deze conditie niet waar is**:
  - voer de bestaande prijslogica uit (stap 4)
- **Terugschakelen** gebeurt pas wanneer:
  - de netto PV-overproductie gedurende een ingestelde periode
    **onder de UIT-drempel blijft** (bijv. 5 minuten)
  - *Door deze tijdsvertraging hebben korte dips — bijvoorbeeld door bewolking —
geen effect op de SG-Ready-stand.*

### Resultaat van stap 6

- Realtime inzicht in **verbruik, opwek en teruglevering**
- PV-overproductie wordt automatisch gedetecteerd
- Bij lokaal overschot schakelt de warmtepomp automatisch naar **SG-Ready Overcapaciteit**
- Zodra het overschot structureel wegvalt, neemt de **prijsgebaseerde sturing**
  het weer over

De warmtepomp fungeert hiermee als een **flexibele, lokale energiebuffer**:
zonnestroom wordt benut wanneer die er is, pieken worden afgevlakt
en het elektriciteitsnet wordt ontlast — volledig automatisch en zonder comfortverlies.

← [Terug naar de inhoudsopgave](https://github.com/tRoOlos/slimme-warmtepomp-sg-ready-home-assistant/tree/main)
