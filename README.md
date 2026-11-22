# Schritt-für-Schritt-Einführung in das Angular-Framework

📖 **Das Tutorial lesen: [https://stahe.github.io/de-angular-etape-par-etape-sept-2026/](https://stahe.github.io/de-angular-etape-par-etape-sept-2026/)**

Dieser Kurs zeigt Ihnen, wie Sie mit dem Framework [Angular](https://angular.dev) 22 eine Webanwendung erstellen: eine **Single-Page-Anwendung** (SPA), deren Seiten **im Browser** aus JSON-Daten eines Servers gerendert werden.

Er knüpft an den Kurs [Schritt-für-Schritt-Einführung in das Web-Framework NestJS](https://stahe.github.io/de-nestjs-html-sept-2026/) an, bietet aber eine andere Perspektive, und folgt dem Aufbau der [Schritt-für-Schritt-Einführung in das Framework Vue.js](https://stahe.github.io/de-vuejs-etape-par-etape-sept-2026/): derselbe Server, dieselben Seiten, im Angular-Stil geschrieben.

| NestJS-Kurs | Angular-Kurs |
|---|---|
| der Server erzeugt die HTML-Seiten (Handlebars) | der Browser erzeugt die Seiten (Angular) |
| der Browser zeigt an, was er empfängt | der Server liefert nur JSON |
| Controller, Views, `res.render` | Komponenten, Router, Services |
| Guards `JwtAuthGuard`, `RolesGuard` | Router-Guards (der Server pflegt seine eigenen) |
| Wörterbücher, die vom Server gelesen werden | Wörterbücher auf dem Client (ngx-translate); der Server liefert nur Schlüssel |
| Flash-Nachricht in einem Cookie | Flash-Nachricht in einem Signal-Service |

Die Seiten selbst bleiben jedoch unverändert: Es sind dieselben der Anwendung **RdvMedecins**, die bereits mit den anderen Frameworks vorgestellt wurde.

## Der Ansatz: viele kurze Beispiele, dann eine Fallstudie

Da Angular funktionsreicher ist als Vue.js, ist der Kurs um **25 kurze Beispiele** aufgebaut, die jeweils auf ein einziges Konzept ausgerichtet sind. Sie bilden einen einzigen Angular-Workspace: ein einziger Befehl `npm install`, danach `npm start <beispiel>`, um eines auszuführen.

| Kapitel | Inhalt | Beispiele |
|---|---|---|
| Erste Schritte | ein Angular-Workspace, Signals (`signal`, `computed`, `effect`, `linkedSignal`), Templates (`@if`, `@for`, `@let`), Ereignisse, `ngModel`, signalbasierte Formulare, Validierung, reaktive Formulare, Pipes | 01–09 |
| Komponenten | `input()`, `output()`, `model()`, Content Projection, Lebenszyklus, Services und Dependency Injection, Direktiven, Bestätigungsdialog | 10–16 |
| Routing | der Router, Parameter, Query, Lazy Loading, Navigations-Guards | 17–18 |
| Observables und gemeinsamer Zustand | RxJS (`debounceTime`, `switchMap`, `toSignal`), Signal-Services, `localStorage` | 19–20 |
| Internationalisierung | ngx-translate: Parameter, Plurale, Datumsangaben, Beträge | 21 |
| Der Server: eine Blackbox | Installation des JSON-Servers, seine API, 48 Beispiele mit `curl` | – |
| Kommunikation mit dem Server | `HttpClient`, Proxy von `ng serve`, `httpOnly`-Cookie, Interceptor, API-Zugriffsschicht, `httpResource`, an Felder gebundene Serverfehler | 22–25 |

Jedes Beispiel wird mit seinem vollständigen, Zeile für Zeile kommentierten Code und einem Screenshot seiner Ausführung präsentiert.

## Der Server: eine Blackbox

Der Server ist der NestJS-Server der vorherigen Lektion, dessen Controller **JSON** zurückgeben – genau wie für den Vue.js-Client. Diese Lektion behandelt ihn als **Blackbox**: Wir installieren ihn, untersuchen seine API und fragen ihn mit `curl` ab – müssen aber seinen Code nicht lesen (er wird für Neugierige mitgeliefert und kommentiert).

- Alle Fehler haben dasselbe Format: `{ "statusCode": 409, "cle": "ERRORS. LOGIN_TAKEN", "params": {...}, "champs": {...} }` — Übersetzungs-**Schlüssel**, niemals tatsächlicher Text;
- Authentifizierung über ein JWT-Token in einem Cookie mit `httpOnly` / `sameSite=strict`: Das clientseitige JavaScript sieht das Token nie;
- CSRF-Schutz: `sameSite`-Cookie, und alle POST-Anfragen müssen in JSON erfolgen;
- Ein „Testmodus" für das CAPTCHA, damit die API mit `curl` abgefragt werden kann.


## Die Fallstudie: der RdvMedecins-Angular-Client

Eine vollständige Anwendung zur **Terminbuchung in einer Arztpraxis**, mit **allen** Dateien (etwa 50), aufgelistet und kommentiert.

- **Modernes Angular**: Standalone-Komponenten, Signals, Change Detection ohne zone.js, signalbasierte Formulare mit eigenen Controls (`FormValueControl`), bedarfsgesteuert geladene Seiten.
- **Drei Rollen**: `ADMIN` (verwaltet Ärzte und Kunden), `DOCTOR` (plant Termine und sagt sie ab), `USER` (der Patient: bucht Termine für sich selbst, verwaltet sein Konto).
- **Datenschutz**: Ein Patient erhält nie die Namen anderer Patienten – der Server sendet sie nicht.
- **Vollständiger Seitenzustand in der URL**: `/agenda?idMedecin=1&jour=2026-10-05&reserver=7` öffnet das Buchungsfenster nach F5 erneut; die Schaltflächen Zurück/Weiter funktionieren.
- **Serverseitige Validierung**: Formulare zeigen die von der API zurückgegebenen Fehler unter jedem Feld an; optimistisches Sperren, doppelte Namen, bereits vergebener Login usw.
- **Sitzung**: Wird nach F5 wiederhergestellt (`GET /api/auth/moi`); der Ablauf wird an einer einzigen Stelle verwaltet (ein HTTP-Interceptor, Antwort 401).
- **Französisch / Englisch**, übersetzte Tab-Titel, per Tastatur bedienbares Bestätigungsfenster, Bootstrap 5.
- **Bereitstellung**: Der kompilierte Client wird vom JSON-Server selbst ausgeliefert (gleicher Ursprung, kein CORS).

## Technologien

Angular 22.2 (Standalone-Komponenten, Signals, signalbasierte Formulare, zoneless) · TypeScript 6.0 · Angular CLI · RxJS 7.8 · ngx-translate 18 · Bootstrap 5.3 · serverseitig: NestJS 10 · TypeORM · MySQL 8 / MariaDB · Passport JWT · svg-captcha

## Voraussetzungen

- Grundlegende Kenntnisse in JavaScript (oder TypeScript), HTML und dem HTTP-Protokoll.
- Node.js 24 (oder mindestens 22.22.3), Visual Studio Code mit der Erweiterung Angular Language Service und ein MySQL-Server (z. B. Laragon unter Windows) für den JSON-Server. Installationsanweisungen finden Sie in den Anhängen des Kurses.

## Autor

Dieser Kurs, seine Beispiele, der JSON-Server und die Fallstudie wurden von **Claude**, der KI von [Anthropic](https://www.anthropic.com) (September 2026), im Auftrag von **Serge Tahé** geschrieben
