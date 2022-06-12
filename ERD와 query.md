## ERD and Query

### ■ ERD
---------------------------------------
<img src="../img/erd.png" alt="이체 내역 조회" size="width:90%"/>

### ■ Query
```sql
SELECT "", "이체 완료건수", "이체 오류건수", "처리 중 건수"

UNION

SELECT
    DISTINCT DATE_FORMAT(registDate, "%Y.%m") AS FDATE,
    SUM(case when statusIdx=1 then 1 END) over(PARTITION BY DATE_FORMAT(registDate, "%Y.%m")) s1,
    SUM(case when statusIdx=2 then 1 END) over(PARTITION BY DATE_FORMAT(registDate, "%Y.%m")) s2,
    SUM(case when statusIdx=3 then 1 END) over(PARTITION BY DATE_FORMAT(registDate, "%Y.%m")) s3
    FROM transfer
WHERE registDate BETWEEN "2021-01-01" AND "2021-03-31"

UNION

SELECT
    "합계",
    COUNT(case when statusIdx=1 then 1 END),
    COUNT(case when statusIdx=2 then 1 END),
    COUNT(case when statusIdx=3 then 1 END)
FROM transfer
WHERE registDate BETWEEN "2021-01-01" AND "2021-03-31"
```