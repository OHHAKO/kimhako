---
layout: post
title: "[데이터베이스] Rest Server로 Mongodb 연동하기"
excerpt: "Atlas Mongod를 이용하기 위한 Rest API를 작성해보자"
categories: [database]
modified: 2019-03-11
comments: true
---

### 테스트 개발환경
ubuntu 16.04.5 LTS , nodejs(v8,15,0) , mongodb

우분투 버전 확인하는 방법
{% highlight console %}
$ cat /etc /issue
{% endhighlight %}

<사진>
### 디렉토리 구조 및 설명

* models - mongodb에서 가져올 데이터의 형식을 명시한다
* routs - rest server가 받는 api 요청을 처리한다
* package.json - 프로젝트의 기본 환경설정을 구성한다
* server.js - 서버 설정 및 실행 명령



서버를 실행하는 server.js 파일 코드입니다.

<code>
//패키지 로드하기
var express = require('express');
var app = express();
var path = require('path');
var mongoose = require('mongoose');
var bodyParser = require('body-parser');

//mongodb server에 연결
var db = mongoose.connection;
db.on('error',console.error);
db.once('open',function(){
    //mongodb server에 연결된 경우
    console.log("conneced to mongodb Server");
});

//se.connect('mongodb://admin:1234@localhost:27017/test',{useNewUrlParser: true});
mongoose.connect('mongodb://admin:1234@localhost:27017/test?authSource=admin',{useNewUrlParser: true});

//모델 정의하기.
//model 은 db에서 데이터 읽기,생성,수정하는 프로그래밍 인터페이스를 정의한다.
//첫번째 인자 book은 해당 document가 사용할 collection(테이블)의 단수 표현. 
//자동으로 복수형태로 변환해 collection이름으로 사용한다.
var Book = require('./models/book');


//바디파서 이용하기 위한 앱설정
app.use(bodyParser.urlencoded({extended: true}));
app.use(bodyParser.json());

//기본 프로미스를 노드프로미스로 교체함
// mongoose.Promise = global.Promise;
// mongoose.connect(precess.env.MONGO_DB,{useMongoClient: true});


//server port 설정
var port = process.env.PORT || 8080;

//router 설정 
var router = require('./routes')(app,Book); 

//server 실행 
var server = app.listen(port,function(){
    console.log("express server has started on port"+port)
});

</code>

### Rest server 실행 전, mongodb를 먼저 실행 시키기
1. ubuntu에서 터미널을 열고 mongodb 실행하는 명령어 입력
{% highlight console %}
$ sudo systemctl start mongod.service
{% endhighlight %}

2. mongodb가 잘 실행되었는지 확인 명령 입력 
{% highlight console %}
$ sudo systemctl status mongod
{% endhighlight %}

<사진삽입>
위처럼 active(Running) 표시가 뜨면 정상 동작.

### mongodb shell 실행시키기 
shell 을 실행해 database와 collection을 삽입,수정,삭제,검색 할 수 있다.
우분투 터미널에서 mongodb shell 실행 명령어 입력
{% highlight console %}
$ mongo -u admin -p 1234 --authenticationDatabase admin
{% endhighlight %}





