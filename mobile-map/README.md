# Mobile Map (Aeki)

Mobiilisõbralik kaardibaas, mida saab kasutada suurema projekti sees.

Excelit ei ole enam vaja, kui kasutad olemasolevaid faile siin kaustas:
- `map-data.json` (markerite asukohad)
- `assets/image1.png`, `assets/image2.png` (taustakaardid)
- `index.html` (töötav demo / referents)

## Mis siin on

- 2 korruse kaarti (eraldi pildid)
- markerid tekstidega (`A`, `AE`, `MNOP`, `iHGFE`, jne)
- markerite koordinaadid pildi suhtes protsentides (`xPct`, `yPct`, `wPct`, `hPct`)
- mobiili crop loogika `2nd Floor` jaoks (vasakult `1/3`)

## Soovitus suuremas projektis

Kasuta `map-data.json` kui tõeallikat ja renderda kaardid + markerid oma UI komponendis.

Põhimõte:
- lae põrandad `map-data.json`-st
- kuva `floor.image`
- joonista markerid absoluutpositsiooniga (`left/top/width/height` protsentides)
- rakenda sama crop pildile ja markeri overlayle korraga

## `map-data.json` struktuur

Iga korrus sisaldab:
- `id`
- `title`
- `image`
- `pixelWidth`, `pixelHeight`
- `markers[]`
- `cropLeftPct`, `cropRightPct`, `visibleFrac`, `viewPixelWidth`, `viewPixelHeight` (mobiilivaate crop info)

Marker sisaldab:
- `label`
- `xPct`, `yPct` (vasak/ülalt protsent pildist)
- `wPct`, `hPct` (markeri laius/kõrgus protsent pildist)

## Praegused käsitsi parandused (juba andmetes sees)

- `2nd Floor`: dubleeritud `AK`, `AJ`, `Ai`, `AH`, `AG` eemaldatud
- `2nd Floor`: `iHGFE` marker lubatud (pikem silt)
- `2nd Floor`: `AX` tõstetud `U` ja `AV` vahele
- `2nd Floor`: mobiilis crop vasakult `33.3333%`

## Mida kasutada referentsina

- `index.html` on visuaalne referents (stiil + crop + overlay loogika)
- `map-data.json` on integratsiooni jaoks olulisem kui `index.html`

## Kui tahad hiljem muuta andmeid

Ajalooline generaator on:
- `tools/generate-mobile-map.ps1`

See loeb Excelit ja genereerib `mobile-map` kausta uuesti. Praeguse projekti jaoks ei ole see vajalik, kui markerid/pildid on juba õiged.

## Uues chatis / teises projektis (mida mulle öelda)

Piisab umbes sellisest juhisest:

`Kasuta c:\Cursor\aeki-map\mobile-map kausta. Võta map-data.json markerite allikaks ja ehita selle peale interaktiivne UI.`

Lisaks kasulik täpsustus:

`2nd Floor on mobiilis cropitud vasakult 1/3 ja see peab rakenduma nii pildile kui markeritele.`
