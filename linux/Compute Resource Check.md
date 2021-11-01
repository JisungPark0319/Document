# Compute Resource Check

## CPU

### CPU 코어 전체 개수

```bash
grep -c processor /proc/cpuinfo
ll -d /sys/devices/system/cpu/cpu? | wc -l
```

### CPU 수

```bash
grep ^processor /proc/cpuinfo | wc -l
```

### CPU당 물리 코어 수

```bash
grep 'cpu cores' /proc/cpuinfo | tail -1
```

## Memory

### Memory 크기 확인

```bash
free -h
```

### 슬롯에 장착된 Memory 확인

```bash
dmidecode -t 17 | egrep 'Memory|Size
```

## Disk

### 파티션 및 용량 확인

```bash
df -h
```

### 연결된 디스크 리스트 정보 확인

```bash
fdisk -l

lsblk
```