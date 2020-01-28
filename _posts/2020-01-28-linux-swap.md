---
title:  "Swap Memory 설정"
excerpt: "Swap Memory 설정"
categories:
  - linux
tags:
  - linux
last_modified_at: 2020-01-28T21:03:00-09:00
---

### Swap Memory 확인

sudo swapon --show

free -h

total        used        free      shared  buff/cache   available
Mem:           3.6G        1.7G        789M         26M        1.2G        1.8G
Swap:            0B          0B          0B

### Swap File 생성

sudo fallocate -l 1G /path/to/create/file

#### 생성된 Swap File 확인

ls -lh /path/to/create/file

### Swap File 활성화

#### Swap File 권한 부여

sudo chmod 600 /path/to/create/file

#### Swap File Marking

sudo mkswap /path/to/create/file

#### Enable

sudo swapon /path/to/create/file

#### Check

sudo swapon --show
free -h

### Making the Swap File Permanent

echo '/path/to/create/file none swap sw 0 0' | sudo tee -a /etc/fstab
