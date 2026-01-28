# 3. Stap-voor-stap implementatie

Nu duidelijk is **waarom** slimme sturing werkt en **wat** je nodig hebt,
volgt hier de praktische inrichting.

---

## Stap 1 — SG-Ready ingangen aansluiten

Elke SG-Ready warmtepomp heeft **twee ingangen** (A en B).
Deze bepalen samen het gedrag.

Gebruik twee schakelmodules:
- relais 1 → SG-Ready A
- relais 2 → SG-Ready B

### Voeden van de schakelmodules

- Bruin → L  
- Blauw → N  
- Aarde → lasklem (niet aansluiten)

Doorlus L en N naar beide modules.
Stekker pas aansluiten na afronden bedrading.

### Verbinden met de warmtepomp

- Maak de warmtepomp spanningsloos
- Gebruik UTP-kabel (potentiaalvrij)
- Volgorde aders is niet relevant (open/dicht)

**Resultaat stap 1**
- SG-Ready A en B afzonderlijk schakelbaar
- Warmtepomp klaar voor externe gedragssturing

---

## Stap 2 — Home Assistant inrichten

- Installeer Home Assistant
- Voeg de schakelmodules toe via *Apparaten & diensten*
- Controleer handmatig schakelen
- Beide relais standaard **uit** (normaal bedrijf)

---

## Stap 3 — Energieprijzen en prijsniveaus

Gebruik uurprijzen via energieleverancier.
Werk met **prijsniveaus** t.o.v. daggemiddelde.

(Template sensor – zie code in README / vorige sectie)

---

## Stap 4 — SG-Ready automatisering op basis van prijs

Koppel prijsniveaus aan SG-Ready gedrag:

| Prijsniveau | A | B | Gedrag |
|------------|---|---|--------|
| Normaal | open | open | Normaal |
| Duur | dicht | open | Beperken |
| Goedkoop | open | dicht | Mild boosten |
| Zeer goedkoop | dicht | dicht | Overcapaciteit |

Failsafe: **Onbekend → normaal bedrijf**

---

## Stap 5 — Verifiëren en monitoren

- Controleer SG-Ready status in warmtepompmenu
- Gebruik HA-geschiedenis voor gedrag en stabiliteit

---

## Stap 6 — (Optioneel) PV-overproductie

- Netto overproductie = opwek − verbruik
- Werk met AAN/UIT-drempels (hysterese)
- Voeg tijdsvertraging toe om flapperen te voorkomen

---

## Resultaat

De warmtepomp wordt een flexibel systeemonderdeel:
- lagere kosten;
- beter benut lokaal overschot;
- minder piekbelasting;
- schaalbaar fundament voor verdere energiesturing.
