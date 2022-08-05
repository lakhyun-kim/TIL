# 정규식(regexp)검사를 통한 숫자만, 특수문자 포함

// 숫자만 있는 컬럼

SELECT * FROM temp_table WHERE column1 REGEXP '^[0-9]+$';

// 특수문자가 포함된 경우

SELECT * FROM temp_table WHERE column1 REGEXP '[`~!#$%^&*|\\\'\";:\/?]';
