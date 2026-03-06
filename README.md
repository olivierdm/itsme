# [https://raw.githubusercontent.com/olivierdm/itsme/main/profile.ttl](https://raw.githubusercontent.com/olivierdm/itsme/main/profile.ttl)

### Part 5: Curl Command Output

```bash

HTTP/2 200 
cache-control: max-age=300
content-security-policy: default-src 'none'; style-src 'unsafe-inline'; sandbox
content-type: text/plain; charset=utf-8
etag: "0dbc03c37fa42d42517017b366811f1b9aad9fac6290758ffe2465cfad06b828"
strict-transport-security: max-age=31536000
x-content-type-options: nosniff
x-frame-options: deny
x-xss-protection: 1; mode=block
x-github-request-id: 0C84:319AE3:35C70:3B04A:69A94973
accept-ranges: bytes
date: Thu, 05 Mar 2026 09:14:27 GMT
via: 1.1 varnish
x-served-by: cache-bru1480059-BRU
x-cache: MISS
x-cache-hits: 0
x-timer: S1772702067.415889,VS0,VE171
vary: Authorization,Accept-Encoding
access-control-allow-origin: *
cross-origin-resource-policy: cross-origin
x-fastly-request-id: b823f2daf43467e5ff6ff6983dcd3defcc021346
expires: Thu, 05 Mar 2026 09:19:27 GMT
source-age: 0
content-length: 1547

```

### Comments on Publication Features

* **Content-Type:** The server returns `text/plain; charset=utf-8`. While the official IANA media type for Turtle is `text/turtle`, GitHub Raw defaults to plain text. Most RDF parsers handle this correctly as long as the syntax is valid.
* **etag:** contains a hash and is used to see if header content has changed. If it hasn't, the cached data can be used.
* **CORS (Cross-Origin Resource Sharing):** The header `access-control-allow-origin: *` is present. This is a critical feature that allows web-based applications (like `rdf-play` or other SPARQL clients) to fetch my RDF data directly from the browser without security blocks.
* **Caching:** The `cache-control: max-age=300` and `expires` headers indicate a 5-minute cache window. This balances server performance with the need for data freshness when I push updates to my profile.
* **Compression:** Although not explicitly shown as `gzip` in this specific header dump, the `vary: Accept-Encoding` header indicates the server is prepared to serve compressed versions of the file to save bandwidth.
