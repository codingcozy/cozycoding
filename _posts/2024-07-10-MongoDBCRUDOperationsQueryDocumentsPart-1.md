---
title: "MongoDB CRUD 작업  문서 쿼리하는 방법 Part-1"
description: ""
coverImage: "/blocktong.github.io/assets/no-image.jpg"
date: 2024-07-10 03:55
ogImage: 
  url: /blocktong.github.io/assets/no-image.jpg
tag: Tech
originalTitle: "MongoDB CRUD Operations — Query Documents Part-1."
link: "https://medium.com/@yogeshchiluka/mongodb-crud-operations-query-documents-part-1-9eda8b14d6dd"
isUpdated: true
---




## 쿼리 문서와 중첩/내장

**쿼리 문서**  
몽고디비(MongoDB)에서 `find()` 메서드를 사용하여 문서를 쿼리합니다.  

**예시 문서:**

<div class="content-ad"></div>

```js
 {
    item: 'journal',
    qty: 25,
    size: { h: 14, w: 21, uom: 'cm' },
    status: 'A'
  }
```

db.inventory.find('')

```js
db.inventory.find({})

//retrieves all documents in collection.
```

db.inventory.find('`field`:`value`')

<div class="content-ad"></div>

```js
db.inventory.find({ status: 'D' });
// 모든 문서를 검색합니다. sales 컬렉션에서,
// 상태가 "D"와 동일한 경우. 
```

db.inventory.find('`field1`:`value1`,`field2`:`value2`,..')
AND 연산

```js
db.inventory.find({ status: 'D', qty: '30'});

// 위는 AND 연산입니다,
// 컬렉션 내 모든 문서를 검색합니다.
// 여기서 상태가 "D"이고 수량이 "30"인 경우.
```

## 점 표기법을 사용한 중첩 필드 쿼리

<div class="content-ad"></div>

```js
{
    item: 'journal',
    qty: 25,
    size: { h: 14, w: 21, uom: 'cm' },
    status: 'A'
}
```

```js
db.inventory.find({'size.uom': 'in'})
// retrieves all documents in collection,
// where value size.uom equals "in".
```

**Note:** The structure of nested fields is important to avoid unpredictable behavior. Make sure the nested fields are in order.

<div class="content-ad"></div>

```js
db.inventory.find({
size: { w: 21, h: 14, uom: 'cm' }
});
```

올바른 사용법:

```js
db.inventory.find({
size: { h: 14, w: 21, uom: 'cm' }
});
```

몽고디비 비교 연산자


<div class="content-ad"></div>

`$eq`는 지정된 값과 같은 값을 일치합니다.

`$gt`는 지정된 값보다 큰 값을 일치시킵니다.

`$gte`는 지정된 값보다 크거나 같은 값을 일치시킵니다.

`$in`은 배열에 지정된 값 중 어느 것이든 일치합니다.

<div class="content-ad"></div>

`$lt`은 지정된 값보다 작은 값을 찾습니다.

`$lte`는 지정된 값보다 작거나 같은 값을 찾습니다.

`$ne`은 지정된 값과 다른 모든 값을 찾습니다.

`$nin`은 배열에 지정된 값 중 어느 것도 포함되지 않은 값을 찾습니다.

<div class="content-ad"></div>

예시 문서:

```js
{
    item: 'journal',
    qty: 25,
    size: { h: 14, w: 21, uom: 'cm' },
    status: 'A'
}
```

비교 연산자

Equal — $eq

<div class="content-ad"></div>

```js
db.inventory.find({qty: {$eq: 45}})
// 혹은 둘 다 같은 명령어입니다
db.inventory.find({qty: 45})
```

```js
// qty가 45와 동일한 값을 갖는 모든 문서를 반환합니다.
```

Greater than — $gt

```js
db.inventory.find({qty: {$gt: 45}})
// qty 값이 45보다 큰 모든 문서를 반환합니다.
// 만약 qty에 100, 45, 55, 60, 75 값이 있다면
// 결과는 100, 55, 60, 75가 됩니다.
```

<div class="content-ad"></div>

크거나 같음 — $gte

```js
db.inventory.find(qty: {$gte: 45})\
// 컬렉션에서 45보다 크거나 같은 모든 문서를 검색합니다.
// 만약 qty에 값이 100, 45, 55, 60, 75가 있다면
// 결과는 100, 45, 55, 60, 75가 됩니다.
```

작거나 같음 — $lt

```js
db.inventory.find(qty: {$lt: 45})
// 컬렉션에서 45보다 작은 모든 문서를 검색합니다.
// 만약 qty에 값이 100, 45, 55, 60, 75가 있다면
// 결과는 45가 됩니다.
```

<div class="content-ad"></div>

서는 MongoDB 쿼리 표현식입니다.

작거나 같음 — $lte

```js
db.inventory.find(qty: {$lte: 45})
// 45 이하의 모든 문서를 검색합니다.
// 만약 qty에 100, 45, 55, 60, 75가 있으면
// 결과는 45이 출력됩니다.
```

배열 안 값과 일치 — $in

```js
db.inventory.find(qty: {$in: [45, 100] })
// 배열 안에 지정된 값과 일치하는 모든 문서를 검색합니다.
// 만약 qty에 100, 45, 55, 60, 75가 있으면
// 결과는 45와 100이 출력됩니다.
```

<div class="content-ad"></div>

배열 안에 있는 값과 일치하지 않는 항목 찾기 — $nin

```js
db.inventory.find({ qty: { $nin: [45, 100] } })
// 배열에 지정된 값 중 일치하는 모든 문서를 검색합니다.
// qty 값이 100, 45, 55, 60, 75인 경우
// 55, 60, 75이 출력됩니다.
```

값이 특정 값과 일치하지 않는 항목 찾기 — $ne

```js
db.inventory.find({ qty: { $ne: 100 } })
// 특정 값과 일치하지 않는 모든 값에 해당하는 문서를 검색합니다.
// qty 값이 100, 45, 55, 60, 75인 경우
// 45, 55, 60, 75가 출력됩니다.
```

<div class="content-ad"></div>

비교 연산자에 대한 자세한 내용은 [Part 2](here)를 클릭하세요.