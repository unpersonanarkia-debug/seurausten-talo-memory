# Seurausten Talo — Muistipankki
*seurausten-talo-memory*

> **"Me emme unohda."**
> 
> Tämä repo on Seurausten Talon institutionaalinen muisti. Jokainen onnettomuusraportti, tarkastuskertomus ja epäonnistunut hanke kirjautuu tänne — ja tarkentaa seuraavaa analyysiä.

---

## Rakenne

```
memory/
  hanke/
    casebook/          ← Hanke-caset (VTV, Flyvbjerg, OTKES)
    onnettomuudet/     ← Onnettomuusraportit ja tarkastukset
    feedback/          ← Käyttäjien seurantatiedot
    weights/           ← Oppimisen painokertoimet
  paatos/
    casebook/          ← Päätös-caset
    ennakkotapaukset/  ← KKO, KHO, EIT (181 tapausta)
    onnettomuudet/     ← Poliittiset ja hallinnolliset virheet
    feedback/
  materiaali/
    casebook/          ← Materiaali-caset (ISO, EN, RILEM)
    onnettomuudet/     ← Tukes, VTT, BRE raportit
    feedback/
  yhteinen/
    lahteet/           ← Kaikille yhteinen lähdekirjasto
    indeksi/           ← Yhteinen hakuindeksi
```

---

## Periaatteet

1. **Fysiikka ensin** — materiaalidatan luvut ovat deterministisiä, ei arvailua
2. **Lähde aina** — jokainen case vaatii dokumentoidun lähteen
3. **Virhetyyppi aina** — mikä meni pieleen ja miksi
4. **Oppi aina** — mitä pitää tehdä toisin seuraavalla kerralla

---

## Oppimislogiikka

```
1. Raportti syötetään → parsitaan casebook-formaattiin
2. Case lisätään moottorin muistipankkiin
3. Sama virhetyyppi esiintyy 3+ kertaa → painokerroin nousee
4. Käyttäjä saa varoituksen: "3 samankaltaista hanketta on kaatunut tähän"
5. Feedback-loop: käyttäjä raportoi lopputuloksen → moottori oppii
```

---

## Yhteys päärepoon

- **Backend:** `seurausten-talo-core` — moottori lukee muistipankista
- **Frontend:** `seurausten-talo` — käyttöliittymä
- **Muistipankki:** tämä repo — data ja oppiminen

---

*Seurausten Talo Memory Bank v1.0 · 2026-04-27*
