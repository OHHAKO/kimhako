---
layout: post
title: "[mongoDB] mongodb atlas 사용기"
excerpt: "angular에 사용할 데이터베이스를 mongodb altas에서 생성하고 호출하는 방법을 익히도록 하자."
categories: [MongoDB]
comments: true
---

## Mongodb Atlas 자주 나오는 용어 정리

- cluster: whitelist 목록에 있는 ip주소만 클러스터에 연결할 수 있다. 즉 클러스터에 접근을 허용한 ip주소 목록이다.
- whitelist: 연결 ip주소 목록. 클러스터에 연결해 데이터에 접근할 수 있는 ip 목록이다. (CIDR주소에 대한 API 접근 제한)
- compass: GUI로 mongodb를 사용할 수 있는 도구(설치 프로그램).
- shell: 데이터베이스 동작과 DB 데이터 서치를 위한 명령도구
- user: 아틀라스에 호스트된 database에 접근하는 유저. 배포 기능에 접근하려면 mongodb user를 생성해야 한다.
- CIDR : 도메인 간의 라우팅에 사용되는 인터넷 주소
- VPC : Virtual Private Cloud

### 주의

- 프로젝트 whitelist/api whitelist 는 별개이다.

## Atlas 수행 순서

<img src='{{ "/img/mongodb_cluster.JPG" | relative_url }}' alt="mongodb-cluster" width="700px" height="400px">

- Atlas 계정 생성 후 로그인
- 클러스터 생성. free 버전/GCP/싱가폴 리전 선택
- 데이터베이스 user 생성
  - 왼쪽 메뉴 Database Access에서 DB user 생성
- Whitelist ip address 등록
  - 왼쪽 메뉴 Network Access 에서 Whitelist에 개인 네트워크 ip 등록
- Database Sample Data 생성
  - Cluster에서 `Collections` 버튼 선택
  - Database, collection(table), document(record) 생성
- 클러스터와 애플리케이션 연결
  - Cluster에서 `Connect` 버튼 선택
  - 드라이버는 Node.js 선택. 즉 Node.js 서버 파일을 작성해야 한다.

* Cluster에서 확인해볼 것
  - Region은 내 데이터를 분산시켜서 저장하고 있는 mongodb 서버 지역을 말한다.
  - Operation은 시간 단위로 저장소의 변화를 나타낸다. db, collection, document를 생성하고 수정한 시간이 실시간으로 그래프에 반영된다.

## Node.js 서버 파일 작성

```javascript
const MongoClient = require("mongodb").MongoClient;
const uri =
  "mongodb+srv://admin:<password>@cluster0-d2pqt.gcp.mongodb.net/test?retryWrites=true&w=majority";
const client = new MongoClient(uri, { useNewUrlParser: true });
client.connect((err) => {
  const collection = client.db("test").collection("devices");
  // perform actions on the collection object
  client.close();
});
```

---

참고하면 좋은 사이트

- [https://www.a-mean-blog.com/ko/blog/Node-JS-API/\_/REST%EC%99%80-RESTful-API](https://www.a-mean-blog.com/ko/blog/Node-JS-API/_/REST%EC%99%80-RESTful-API)
- [https://www.a-mean-blog.com/ko/blog/Node-JS-API/\_/%EA%B8%B0%EB%B3%B8-REST-API-%EB%A7%8C%EB%93%A4%EA%B8%B0](https://www.a-mean-blog.com/ko/blog/Node-JS-API/_/%EA%B8%B0%EB%B3%B8-REST-API-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- [https://angular.io/tutorial](https://angular.io/tutorial)
