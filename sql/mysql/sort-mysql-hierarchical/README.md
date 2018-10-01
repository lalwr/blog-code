# 계층형 메뉴 정렬 하는 쿼리

```sql
CREATE DATABASE test default CHARACTER SET UTF8;

use test;

DROP table IF EXISTS menu;
CREATE TABLE menu (
  no int PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  parent int NULL,
  depth int NOT NULL,
  sequence int,
  reg_date DATETIME,
  modify_date DATETIME
)ENGINE=INNODB;
DESCRIBE  menu;

DROP TABLE menu;

INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date) VALUES("메뉴1" ,0 ,1, 1, now(), now());
INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴1-1" ,1 ,2, 1, now(), now());
INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴1-2" ,1 ,2, 2, now(), now());
INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴1-1-1" ,2 ,3, 1, now(), now());

INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴2" ,0 ,1, 2, now(), now());
INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴2-1" ,5 ,2, 1, now(), now());
INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴2-2" ,5 ,2, 2, now(), now());
INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴2-1-1" ,6 ,3, 1, now(), now());

INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴1-1-2" ,2 ,3, 2, now(), now());
INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴2-1-2" ,6 ,3, 2, now(), now());
INSERT INTO menu(name, parent, depth , sequence, reg_date, modify_date)  VALUES("메뉴2-1-3" ,6 ,3, 3, now(), now());
SELECT * FROM menu;

/* 계층형 mysql 쿼리 정렬 함수 */
DROP FUNCTION IF EXISTS hierarchy_connect_by_parent_eq_prior_no;
DELIMITER $$
CREATE FUNCTION hierarchy_connect_by_parent_eq_prior_no(value INT) RETURNS INTEGER
NOT DETERMINISTIC
READS SQL DATA
  BEGIN
    DECLARE _parent INT;
    DECLARE _rank INT;
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET @id = NULL;

    SET _parent = @id;
    SET _rank = 0;

    IF @id IS NULL THEN
      RETURN NULL;
    END IF;

    LOOP
      SET @innerrank = 0;
      SELECT p.no
          INTO   @id
      FROM   (
             SELECT   no, @innerrank := @innerrank+1 AS ran
             FROM     menu
             WHERE    COALESCE(parent, 0) = _parent
             ORDER BY sequence
             ) p
      WHERE   p.ran > _rank LIMIT 0, 1;
      IF @id IS NOT NULL OR _parent = @start_with THEN
        SET @level = @level + 1;
        RETURN @id;
      END IF;
      SET @level := @level - 1;
      SET @innerrank = 0;
      SELECT COALESCE(p.parent, 0), p.ran
          INTO   _parent, _rank
      FROM   (
             SELECT no, parent, @innerrank := @innerrank+1 AS ran
             FROM    menu
             WHERE   COALESCE(parent, 0) = (
                                           SELECT COALESCE(parent, 0) FROM menu WHERE no = _parent
                                           )
             ORDER BY sequence
             ) p
      WHERE p.no = _parent;
    END LOOP;
  END;
$$
DELIMITER ;

/* 계층형 메뉴 정렬*/
SELECT  CONCAT(REPEAT('    ', depth  - 1), CAST(ou.no AS CHAR)) AS no, parent, depth, CONCAT(REPEAT('    ', depth  - 1), CAST(ou.name AS CHAR)) AS name, sequence, level FROM (
                 SELECT mn.name , mn.no, depth , parent, sequence, level FROM (
                                                               SELECT hierarchy_connect_by_parent_eq_prior_no(no) AS no,
                                                                      @level AS level
                                                               FROM (
                                                                    SELECT @start_with := 0, @id := @start_with, @level := 0
                                                                    ) vars, menu
                                                               WHERE @id IS NOT NULL
                                                               ) m
                                                                 JOIN menu mn ON mn.no = m.no
                 ) ou;

/* 조건에 따라 메뉴 출력 */
SELECT mn.name , mn.no, depth , parent, sequence
FROM    (
        SELECT no, @depth  AS vars_depth FROM menu WHERE no= '원하는메뉴번호'
        UNION ALL
        SELECT  hierarchy_connect_by_parent_eq_prior_id(no) AS no, @depth  AS vars_depth
        FROM    (
                SELECT  @start_with := '원하는메뉴번호',
                        @no := @start_with,
                        @depth  := 0
                ) vars, menu
        WHERE   @no IS NOT NULL
        ) fn
          JOIN    menu mn
            ON      mn.no = fn.no;
```

##### 참고사이트

- https://explainextended.com/2009/03/17/hierarchical-queries-in-mysql/
- https://www.codesd.com/item/sort-mysql-hierarchical-data.html