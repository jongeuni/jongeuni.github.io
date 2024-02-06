---
title: Update document requires atomic operators
next: first-page
---

`#mongoose`

## Hello, World!


```json
{
    "code": "301",
    "message": "Update document requires atomic operators",
    "timestamp": "2023-10-23T07:09:12.181Z"
}
```

```ts
// before
async update(rq: AListRq) {  
    const aWrites: AnyBulkWriteOperation<Schemat>[] = [];  
  
    rq.aList.forEach(a => {  
        aWrites.push({  
            updateOne: {  
                filter: {_id: a.id},  
                update: {$set: {order: a.order}}
            }  
        });  
    });  
  
    await this.aRepository.model.bulkWrite(aWrites);  
}
```
한 번의 요청으로 리스트의 순서를 변경해야 하는 니즈가 있었다. 일일히 변경하기 보다는 bulkWrite를 이용해서 한 번에 업데이트가 일어나도록 구현하려고 했는데, 분명 맞게 작성한 것 같았지만 동작하지 않았다.

[bulk wirte mongoDB 공식 문서](https://www.mongodb.com/docs/manual/core/bulk-write-operations/) 예시
```json
db.pizzas.bulkWrite([
    {
        updateOne: {
            filter: {type: "cheese"},
            update: {$set: {price: 8}}
        }
    }
])
```

이리저리 서치를 해보았고 [이 문제](https://github.com/Automattic/mongoose/issues/9737)가 내가 겪고 있는 것과 가장 유사하다고 느꼈다. 하지만 여기서도 다른 문제가 있는거지 나랑 거의 동일하게 작성한 업데이트 배열은 문제라고 여겨지는 것 같지 않았다. (나의 영어 실력 부족일 수도 있다.)

그렇게 해매고 있다가 [bulk write updateOne mongoDB 공식 문서](https://www.mongodb.com/docs/manual/reference/method/db.collection.bulkWrite/#std-label-bulkwrite-write-operations-updateOneMany)를 다시 보았다.
![[Pasted image 20231027101327.png]]
![[Pasted image 20231027101833.png]]
pipeline...? pipeline...? pipeline...?
![[Pasted image 20231027102256.png]]
엄청나게 친절하게 밑에 설명도 써져 있었다. 이걸 확인을 못하다니. 
```ts
// after
aWrites.push({  
    updateOne: {  
       filter: {_id: a.id},  
       update: [{$set: {order: a.order}}]
    }  
});  
```
update 해주는 부분을 pipeline으로(대괄호) 묶어주었더니 언제 그랬냐는듯 잘 동작하기 시작했다. 해결 완료.

[pipeline](https://www.mongodb.com/docs/manual/aggregation/#aggregation-framework)은 stage의 묶음이다. 그리고 stage는 내가 처음에 썼던 `{$set: {price: 8}}`이다. 나는 pipeline을 사용해야 하는 곳에 stage를 쓰고 있었던 것이다. 공식 문서를 꼼꼼히 읽자. 잘 발견해서 다행이다.
