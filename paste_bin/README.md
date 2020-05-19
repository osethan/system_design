# Paste Bin

## (1) Problem
Design a Pastebin service

## (2) Requirements
- Functional Requirements
  - Give text, get link to text paste
  - Give optional expire time, title, folder for paste, paste link hash
  - Allow users for private pastes
  - Give link, get text paste
- Non-Functional Requirements
  - Service should be available and efficient
  - Links shouldn't be guessable
- Extended Requirements
  - Analytics of paste creation and sharing

## (3) Estimates
What's the traffic needs? Assume 15M new pastes per month and 100:1 read:write ratio. What's the QPS?

(15M write/month) x (month/30 day) x (day/24 hour) x (hour/3600 sec) = 6 write/sec

(6 write/sec) x (100 read/write) = 600 read/sec

What's the storage needs? Assume a paste has at most 512KB and pastes are saved for at most 5 years.

(512 KB/write) x (15M write/month) x (12 month/year) x (5 year) = 460 TB

What's the bandwidth need?

(512 KB/write) x (6 write/sec) = 3MBps

(512 KB/read) x (600 read/sec) = 300MBps

A cache could be used for hot pastes. Assume an 80/20 rule for hot pasts. What's the memory needs?

(512 KB/read) x (600 read/sec) x (3600 sec/hour) x (24 hour/day) x 0.2 = 5.3 GB/day

|||
|---|---|
| read bandwidth | 300 MBps |
| write bandwidth | 3 MBps |
| storage | 460 TB |
| memory | 5.3 GB |
| no. pastes | 900 M |

## (4) System APIs
- createPaste(apiKey, text, title="", folder="", expireAt="", username="")
  - apiKey: Backend service authorization
  - text: Paste text
  - folder: Paste folder
  - expireAt: Expire time
  - username: Paste creator
  - -> paste link on success, null on failure
- readPaste(apiKey, pasteKey)
  - apiKey: Backend service authorization
  - pasteKey: Object storage key for paste text object
  - -> paste text on succes, null on failure
- deletePaste(apiKey, pasteKey)
  - apiKey: Backend service authorization
  - pasteKey: Object storage key for paste text object
  - -> true on success, false on failure

## (5) Database Design

| Paste |
|---|
| pk: hash: varchar(16) |
| text: varchar(512) |
| title: varchar(32) |
| folder: varchar(32) |
| expireAt: datetime |
| userID: int |

| User |
|---|
| pk: userID: int |
| username: varchar(32) |
| email: varchar(32) |

A Paste's text can be stored in object storage which let's the database scale better. The database is expected to grow which suggests using a NoSQL database.
