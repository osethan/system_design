# Instagram

## (1) Problem
Design Instagram

## (2) Requirements
- Functional Requirements
  - User can post photos
  - User can see other users' photos
  - User can like other users' photos
  - User can message other users
- Non-Functional Requirements
  - Service should be reliable to handle peak hours, efficient for user experience

## (3) Estimates
What's the traffic? Assume 500M posts each day, 30 likes each post, 50:1 read:write ratio, and 1B messages each day. What's the QPS?

(500M post/day) x (day/24 hour) x (hour/3.6K sec) = 6K post/sec

(6K post/sec) x (30 like/post) = 180K like/sec

(1B msg/day) x (day/24 hour) x (hour/3.6K sec) = 12K msg/sec

Assume 1.2 MB/post, 1 byte/like, 1 KB/msg. What's the bandwidth?

(6K post/sec) x (1.2 MB/post) = 7.2 GB/s

(180K like/sec) x (1 byte/like) = 180 KB/s

(12K msg/sec) x (1 KB/msg) = 12 MB/s

(6K write (post)/sec) x (50 read/write) x (1.2 MB/read) = 360 GB/s

What's the storage? Assume photos are kept forever.

(500M post/day) x (365 day/year) x (1.2 MB/post) = 219 TB/year

(1B msg/day) x (365 day/year) x (1 KB/msg) = 365 TB/year

(365 (msg) + 219 (post)) TB/year = 584 TB/year

A cache can be used for popular users who have their photos seen by many. Assume an 80/20 rule. What's the memory?

(0.2) x (219 TB/year) x (year/365 day) = 120 GB/day

|||
|---|---|
| read bandwidth | 7.2 GB/s |
| write bandwidth | 360 GB/s |
| storage | 584 TB/year |
| memory | 120 GB |

## (4) API Design
- createPost(apiKey, photo, username)
  - apiKey: Backend service authorization
  - photo: Post photo
  - Username: User posting photo
- readPost(apiKey, photoKey)
  - ""
  - photoKey: Object storage photo id
- deletePost(apiKey, photoKey)
  - ""
  - ""
- sendMsg(apiKey, sender, receiver, text)
  - ""
  - sender: User to send msg
  - receiver: User to receive msg
  - text: Msg text
- readMsgs(apiKey, reader, sender)
  - ""
  - reader: User to read msgs
  - sender: User with traded msgs
