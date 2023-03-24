## JOIN 
<img width="700" alt="image" src="https://user-images.githubusercontent.com/63537847/227429501-737ed0d9-a2ed-4c8c-bb57-0644ef95bbd5.png">

- 둘 이상의 테이블을 연결해서 데이터를 검색하는 방법 
- 테이블은 적어도 하나의 칼럼을 공유하고 있어야 함 
  - 공유하는 칼럼을 PK or FK 값으로 사용 

## JOIN의 종류 및 예시 
<img width="700" alt="image" src="https://user-images.githubusercontent.com/63537847/227431024-6b75222f-73ed-4a1d-94d1-ec9ab1319e82.png">

### 1️⃣ INNER JOIN
<img width="300" alt="image" src="https://user-images.githubusercontent.com/63537847/227433526-bbe5dd17-a5d5-4fab-a210-af717a203b38.png">

교집합, 공통적인 부분만 SELECT 되는 경우              
```SQL 
SELECT A.ID, A.A_NAME, A.B_NAME 
FROM A INNER JOIN B
ON A.ID = B.ID 
```
|ID|A_NAME|B_NAME|
|---|---|---|
|1|AAA|가|
|2|BBB|나|

### 2️⃣ LEFT JOIN
<img width="300" alt="image" src="https://user-images.githubusercontent.com/63537847/227433572-8b1c1368-65d1-4362-9e91-35853f002a24.png">

JOIN 기준 왼쪽에 있는 튜플 모두 SELECT                
공통적인 부분 + LEFT에 있는 튜플 
```SQL 
SELECT A.ID, A.A_NAME, A.B_NAME 
FROM A LEFT OUTER JOIN B
ON A.ID = B.ID 
```
|ID|A_NAME|B_NAME|
|---|---|---|
|1|AAA|가|
|2|BBB|나|
|4|DDD|NULL|
|5|EEE|NULL|

### 3️⃣ RIGHT JOIN
<img width="300" alt="image" src="https://user-images.githubusercontent.com/63537847/227433633-4ff122ec-a29b-4705-ae4c-3d1cb783b0b8.png">

JOIN 기준 오른쪽에 있는 튜플 모두 SELECT                
공통적인 부분 + RIGHT에 있는 튜플 
```SQL 
SELECT A.ID, A.A_NAME, A.B_NAME 
FROM A RIGHT OUTER JOIN B
ON A.ID = B.ID 
```
|ID|A_NAME|B_NAME|
|---|---|---|
|1|AAA|가|
|2|BBB|나|
|3|NULL|다|

### 4️⃣ OUTER JOIN 
<img width="300" alt="image" src="https://user-images.githubusercontent.com/63537847/227433673-a71b7a4f-efcc-4617-8ea5-86e05547d912.png">

두 테이블이 갖고 있는 튜플 모두 SELECT 
```SQL 
SELECT A.ID, A.A_NAME, A.B_NAME 
FROM A FULL OUTER JOIN B
ON A.ID = B.ID 
```
|ID|A_NAME|B_NAME|
|---|---|---|
|1|AAA|가|
|2|BBB|나|
|3|NULL|다|
|4|DDD|NULL|
|5|EEE|NULL|

</br> 
</br> 

> 참고              
> https://pearlluck.tistory.com/46              
