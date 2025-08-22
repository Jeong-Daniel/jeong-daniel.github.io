---
title: OUTER JOIN 사용방법 (LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN)
date:   2022-05-13 11:38:46 +0900
categories: [Tech, SQL]
tags: [sql, db]
---
Oracle에서 INNER(내부) Join과 Outer(외부) 조인이 있는데 여기서 외부조인은 동일한 값이 없는 행도 반환할 때 사용하는 구문이라고 한다. 즉 두 테이블을 조인할때 조건에 해당하지 않는 데이터도 표시한다.

![JOIN IMAGE](https://user-images.githubusercontent.com/85277660/210567056-bb61b72a-e484-4279-8110-b979521b37b1.png)

OUTER JOIN은 두 테이블을 지정을 하고 나서 이어서 조인 USING, ON 조건절을 필수적으로 사용해야 한다. 

이때 기준이 되는 테이블이 조인 수행시 드라이빙 테이블이 된다고 한다. 

```py
SELECT NVL(B.EMP_NO , '(Null)') AS B_EMP_NO
     , NVL(B.EMP_NM , '(Null)') AS B_EMP_NM
     , NVL(B.JOB_NM , '(Null)') AS B_JOB_NM
     , NVL(B.DEPT_NO, '(Null)') AS B_DEPT_NO
     , NVL(A.DEPT_NO, '(Null)') AS A_DEPT_NO
     , NVL(A.DEPT_NM, '(Null)') AS A_DEPT_NM
  FROM TB_DEPT_51 A RIGHT OUTER JOIN TB_EMP_51 B
  ON (
          A.DEPT_NO = B.DEPT_NO AND A.DEPT_NO = 'D101'
      AND B.DEPT_NO IS NOT NULL
     )
  ORDER BY B.EMP_NO
  ;
```