# PONUDA br. RDD-2026-09-01

> Konačna verzija za klijenta je PDF `Ponuda_Sidrena_cijena_i_cjenik_ReViv_Plus.pdf`.

**Naručitelj:** [naziv tvrtke], reviv-plus.com, n/p Nicola
**Izvršitelj:** Milan Trbojević
**Datum:** 28.9.2026.
**Predmet:** Usklađivanje web trgovine reviv-plus.com s obvezom isticanja sidrene cijene i objave dnevnog cjenika

---

## 1. Kratki odgovori na pitanja

**Jesam li upoznat s pravilima?**
[Miki: upiši svoje iskustvo. Npr. „Da, pravila su ista kakva od 2025. vrijede za trgovačke lance (sidrena cijena, dnevni cjenik do 8:00, arhiva 30 dana). Primjena na web trgovinu je ista.” / „Radim to i za …”]

**Može li se CSV generirati i objavljivati automatski s naše platforme?**
Da. Cijene za reviv-plus.com već se čitaju iz Stripea preko našeg API-ja, a API već ima sustav za zakazane poslove. Cjenik će se svaki dan u 07:30 sam generirati iz istih cijena koje kupac vidi na stranici i odmah objaviti. Svakodnevno ručno ništa ne treba raditi. Cijena u cjeniku i cijena na stranici uvijek će se podudarati, jer dolaze iz istog izvora.

**Koliko vremena i koliko košta?**
Ukupno **27 sati rada**, odnosno **1.080,00 €** (vidi točku 3). Najbitniji dio isporučujem do **1.10.2026.**, a ostatak do **6.10.2026.** (vidi točku 4).

## 2. Opseg posla

### A) Sidrena cijena na stranici
- Uz svaku trenutnu cijenu ispisuje se i sidrena cijena, npr.
  `33,15 €` · *Sidrena cijena na 10.9.2026.: 33,15 €*
  Sidrena cijena je manjim fontom, ali jasno čitljiva i na mobitelu.
- Prikaz na svim mjestima gdje se vidi cijena: kartice proizvoda, košarica, stranica potvrde narudžbe te opis proizvoda na Stripe stranici za plaćanje.
- Sidrena cijena je fiksna (10.9.2026.), pa se ne čuva u Stripeu nego u našem sustavu. Unosim je jednom i ne može se slučajno obrisati pri uređivanju proizvoda u Stripeu.
- Klijent i dalje sam mijenja cijene u Stripeu. Stranica odmah prikazuje novu trenutnu cijenu, a sidrena ostaje ista.

### B) Dnevni cjenik (CSV)
- **Automatsko generiranje** svaki dan u 07:30 (po zagrebačkom vremenu), s trenutnom i sidrenom cijenom za sve proizvode.
- **Propisani naziv datoteke**, npr. `INTERNET_TRGOVINA_[ADRESA]_[OZNAKA]_[ŠIFRA]_01102026_0730.csv`. Točan oblik uskladit ću s obaviješću knjigovodstva.
- **Stupci** prema obavijesti knjigovodstva. Radni prijedlog je u prilogu A.
- **Promjena cijene tijekom dana.** API prati promjene cijena u Stripeu i u roku od nekoliko minuta objavljuje novu verziju cjenika s novim vremenom u nazivu datoteke.
- **Arhiva od 30 dana.** Svaka datoteka čuva se točno onakva kakva je objavljena, a starije od 30 dana brišu se automatski.
- **Stranica „Cjenici”** na reviv-plus.com (npr. `reviv-plus.com/cjenici`) s popisom datoteka za zadnjih 30 dana i poveznicom u podnožju svih stranica.
- **Automatsko preuzimanje.** Svaka datoteka ima stalan izravni URL. Uz to postoji i strojno čitljiv popis (JSON), kako bi ga alati za prikupljanje cijena mogli preuzimati bez ručnog klikanja.
- **Nadzor.** Ako cjenik iz bilo kojeg razloga nije generiran do 07:45, stiže e-mail upozorenje meni i vama, pa ima vremena reagirati prije 8:00.

