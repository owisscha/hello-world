# Claude Code remote bedienen vanaf je iPhone via Tailscale + SSH + tmux

## Wat je krijgt

Een stabiele, persistente Claude Code sessie op je MacBook die je vanaf je iPhone (of elk ander apparaat) kunt bedienen — zonder dat je sessie, context of toestemmingen verloren gaan.

## Wat je nodig hebt

- MacBook met Claude Code geïnstalleerd
- iPhone
- Tailscale account (gratis voor persoonlijk gebruik)

---

## Stap 1: Tailscale installeren

### MacBook

1. Installeer Tailscale via Homebrew:
   ```bash
   brew install --cask tailscale
   ```
2. Open de Tailscale app en log in met je account (Google, Apple, Microsoft, etc.)
3. Zet Tailscale **aan** via het menu bar icoon
4. Noteer je Tailscale IP (zichtbaar in de app, begint met `100.x.x.x`)

### iPhone

1. Installeer **Tailscale** uit de App Store
2. Log in met **hetzelfde account** als op je MacBook
3. Zet de VPN-verbinding aan wanneer gevraagd
4. Controleer dat je MacBook zichtbaar is in de apparatenlijst

---

## Stap 2: SSH aanzetten op je MacBook

1. Open **Systeeminstellingen**
2. Ga naar **Algemeen → Delen**
3. Zet **Externe login** (Remote Login) aan
4. Onder "Sta toegang toe voor" kies **Alleen deze gebruikers** en voeg jezelf toe

### Controleer of het werkt

Open een terminal op je MacBook en test:

```bash
ssh localhost
```

Als je kunt inloggen, werkt SSH.

---

## Stap 3: tmux installeren en configureren op je MacBook

### Installeren

```bash
brew install tmux
```

### Basisconfiguatie aanmaken

Maak een configuratiebestand aan voor een betere ervaring (vooral via telefoon):

```bash
cat > ~/.tmux.conf << 'EOF'
# Scrollback buffer vergroten
set -g history-limit 50000

# Muis-ondersteuning aan (handig voor scrollen op iPhone)
set -g mouse on

# Betere kleuren
set -g default-terminal "screen-256color"

# Status bar informatief maken
set -g status-left "[#S] "
set -g status-right "%H:%M"
EOF
```

---

## Stap 4: SSH-app installeren op je iPhone

Installeer **Blink Shell** (betaald, beste optie) of **Termius** (gratis tier beschikbaar) uit de App Store.

### Blink Shell configureren

1. Open Blink Shell
2. Typ `config` en ga naar **Keys**
3. Maak een nieuwe SSH-key aan (of importeer een bestaande)
4. Ga naar **Hosts** en maak een nieuwe host aan:
   - **Host**: `macbook` (of een naam naar keuze)
   - **Hostname**: je Tailscale IP (`100.x.x.x`)
   - **User**: je macOS gebruikersnaam
   - **Key**: selecteer de key die je net aanmaakte
5. Sla op

### Termius configureren

1. Open Termius
2. Tik op **+** → **New Host**
3. Vul in:
   - **Alias**: `macbook`
   - **Hostname**: je Tailscale IP (`100.x.x.x`)
   - **Username**: je macOS gebruikersnaam
   - **Password**: je macOS wachtwoord (of configureer een SSH-key)
4. Sla op

---

## Stap 5: SSH-key instellen (geen wachtwoord nodig)

Dit zorgt ervoor dat je vanaf je iPhone kunt verbinden zonder elke keer je wachtwoord in te typen.

### Vanuit je SSH-app op je iPhone

In Blink Shell:

```bash
ssh-copy-id gebruikersnaam@100.x.x.x
```

Of handmatig: kopieer de publieke key van je iPhone en voeg deze toe aan `~/.ssh/authorized_keys` op je MacBook.

---

## Stap 6: Dagelijks gebruik

### Een Claude sessie starten (op je MacBook of via SSH)

```bash
# Nieuwe tmux sessie aanmaken met de naam "claude"
tmux new -s claude

# Claude Code starten
claude
```

### Verbinden vanaf je iPhone

Open je SSH-app en verbind:

```bash
# In Blink Shell
ssh macbook

# Of met IP
ssh gebruikersnaam@100.x.x.x
```

Koppel aan de draaiende sessie:

```bash
tmux attach -t claude
```

Je bent nu exact waar je gebleven was — volledige context, toestemmingen, alles intact.

### Loskoppelen zonder de sessie te stoppen

Druk op:

```
Ctrl+B, daarna D
```

Dit detacht je van tmux. De Claude sessie draait gewoon door op je MacBook.

---

## Stap 7: Claude Code toestemmingen permanent maken

Zo hoef je niet elke sessie opnieuw toestemmingen te geven.

### Per project

Maak in je projectmap een bestand `.claude/settings.json` aan:

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Edit",
      "Write",
      "Bash(git *)",
      "Bash(npm *)"
    ]
  }
}
```

### Globaal

```bash
claude config write allowedTools '["Read", "Edit", "Write", "Bash"]'
```

---

## Stap 8: MacBook wakker houden

Voorkom dat je MacBook in slaap valt terwijl je remote werkt.

### Optie A: caffeinate (tijdelijk)

```bash
# Voorkom slaap zolang het commando draait (8 uur)
caffeinate -dims -t 28800 &
```

### Optie B: Amphetamine (permanent)

Installeer **Amphetamine** uit de Mac App Store. Configureer het om je Mac wakker te houden zolang er een SSH-verbinding actief is.

### Optie C: Systeeminstellingen

1. **Systeeminstellingen → Beeldschermen → Geavanceerd**
2. Zet "Voorkom automatisch in slaapstand bij aangesloten voeding" aan
3. **Systeeminstellingen → Batterij → Opties**
4. Zet "Wek voor netwerktoegang" op **Altijd**

---

## Veelvoorkomende tmux commando's

| Actie | Toetsen |
|---|---|
| Loskoppelen van sessie | `Ctrl+B`, `D` |
| Scrollen | `Ctrl+B`, `[` (pijltjes of swipe om te scrollen, `Q` om te stoppen) |
| Nieuwe sessie aanmaken | `tmux new -s naam` |
| Lijst van sessies | `tmux ls` |
| Aankoppelen aan sessie | `tmux attach -t naam` |
| Sessie beëindigen | `exit` in de sessie |

---

## Troubleshooting

### Kan niet verbinden via SSH

- Controleer of Tailscale aan staat op **beide** apparaten
- Controleer of Externe Login aan staat op je MacBook
- Test met `ping 100.x.x.x` vanuit je SSH-app

### tmux sessie is weg

- `tmux ls` — staat de sessie er nog?
- Als je MacBook herstart is, zijn alle tmux sessies weg. Start opnieuw met `tmux new -s claude`

### Claude reageert traag

- Controleer je internetverbinding op de MacBook
- Tailscale zelf voegt minimale latency toe, het probleem ligt waarschijnlijk bij de internetverbinding

### Scherm ziet er raar uit na reconnect

```bash
# Reset de terminal
tmux attach -t claude
# Druk Ctrl+B, daarna :
# Typ: refresh-client
```
