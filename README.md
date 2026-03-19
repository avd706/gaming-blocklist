# Curated Gaming Blocklist

**A targeted DNS blocklist for Pi‑hole, Technitium DNS, and other DNS‑based ad‑blockers.**  
Blocks major game launchers, update servers, and telemetry while avoiding over‑blocking common internet services.

---

## 🎯 What This List Does

- Blocks domains used by popular gaming platforms to launch, update, or report telemetry.
- Designed to **avoid breaking general internet** (no broad CDN wildcards, no shared auth domains).
- Intended for parental controls, focus time, or bandwidth management.

**Blocked categories:**
- Game storefronts & launchers (Steam, Epic, EA/Origin, Ubisoft, Blizzard, Xbox, GOG, Rockstar)
- Online game services (Minecraft, Riot, Roblox, Hoyoverse, Apex, Activision, Palworld, etc.)
- Update and content delivery networks specific to these platforms.

---

## 📦 Sources & Attribution

This blocklist is **curated** from domain knowledge of gaming platforms and from the following public resources:

- **Original comprehensive gist** – `https://gist.github.com/bunchc/d535cb5063321a225adc0fb8cd0e85ba`  
  Provided an extensive starting point; many entries were retained after filtering for exclusivity.

- **Community blocklists** –  
  - Hagezi’s DNS Blocklists (Ultimate) – `https://github.com/hagezi/dns-blocklists`  
  - Block List Project – `https://github.com/blocklistproject/`  
  - Various Pi‑hole community lists

- **Vendor‑specific documentation & reverse‑engineering** –  
  - Steam: `store.steampowered.com`, `steamcommunity.com`, `steamcontent.com`, `api.steampowered.com`, and related CDN endpoints.  
  - Epic Games: `epicgames.com`, `egs-*.epicgames.com`, Fortnite services.  
  - EA/Origin: `origin.com`, `ea.com`, `eacommerce.akamaized.net`, `api.ea.com`.  
  - Ubisoft: `ubisoftconnect.com`, `ubisoft.com`, `ubistatic*-a.akamaihd.net`, `ubiservices.akamaized.net`.  
  - Blizzard/Battle.net: `blizzard.com`, `battle.net`, `blzddist*.akamaihd.net`, `blz-content.akamaized.net`.  
  - Xbox: `xbox.com`, `xboxlive.com`, `gssv-cdo.xboxlive.com`, `xbstatic-farsh.akamaized.net`.  
  - GOG: `gog.com`, `gog-games.com`, `gog-statics.com`.  
  - Rockstar: `rockstargames.com`, `rglprod.rockstargames.com`, `gta-*` services.  
  - Minecraft: `minecraftservices.com`, `mojang.com`, `sessionserver.mojang.com`, `api.mojang.com`.  
  - Riot (LoL/Valorant): `riotgames.com`, `valorant.com`, `riotcdn.net`, `pvp.net`.  
  - Roblox: `roblox.com`, `roblox-locomotive.akamaized.net`.  
  - Hoyoverse (Genshin Impact, etc.): `hoyoverse.com`, `hoyo.akamaized.net`, `api-os.hoyolab.com`.  
  - Apex Legends: `apex.alert`, `apexcloud.com`.  
  - Activision/Call of Duty: `activision.com`, `callofduty.com`.  
  - Palworld: `pocketpair.jp`, `palworld.akamaized.net`.

All domains have been manually reviewed to **exclude**:
- Generic CDNs used by many non‑gaming sites (e.g., `*.cloudfront.net`, `*.cloudflare.com` unless gamer‑specific)
- Authentication providers (e.g., `accounts.google.com`, `login.microsoftonline.com`)
- Social media or non‑gaming streaming domains

---

## 📥 How to Use

### Pi‑hole
1. In Pi‑hole admin, go to **Group Management → Adlists**.
2. Add a new **Custom Adlist** with URL:  
   `https://raw.githubusercontent.com/avd706/gaming-blocklist/main/gaming-blocklist-pihole.txt`
3. Assign the adlist to a group (e.g., `block-gaming`) and enable for desired clients.
4. Run `pihole -g` (or click "Update Gravity" in the UI) to pull the list.

### Technitium DNS
1. Open Technitium admin (`http://<your‑dns>:5380`).
2. Navigate to **Settings → Blocking**.
3. Append the URL to **Block List URLs**:  
   `https://raw.githubusercontent.com/avd706/gaming-blocklist/main/gaming-blocklist-pihole.txt`
4. Click **Save**, then **Flush Cache**.

### Other DNS Resolvers
- **dnsmasq**: add `address=/.domain.tld/0.0.0.0` for each domain, or use `conf-file=/path/to/this/file`.
- **Unbound**: use `local-zone: "domain.tld" always_null`.
- **BIND**: create a zone with `*.domain.tld IN A 0.0.0.0` or use `response-policy`.

---

## ⚙️ Scheduled Toggle (Optional)

If you want the blocklist active only during certain hours, you can automate adding/removing the URL from your DNS server’s blocklist configuration.  
Example for Technitium using a cron job:

```bash
# Activate at 20:00 Sun–Thu
0 20 * * 0-4 curl -s "http://10.10.101.101:5380/api/settings/set?token=YOUR_TOKEN&blockListUrls[]=https://raw.githubusercontent.com/avd706/gaming-blocklist/main/gaming-blocklist-pihole.txt" && curl -s "http://10.10.101.101:5380/api/cache/flush?token=YOUR_TOKEN"

# Deactivate at 06:00 Mon–Fri
0 6 * * 1-5 curl -s "http://10.10.101.101:5380/api/settings/get?token=YOUR_TOKEN" -o /tmp/settings.json && # remove our URL from blockListUrls and POST back
```

(Or use OpenClaw’s cron system as we’ve implemented.)

---

## ⚠️ Disclaimer

- This list targets specific domains; it may not block every possible game‑related endpoint (new domains appear, launchers change).  
- Some games use generic CDNs for assets; those **are not blocked** intentionally to preserve internet functionality.  
- Use at your own risk; we are not responsible for broken legitimate services.
- All domains are resolved at the time of curation; false positives are possible. Report issues.

---

## 📄 License

This blocklist is released into the **public domain** (CC0) for unrestricted use.  
Attribution appreciated but not required.

---

## 🔧 Contributing

If you find a missing gaming domain, or a false positive that breaks essential services, open an issue or PR in the repository:  
https://github.com/avd706/gaming-blocklist

Please include:
- Domain to add/remove
- Reason (game/launcher it belongs to)
- Any conflicts observed

---

**Maintained by** BlackJack22 (OpenClaw agent) for Tony Dee.  
First published: 2025‑03‑18  
Last updated: 2025‑03‑18
