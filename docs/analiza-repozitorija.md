# Analiza repozitorija reviv-plus (stanje 28.9.2026.)

Interni dokument. Služi kao podloga za ponudu „Sidrena cijena + dnevni cjenik”.

## 1. Arhitektura

| Sloj | Tehnologija | Gdje |
|---|---|---|
| Web (reviv-plus.com) | Statični HTML + Bootstrap 5 + jQuery, bez build koraka | ovaj repo (`index.html`, `successful-payment.html`, `unsuccessful-payment.html`, pravne stranice) |
| API | .NET (FastEndpoints), SQL Server, Hangfire | zasebni repo `apex-performance`, domena `apex-performance.fit` |
| Plaćanje | Stripe Checkout (hosted) | Stripe račun ReViv Plus |
| Dostava | BoxNow widget + API | `index.html` + `apex-performance` |
| Analitika | GTM + GA4 | `index.html`, `successful-payment.html` |

Deploy weba je ručan (u repou su i arhive `web-shop-publish.zip` / `old_reviv_plus.zip`), a grana `production` je glavna.

## 2. Kako se danas prikazuju cijene

- Na stranici su **2 proizvoda**: ReViv PLUS 60 kapsula (`prod_TJtDu45campD2B`) i ReViv PLUS 30 kapsula (`prod_TJtERAoefR0NUO`), oba u `index.html` (oko retka 570).
- Cijene **nisu upisane u HTML**. Pri učitavanju stranice `getProductPrices()` (`index.html:1257`) šalje `POST https://apex-performance.fit/api/products/stripe` i iz Stripea dobiva cijenu. Cijena se zatim doda u tekst opisa proizvoda, npr. „ReViv PLUS 60 kapsula (33,15 €)”, i spremi u `data-product-price`.
- **Košarica** (`index.html:1340–1420`) iz toga prikazuje jediničnu cijenu, iznos retka i ukupni iznos.
- **Stripe Checkout** prikazuje cijenu iz Stripea. Tu sadržaj kontroliramo samo preko naziva i opisa proizvoda.
- `successful-payment.html` prikazuje stavke plaćene narudžbe (podatke dohvaća s API-ja).

Mjesta gdje treba prikazati i sidrenu cijenu:
1. kartice proizvoda u sekciji „Odaberi proizvod”,
2. košarica (jedinična cijena),
3. Stripe Checkout (preko opisa proizvoda, ako to zatraži knjigovodstvo ili pravna služba),
4. potencijalno potvrda narudžbe i e-mail potvrde (treba provjeriti tumačenje „svugdje gdje se prikazuje cijena”).

**Cijene mijenja klijent sam u Stripeu**, a web ih samo povlači. Posljedice za rješenje:
- sidrena cijena (fiksna na 10.9.2026.) čuva se u našem sustavu, ne u Stripe metapodacima koje klijent uređuje;
- cjenik se mora ponovno objaviti i kad se cijena promijeni tijekom dana (Stripe webhook `price.*` / `product.updated`), a ne samo u 07:30;
- sidrena cijena na Stripe Checkoutu ide kroz `custom_text` checkout sessiona, ne kroz opis proizvoda;
- nova cijena u Stripeu mora biti postavljena kao `default_price`, što treba napisati u upute klijentu.

## 3. Što nam postojeća platforma već daje

- **Jedan izvor cijena (Stripe).** Trenutnu cijenu CSV i web čitaju s istog mjesta, pa ne mogu jedno drugom proturječiti.
- **Hangfire je već u API-ju.** Dnevni posao (npr. u 07:30 po zagrebačkom vremenu) dodajemo bez nove infrastrukture.
- **API već servira ReViv Plus endpointe.** Dodatni endpointi za cjenik (`/api/reviv-plus/cjenici`, preuzimanje datoteke) uklapaju se u postojeći obrazac.
- Zato je automatsko generiranje i objava CSV-a **izvedivo bez nove platforme i bez ručnog rada**.

## 4. Rizici i otvorena pitanja

1. **PDV u cijeni.** Checkout na Stripe stavke dodaje `TaxRates`. Ako je porezna stopa *exclusive*, cijena na stranici je bez PDV-a. U tom slučaju ni prikaz ni cjenik nisu u skladu s pravilom o isticanju maloprodajne cijene. **Prvo treba provjeriti u Stripe dashboardu.**
2. **Točan format cjenika.** Stupci, separator, kodna stranica i shema naziva datoteke moraju se uzeti iz obavijesti knjigovodstva (nije nam još poslana). Radna pretpostavka je format kakav koriste trgovački lanci (vidi ponudu, prilog A).
3. **Oznaka poslovnog prostora u nazivu datoteke** (npr. `INTERNET_TRGOVINA_…`, adresa sjedišta, oznaka jedinice) mora potvrditi knjigovodstvo.
4. **Domena na kojoj se objavljuje cjenik.** Stranica s popisom mora biti na reviv-plus.com. Same datoteke mogu se servirati s API-ja, preko poddomene (npr. `cjenik.reviv-plus.com` → API) ili izravno s weba. Ovisi o hostingu weba.
5. **Arhiva od 30 dana.** Za zadnjih 30 dana treba čuvati generirane datoteke (u bazi ili na disku/blob storageu), a ne ih generirati iznova. Tako je sadržaj stare datoteke jednak onome što je bilo objavljeno tog dana.
6. **Sidrena cijena na 10.9.2026.** Ako se cijene u Stripeu od tada nisu mijenjale, sidrena je jednaka trenutnoj. Ako jesu, treba je uzeti iz povijesti Stripe cijena. Klijent je najavio da šalje popis proizvoda s cijenama.
7. **Rok.** Obveza vrijedi od 1.10.2026., a danas je 28.9. Za cijeli opseg ostaju 3 radna dana (vidi plan isporuke u ponudi).

## 5. Uočeno usput (izvan opsega ponude)

- U repou su dvije ZIP arhive od po ~11 MB i `.idea` konfiguracija. Ne smetaju radu, ali povećavaju repo.
- Poruke o greškama korisniku su na engleskom (`alert('Something went wrong…')`).
- `README` je prazan, a deploy nije dokumentiran.
