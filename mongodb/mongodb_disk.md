# MongoDB Disk

- MongoDB에서 데이터를 삭제하더라도 실젱 사용 가능한 용량은 그대로 입니다.
- MongoDB는 디스크를 미리 할당해서 사용하는 방식인 스토리지 엔진 방식을 사용하기 때문입니다.
- 스토리지 엔진은 디스크에서 데이터를 읽어오고 저장하는 역할을 최적으로 수행 될 수 있도록 지원하는 역할로서 데이터베이스의 일부로 메모리와 디스크 모두에서 데이터가 저장되는 방식을 사용하는데 이는 더 나은 성능을 제공하고 다른 스토리지 엔진은 쓰기 작업에 대한 높은 처리량을 지원할 수 있기 때문입니다.

### 물지적인 DISK의 가용공간을 확보하는 방법

1. db.repairDatabase()
   - 실제 물리적인 디스크 가용공간이 확보되며, DB 손상 상태를 복구할 수 있습니다.
   - 현재 사용중인 디스크 공간의 두 배의 가용공간이 필요합니다.
   - 실행 시간이 길며 lock이 걸립니다.

```bash
db.repairDatabase()

# 내부 process
# 1. mongoexport -> tmp create
# 2. mongoimport -> temp
# 3. 기존 disk 삭제 -> temp를 disk로 이전
```

2. compact ()
   - collection 대상으로 실행하기 때문에 repairDatabase보다는 훨씬 적은 디스크 용량을 사용합니다.
   - 실행 후 실제 디스크 용량은 늘지 않고, 이미 할당된 공간의 파현화를 정리하여 사용 가능하게 만들어줍니다.

```bash
# 전체컬렉션 수행 시
db.getCollectionNames().forEach(function (collectionName) { 
  print('Compacting: ' + collectionName);
  db.runCommand({
    compact: collectionName
  });
});

# 대용량 또는 단일 컬렉션 수행
db.runCommand({ compact: "collection name" })
```



### DB Backup/Restroe

##### Backup

```bash
mongodump --host 127.0.0.1 --port 27017 --out $(echo /mongod_data_210713/`date +%Y%m%d%H%M%S`_mongo_dump)
```

##### Restore

```bash
mongorestore 00000000_dump/
```

