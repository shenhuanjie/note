### 查询考试结果表中假删的数据

```sql
-- 七年级
SELECT
	*
FROM
	t_exam_result_d4a59282_f629_4913_8ebe_7946407a6246
WHERE
	RECORD_STATUS = 0;
-- 八年级
SELECT
	*
FROM
	t_exam_result_80d70497_d78f_42ab_a5ab_ff15eb3df42f
WHERE
	RECORD_STATUS = 0;
-- 九年级
SELECT
	*
FROM
	t_exam_result_f4270f2d_f9a7_4d98_a619_767771f66efd
WHERE
	RECORD_STATUS = 0;
```

### 查询考试结果表中重复的数据

```sql
-- 七年级

SELECT t1.ID,
	t1.EXAMINEE_ID,
	t1.SUBJECT_ID,
	t1.count 
FROM
	( SELECT ID, EXAMINEE_ID, SUBJECT_ID, COUNT( * ) count FROM t_exam_result_d4a59282_f629_4913_8ebe_7946407a6246 WHERE RECORD_STATUS = 1 GROUP BY EXAMINEE_ID, SUBJECT_ID ) t1 
WHERE
	t1.count > 1 
ORDER BY
	t1.count DESC,
	SUBJECT_ID;
	
-- 八年级
SELECT
	t1.EXAMINEE_ID,
	t1.SUBJECT_ID,
	t1.count 
FROM
	( SELECT EXAMINEE_ID, SUBJECT_ID, COUNT( * ) count FROM t_exam_result_80d70497_d78f_42ab_a5ab_ff15eb3df42f WHERE RECORD_STATUS = 1 GROUP BY EXAMINEE_ID, SUBJECT_ID ) t1 
WHERE
	t1.count > 1 
ORDER BY
	t1.count DESC,
	SUBJECT_ID;
	
-- 九年级
SELECT
	t1.EXAMINEE_ID,
	t1.SUBJECT_ID,
	t1.count 
FROM
	( SELECT EXAMINEE_ID, SUBJECT_ID, COUNT( * ) count FROM t_exam_result_f4270f2d_f9a7_4d98_a619_767771f66efd WHERE RECORD_STATUS = 1 GROUP BY EXAMINEE_ID, SUBJECT_ID ) t1 
WHERE
	t1.count > 1 
ORDER BY
	t1.count DESC,
	SUBJECT_ID;
```

### 更新(假删)考试结果表中重复的数据

```sql
UPDATE t_exam_result_f4270f2d_f9a7_4d98_a619_767771f66efd t_update,
(
SELECT
	t1.ID,
	t1.EXAMINEE_ID,
	t1.SUBJECT_ID,
	t1.count 
FROM
	( SELECT ID, EXAMINEE_ID, SUBJECT_ID, COUNT( * ) count FROM t_exam_result_f4270f2d_f9a7_4d98_a619_767771f66efd WHERE RECORD_STATUS = 1 GROUP BY EXAMINEE_ID, SUBJECT_ID ) t1 
WHERE
	t1.count > 1 
ORDER BY
	t1.count DESC,
	SUBJECT_ID 
	) t2 
	SET t_update.RECORD_STATUS = 0 
WHERE
	t_update.ID = t2.ID
```

### 扫描进度查询

```sql
SELECT * FROM (
			SELECT
				etp.ID AS SCHOOL_ID,
				result.EXAM_SUBJECT_ID,
				etp.ETP_NAME AS SCHOOL_NAME,
				sub.SUBJECT_NAME,
				result.STUDENT_NUM,
				result.SCAN_NUM,
				(
					result.STUDENT_NUM - SCAN_NUM
				) AS UNTREATED_NUM,
				result.PERCENT,
				IF(result.PERCENT=100,1,0) AS IS_COMPLETE,
				sub.SORT_NUM
			FROM
				(
					SELECT
						student.*, IFNULL(scan.SCAN_NUM, 0) AS SCAN_NUM,
						FORMAT(
							IFNULL(
								(
									SCAN_NUM / student.STUDENT_NUM * 100
								),
								0
							),
							2
						) AS PERCENT
					FROM
						(
							SELECT
								t1.ID AS EXAM_SUBJECT_ID,
								t1.SUBJECT_ID,
								t2.SCHOOL_ID,
								t3.STUDENT_NUM
							FROM
								t_exam_subject t1,
								t_exam_school t2
							INNER JOIN (
								SELECT
									SCHOOL_ID,
									count(1) STUDENT_NUM
								FROM
									t_examinee
								WHERE
									1=1
									AND RECORD_STATUS = 1
								GROUP BY
									SCHOOL_ID
							) t3 ON t2.SCHOOL_ID = t3.SCHOOL_ID
							WHERE
								1=1
							AND t2.PARTICIPATE_TYPE = 2
							AND t1.RECORD_STATUS = 1
							AND t2.RECORD_STATUS = 1
						) student
					LEFT JOIN (
						SELECT
							t1.SUBJECT_ID EXAM_SUBJECT_ID,
							t1.SCHOOL_ID,
							t1.NUM AS SCAN_NUM
						FROM
							(
								SELECT
									count(1) NUM,
									SCHOOL_ID,
									SUBJECT_ID
								FROM
									t_exam_result
								WHERE
									1=1
								AND RECORD_STATUS = 1
								GROUP BY
									SUBJECT_ID,
									SCHOOL_ID
							) t1
						INNER JOIN sys_enterprise t2 ON t1.SCHOOL_ID = t2.ID
						WHERE
							t2.RECORD_STATUS = 1
					) scan ON student.EXAM_SUBJECT_ID = scan.EXAM_SUBJECT_ID
					AND student.SCHOOL_ID = scan.SCHOOL_ID
				) result,
				sys_enterprise etp,
				t_subject sub
			WHERE
				etp.RECORD_STATUS = 1
			AND sub.RECORD_STATUS = 1
			AND result.SUBJECT_ID = sub.ID
			AND result.SCHOOL_ID = etp.ID
			) list
		WHERE
			1 = 1
		ORDER BY
			IS_COMPLETE,
			SORT_NUM,
			SCHOOL_ID
```

