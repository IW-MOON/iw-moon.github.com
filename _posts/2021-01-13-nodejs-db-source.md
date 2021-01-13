---
title: Node.js DB Source 분리
categories:
- Project
---

Nodejs는 기본적으로 비동기 기반으로 처리가 된다.
따라서 아래의 소스코드에서 getMyWc함수 처리와 무관하게 console.log를 통해 'result2후'가 출력이 된다.


```javascript
const result2 = dao.getMyWc(user_idx);
if(result2){
             console.log('result2후');
}
```

이를 해결하기 위해서 이 부분을 '동기'처리 해주어야 한다. await를 이용하면 가능하다.
그러나 await는 async함수내에서만 가능하기에, 처리 함수를 async화 해야한다.

```javascript
async function getMyWc(){
                const result2 = await dao.getMyWc(user_idx);

}
```

getMyWc함수에서는 Promise를 통해 DB 결과 값을 반환한다.
"ES2016의 promise 이기만 하면 우리는 기다릴(await) 수 있어요."
"말 그대로 promise에서 '.then()'을 쓰는거랑 똑같은거에요. (콜백 함수를 요구하지 않는다는점은 다르지만요)"

```javascript
getMyWc :  function(user_idx){
        console.log(user_idx);
        var params = {
            user_idx : user_idx,
            use_yn : 'y'
        };
        var query = mybatisMapper.getStatement('workoutMapper', 'selectMyWc', params, format);
        return common.returnSelectPromise(query);
 },

returnSelectPromise : function(query){

        try {
            return new Promise(function(resolve, reject){
                getConnection((conn) => {
                    conn.query(query, function(err,rows){
                        if(err) {
                            console.log(err);
                            reject(err);
                        } else {
                            json_rows = JSON.parse(JSON.stringify(rows));
                            console.log(json_rows);
                            resolve(json_rows);
                        }
                    });
                    conn.release();
                });
            });
        } catch(err){
            console.log('query is not excuted. select fail...\n' + err);
            conn.release();
            return;
        }
 
    },

}
```
