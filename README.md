Custom GeoIP Blacklist for Xray
This repository provides a specialized custom_geoip.dat file designed to enhance server security and prevent automated scanning. It focuses on blocking malicious traffic, specifically targeting IP addresses associated with active attacks and probes.
Purpose
The primary goal of this project is to provide a reliable blacklist for Xray-core users. By routing traffic from these IPs to a "block" outbound, you can effectively shield your server from known scanners and malicious actors.

Content
File: custom_geoip.dat
Category: firehol (Aggregated list of attacking IPs)
Update Frequency: [Weekly]
Usage in Xray

To use this blacklist, download the .dat file to your Xray assets directory (usually where the xray binary or geoip.dat is located) and add the following rule to your routing configuration:


{
  "routing": {
    "domainStrategy": "IPOnDemand",
    "rules": [
      {
        "type": "field",
        "outboundTag": "block",
        "ip": [
          "ext:custom_geoip.dat:firehol"
        ]
      }
    ]
  }
}


Direct Download
You can use the following link to automate updates or manual downloads:
https://raw.githubusercontent.com/d010b/custom_geoip/main/custom_geoip.dat
     
