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

← [Terug naar de inhoudsopgave](https://github.com/tRoOlos/slimme-warmtepomp-sg-ready-home-assistant/tree/main)
