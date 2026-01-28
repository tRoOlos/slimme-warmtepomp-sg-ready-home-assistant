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

### 1.3 Home Assistant als coördinatielaag

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
