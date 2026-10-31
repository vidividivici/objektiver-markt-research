---
name: objektiver-markt-research
description: Findet zu einem Produkt (Screenshot oder Name) eine objektiv gleichwertige, günstigere Alternative – gleiches Endergebnis statt gleicher Marke – auf Amazon.de und im Laden, inkl. Sicherheits- und Echtheitscheck.
---

# Objektiver Markt-Research

Der Nutzer zeigt ein Produkt (meist Screenshot aus Amazon- oder dm-App) und will wissen: **Was liefert objektiv dasselbe Endergebnis – günstiger?** Marke, Werbeversprechen und Etiketten-Claims zählen nicht. Es zählt, was das Produkt nachweislich tut.

## Nutzerkontext
- Zu Beginn einmal kurz fragen, falls nicht bekannt: **Postleitzahl** (für Lieferzeiten) und ob Amazon Prime vorhanden ist. Ohne Antwort: ohne PLZ suchen und Lieferzeiten als „bundesweit, ungefähr“ kennzeichnen.
- Verglichen wird **Amazon.de und dm** (online und Filiale).
- Antwort kurz und klar auf Deutsch (mobil lesbar), mit Tabelle und klarer Empfehlung.
- Es wird weitergesucht, bis eine belastbare Antwort steht – keine halben Aussagen.

## Datenquellen
Das Plugin bringt einen eigenen Server **markt-research** mit (Tools `dm_search`, `amazon_search`, `amazon_product_details`, `amazon_critical_reviews`, `get_run_result`). Der Nutzer braucht dafür kein eigenes Konto. Die Kosten trägt der Betreiber – deshalb sparsam:
- Pro Produktanfrage höchstens: 1× `amazon_search`, 1× `amazon_product_details`, 1× `amazon_critical_reviews` (nur für den Top-Pick), dazu beliebig `dm_search`.
- Antwortet ein Tool mit `status: "running"`, `get_run_result` mit der `run_id` aufrufen (ggf. mehrmals).
- Meldet ein Tool `status: "limit"` oder ist der Server nicht verfügbar: mit normaler Websuche weitermachen (Abruf von amazon.de-/dm.de-Seiten, idealo, geizhals) und Lieferzeiten als „ungeprüft“ kennzeichnen.

## Ablauf

### 1. Produkt verstehen – was ist das „Endergebnis“?
Aus Screenshot/Name ermitteln: Wirkstoffe/Zusammensetzung, Menge, Preis, Grundpreis. Dann festlegen, woran Gleichwertigkeit gemessen wird, z. B.:
- **Seife:** Tenside reinigen mechanisch – jede normale Flüssigseife = gleiches Ergebnis (antibakteriell ist Marketing, FDA 2016)
- **Proteinpulver:** Preis pro kg *reines Protein*, Proteinquelle (Geschmack!), Aminosäureprofil
- **Desinfektion:** Wirkspektrum + Einwirkzeit (EN 1500, begrenzt viruzid PLUS/Noro, mykobakterizid), Status (Arzneimittel vs. Biozid), Listung (RKI/VAH/IHO)
- **Sonnenschutz:** Stiftung-Warentest-Ergebnis (UVA/UVB gemessen), parfümfrei, Hauttyp
- **Verbrauchsartikel:** Preis pro Stück bei gleicher Größe/Material/Form

Wenn der Nutzer frühere Erfahrungen nennt (z. B. „ohne Soja hat ekelhaft geschmeckt“), Inhaltsstoffe des Originals recherchieren und Alternativen danach filtern.

### 2. dm-Referenz
`dm_search` mit 1–2 Suchbegriffen. Prüfen, ob es eine dm-Eigenmarke gibt, die objektiv gleich ist (z. B. Sundance, ebelin, Balea, Mivolis, Dontodent). Grundpreis aus `unitPrice` übernehmen.

### 3. Amazon-Scan
**Breit:** `amazon_search` mit 3–4 Suchbegriffen (Hauptbegriff, Synonym, ggf. Bulk/Vorratspack), `max_per_query` 30; bei vielen Billigangeboten zusätzlich `sort_by_reviews: true`.

Achtung: `price.value` aus der Suche ist oft mit dem Grundpreis verklebt (z. B. `14704.9` = 14,70 € und 4,90 €/l). Erste Ziffern = Preis, Rest = Grundpreis. Verlässliche Preise liefern erst die Details.

**Shortlist (8–12)** nach Gleichwertigkeit und Preis pro Einheit, dann `amazon_product_details` mit diesen ASINs und – falls bekannt – `plz` des Nutzers.

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
- Kritische Amazon-Bewertungen des Top-Picks mit `amazon_critical_reviews` auf wiederkehrende Probleme prüfen (ohne Server: kritische Bewertungen auf der Produktseite lesen)
- Gesundheitliche Unterschiede zum Original benennen (z. B. Allergene, Duftstoffe, fehlende Rückfetter)

### 6. Antwort
1. **Top-Pick** in einem Satz mit Preis und Ersparnis in % bzw. pro Einheit, Link `https://www.amazon.de/dp/ASIN` oder dm-Link
2. **Warum objektiv gleich** – mit unabhängigem Beleg
3. **Tabelle** (5–7 Zeilen): Produkt, Preis, Preis/Einheit, entscheidendes Kriterium, Bewertung, Lieferung; Original als letzte Zeile zum Vergleich
4. **Ehrlicher Unterschied** zum Original (was fehlt, was nur Eigenangabe ist)
5. **Sicherere Alternative**, falls der Top-Pick nur Eigenangaben hat (z. B. geprüftes Arzneimittel statt Biozid)
6. Eine Zeile: wie viele Produkte gescannt, welche Kriterien, Preise = Momentaufnahme
7. Sources-Liste

**Wenn das Original schon das günstigste Gleichwertige ist, klar sagen: „Bleib dabei“** – nicht künstlich etwas empfehlen.

## Regeln
- Kein Shop und keine Marke wird bevorzugt. Ist der günstigste gleichwertige Kauf bei dm, ist das der Top-Pick.
- Niemals Wirksamkeit oder Gleichwertigkeit behaupten, die nicht belegt ist. Lieber weitersuchen.
- Preis pro *wirksamer Einheit* vergleichen (pro Liter, pro kg Protein, pro Stück), nicht pro Packung.
- Liefer- und Lagerstatus des Originals mitprüfen.
- Links immer als saubere Produktlinks ohne Tracking-Parameter ausgeben.
- Server-Tools sparsam nutzen (siehe Datenquellen); Ergebnisse werden einige Stunden gecacht.
