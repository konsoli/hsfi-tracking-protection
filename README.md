# HS Tracking Protector

Estolista Helsingin Sanomien (hs.fi ja HS-sovellus) seuranta- ja mainosdomaineille. Toimii Pi-holessa, uBlock Originissa ja Bravessa.

Lista perustuu oikeasta hs.fi- ja HS-sovellusliikenteestä kerättyihin DNS-lokeihin sekä Sanoman omiin evästekäytäntöihin.

## Mitä lista estää

- Sanoman oma Google Tag Manager -proxy (pongo.hs.fi). Lataa saman GTM-kontin kuin googletagmanager.com, mutta HS:n omalta alidomainilta, joten yleiset listat eivät sitä tunnista.
- AudienceProject (userreport.com, dnt-userreport.com). Lähettää jokaisen luetun artikkelin osoitteen.
- Tag managerit, jotka lataavat mainos- ja seurantakumppanit (Adobe Launch, Google Tag Manager).
- A/B-testaus (Conductrics), sovellusalustan analytiikka (Richie) ja Google Ad Manager.

## Mitä lista ei estä

- Sanoman palvelimilla tapahtuvaa tietojen luovutusta. Kun data on Sanomalla, se voi lähteä kumppaneille ilman yhtään pyyntöä laitteeltasi.
- Seurantaa, joka kulkee samassa domainissa kuin sisältö (esim. www.hs.fi/hs/api/pe/…).
- Sanoma-tiliin kirjautuneen käyttäjän tunnistetta. Tilin tunniste yhdistää laitteesi ja käyttösi.

Siksi tee myös kohta 1 alla. Se on ilmainen ja kattaa sen, mihin estolista ei yllä.

## Tiedostot

- hs-blocklist.txt: päälista. Pi-hole, uBlock Origin ja Brave.
- hs-ublock.txt: vain uBlock Originiin. Stripen petostorjuntasignaalit (Stripe saa jokaisen HS-artikkelin osoitteen ja otsikon). Voi estää maksamisen, myös muilla sivustoilla.

Listojen osoitteet:

```
https://raw.githubusercontent.com/<github-käyttäjä>/hsfi-tracking-protection/main/hs-blocklist.txt
https://raw.githubusercontent.com/<github-käyttäjä>/hsfi-tracking-protection/main/hs-ublock.txt
```

## Käyttöönotto

### 1. Suostumukset ja Sanoma-tili

1. hs.fi: linkki "Evästeasetukset" → kiellä kaikki paitsi välttämättömät. Tee tämä jokaisessa selaimessa erikseen.
2. HS-sovellus: sovelluksen asetuksista tietojen käyttöä koskeva kohta → kiellä.
3. iPhone: Asetukset → Tietosuoja ja turvallisuus → Seuranta → HS pois päältä.
4. Sanoma-tili: oma.sanoma.fi → kohdentamisvalinnat pois.

### 2. Pi-hole (v6)

Lista käyttää ABP-muotoa (||domain^), joka estää myös alidomainit. Vaatii Pi-hole FTL v5.22 tai uudemman.

1. Avaa Pi-holen hallintapaneeli.
2. Valitse vasemmasta valikosta Lists.
3. Liitä Address-kenttään hs-blocklist.txt:n osoite.
5. Klikkaa Add blocklist.
6. Päivitä gravity: Tools → Update Gravity → Update.

Pi-hole päivittää listan automaattisesti.

### 3. uBlock Origin

hs-ublock.txt on tarkoitettu vain uBlock Originiin.

1. Avaa uBlock Originin hallintapaneeli (rataskuvake).
2. Valitse välilehti Filter lists.
3. Valitse alhaalta Custom-osiosta Import.
4. Liitä tekstikenttään molempien listojen osoitteet, yksi per rivi.
5. Klikkaa Apply changes.
6. Firefox ja LibreWolf: tarkista välilehdeltä Settings → Privacy, että "Uncloak canonical names" on päällä (oletuksena on).

Kun maksat jotain Stripen kautta, poista hs-ublock.txt käytöstä Filter lists -välilehdellä ja klikkaa Apply changes.

### 4. Brave

1. Mene osoitteeseen brave://settings/shields/filters. Jos sivulla ei ole listan lisäystä, kokeile brave://adblock.
2. Valitse Add custom filter list (tai Add filter list via URL).
3. Liitä hs-blocklist.txt:n osoite ja tallenna.

## Tarkistus

1. Avaa hs.fi ja lue yksi artikkeli.
2. Pi-holen Query Log: hae pongo.hs.fi. Rivin pitää näkyä estettynä.
3. Varmista, että kirjautuminen, artikkelit, kuvat, video ja kommentit toimivat.

Jos jokin rikkoutuu, avaa issue ja kerro, mikä toiminto ja millä laitteella.
