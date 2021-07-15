# sqlTest

1. SELECT CONCAT('T0000', tt.`id`) AS 'ID', tt.`nickname`, CASE WHEN tt.status = 0 THEN 'Discontinued' WHEN tt.status = 1 THEN 'Active' ELSE 'Deactivated' END AS 'Status', 
CASE WHEN tr.role = 1 THEN 'Trainer' WHEN tr.role = 2 THEN 'Assessor' ELSE 'Reserved' END AS 'Roles'
FROM trn_teacher tt
LEFT JOIN trn_teacher_role tr ON tr.`teacher_id` = tt.`id`;

2. SELECT t.`id`, t.`nickname`, COUNT(DISTINCT(IF(tt.`status` = 1, 1, 0))) AS 'Open', COUNT(DISTINCT(IF(tt.`status` = 2, 1, 0))) AS 'Reserved', COUNT(DISTINCT(IF(te.`result` = 1, 1, 0))) AS 'Taught', COUNT(DISTINCT(IF(te.`result` = 2, 1, 0))) AS 'No Show'
FROM trn_teacher t
LEFT JOIN trn_time_table tt ON t.`id`= tt.`teacher_id`
LEFT JOIN trn_evaluation te ON t.`id` = te.`teacher_id`
LEFT JOIN trn_teacher_role tr ON tr.`teacher_id` = t.`id`
WHERE t.`status` IN (1,2)
AND tr.`role` IN (1,2)
GROUP BY t.`id`
