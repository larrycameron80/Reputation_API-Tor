<p align="center">
<a href="https://secthemall.com"><img width="400" src="https://secthemall.com/img/sta_logo_white.png"></a><br><br>
<img src="https://img.shields.io/badge/style-GPL-blue.svg?style=flat&label=license&t=2">
<a href="https://twitter.com/secthemall"><img src="https://img.shields.io/twitter/follow/secthemall.svg?style=social&label=Follow&maxAge=2592000"></a>
</p>

<br >

# Tor exit nodes
What you can find on https://check.torproject.org/exit-addresses is not a complete list of Tor exit node IP Addresses. SECTHEMALL try to connect each 2 seconds on random Tor node, saving the exit IP Address. At the first test, we've discovered 35 IP Addresses which are not listed on the check.torproject.org public list.

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
    "waring": "To get more then 20 results per query, please create a free account.",
    "total_count": 970,
    "result_count": 1,
    "secthemall_count": 35
}
```
