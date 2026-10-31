# Objektiver Markt-Research

Ein Skill für Claude: Zeig ein Produkt (Screenshot aus der Amazon- oder dm-App oder einfach den Namen) – Claude sucht, was **objektiv dasselbe Ergebnis liefert, aber günstiger ist**. Marke und Werbeversprechen zählen nicht, nur nachweisbare Wirkung, Zusammensetzung und Preis pro wirksamer Einheit.

## Was der Skill macht
1. Ermittelt, woran Gleichwertigkeit gemessen wird (z. B. Preis pro kg Protein, Wirkspektrum einer Desinfektion, Stiftung-Warentest-Ergebnis bei Sonnenschutz).
2. Vergleicht Ladenpreise (dm, Rossmann, REWE, Knuspr) inkl. Eigenmarken.
3. Durchsucht Amazon.de – bei verbundenem Apify-Connector mit echten Lieferzeiten für deine Postleitzahl.
4. Prüft unabhängig: Stiftung Warentest, Öko-Test, Datenblätter, Studien.
5. Macht einen Sicherheits- und Echtheitscheck (Hersteller, Zertifikate, Rückrufe, kritische Bewertungen).
6. Antwortet kurz auf Deutsch: Top-Pick, Begründung mit Beleg, Vergleichstabelle, ehrliche Unterschiede zum Original.

Ist dein Produkt schon das günstigste Gleichwertige, sagt der Skill: „Bleib dabei.“

Kein Shop und keine Marke wird bevorzugt.

## Installation
- **Claude-Verzeichnis:** In Claude unter *Anpassen* → Verzeichnis nach „Objektiver Markt-Research“ suchen → *Installieren*.
- **Claude Code:** `/plugin install objektiver-markt-research --marketplace vidividivici/objektiver-markt-research`
  (oder `/plugin marketplace add vidividivici/objektiver-markt-research`, danach `/plugin install objektiver-markt-research@objektiver-markt-research`)
- **Manuell:** Den Ordner `skills/objektiver-markt-research` als ZIP in Claude unter *Anpassen → Skills → + → Skill hochladen* hochladen.

## Optional
- **Apify-Connector** für genauere Amazon-Daten (Preise, Lieferzeit zur eigenen PLZ, Bewertungen). Ohne Apify nutzt der Skill die normale Websuche.

## Datenschutz
Der Skill speichert und sendet keine Daten. Er nutzt nur Claudes eigene Websuche und – falls von dir verbunden – deinen Apify-Connector. Die abgefragte Postleitzahl wird nur für die Lieferzeit-Abfrage im aktuellen Chat verwendet.

## Hinweis
Preise und Verfügbarkeit sind Momentaufnahmen. Keine medizinische Beratung.

## Lizenz
MIT
