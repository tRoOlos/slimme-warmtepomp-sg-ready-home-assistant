# 1. Hoe werkt dit principe?

Slimme warmtepompsturing draait niet om het continu bijregelen van setpoints of vermogens,
maar om **het sturen van gedrag op het juiste moment**.

Drie eigenschappen maken dit mogelijk:

- de **thermische massa** van een woning;
- een **open standaard voor gedragssturing** van de warmtepomp;
- het combineren van **externe context**, zoals prijs en lokaal overschot.

## 1.1 De warmtepomp als flexibel systeemonderdeel

Een warmtepomp is één van de grootste elektrische verbruikers in huis.
Tegelijkertijd beschikt een woning over natuurlijke thermische opslag:
vloerverwarming, radiatoren, buffervaten en de gebouwschil zelf.

Daardoor hoeft warmte (of koeling) **niet exact op het moment van gebruik**
opgewekt te worden. Productie kan in de tijd worden verschoven zonder merkbaar
comfortverlies.

Dit maakt de warmtepomp bij uitstek geschikt voor flexibiliteit:

- verbruik verschuiven naar goedkope uren;
- extra draaien bij lokaal overschot;
- rustiger gedrag bij schaarste op het elektriciteitsnet.

De kernvraag is dus niet *of* dit kan, maar *hoe* je dit eenvoudig en betrouwbaar organiseert.

## 1.2 SG-Ready: sturen op gedrag in plaats van micromanagement

**Smart Grid Ready (SG-Ready)** is een eenvoudige maar krachtige standaard
die al sinds circa 2012 bestaat en door veel fabrikanten wordt ondersteund.

Door het schakelen van **twee potentiaalvrije ingangen (A en B)** kunnen
vier gedragsmodi worden geselecteerd:

1. **Normaal bedrijf**  
2. **Beperken / blokkeren** bij schaarste of hoge prijzen  
3. **Mild boosten** bij goedkope stroom  
4. **Overcapaciteit / vol vermogen** bij lokaal overschot  

De interne uitwerking verschilt per warmtepomp en fabrikant, maar komt
meestal neer op combinaties van:

- aanpassen van stooklijn of tapwatertemperatuur;
- begrenzen of benutten van compressorvermogen;
- toestaan of blokkeren van elektrische bijverwarming;
- vooruit verwarmen en opslag in woning of buffervat;
- verschuiven van prioriteit tussen ruimteverwarming en tapwater.

Belangrijk ontwerpprincipe:

- externe systemen bepalen **wanneer** bepaald gedrag wenselijk is;
- de warmtepomp bepaalt **hoe** dit intern wordt ingevuld.

Er worden geen setpoints geforceerd.
Comfort, efficiëntie en veiligheid blijven bij de fabrikant.

Dit maakt SG-Ready robuust, uitlegbaar en breed toepasbaar.

## 1.3 Home Assistant als coördinatielaag

Om SG-Ready daadwerkelijk slim te benutten, is een regielaag nodig
die externe signalen combineert en vertaalt naar gewenst gedrag.

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

Concreet:

- HA haalt dynamische energieprijzen op;
- bepaalt prijsniveaus;
- monitort verbruik en (optioneel) opwek;
- schakelt via twee relais SG-Ready A en B.
