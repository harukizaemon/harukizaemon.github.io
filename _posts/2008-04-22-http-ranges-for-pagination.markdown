---
layout: post
title: "HTTP Ranges for Pagination?"
alias: /2008/04/http-ranges-for-pagination.html
categories:
---
Would it be a gross perversion to use HTTP ranges for pagination?:

Client asks the server what range types it accepts for people:

```
HEAD /people HTTP/1.1
```

Server responds:

```
Status: 200Accept-Ranges: pages; records
```

Client requests the first page of people:

```
GET /people HTTP/1.1Range: pages=1-1
```

Server Responds:

```
Status: 206Content-Range: pages 1-1/13
```
