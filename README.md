# n8n-Vorlagen

Sofort importierbare [n8n](https://n8n.io)-Workflows für kleine Betriebe, Websites und Communities. Die Vorlagen sind aus Automatisierungen entstanden, die ich selbst im Einsatz habe, und enthalten keine Zugangsdaten.

| Workflow | Was er macht |
|---|---|
| [guten-morgen-telegram.json](workflows/guten-morgen-telegram.json) | Schickt jeden Morgen um 7 Uhr einen zufälligen Gruß mit Datum in einen Telegram-Chat oder -Kanal |
| [rss-zu-telegram.json](workflows/rss-zu-telegram.json) | Postet neue Einträge eines RSS-Feeds (z. B. Blog oder Shop) nach Telegram – ohne Dubletten und mit Ruhezeit von 22 bis 7 Uhr |
| [kontaktformular-zu-telegram.json](workflows/kontaktformular-zu-telegram.json) | Nimmt Anfragen aus dem Kontaktformular der Website per Webhook an, prüft sie (inkl. Honeypot gegen Spam) und meldet sie sofort per Telegram |

## Import

1. JSON-Datei öffnen → **Raw** → Inhalt kopieren
2. In n8n einen neuen Workflow anlegen und mit `Strg + V` einfügen
3. Im Telegram-Node ein Credential mit deinem Bot-Token anlegen (Bot erstellen über [@BotFather](https://t.me/BotFather))
4. `DEINE_CHAT_ID` durch die ID deines Chats oder Kanals ersetzen (z. B. `@meinkanal` oder `-100…`)
5. Workflow aktivieren

Getestet mit n8n 1.x. Die Zeitzone ist in den Workflow-Einstellungen auf `Europe/Berlin` gesetzt.

## Hinweise zu den Workflows

**RSS an Telegram:** Feed-URL im Node „RSS-Feed lesen“ eintragen. Ruhezeiten und maximale Posts pro Lauf stehen oben im Code-Node. Welche Links schon gesendet wurden, merkt sich n8n nur bei aktivem Workflow (Static Data).

**Kontaktformular:** Das Formular schickt `name`, `email` und `nachricht` per POST an die Production-URL des Webhooks. Zusätzlich ein verstecktes Feld `website` einbauen: Wird es ausgefüllt, war es ein Bot.

```html
<form id="kontakt">
  <input name="name" required>
  <input name="email" type="email" required>
  <textarea name="nachricht" required></textarea>
  <input name="website" style="display:none" tabindex="-1" autocomplete="off">
  <button>Senden</button>
</form>
<script>
document.getElementById('kontakt').addEventListener('submit', async (e) => {
  e.preventDefault();
  const daten = Object.fromEntries(new FormData(e.target));
  const r = await fetch('https://DEIN-N8N/webhook/kontaktformular', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(daten),
  });
  alert(r.ok ? 'Danke, wir melden uns!' : 'Bitte alle Felder korrekt ausfüllen.');
});
</script>
```

Wenn Formular und n8n auf unterschiedlichen Domains laufen, muss n8n CORS für die Website erlauben.

## Lizenz

MIT – siehe [LICENSE](LICENSE).

---

Von [Frank Panzer – Panzer IT](https://panzerit.de). Du brauchst eine eigene Automatisierung? Meld dich gern.
