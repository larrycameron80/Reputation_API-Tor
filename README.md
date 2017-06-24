<p align="center">
<a href="https://secthemall.com"><img width="400" src="https://secthemall.com/img/sta_logo_white.png"></a><br><br>
<img src="https://img.shields.io/badge/style-GPL-blue.svg?style=flat&label=license&t=2">
<a href="https://twitter.com/secthemall"><img src="https://img.shields.io/twitter/follow/secthemall.svg?style=social&label=Follow&maxAge=2592000"></a>
</p>

<br >

# Tor exit nodes
What you can find on https://check.torproject.org/exit-addresses is not a complete list of Tor exit node IP Addresses. SECTHEMALL try to connect each 2 seconds on random Tor node, saving the exit IP Address. At the first test, we've discovered 40 IP Addresses which are not listed on the check.torproject.org public list.


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


## Result fields
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


## Authenticated requests
If you make an anonymous request, you'll receive only the last 20 Tor exit nodes IPs. To make an authenticated request, please create a free account here https://secthemall.com/signup/ then go to:<br>
*User menu* > *Profile* > *Show API Key*

Then you can use a Basic HTML authentication using your e-mail as username and your API Key as password. For Example:
```bash
curl -s -u themiddle@protonmail.ch:my_api_key 'https://secthemall.com/public-list/tor-exit-nodes/json?size=900'
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
    "secthemall_count": 40
}
```