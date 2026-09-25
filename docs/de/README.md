# Kopist: Kurzfassung auf Deutsch

Diese Kurzfassung ist nicht normativ. Maßgeblich ist die englische Dokumentation; bei Abweichungen
gilt der englische Text.

## Worum geht es?

KI-Agenten lesen Webseiten, Dokumente, Bilder und Tool-Ergebnisse und handeln danach. Jeder dieser
Inhalte kann versteckte Anweisungen enthalten, die sich an das Modell richten statt an den Menschen.
Belegte Folgen sind unter anderem:

- gekaperte Browser-Agenten
  ([Brave, 2025](https://brave.com/blog/comet-prompt-injection/));
- Codeausführung auf Entwicklerrechnern
  ([CVE-2025-53773](https://msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2025-53773));
- Datenabfluss ohne Nutzerinteraktion
  ([EchoLeak, CVE-2025-32711](https://arxiv.org/html/2509.10540v1)).

Die Modellhersteller härten ihre Modelle, halten Prompt Injection aber selbst für wahrscheinlich nie
vollständig lösbar
([OpenAI, 2025-12-22](https://openai.com/index/hardening-atlas-against-prompt-injection/)).

## Was Kopist macht

Kopist ist ein lokal betriebenes Gateway, über das Agenten Inhalte beziehen. Der Name steht für das
Prinzip: Ein Kopist fertigt eine saubere Abschrift an und gibt nie das Original weiter.

1. **Neu aufbauen statt filtern:** Inhalte werden in ein geschlossenes internes Format übersetzt und
   daraus neu erzeugt. Versteckter Text, unsichtbare Unicode-Zeichen, Skripte, Metadaten und
   Skalierungs-Payloads in Bildern erreichen das Modell nicht.
2. **Alles kennzeichnen:** Jede Ausgabe trägt ein Label (Herkunft, Integrität, Vertraulichkeit, Form)
   und eine signierte, offline prüfbare Quittung nach der offenen Spezifikation
   [Agent Content Trust](../../spec/README.md).
3. **Typisierte Extraktion:** Ein isoliertes Lese-Modell ohne Tools und ohne Netz liefert geprüfte
   Werte statt Fließtext.
4. **Egress standardmäßig gesperrt:** Ausgehende Anfragen, Tool-Aufrufe, Markdown-Bilder und
   Link-Vorschauen laufen durch eine Kontrollinstanz.
5. **Lokal und offen:** eine Binärdatei auf `127.0.0.1`, keine Telemetrie, jedes Modell, jedes
   Agent-Framework. Lizenz Apache-2.0 (Code) und CC-BY-4.0 (Doku, Spezifikation).

Kopist behauptet nicht, Prompt Injection zu "verhindern". Die Rekonstruktion beseitigt versteckte
Kanäle. Sichtbare Anweisungen im Inhalt werden gekennzeichnet, über typisierte Extraktion umgangen und
über die Egress-Kontrolle in ihren Folgen begrenzt. Beides wird getrennt gemessen. Details:
[Garantie-Register](../security/guarantees.md).

## Stand und Fahrplan

Das Projekt ist in der Konzeptphase (Stand 2026-09). Der [Fahrplan](../../ROADMAP.md) reicht bis 2036:
erste Bausteine Ende 2026, lokales MVP 2027, Version 1.0 und Übergabe an eine neutrale Stiftung 2028.

## Mitmachen

Besonders hilfreich sind jetzt:

- Review von [Threat Model](../threat-model.md) und [Architektur](../architecture.md);
- öffentlich dokumentierte Angriffstechniken für den Test-Korpus (Issue-Vorlage "attack technique");
- Hinweise auf fehlende Forschung oder Werkzeuge in [landscape.md](../landscape.md).

Regeln: [CONTRIBUTING.md](../../CONTRIBUTING.md). Sicherheitslücken bitte nur über
[SECURITY.md](../../SECURITY.md) melden.

## Checkliste für Maintainer: Einrichtung

Diese Schritte lassen sich nicht über Dateien im Repository erledigen:

1. GitHub-Organisation anlegen, Repository übertragen und in `kopist` umbenennen (GitHub leitet die
   alte Adresse weiter). Danach Links in `REUSE.toml`, `CITATION.cff` und `security-insights.yml`
   anpassen.
2. Projekt-Domain registrieren und den Platzhalter `<spec-domain>` in der Spezifikation ersetzen.
3. Markenrecherche zu "Kopist" (TMview, Nizza-Klassen 9 und 42).
4. Repository-Einstellungen: Private Vulnerability Reporting, Secret Scanning mit Push Protection,
   Discussions, Ruleset für `main` (Pull Request Pflicht, Status-Checks, nur Squash-Merge, keine
   Force-Pushes), DCO-App installieren.
5. GitHub Sponsors aktivieren und `.github/FUNDING.yml` prüfen.
6. OpenSSF Best Practices Badge beantragen (<https://www.bestpractices.dev/>).
