<p align="center">
<a href="https://secthemall.com"><img width="400" src="https://secthemall.com/img/sta_logo_white.png"></a><br><br>
<img src="https://img.shields.io/badge/style-GPL-blue.svg?style=flat&label=license&t=2">
<a href="https://twitter.com/secthemall"><img src="https://img.shields.io/twitter/follow/secthemall.svg?style=social&label=Follow&maxAge=2592000"></a>
</p>

<br >

# Tor exit nodes
What you can find on https://check.torproject.org/exit-addresses is not a complete list of Tor exit node IP Addresses. SECTHEMALL is able to collect more IPs, trying to connect each 2 seconds through a random Tor node, saving the exit IP Address. During the first test, we've collected 70 IP Addresses which are not listed on the check.torproject.org public list.

Take a look by clicking here: https://secthemall.com/public-list/tor-exit-nodes/json?size=20

Real time stats: https://secthemall.com/reputation-api/tor

<br>

# Table of contents
- [Usage](#usage)
- [Results](#results)
- [Authenticated requests](#authenticated-requests)
- [Lastid](#lastid)
- [Block Tor with ModSecurity](#block-tor-with-modsecurity)
- [Import to Elasticsearch](#import-to-elasticsearch)
- [Contributions](#contributions)

<br>

# Usage
Query by anonymous users are limited to 20 results:
```bash
$ curl -s 'https://secthemall.com/public-list/tor-exit-nodes/json?size=1'
{
    "results": [
        {
            "ip": "178.20.55.18",
            "ptr": "marcuse-2.nos-oignons.net.",
            "expire": 1498914903,
            "geo": {
                "isocode": "FR",
                "countryname": "France",
                "subdivision": "Paris",
                "city": "Paris",
                "lat": 48.8628,
                "lng": 2.3292000000000002
            },
            "urlscan": {
                "server": "Apache",
                "domain": "178.20.55.18",
                "ip": "178.20.55.18",
                "asnname": "LIAZO, FR",
                "asn": "AS50618",
                "url": "http:\/\/178.20.55.18\/",
                "ptr": "marcuse-2.nos-oignons.net"
            },
            "source": "https:\/\/check.torproject.org\/exit-addresses",
            "created": 1498310103,
            "geopoint": "48.8628,2.3292"
        }
    ],
    "lastid": "AVzV_UzSoy8zi76Jo4lT9b727444706ff9e40f59adbf962d3e0c",
    "warning": "To get more then 20 results per query, please create a free account here: https:\/\/secthemall.com\/signup\/",
    "total_count": 1075,
    "result_count": 1,
    "secthemall_count": 71
}
```

<br>

## Results
<table>
<tbody>
    <tr><td><b>ip</b></td> <td>Tor exit node IP address</td></tr>
    <tr><td><b>ptr</b></td> <td>Reverse lookup</td></tr>
    <tr><td><b>expire</b></td> <td>The unix epoch when secthemall will remove this IP from database</td></tr>
    <tr><td><b>geo</b></td> <td>Information about the geographic location of the Tor exit node</td></tr>
    <tr><td><b>urlscan</b></td> <td>Information about a webserver listening on the Tor exit node</td></tr>
    <tr><td><b>source</b></td> <td>Who discovered this exit node</td></tr>
    <tr><td><b>created</b></td> <td>Unix epoch of when was discovered this exit node</td></tr>
    <tr><td><b>geopoint</b></td> <td>Latitude and longitude</td></tr>
    <tr><td><b>lasid</b></td> <td>Databse ID token</td></tr>
    <tr><td><b>total_count</b></td> <td>How many Tor exit node IPs there are in database</td></tr>
    <tr><td><b>result_count</b></td> <td>How many IPs you ask</td></tr>
    <tr><td><b>secthemall_count</b></td> <td>How many IPs SECTHEMALL found that are not present in the Tor website list</td></tr>
</tbody>
</table>

by replacing the last part of the URL (`https://secthemall.com/public-list/tor-exit-nodes/`**json**`?size=20`), you can get different format results:
<table>
<tr><td><b>json</b></td><td>returns results as JSON</td></tr>
<tr><td><b>iplist</b></td><td>returns just a list of IPs</td></tr>
<tr><td><b>elasticsearch</b></td><td>returns all results as an elasticsearch bulk API syntax</td></tr>
</table>

<br>

## Authenticated requests
If you send an anonymous request, you'll receive only the last 20 Tor exit nodes IPs. In order to send an authenticated request, please create a free account here https://secthemall.com/signup/ then go to:<br>
*User menu* > *Profile* > *Show API Key*

Then you can use a Basic HTML authentication using your e-mail as username and your API Key as password. For Example:
```bash
curl -s -u themiddle@secthemall.com:my_api_key 'https://secthemall.com/public-list/tor-exit-nodes/json?size=900'
{
    "results": [
        {
            "ip": "196.54.55.37",
            "ptr": "ip-37-55-54-196.fr.amsterdamresidential.com.",
            "expire": 1498929779,
            "geo": {
                "isocode": "FR",
                "countryname": "France",
                "subdivision": "Paris",
                "city": "Paris",
                "lat": 48.8628,
                "lng": 2.3292000000000002
            },
            "source": "https:\/\/secthemall.com",
            "created": 1498324979,
            "geopoint": "48.8628,2.3292"
        },
        {
            ... 900 results ...
        },
    "lastid": "AVzbIM7XGZyk6e6q0DwV07959b483e5fbc9d1d1e71d3f2188945",
    "user_auth": true,
    "total_count": 975,
    "result_count": 9,
    "secthemall_count": 67
}
```

<br>

## Lastid
The `lastid` field could be used in order to know when the database has changed. For example you can save the `lastid` in a file and check if it's equal then the actual database id before download the whole list of exit nodes. For example:
```php
<?php

    $l = json_decode( file_get_contents('https://secthemall.com/public-list/tor-exit-nodes/json/?lastid=true'), true );

    $lastidfile = __DIR__.'/lastid';

    if(file_exists($lastidfile) && file_get_contents($lastidfile) == $l['lastid']) {
        echo "Tor exit nodes list not changed.\n";
    } else {
        echo "Tor exit nodes list need a sync...\n";
        exec('curl -s -u themiddle@secthemall.com:my_apy_key https://secthemall.com/public-list/tor-exit-nodes/iplist/?size=10000 > /opt/tor-exit-nodes/listip.txt');
        file_put_contents($lastidfile, $l['lastid']);
    }
    
```

<br>

# Block Tor with ModSecurity
You can use this free service to block all traffic from Tor to your website using ModSecurity. For example, you can configure a SecRule like this:
```bash
SecRule REMOTE_ADDR "@ipMatchFromFile /opt/tor-exit-nodes/listip.txt" "id:6000,\
    phase:request,log,\
    msg:'Tor exit node',\
    tag:'bad-reputation/Tor',\
    severity:'CRITICAL',\
    maturity:'9',\
    accuracy:'9',\
    rev:'1',\
    ver:'SECTHEMALL_1.0',\
    capture,\
    drop"
```
then download the whole list (authentication needed for `size=5000`):
```bash
$ mkdir /opt/tor-exit-nodes
$ cd /opt/tor-exit-nodes
$ curl -s -u themiddle@secthemall.com:my_apy_key \
    https://secthemall.com/public-list/tor-exit-nodes/iplist/?size=5000 > /opt/tor-exit-nodes/listip.txt

$ # reload your http server, example:
$ /etc/init.d/nginx reload
```

<br>

# Import to Elasticsearch
First download the whole database using type `elasticsearch` instead of `json`, and save the output to a file:
```bash
curl -s -u themiddle@secthemall.com:your_api_key \
     'https://secthemall.com/public-list/tor-exit-nodes/elasticsearch?size=5000' > \
     torexitnodes.json
```

Then import `torexitnodes.json` to your elasticsearch using bulk action:

```bash
curl -s -H "Content-Type: application/x-ndjson" \
     -XPOST http://localhost:9200/_bulk \
     --data-binary "@torexitnodes.json"
```

Now you can find all data inside the index `secthemall_reputation_tor`:
```bash
curl -s 'http://localhost:9200/secthemall_reputation_tor/_search?pretty'
```
the result will be something like this:
```json
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 2,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "secthemall_reputation_tor",
        "_type" : "reputation",
        "_id" : "c0c5493148d31f3f0d2e9c26a8b0e9c1",
        "_score" : 1.0,
        "_source" : {
          "ip" : "158.255.211.9",
          "ptr" : "9.211.255.158.in-addr.arpa.",
          "expire" : 1499153031,
          "geo" : {
            "isocode" : "AT",
            "countryname" : "Austria",
            "subdivision" : null,
            "city" : null,
            "lat" : 48.2,
            "lng" : 16.3667
          },
          "source" : "https://check.torproject.org/exit-addresses",
          "created" : 1498548231,
          "geopoint" : "48.2,16.3667"
        }
      },
      {
        "_index" : "secthemall_reputation_tor",
        "_type" : "reputation",
        "_id" : "dcdc1730eae01b2d655ab413a7c1bdc3",
        "_score" : 1.0,
        "_source" : {
          "ip" : "77.250.227.12",
          "ptr" : "dhcp-077-250-227-012.chello.nl.",
          "expire" : 1499153031,
          "geo" : {
            "isocode" : "NL",
            "countryname" : "Netherlands",
            "subdivision" : "South Holland",
            "city" : "Rotterdam",
            "lat" : 51.895,
            "lng" : 4.5111
          },
          "source" : "https://check.torproject.org/exit-addresses",
          "created" : 1498548231,
          "geopoint" : "51.895,4.5111"
        }
      }
    ]
  }
}
```

View it on Kibana
![kibana](http://i.imgur.com/hZG5frw.png)


# Contributions
All your success cases are important for us! Please, share with us how you use this service. If you want to publish your own integration script, please make a pull request. For more information, please don't hesitate to write us at support@secthemall.com.
