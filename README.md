# MHD Tankstelle Smartphone App

Enthalten:
- Mitarbeiter-Login über Nummer + Passwort
- Smartphone-optimiertes Dashboard
- Barcode-Scanner per Kamera
- kostenlose Produktdaten/Bilder über OpenFoodFacts
- gleiche Produkte mit mehreren MHDs möglich
- eigene Produktbilder per Upload
- Abschriftenliste
- Backwaren Tagesende
- Benachrichtigungen bei bald ablaufender Ware

## Hochladen
1. ZIP entpacken.
2. Inhalt in GitHub Repository hochladen, nicht die ZIP selbst.
3. Vercel deployed automatisch.
4. In Vercel müssen gesetzt sein:
   - VITE_SUPABASE_URL
   - VITE_SUPABASE_ANON_KEY
5. Danach Redeploy/Wiederverlegen.

## Supabase
SUPABASE_SQL.sql im SQL Editor ausführen.

## Login
Beispiel:
- Nummer: 9
- Passwort: 1994

Mitarbeiter können in Supabase Tabelle `mitarbeiter` geändert werden.


## Push-Benachrichtigungen
Diese Version enthält Browser-/PWA-Benachrichtigungen:
- In der App auf „Push aktivieren/testen“ klicken.
- Benachrichtigung im Browser erlauben.
- Android: Chrome unterstützt das sehr gut.
- iPhone: Die Seite über Safari öffnen und „Zum Home-Bildschirm“ hinzufügen, dann als App starten.
- Die App prüft beim Öffnen und regelmäßig während der Nutzung, ob Artikel innerhalb von 2 Tagen ablaufen, und zeigt eine Warnung.

Hinweis: Komplett serverseitige Pushs, auch wenn niemand die App öffnet, benötigen später zusätzlich OneSignal/Firebase oder einen Server-Cron. Diese App ist dafür mit Service Worker vorbereitet.


## Standardpasswort
Neue Mitarbeiter können mit Passwort `0000` angelegt werden. Beim ersten Login fordert die App automatisch ein neues 4-stelliges Passwort an.
Wenn deine Tabelle schon existiert, führe im Supabase SQL Editor aus:

```sql
alter table public.mitarbeiter add column if not exists muss_passwort_aendern boolean default true;
```

Optional für bestehende Mitarbeiter:

```sql
update public.mitarbeiter set passwort = '0000', muss_passwort_aendern = true;
```


## Mitarbeiter & Rollen
Enthaltene Mitarbeiter: Denis, Michael, Chris, Tizi, Philip, Anne, Lars, Mira.
Alle starten mit Passwort 0000 und müssen beim ersten Login ein neues 4-stelliges Passwort setzen.
Rollen: Chef, Stationsleitung, Mitarbeiter. Chef und Stationsleitung dürfen Artikel löschen; Chef sieht zusätzlich den Mitarbeiter-Hinweisbereich.
Rollen und Mitarbeiter können jederzeit in Supabase in der Tabelle `mitarbeiter` geändert werden.
