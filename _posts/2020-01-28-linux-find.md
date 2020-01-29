---
title:  "Linux 파일 및 내용 찾기"
excerpt: "Linux 파일 및 내용 찾기"
categories:
  - linux
tags:
  - linux
last_modified_at: 2020-01-29T21:03:00-09:00
---

### 현재 위치에서 파일 찾기
```bash
find -name 'myfile.txt'
```

```bash
find -name '*.txt'
```

### 특정 위치에서 파일 찾기
```bash
find /path/to/find -name '*.txt'
```

### ls 형식으로 파일 찾기
```bash
find /path/to/find -name '*.txt' -ls
```

### 특정 위치에서 디렉토리 찾기
```bash
find /path/to/find -name 'mydir' -type d
```

### 파일에서 내용 찾기
```bash
grep 'word' filename
```

### 특정 위치 하위 경로내 모든 파일에서 내용 찾기
```bash
grep -r 'word' /path/to/find
```
### 라인 표시
```bash
grep -r -n 'word' /path/to/find
```
