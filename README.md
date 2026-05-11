# Seurausten Talo — Muistipankki
### `seurausten-talo-memory`

> *"Me emme unohda."*

Tämä repo on Seurausten Talon institutionaalinen muisti. Jokainen onnettomuusraportti, tarkastuskertomus ja epäonnistunut hanke kirjautuu tänne — ja tarkentaa seuraavaa analyysiä.

---

## Rakenne

```
seurausten-talo-memory/
├── schemas/
│   ├── hanke_schema_v1.json           ← Hanke-casen JSON-skeema
│   ├── paatos_schema_v1.json          ← Päätös-casen JSON-skeema
│   └── materiaalidomain_schema_v1.json ← Materiaali-casen JSON-skeema
├── memory/
│   ├── hanke/
│   │   ├── casebook/                  ← Hanke-caset (VTV, Flyvbjerg, OTKES)
│   │   ├── onnettomuudet/             ← Onnettomuusraportit ja tarkastukset
│   │   ├── feedback/                  ← Käyttäjien seurantatiedot
│   │   └── weights/                   ← Oppimisen painokertoimet
│   ├── paatos/
│   │   ├── casebook/                  ← Päätös-caset
│   │   ├── ennakkotapaukset/          ← KKO, KHO, EIT
│   │   ├── onnettomuudet/             ← Poliittiset ja hallinnolliset virheet
│   │   └── feedback/
│   ├── materiaali/
│   │   ├── casebook/                  ← Materiaali-caset (ISO, EN, RILEM)
│   │   ├── onnettomuudet/             ← Tukes, VTT, BRE raportit
│   │   └── feedback/
│   └── yhteinen/
│       ├── index.json                 ← Muistipankki-indeksi
│       ├── lahteet/                   ← Kaikille yhteinen lähdekirjasto
│       └── indeksi/                   ← Yhteinen hakuindeksi
```

---

## Periaatteet

1. **Fysiikka ensin** — materiaalidatan luvut ovat deterministisiä, ei arvailua
2. **Lähde aina** — jokainen case vaatii dokumentoidun lähteen (`lahde.url_tai_viite`)
3. **Virhetyyppi aina** — mikä meni pieleen ja miksi (`virhetyyppi` + `virhetyyppi_kuvaus`)
4. **Oppi aina** — mitä pitää tehdä toisin seuraavalla kerralla (`oppi`)

---

## Oppimislogiikka

```
1. Raportti syötetään → parsitaan casebook-formaattiin
2. Case lisätään moottorin muistipankkiin
3. Sama virhetyyppi esiintyy 3+ kertaa → painokerroin nousee
4. Käyttäjä saa varoituksen: "3 samankaltaista hanketta on kaatunut tähän"
5. Feedback-loop: käyttäjä raportoi lopputuloksen → moottori oppii
```

Painokertoimet (`paino`) vaihtelevat välillä `0.1–2.0`. Alkuarvo on `1.0`.
- Oikea ennuste → `+0.1`
- Väärä ennuste → `-0.05`
- Kynnys varoitukseen → `3` samaa virhetyyppiä

---

## Case-formaatit

Jokainen case noudattaa moottorinsa skeemaa (`schemas/`-kansio).

### Hanke-case (`HNK-YYYY-NNN`)
```json
{
  "case_id": "HNK-2024-001",
  "lahde": {
    "tyyppi": "VTV",
    "nimi": "VTV Tarkastuskertomus 12/2022",
    "url_tai_viite": "https://www.vtv.fi/...",
    "vuosi": 2022
  },
  "otsikko": "Lyhyt nimi tapaukselle",
  "kuvaus": "Mitä tapahtui — 2-5 virkettä.",
  "virhetyyppi": "kustannusharha",
  "vaihe": "suunnittelu",
  "seuraus": {
    "vakavuus": "vakava",
    "kuvaus": "Seurausten kuvaus.",
    "kustannus_ylitys_prosentti": 97
  },
  "oppi": "Konkreettinen toimenpide tai varoitusmerkki.",
  "riskipisteytys": 82,
  "referenssiluokka": "IT-hanke",
  "paino": 1.0,
  "created": "2026-04-27",
  "feedback_count": 0
}
```

