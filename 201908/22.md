## Oracle - Pivot, Unpivot

데이터 마이그레이션 작업을 하던 중 
'열' 로 된 데이터를 '행' 으로 옮겨야 할 일이 생겼다.
방법을 찾아보다가 발견한 것이 pivot 이라는 함수다.
(oracle11g 부터 사용 가능)

### Pivot
단어 그대로 가로/세로 축을 바꾸는 것이다.
Pivot 은 세로로 된 데이터를 가로로 (행 -> 열),
Unpivot 은 가로로 된 데이터를 세로로 (열 -> 행) 바꿔준다.

Group By 와 집계함수(Sum, Max 등) 을 합친 기능이라고 생각하면 된다.

Pivot 함수가 없을 땐 case 문을 사용해서 같은 기능을 구현할 수 있었는데,
Pivot 함수를 사용하면 훨씬 간단하다.
[참고 : case, groupby 를 이용한 pivot](https://m.blog.naver.com/PostView.nhn?blogId=silentis&logNo=220943488844&proxyReferer=https%3A%2F%2Fwww.google.com%2F)


문법은 다음과 같다.

```sql
SELECT 
PIVOT 뽑아낼 대상컬럼(GROUP 함수 적용)
FOR IN절의 값 대상이 되는 컬럼명 
IN 매핑시킬 값(행->열 변경 시 컬럼이 된다)
```

이해가 잘 안갈 것이다 (내가 그랬다). 다음 예제를 보자.

예제
```sql
with three_kingdoms_birth as (
    select '1월' 생일, '유비' 이름 from dual union all
    select '2월' 생일, '관우' 이름 from dual union all
    select '3월' 생일, '장비' 이름 from dual union all
    select '4월' 생일, '조조' 이름 from dual union all
    select '5월' 생일, '전위' 이름 from dual union all
    select '4월' 생일, '하후돈' 이름 from dual union all
    select '2월' 생일, '손권' 이름 from dual union all
    select '2월' 생일, '주유' 이름 from dual union all
    select '6월' 생일, '노숙' 이름 from dual
)
select * from three_kingdoms_birth
pivot (
    count(이름) for 생일 in ('1월', '2월', '3월', '4월', '5월', '6월')
)

```

이렇게 하면

![Pivot전](/images/201908/Pivot_1.png)

이랬던 데이터를


![Pivot후](/images/201908/Pivot_2.png)

이렇게 추출할 수 있다.



### Unpivot
Pivot 과 반대로 '열' 데이터를 '행' 으로 바꿔준다.

내가 하려고 했던 것은 이런거였다.

![Unpivot전](/images/201908/Unpivot_1.png)

이런 테이블 구조에서는, 국민이 늘어날 때마다 국민3, 국민4 ... 컬럼을 추가해야 한다.
따라서 좀 더 유연하게 사용할 수 있도록 국민 번호를 '행' 데이터로 바꾸고 싶었다.
그래서 테이블을 새로 설계했고, 데이터를 마이그레이션 해야 했다.

Unpivot 을 이용하면 다음과 같이 할 수 있다.


```sql
with three_kingdoms as (
    select '위' 나라, '조조' 국민0, '전위' 국민1, '하후돈' 국민2 from dual union all
    select '촉' 나라, '유비' 국민0, '관우' 국민1, '장비' 국민2 from dual union all
    select '오' 나라, '손권' 국민0, '주유' 국민1, '노숙' 국민2 from dual 
)
select * from three_kingdoms

unpivot (
    이름 for 순서 in (국민0, 국민1, 국민2)
)

```

![Unpivot전](/images/201908/Unpivot_2.png)

그러면 이렇게 원하던 결과가 나오고, 데이터를 '행' 으로 쌓을 수 있다.