### C) Provjera i puštanje u rad
- Provjera je li cijena na stranici iskazana s PDV-om, kako zakon zahtijeva (provjera postavki poreza u Stripeu).
- Testiranje na mobitelu i računalu, puštanje u rad i provjera prvog automatski objavljenog cjenika.
- Kratke upute (1 stranica): kako promijeniti cijenu i gdje se vidi cjenik.

**Nije uključeno:** XML format (može se dodati, oko 2 h), izmjene poslovnog procesa knjigovodstva i pravno tumačenje propisa.

## 3. Cijena

| Stavka | Sati |
|---|---:|
| A) Sidrena cijena: API, prikaz na stranici, košarici, potvrdi i u Stripeu | 6 |
| B1) Generiranje CSV-a, naziv datoteke, dnevni zakazani posao | 6 |
| B1a) Praćenje promjena cijena u Stripeu i automatska nova verzija cjenika | 3 |
| B2) Arhiva 30 dana i javni URL-ovi za preuzimanje | 4 |
| B3) Stranica „Cjenici” i poveznica u podnožju | 3 |
| B4) Nadzor i e-mail upozorenje | 2 |
| C) Provjera, testiranje, puštanje u rad, upute | 3 |
| **Ukupno** | **27 h** |

**Ukupno: 1.080,00 €** (40,00 €/h). [PDV nije uključen / nisam u sustavu PDV-a]

Hitna isporuka do 1.10. uključena je u cijenu.

**Opcionalno, održavanje:** [20,00 €/mj.] za dnevni nadzor cjenika, reakciju na upozorenja i ažuriranje sidrenih cijena ili novih proizvoda.

## 4. Plan isporuke

| Rok | Isporuka |
|---|---|
| **1.10.2026.** (do 8:00) | Sidrena cijena vidljiva na stranici i u košarici. Prvi CSV cjenik objavljen s ispravnim nazivom datoteke, automatsko dnevno generiranje uključeno. |
| **do 6.10.2026.** | Arhiva od 30 dana, stranica „Cjenici”, strojno čitljiv popis, nadzor i upozorenja, prikaz na Stripe stranici za plaćanje, upute. |

Za rok od 1.10. od vas trebam **do utorka, 29.9. u 17:00**:
1. obavijest knjigovodstva (točan popis stupaca i shema naziva datoteke),
2. popis proizvoda sa sidrenom cijenom na 10.9.2026. i barkodom (EAN), ako postoji,
3. podatke za naziv datoteke: vrstu i adresu poslovnog prostora, oznaku poslovne jedinice i šifru,
4. potvrdu da mogu uključiti metapodatke proizvoda u Stripeu (pristup već imam).

## 5. Uvjeti
- Plaćanje: [50 % avansno, 50 % po isporuci] / [po isporuci, rok 8 dana].
- Ponuda vrijedi 7 dana.
- Izmjene opsega nakon prihvaćanja naplaćuju se po satnici iz točke 3.

---

## Prilog A: radni prijedlog stupaca cjenika

Prema formatu koji trgovački lanci koriste od 2025. (npr. Konzum). Konačni popis ovisi o obavijesti knjigovodstva.

| Stupac | Primjer |
|---|---|
| NAZIV PROIZVODA | ReViv PLUS 60 kapsula |
| ŠIFRA PROIZVODA | RVP-60 |
| MARKA PROIZVODA | ReViv PLUS |
| NETO KOLIČINA | 60 |
| JEDINICA MJERE | kom |
| MALOPRODAJNA CIJENA | 33,15 |
| CIJENA ZA JEDINICU MJERE | 0,55 |
| MPC ZA VRIJEME POSEBNOG OBLIKA PRODAJE | (prazno ako nema akcije) |
| NAJNIŽA CIJENA U POSLJEDNIH 30 DANA | 33,15 |
| SIDRENA CIJENA NA 10.9.2026. | 33,15 |
| BARKOD | 385… |
| KATEGORIJA PROIZVODA | Dodaci prehrani |

Format: CSV, UTF-8, separator `;`, decimalni zarez, cijene u EUR s PDV-om.
