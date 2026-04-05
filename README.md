Custom GeoIP Blacklist for Xray
Lean GeoIP and Geosite datasets are now available for download — stripped of all unnecessary content, offering a minimal, purpose-built alternative for users who value precision
This repository provides a specialized custom_geoip.dat file designed to enhance server security and prevent automated scanning. It focuses on blocking malicious traffic, specifically targeting IP addresses associated with active attacks and probes.
Purpose
The primary goal of this project is to provide a reliable blacklist for Xray-core users. By routing traffic from these IPs to a "block" outbound, you can effectively shield your server from known scanners and malicious actors.

Content
File: custom_geoip.dat
Category: firehol (Aggregated list of attacking IPs)
Update Frequency: [Weekly]
Usage in Xray

To use this blacklist, download the .dat file to your Xray assets directory (usually where the xray binary or geoip.dat is located) and add the following rule to your routing configuration:

###  JSON

```json
{
  "rules": [
    {
      "type": "field",
      "outboundTag": "block",
      "source": [
        "ext:custom_geoip.dat:firehol"
      ]
    },
    {
      "type": "field",
      "outboundTag": "block",
      "ip": [
        "ext:custom_geoip.dat:firehol"
      ]
    }
  ]
}
```
The `source` rule blocks incoming traffic originating from the specified IP addresses.
The `ip` rule blocks outgoing traffic destined for the specified IP addresses.

When used together, these rules ensure bidirectional blocking for all IP addresses contained in the firehol list.


Direct Download
You can use the following link to automate updates or manual downloads:
https://raw.githubusercontent.com/d010b/custom_geoip/main/custom_geoip.dat


Using full files, which often weigh tens of megabytes, overloads the RAM at startup. The geoip.dat and geosite.dat files, free of unnecessary content…

geosite:

url: 
	https://raw.githubusercontent.com/d010b/custom_geoip/main/filtered/geosite.dat
```json
  keep_categories:
    - "category-ru"
    - "tiktok"
    - "category-media-ru"
    - "vk"
    - "meta"
    - "category-game-platforms-download"
    - "yandex"
    - "zoom"
    - "win-spy"
    - "category-ai-chat-!cn"
    - "tld-ru"
    - "youtube"
    - "ru-available-only-inside"
    - "steam"
    - "whatsapp"
    - "category-ads-all"
    - "private"
    - "roblox"
    - "ru-blocked-all"
    - "wildberries"
    - "category-ads"
    - "category-porn"
    - "category-games"
    - "ozon"
    - "win-extra"
    - "antifilter-download-community"
    - "avito"
    - "category-retail-ru"
    - "microsoft"
    - "category-speedtest"
    - "category-bank-ru"
    - "category-anticensorship"
    - "noip"
    - "pinterest"
    - "amazon"
    - "mailru-group"
    - "category-entertainment-ru"
    - "cloudflare"
    - "hdrezka"
    - "google"
    - "ru-blocked"
    - "telegram"
    - "instagram"
    - "category-travel-ru"
    - "category-ecommerce-ru"
```
  geoip:

  url: 
  	https://raw.githubusercontent.com/d010b/custom_geoip/main/filtered/geoip.dat
  ```json
  keep_categories:
    - "cloudflare"
    - "cloudfront"
    - "firehol"
    - "facebook"
    - "google"
    - "netflix"
    - "private"
    - "re-filter"
    - "ru"
    - "ru-blocked"
    - "ru-blocked-community"
    - "telegram"
    - "twitter"
    - "yandex"
    - "tor"
    - "whitelist"
    - "max"
    - "avito"
    - "wb"
    - "ozon"
    - "sber"
 ```
  ```json
{
  "routing": {
"domainStrategy": "IPOnDemand",
    "rules": [
      {
        // RULE 1: Block "bad" IPs (FireHOL) for !!!!Server!!!!
        // If the incoming IP falls into the firehol category,
        // the connection is immediately blocked (blackhole).
        "type": "field",
        "outboundTag": "block",
        "source":  [
        "geoip:firehol"
      ]
      },
      {
        // RULE 2: Whitelist direct for client or BLOCK for server
        // If the destination IP falls into the whitelist category,
        // traffic is sent directly, bypassing the proxy.
        // This is useful to avoid proxying traffic to the Russian whitelist.
        "type": "field",
        "ip": ["geoip:whitelist"],
        "outboundTag": "direct"
      },
      {
        "type": "field",
        "ip": ["geoip:max"],
        "outboundTag": "direct"
      }
      // ... other rules
    ],
    
    "outbounds": [
      // Nothing will work without these lines
      {
        "tag": "direct",
        "protocol": "freedom"
      },
      {
        "tag": "block",
        "protocol": "blackhole"
      }
    ]
  }
}
  ```
