---
name: objektiver-markt-research
description: Findet zu einem Produkt (Screenshot oder Name) eine objektiv gleichwertige, günstigere Alternative – gleiches Endergebnis statt gleicher Marke – auf Amazon.de und im Laden, inkl. Sicherheits- und Echtheitscheck.
---

# Objektiver Markt-Research

Der Nutzer zeigt ein Produkt (meist Screenshot aus Amazon- oder dm-App) und will wissen: **Was liefert objektiv dasselbe Endergebnis – günstiger?** Marke, Werbeversprechen und Etiketten-Claims zählen nicht. Es zählt, was das Produkt nachweislich tut.

## Nutzerkontext
- Zu Beginn einmal kurz fragen, falls nicht bekannt: **Postleitzahl** (für Lieferzeiten) und ob Amazon Prime vorhanden ist. Ohne Antwort: ohne PLZ suchen und Lieferzeiten als „bundesweit, ungefähr“ kennzeichnen.
- Kauft ggf. auch bei dm/Rossmann vor Ort.
- Antwort kurz und klar auf Deutsch (mobil lesbar), mit Tabelle und klarer Empfehlung.
- Es wird weitergesucht, bis eine belastbare Antwort steht – keine halben Aussagen.

## Ablauf

### 1. Produkt verstehen – was ist das „Endergebnis“?
Aus Screenshot/Name ermitteln: Wirkstoffe/Zusammensetzung, Menge, Preis, Grundpreis. Dann festlegen, woran Gleichwertigkeit gemessen wird, z. B.:
- **Seife:** Tenside reinigen mechanisch – jede normale Flüssigseife = gleiches Ergebnis (antibakteriell ist Marketing, FDA 2016)
- **Proteinpulver:** Preis pro kg *reines Protein*, Proteinquelle (Geschmack!), Aminosäureprofil
- **Desinfektion:** Wirkspektrum + Einwirkzeit (EN 1500, begrenzt viruzid PLUS/Noro, mykobakterizid), Status (Arzneimittel vs. Biozid), Listung (RKI/VAH/IHO)
- **Sonnenschutz:** Stiftung-Warentest-Ergebnis (UVA/UVB gemessen), parfümfrei, Hauttyp
- **Verbrauchsartikel:** Preis pro Stück bei gleicher Größe/Material/Form

Wenn der Nutzer frühere Erfahrungen nennt (z. B. „ohne Soja hat ekelhaft geschmeckt“), Inhaltsstoffe des Originals recherchieren und Alternativen danach filtern.

### 2. Ladenpreis-Referenz
Websuche für dm, Rossmann, REWE, Knuspr. Prüfen, ob es eine Eigenmarke gibt, die objektiv gleich ist (z. B. dm Sundance, ebelin, Balea).

### 3. Amazon-Scan

**Ohne Apify (Standard):** Websuche und Abruf von amazon.de-Suchergebnissen und Produktseiten sowie Preisvergleichen (idealo, geizhals). Lieferzeiten dann als „ungeprüft“ kennzeichnen.

**Mit Apify (nur wenn der Nutzer den Apify-Connector verbunden hat):** Tools laden: `ToolSearch select:mcp__Apify__call-actor,mcp__Apify__get-dataset-items,mcp__Apify__get-actor-run`

**Breit (ohne Postleitzahl):** `junglee/amazon-crawler`, 3–4 Such-URLs (Hauptbegriff, Synonym, `&s=review-rank`, ggf. Bulk/Vorratspack), `maxItemsPerStartUrl: 30`, `scrapeProductDetails: false`, `language: de`, `waitSecs: 45`. Felder: `asin,title,price.value,stars,reviewsCount,isSponsored`.

Achtung: `price.value` aus der Suche ist oft mit dem Grundpreis verklebt (z. B. `14704.9` = 14,70 € und 4,90 €/l). Erste Ziffern = Preis, Rest = Grundpreis.

