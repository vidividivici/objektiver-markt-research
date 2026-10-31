# Objektiver Markt-Research

Ein Skill für Claude: Zeig ein Produkt (Screenshot aus der Amazon- oder dm-App oder einfach den Namen) – Claude sucht, was **objektiv dasselbe Ergebnis liefert, aber günstiger ist**. Marke und Werbeversprechen zählen nicht, nur nachweisbare Wirkung, Zusammensetzung und Preis pro wirksamer Einheit.

## Was der Skill macht
1. Ermittelt, woran Gleichwertigkeit gemessen wird (z. B. Preis pro kg Protein, Wirkspektrum einer Desinfektion, Stiftung-Warentest-Ergebnis bei Sonnenschutz).
2. Durchsucht das dm-Sortiment inkl. Eigenmarken.
3. Durchsucht Amazon.de mit echten Preisen, Lieferzeiten für deine Postleitzahl und kritischen Bewertungen.
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

## Live-Daten ohne eigenes Konto
Das Plugin bringt einen kleinen Server mit (`markt-research`), der Amazon.de- und dm.de-Daten abruft. Du brauchst dafür kein eigenes Konto und zahlst nichts. Es gibt ein Tages- und Monatslimit; ist es erreicht, nutzt der Skill automatisch die normale Websuche.

## Datenschutz
- An den Server gehen nur Suchbegriffe, Amazon-Produktnummern (ASIN) und – wenn du sie nennst – deine Postleitzahl für die Lieferzeit.
- Amazon-Abfragen laufen über den Dienstleister Apify (Apify Technologies s.r.o., Prag), der die öffentlichen Amazon.de-Seiten abruft. dm-Abfragen gehen direkt an die öffentliche dm-Produktsuche.
- Ergebnisse werden bis zu 6 Stunden zwischengespeichert, damit gleiche Anfragen nichts kosten.
- Für die Limits wird deine IP-Adresse nur als gesalzener Hash gespeichert und nach 2 Tagen gelöscht. Suchbegriffe und Postleitzahlen werden nicht protokolliert.
- Der Server läuft bei Cloudflare (Cloudflare Workers).
- Sonst speichert der Skill nichts.

Vollständige Datenschutzerklärung: https://markt-research-mcp.vvv-tools.workers.dev/datenschutz

## Anbieter
Veni Vidi Validus GmbH, Kempten (Allgäu) – [Impressum](https://markt-research-mcp.vvv-tools.workers.dev/impressum)

## Hinweis
Preise und Verfügbarkeit sind Momentaufnahmen. Keine medizinische Beratung.

## Lizenz
MIT
