<h1 align="center">🔗 Anonyme Links</h1>

<p align="center">
  Einzelne HTML-Datei, die URLs von Tracking-Parametern befreit und über einen
  Referrer-Filter umleitet.<br>Kein Server, kein Build, keine Abhängigkeiten.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Typ-Single--File_HTML-0ea5e9?style=flat-square" alt="Single-File HTML">
  <img src="https://img.shields.io/badge/Abhängigkeiten-keine-22c55e?style=flat-square" alt="Keine Abhängigkeiten">
  <img src="https://img.shields.io/badge/Offline-lauffähig-22c55e?style=flat-square" alt="Offline lauffähig">
  <img src="https://img.shields.io/badge/Lizenz-Apache_2.0-64748b?style=flat-square" alt="Apache 2.0">
</p>

---

## Was es macht

Beim Anklicken eines Links überträgt der Browser mit dem `Referer`-Header,
von welcher Seite du kommst. Zusätzlich hängen die meisten geteilten URLs
Tracking-Parameter an, über die sich Kampagne, Kanal und oft die einzelne
Person zurückverfolgen lassen.

Dieses Werkzeug tut zweierlei:

1. **Tracking-Parameter entfernen** — `utm_*`, `fbclid`, `gclid`, `msclkid`,
   `mc_eid` und rund 45 weitere bekannte Marker werden aus der URL geschnitten.
   Funktionale Parameter bleiben unangetastet.
2. **Referrer verbergen** — die bereinigte URL wird über einen
   Weiterleitungsdienst gekapselt, sodass die Zielseite nicht sieht, woher der
   Klick kam.

Alles passiert **lokal im Browser**. Es gibt keinen Server, keine Analyse und
keine Speicherung — die Datei lädt auch keine externen Skripte, Schriften oder
Bilder nach.

---

## Verwendung

```
1. Repository klonen oder link-anonym.html herunterladen
2. Datei per Doppelklick im Browser öffnen
3. URL eingeben → "Anonymisieren"
4. Gewünschtes Format über den Kopieren-Knopf übernehmen
```

Die Datei ist vollständig eigenständig. Sie funktioniert per `file://` ohne
Internetverbindung, lässt sich aber genauso auf einen Webspace legen oder
über GitHub Pages ausliefern.

---

## Ausgabeformate

| Format | Beispiel |
| :--- | :--- |
| **Direktlink** | `https://anonym.to/?https%3A%2F%2Fexample.com` |
| **HTML** | `<a href="…" target="_blank" rel="noreferrer noopener">…</a>` |
| **BBCode** | `[url=…]https://example.com[/url]` |
| **Markdown** | `[https://example.com](…)` |

Zusätzlich gibt es einen Knopf **„Anonymisiert öffnen"**, der den Link direkt
in einem neuen Tab aufruft.

---

## Einstellungen

| Einstellung | Standard | Wirkung |
| :--- | :--- | :--- |
| **Dienst** | `anonym.to` | Weiterleitungsdienst. Alternativ `href.li` oder *nur Parameter säubern* (ganz ohne Drittdienst). |
| **Tracking-Parameter entfernen** | an | Schneidet bekannte Marker aus der URL. |
| **Anker beibehalten** | an | Behält den `#abschnitt`-Teil der URL. |

Änderungen an den Einstellungen berechnen ein bereits angezeigtes Ergebnis
sofort neu.

### Eigenen Dienst eintragen

Im `<script>`-Block, Objekt `SERVICES`:

```js
var SERVICES = {
  "anonym.to": function (url) { return "https://anonym.to/?" + encodeURIComponent(url); },
  "href.li":   function (url) { return "https://href.li/?" + url; },
  "degoogle":  function (url) { return url; }
};
```

Neuen Eintrag ergänzen und im `<select id="service">` eine passende `<option>`
mit demselben `value` hinzufügen. Beachte, dass manche Dienste die Ziel-URL
kodiert erwarten und andere unkodiert.

### Parameterliste erweitern

Die Erkennung arbeitet mit zwei Listen — `TRACKING_EXACT` für exakte Namen und
`TRACKING_PREFIX` für Präfixe wie `utm_`. Der Vergleich ist unabhängig von
Groß- und Kleinschreibung.

---

## Sicherheit

- **Protokoll-Weißliste:** nur `http:` und `https:` werden akzeptiert.
  `javascript:`, `data:`, `file:` und andere Schemata werden mit einer
  Meldung abgewiesen.
- **Kein `innerHTML` mit Nutzereingaben.** Alle Ausgaben laufen über
  `textContent` beziehungsweise `createTextNode`, wodurch die Seite gegen
  eingeschleusten Code aus der eingegebenen URL unempfindlich ist.
- **`rel="noreferrer noopener"`** an allen erzeugten Links, dazu
  `<meta name="referrer" content="no-referrer">` für die Seite selbst.
- **Keine externen Ressourcen**, also auch keine Anfragen an CDNs, aus denen
  sich die Nutzung ableiten ließe.

> [!WARNING]
> Das Werkzeug verbirgt den **Referrer** und entfernt **Tracking-Parameter**.
> Es ersetzt **kein VPN**: deine IP-Adresse bleibt sichtbar, und gegen
> Browser-Fingerprinting hilft es nicht. Für umfassenden Schutz zusätzlich
> ein VPN oder einen eigenen Proxy verwenden.

---

## Barrierefreiheit und Kompatibilität

- Bedienbar per Tastatur, sichtbarer Fokusrahmen an allen Bedienelementen.
- Fehler- und Statusmeldungen über `role="alert"` und `aria-live`, damit
  Screenreader sie ansagen.
- Helles und dunkles Farbschema über `prefers-color-scheme`.
- **Kopieren funktioniert auch per `file://`.** `navigator.clipboard` steht
  nur in sicheren Kontexten zur Verfügung; ist es nicht vorhanden, greift
  automatisch eine `execCommand`-Rückfallebene.

---

## Aufbau

```
anonym-link/
├── link-anonym.html    Anwendung (HTML, CSS und JS in einer Datei)
├── README.md
├── LICENSE             Apache License 2.0
└── .gitignore
```

---

## Lizenz

Apache License 2.0 — siehe [LICENSE](LICENSE).

---

<p align="center">
  <a href="https://discord.gg/scg3YqZFU6"><img src="https://img.shields.io/badge/Discord-beitreten-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Discord"></a>
  <a href="https://linktr.ee/nebelbanknet"><img src="https://img.shields.io/badge/Nebelbank.net-Linktree-39E09B?style=for-the-badge&logo=linktree&logoColor=white" alt="Linktree"></a>
</p>

<p align="center">
  <strong>NebelRebell</strong> · <em>The Power of Perfection</em>
</p>