### Päätös-case (`PAA-YYYY-NNN`)
```json
{
  "case_id": "PAA-2024-001",
  "lahde": {
    "tyyppi": "KHO",
    "nimi": "KHO 2019:93",
    "url_tai_viite": "https://www.finlex.fi/...",
    "vuosi": 2019,
    "diaarinumero": "KHO:2019:93"
  },
  "otsikko": "Lyhyt nimi tapaukselle",
  "kuvaus": "Mitä päätettiin ja miksi.",
  "paatostyyppi": "hankintapäätös",
  "virhetyyppi": "perusteluvelvollisuuden_laiminlyönti",
  "oikeusala": "hankintaoikeus",
  "seuraus": {
    "vakavuus": "kohtalainen",
    "kuvaus": "Seurausten kuvaus.",
    "kumottu": true
  },
  "oppi": "Konkreettinen toimenpide.",
  "riskipisteytys": 58,
  "ennakkotapaus": true,
  "sovellettava_laki": ["Hankintalaki 1397/2016 § 123"],
  "paino": 1.0,
  "created": "2026-04-27",
  "feedback_count": 0
}
```

### Materiaali-case (`MAT-YYYY-NNN`)
```json
{
  "case_id": "MAT-2024-001",
  "lahde": {
    "tyyppi": "Tukes",
    "nimi": "Tukes — Tapauksen nimi",
    "url_tai_viite": "https://tukes.fi/...",
    "vuosi": 2018
  },
  "otsikko": "Lyhyt nimi tapaukselle",
  "kuvaus": "Mitä tapahtui.",
  "materiaali": {
    "paatyyppi": "teräs",
    "tarkenne": "S355J2 rakenneteräs",
    "standardi": "EN 1993-1-9"
  },
  "vauriotyyppi": "väsymismurtuma",
  "seuraus": {
    "vakavuus": "kriittinen",
    "kuvaus": "Seurausten kuvaus."
  },
  "oppi": "Konkreettinen toimenpide.",
  "standardiviite": ["EN 1993-1-9:2005"],
  "riskipisteytys": 85,
  "paino": 1.0,
  "created": "2026-04-27",
  "feedback_count": 0
}
```

---

## Uuden casen lisääminen

### Automaattisesti (tuotanto)
Pöytäkirja-endpoint kirjoittaa casebookiin automaattisesti:
```
POST /api/poytakirja/submit
```
Vaatii `GITHUB_TOKEN` ympäristömuuttujana backendissä.

### Manuaalisesti
1. Kopioi oikean moottorin skeema (`schemas/`)
2. Täytä kaikki pakolliset kentät
3. Lisää `memory/<moottori>/casebook/initial_cases.json`-taulukkoon
4. Commitoi: `git commit -m "feat: lisää case HNK-2024-001 (Apotti)"`

### Validointi ennen commitia
```bash
# Python-validointi skeemaa vasten
pip install jsonschema
python3 -c "
import json, jsonschema
schema = json.load(open('schemas/hanke_schema_v1.json'))
case   = json.load(open('memory/hanke/casebook/initial_cases.json'))
for c in case:
    jsonschema.validate(c, schema)
print('Kaikki caset valideja')
"
```

---

## Yhteys päärepoihin

| Repo | Rooli |
|---|---|
| `seurausten-talo-core` | Backend — moottori lukee muistipankista `memory_bank.py`:n kautta |
| `seurausten-talo` | Frontend — käyttöliittymä |
| `seurausten-talo-memory` | Tämä repo — data ja oppiminen |

### Miten backend lukee muistipankin
`memory_bank.py` hakee caset suoraan GitHubin raw-sisällöstä:
```
https://raw.githubusercontent.com/unpersonanarkia-debug/seurausten-talo-memory/main/memory/<moottori>/casebook/initial_cases.json
```
Kirjoitus tapahtuu GitHub Contents API:n kautta (`GITHUB_TOKEN` fine-grained PAT, `contents:write`).

---

## Ympäristömuuttujat (backend)

| Muuttuja | Arvo | Pakollinen |
|---|---|---|
| `GITHUB_TOKEN` | Fine-grained PAT, `contents:write` `seurausten-talo-memory`-repoon | Kyllä (kirjoitus) |
| `MEMORY_REPO_URL` | `https://raw.githubusercontent.com/unpersonanarkia-debug/seurausten-talo-memory/main` | Ei (oletusarvo ok) |

---

*Seurausten Talo Memory Bank v1.1 · 2026-05-12*
