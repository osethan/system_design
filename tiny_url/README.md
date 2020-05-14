# Tiny URL

## (1) Problem
Design a URL short link service

## (2) Requirements
* Functional Requirements
  * Give long link get short link
  * Choose alias for long link
  * Give short link get long link
  * Delete short link
* Non-Functional Requirements
  * Service be available and efficient
  * Short links shouldn't be guessable
* Extended Requirements
  * Analytics of redirects

## (3) Estimates
What are the **traffic** loads? Assume ervice will be read heavy with 100:1 read:write ratio. Assume 500M writes per month.

(500M writes/month) x (100 read/write) = 50B read/month

What's the QPS? (Queries Per Second)

(500M writes/month) x (month/30 days) x (day/24 hour) x (hour/3600 sec) = 200 write/second

(200 write/sec) x (100 read/write) = 20K read/sec

What are the **storage** needs? Assume long links are 500 bytes. Assume long links are stored for 5 years.

(500M write/month) x (12 month/year) x (5 year) x (500 byte/write) = 15 TB

A cache can be used for hot links. Assume 80/20 rule for hot links. What **memory** is needed?

(500 byte/read) x (20K read/sec) x (3600 sec/hour) x (24 hour/day) = 8.7 GB

|||
|---|---|
| read traffic | 200 read/sec |
| write traffic | 20K write/sec |
| storage | 15 TB|
| memory | 8.7 GB |

## System APIs

* createShortLink(apiKey, longLink, alias="", expireAt="", username="")
  * apiKey: Other services may need to interact with data
  * longLink: Link to convert
  * alias: Optional alias for long link
  * expireAt: Optional expire date
  * username: Optional username
  * -> return: shortLink
* readShortLink(apiKey, shortLink)
  * -> return: longLink
* deleteShortLink(apiKey, shortLink)
  * -> return: null

## Database Design
