---
 title: include file
 description: include file
 services: frontdoor
 author: duongau
 ms.service: frontdoor
 ms.topic: include
 ms.date: 02/25/2022
 ms.author: duau
 ms.custom: include file
---

| Resource | Limit |
| --- | --- |
| Azure Front Door resources per subscription | 100 |
| Front-end hosts, which includes custom domains per resource | 500 |
| Routing rules per resource | 500 |
| Back-end pools per resource | 50 |
| Back ends per back-end pool | 100 |
| Path patterns to match for a routing rule | 25 |
| URLs in a single cache purge call | 100 |
| Custom web application firewall rules per policy | 100 |
| Web application firewall policy per subscription | 100 |
| Web application firewall match conditions per custom rule | 10 |
| Web application firewall IP address ranges per custom rule | 600 |
| Web application firewall string match values per match condition | 10 |
| Web application firewall string match value length | 256 |
| Web application firewall POST body parameter name length | 256 |
| Web application firewall HTTP header name length | 256 |
| Web application firewall cookie name length | 256 |
| Web application firewall exclusion limit | 100 |
| Web application firewall HTTP request body size inspected | 128 KB |
| Web application firewall custom response body length | 32 KB |

### Azure Front Door Standard/Premium (Preview) Service Limits

*** *Maximum **500** total Standard and Premium profiles per subscription.*

| Resource | Standard SKU Limit | Premium SKU Limit |
| --- | --- | --- |
| Maximum endpoint per profile	| 10 | 25 |
| Maximum custom domain per profile	| 100 | 200 |
| Maximum origin group per profile | 100 | 200 |
| Maximum secrets per profile | 100 | 200 |
| Maximum security policy per profile | 100 | 200 |
| Maximum rule set per profile | 100 | 200 |
| Maximum rules per rule set | 100 | 100 |
| Maximum origin per origin group | 50 | 50 |
| Maximum routes per endpoint | 100 | 200 |
| URLs in a single cache purge call | 100 | 100 |
| Custom web application firewall rules per policy | 100 | 100 |
| Web application firewall match conditions per custom rule | 10 | 10 |
| Web application firewall IP address ranges per custom rule | 600 | 600 |
| Web application firewall string match values per match condition | 10 | 10 |
| Web application firewall string match value length | 256 | 256 |
| Web application firewall POST body parameter name length | 256 | 256 |
| Web application firewall HTTP header name length | 256 | 256 |
| Web application firewall cookie name length | 256 | 256|
| Web application firewall HTTP request body size inspected | 128 KB | 128 KB |
| Web application firewall custom response body length | 32 KB | 32 KB |

#### Timeout values
##### Client to Front Door
* Front Door has an idle TCP connection timeout of 61 seconds.

##### Front Door to application back-end
* If the response is a chunked response, a 200 is returned if or when the first chunk is received.
* After the HTTP request is forwarded to the back end, Front Door waits for 30 seconds for the first packet from the back end. Then it returns a 503 error to the client. This value is configurable via the field sendRecvTimeoutSeconds in the API.
    * If a request is cached and it takes more than 30 seconds for the first packet from Front Door or from the backend, then a 504 error is returned to the client. 
* After the first packet is received from the back end, Front Door waits for 30 seconds in an idle timeout. Then it returns a 503 error to the client. This timeout value is not configurable.
* Front Door to the back-end TCP session timeout is 90 seconds.

#### Upload and download data limit

|  | With chunked transfer encoding (CTE) | Without HTTP chunking |
| ---- | ------- | ------- |
| **Download** | There's no limit on the download size. | There's no limit on the download size. |
| **Upload** |    There's no limit as long as each CTE upload is less than 2 GB. | The size can't be larger than 2 GB. |

#### Other limits
* Maximum URL size - 8,192 bytes - Specifies maximum length of the raw URL (scheme + hostname + port + path + query string of the URL)
* Maximum Query String size - 4,096 bytes - Specifies the maximum length of the query string, in bytes.
* Maximum HTTP response header size from health probe URL - 4,096 bytes - Specified the maximum length of all the response headers of health probes. 
* Maximum rules engine action header value character: 640 characters.
* Maximum rules engine condition header value character: 256 characters.
* Maximum ETag header size: 128 bytes

For more information about limits that apply to Rules Engine configurations, see [Rules Engine terminology](../articles/frontdoor/front-door-rules-engine.md#terminology)
