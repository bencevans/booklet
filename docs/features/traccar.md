## Traccar

[Traccar](https://www.traccar.org) is a free and open source GPS tracking system for which there exists an `OwnTracks` plugin which, by default, runs on TCP port 5144. You configure our apps [in HTTP mode](../tech/http.md) to your Traccar server on that port, using a URL such as

```
http://traccar.example.net:5144
```

In Traccar proper, you configure your tracking deviceId as `tid` respectively in post-June 2017 versions of our apps, as your configured `topic`:

![Traccar device configuration](images/traccar-device.jpg)

HTTP POST payloads shall contain at least the elements `lat`, `lon`, `_type:location`, `tst`, and either or both of `tid` and `topic`. If `topic` is given in the payload, that will be used as Traccar's _uniqueId_ (in which case `tid` will be added to attributes), else `tid`.

```json
{"lon":2.29513,"lat":48.85833,"_type":"location","tst":1497476456, "tid":"JJ"}
```

```json
{"lon":2.29513,"lat":48.85833,"_type":"location","topic":"owntracks/jane/phone", "tid": "JJ"}
```

The following JSON elements, if they're contained in the HTTP payload, will be added to Traccar's position attributes: `vel`, `alt`, `cog`, `acc`, `t`, `batt`, so with an HTTP payload that an OwnTracks app produces like

```json
{
  "cog": 271,
  "batt": 41,
  "lon": 2.29513,
  "acc": 5,
  "vel": 61,
  "vac": 21,
  "lat": 48.85833,
  "t": "u",
  "tst": 1497508651,
  "alt": 167,
  "_type": "location",
  "topic": "owntracks/jane/iphone",
  "p": 71,
  "tid": "JJ"
}
```

a query on the Traccar API could produce something like this:

```json
[
  {
    "id": 475,
    "attributes": {
      "t": "u",
      "battery": 41,
      "tid": "JJ",
      "ip": "127.0.0.1",
      "distance": 0,
      "totalDistance": 0
    },
    "deviceId": 4,
    "type": null,
    "protocol": "owntracks",
    "serverTime": "2017-06-15T06:37:32.000+0000",
    "deviceTime": "2017-06-15T06:37:31.000+0000",
    "fixTime": "2017-06-15T06:37:31.000+0000",
    "outdated": false,
    "valid": true,
    "latitude": 48.85833,
    "longitude": 2.29513,
    "altitude": 167,
    "speed": 1.18575,
    "course": 271,
    "address": "9 Avenue Anatole France, Paris, Île-de-France, FR",
    "accuracy": 5,
    "network": null
  }
]
```

#### Notes

* Neither encryption nor friends are supported.
* `topic` is populated by iOS, but not by Android
