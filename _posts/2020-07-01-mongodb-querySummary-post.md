---
layout: post
title: "[mongoDB] 자주쓰이는 mongodb 쿼리"
excerpt: "mongdb에 데이터를 읽고 저장하는 쿼리 정리"
categories: [mongoDB]
comments: true
---

프로젝트를 진행하는 동안 mongodb에서 자주 사용했던 쿼리를 정리했습니다. <br/>
간략화를 위해 데이터 무결성은 생략 되었습니다.

## 사용한 스키마 모델

- 대학교 수업 정보
- 한 수업은 **순서대로** 진행되는 분반(type)이 존재한다.
- 분반은 분반식별자, 교수 그리고 학생 목록을 갖고 있다.
- versionKey: 자동 삽입되는 '/v' 필드 생략

```javascript
var classSchema = new mongoose.Schema(
  {
    classId: {
      type: String,
      required: true,
    },
    types: [
      {
        professor: String,
        students: [
          {
            name: String,
            studentId: Number,
          },
        ],
      },
    ],
  },
  {
    versionKey: false,
    collection: "Class",
  }
);
```

## 전체 수업목록 찾기

- `find` : document 찾기
- 매개변수 {}를 넘기면 전체 데이터 반환

```javascript
router.get("/", function (req, res) {
  var query = {};

  Class.find(query)
    .sort({ _id: -1 })
    .then(function (err, classes) {
      if (err) {
        return res.status(500).send(err);
      }
      return res.send(classes);
    });
});
```

## 수업 찾기

- `findOne`: 특정 document 찾기

```javascript
router.get("/:classId", (req, res) => {
  const { classId } = req.params;

  Class.findOne({ classId: classId })
    .then((class) => res.send(class))
    .catch((err) => res.status(500).send(err));
});
```

## 수업 정보 갱신하기

- `findByIdAndUpdate`: 조건을 만족하는 특정 document를 갱신하기
- req에 전달된 갱신 데이터(요청 페이로드)
- `save`: 갱신된 document의 저장을 연결된 DB에 요청

```javascript
router.patch("/:classId", (req, res) => {
  Class.findByIdAndUpdate(req.params.classId, req.body)
    .save()
    .then(() => res.send({ success: true }))
    .catch((err) => res.status(500).send(err));
});
```

## 수업 분반에 학생 추가하기

- document가 가진 객체 배열에 객체 삽입하기
- `push`: 배열에 데이터 삽입
- 중복없이 학생 삽입하는 경우 `addToSet`로 교체

```javascript
router.put("/:classId", (req, res) => {
  const { classId } = req.params;
  const { typeIndex, name, studentId } = req.body;

  Class.findOne({ classId: classId }).then((class, err) => {

    let types = class.types[typeIndex];
    types.students.push({
      name,
      studentId,
    });

    class.save((class,err) => {
      if (err) return res.status(500).send(err);
      return res.send(class);
    });
  });
});
```

## 수업 분반에서 학생 지우기

- 특정 배열에서 데이터 삭제
- `pull`: 배열에서 조건을 만족하는 객체를 삭제

```javascript
router.patch("/subply/:classId", (req, res) => {
  const { classId } = req.params;
  const { typeIndex, studentId } = req.body;

  Class.findOne({ classId: classId })
    .then((class) => {
      let type = class.types[typeIndex];
      type.students.pull({ studentId: studentId });

      class.save((err) => {
        if (err) return res.status(500).send(err);
        return res.send(class);
      });
    })
    .catch((err) => res.status(500).send(err));
});
```

## 수업 분반에서 학생 이름 변경하기

- 특정 분반의 학생 이름 변경하기
- 객체안에 있는 객체 배열(array of object)에서 특정 객체 찾기
- `find`와 함께 `arrow function`사용해 찾는 조건 명시

```javascript
let st = class.types[typeIndex].students
    .find((student) => student.studentId === studentId);
st.name = "Hako";
```
