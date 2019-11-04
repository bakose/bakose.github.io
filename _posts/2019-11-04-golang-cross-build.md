---
title:  "Golang Cross Compile"
excerpt: "Golang Cross Compile"

categories:
  - golang
tags:
  - golang
last_modified_at: 2019-11-04T22:03:00-09:00
---

## Golang Cross Build

```bash
GOOS=linux GOARCH=amd64 | go build test.go
```


| GOOS | GOARCH |
|:----|:----|
| darwin | 386 |
| darwin | amd64 |
| darwin | arm |
| darwin | arm64 | 
| dragonfly | amd64 |
| freebsd | 386 |
| freebsd | amd64 |
| freebsd | arm |
| linux | 386 |
| linux | amd64 |
| linux | arm |
| linux | arm64 |
| linux | ppc64 |
| linux | ppc64le |
| linux | mips |
| linux | mipsle |
| linux | mips64 |
| linux | mips64le |
| linux | s390x |
| netbsd | 386 |
| netbsd | amd64 |
| netbsd | arm |
| openbsd | 386 |
| openbsd | amd64 |
| openbsd | arm |
| plan9 | 386 |
| plan9 | amd64 |
| plan9 | arm |
| solaris | amd64 |
| windows | 386 |
| windows | amd64 |