**Shortlist (8–12)** nach Gleichwertigkeit und Preis pro Einheit, dann **Details mit PLZ des Nutzers:** gleiche Actor, Produkt-URLs, `scrapeProductDetails: true`, `zipCode: "<PLZ>"`, `locationDeliverableRoutes: ["PRODUCT"]` (nie `countryCode` dazu). Felder: `asin,title,price.value,listPrice.value,stars,reviewsCount,inStockText,delivery,fastestDelivery,seller.name,monthlyPurchaseVolume,features,attributes,aiReviewsSummary.text,starsBreakdown.1star`.

- **Verfügbarkeit immer prüfen:** „versandfertig in 1–2 Monaten“ ist ein K.-o.-Kriterium und muss dem Nutzer gesagt werden (auch beim Originalprodukt!).
- `fastestDelivery` ≈ Prime-Datum; die Abfrage ist nicht eingeloggt, Prime ist oft schneller – so formulieren.
- Gesponserte Treffer (`isSponsored`) nicht bevorzugen; sie zählen nur nach denselben Kriterien wie alle anderen.

### 4. Objektiv verifizieren (extern, nicht nur Amazon)
- **Stiftung Warentest / Öko-Test** zuerst suchen – das ist der beste objektive Vergleich.
- **Hersteller-Datenblatt, Sicherheitsdatenblatt, Fachinformation** (Zusammensetzung, Wirkstoffkonzentration, Prüfnormen).
- **Unabhängige Studien**, wenn ein Claim zweifelhaft ist (z. B. WHO-Rezeptur-Desinfektion braucht laut Studien 60 s statt 30 s).
- **„Vergleichssieger“** von Vergleichsportalen sind kein Beleg.
- Tippfehler/unsaubere Claims (z. B. falsche Normnummer) als Warnsignal notieren.

### 5. Sicherheits- und Echtheitscheck (bei Lebensmitteln, Kosmetik, Biozid, Arznei immer)
- Hersteller/Importeur mit EU-Adresse? Eigener Shop mit Impressum? Händlerbund o. Ä.?
- Laborzertifikate/Analysezertifikat (Datum!), HACCP/GMP, Listungen
- Rückrufe (lebensmittelwarnung.de) suchen
- Kritische Amazon-Bewertungen auf wiederkehrende Probleme prüfen (mit Apify: `scrape.badger/amazon-reviews-scraper`, `star: critical`, `verified_only: true`; sonst die kritischen Bewertungen auf der Produktseite lesen)
- Gesundheitliche Unterschiede zum Original benennen (z. B. Allergene, Duftstoffe, fehlende Rückfetter)

### 6. Antwort
1. **Top-Pick** in einem Satz mit Preis und Ersparnis in % bzw. pro Einheit, Link `https://www.amazon.de/dp/ASIN` oder Laden-Link
2. **Warum objektiv gleich** – mit unabhängigem Beleg
3. **Tabelle** (5–7 Zeilen): Produkt, Preis, Preis/Einheit, entscheidendes Kriterium, Bewertung, Lieferung; Original als letzte Zeile zum Vergleich
4. **Ehrlicher Unterschied** zum Original (was fehlt, was nur Eigenangabe ist)
5. **Sicherere Alternative**, falls der Top-Pick nur Eigenangaben hat (z. B. geprüftes Arzneimittel statt Biozid)
6. Eine Zeile: wie viele Produkte gescannt, welche Kriterien, Preise = Momentaufnahme
7. Sources-Liste

**Wenn das Original schon das günstigste Gleichwertige ist, klar sagen: „Bleib dabei“** – nicht künstlich etwas empfehlen.

## Regeln
- Kein Shop und keine Marke wird bevorzugt. Ist der günstigste gleichwertige Kauf im Laden vor Ort, ist das der Top-Pick.
- Niemals Wirksamkeit oder Gleichwertigkeit behaupten, die nicht belegt ist. Lieber weitersuchen.
- Preis pro *wirksamer Einheit* vergleichen (pro Liter, pro kg Protein, pro Stück), nicht pro Packung.
- Liefer- und Lagerstatus des Originals mitprüfen.
- Links immer als saubere Produktlinks ohne Tracking-Parameter ausgeben.
- Budget mit Apify: ca. 3–4 Actor-Runs pro Produkt.
