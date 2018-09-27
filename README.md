# Adatbázisok alapok
### másol ezt:

```SQL
DROP TABLE SZERET; 

CREATE TABLE SZERET
    (NEV         VARCHAR2(15),
     GYUMOLCS    VARCHAR2(15));

INSERT INTO SZERET VALUES ('Malacka','alma');
INSERT INTO SZERET VALUES ('Micimackó','alma');
INSERT INTO SZERET VALUES ('Malacka','körte');
INSERT INTO SZERET VALUES ('Micimackó','körte');
INSERT INTO SZERET VALUES ('Kanga','körte');
INSERT INTO SZERET VALUES ('Tigris','körte');
INSERT INTO SZERET VALUES ('Micimackó','málna');
INSERT INTO SZERET VALUES ('Malacka','málna');
INSERT INTO SZERET VALUES ('Kanga','málna');
INSERT INTO SZERET VALUES ('Tigris','málna');
INSERT INTO SZERET VALUES ('Nyuszi','eper');
INSERT INTO SZERET VALUES ('Malacka','eper');

COMMIT; 

DROP TABLE Fiz_Kategoria;
DROP TABLE Dolgozo;
DROP TABLE Osztaly;

ALTER SESSION SET NLS_DATE_LANGUAGE = ENGLISH;
ALTER SESSION SET NLS_DATE_FORMAT = 'DD-MON-YYYY';

CREATE TABLE Osztaly
    (OAZON             NUMBER(2) NOT NULL,
     ONEV              VARCHAR2(14),
     TELEPHELY         VARCHAR2(13),
     CONSTRAINT OSZT_PK PRIMARY KEY (OAZON));

INSERT INTO Osztaly VALUES (10,'ACCOUNTING','NEW YORK');
INSERT INTO Osztaly VALUES (20,'RESEARCH','DALLAS');
INSERT INTO Osztaly VALUES (30,'SALES','CHICAGO');
INSERT INTO Osztaly VALUES (40,'OPERATIONS','BOSTON');

CREATE TABLE Dolgozo
    (DKOD               NUMBER(4) NOT NULL,
     DNEV               VARCHAR2(10),
     FOGLALKOZAS        VARCHAR2(9),
     FONOKE             NUMBER(4) CONSTRAINT DOLG_SELF_KEY REFERENCES Dolgozo (DKOD),
     BELEPES            DATE,
     FIZETES            NUMBER(7,2),
     JUTALEK            NUMBER(7,2),
     OAZON              NUMBER(2),
     CONSTRAINT DOLG_FK FOREIGN KEY (OAZON) REFERENCES Osztaly (OAZON),
     CONSTRAINT DOLG_PK PRIMARY KEY (DKOD));

INSERT INTO Dolgozo VALUES (7839,'KING','PRESIDENT',NULL,'17-NOV-1981',5000,NULL,10);
INSERT INTO Dolgozo VALUES (7698,'BLAKE','MANAGER',7839,'1-MAY-1981',2850,NULL,30);
INSERT INTO Dolgozo VALUES (7782,'CLARK','MANAGER',7839,'9-JUN-1981',2450,NULL,10);
INSERT INTO Dolgozo VALUES (7566,'JONES','MANAGER',7839,'2-APR-1981',2975,NULL,20);
INSERT INTO Dolgozo VALUES (7654,'MARTIN','SALESMAN',7698,'28-SEP-1981',1250,1400,30);
INSERT INTO Dolgozo VALUES (7499,'ALLEN','SALESMAN',7698,'20-FEB-1981',1600,300,30);
INSERT INTO Dolgozo VALUES (7844,'TURNER','SALESMAN',7698,'8-SEP-1981',1500,0,30);
INSERT INTO Dolgozo VALUES (7900,'JAMES','CLERK',7698,'3-DEC-1981',950,NULL,30);
INSERT INTO Dolgozo VALUES (7521,'WARD','SALESMAN',7698,'22-FEB-1981',1250,500,30);
INSERT INTO Dolgozo VALUES (7902,'FORD','ANALYST',7566,'3-DEC-1981',3000,NULL,20);
INSERT INTO Dolgozo VALUES (7369,'SMITH','CLERK',7902,'17-DEC-1980',800,NULL,20);
INSERT INTO Dolgozo VALUES (7788,'SCOTT','ANALYST',7566,'09-DEC-1982',3000,NULL,20);
INSERT INTO Dolgozo VALUES (7876,'ADAMS','CLERK',7788,'12-JAN-1983',1100,NULL,20);
INSERT INTO Dolgozo VALUES (7934,'MILLER','CLERK',7782,'23-JAN-1982',1300,NULL,10);
INSERT INTO Dolgozo VALUES (7877,'LOLA','CLERK',7902,'12-JAN-1981',800,NULL,NULL);
INSERT INTO Dolgozo VALUES (7878,'BLACK',NULL,7902,'01-MAY-1987',1800,300,NULL);

CREATE TABLE Fiz_Kategoria (
 KATEGORIA          NUMBER,
 ALSO               NUMBER,
 FELSO              NUMBER);

INSERT INTO Fiz_Kategoria VALUES (1,700,1200);
INSERT INTO Fiz_Kategoria VALUES (2,1201,1400);
INSERT INTO Fiz_Kategoria VALUES (3,1401,2000);
INSERT INTO Fiz_Kategoria VALUES (4,2001,3000);
INSERT INTO Fiz_Kategoria VALUES (5,3001,9999);

COMMIT; 

GRANT SELECT ON Osztaly TO PUBLIC; 
GRANT SELECT ON Dolgozo TO PUBLIC;
GRANT SELECT ON Fiz_Kategoria TO PUBLIC;

ALTER SESSION SET NLS_DATE_LANGUAGE = HUNGARIAN;
ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY.MM.DD';

SELECT * FROM Osztaly;
SELECT * FROM Dolgozo;
SELECT * FROM Fiz_Kategoria;
```

### Futtat
`select * from szeret;`

> ezek a feladatok: http://vopraai.web.elte.hu/tananyag/adatb1819/2.ora/feladat.txt

- Melyek azok a gyümölcsök, amelyeket Micimackó szeret

`select gyumolcs from szeret where nev='Micimackó';`

- Melyek azok a gyümölcsök, amelyeket Micimackó nem szeret
```SQL
(SELECT gyumolcs FROM szeret)
MINUS
(SELECT gyumolcs FROM szeret WHERE nev='Micimackó');
```

- Kik szeretik az almát
SELECT nev FROM szeret WHERE gyumolcs='alma';

--Kik nem szeretik a körtét?
```SQL
(SELECT nev FROM szeret)
MINUS
(SELECT nev FROM szeret WHERE gyumolcs='körte');
```
- Kik szeretik vagy az almát vagy a körtét?

`SELECT DISTINCT nev FROM szeret WHERE gyumolcs='alma' or gyumolcs='körte';`
VAGY
```SQL
SELECT nev FROM szeret WHERE gyumolcs='alma' 
UNION 
SELECT nev FROM szeret WHERE gyumolcs='körte';
```
- Kik szeretik az almát is és a körtét is?
```SQL
SELECT nev FROM szeret WHERE gyumolcs='alma' 
INTERSECT
SELECT nev FROM szeret WHERE gyumolcs='körte';
```
- Kik azok, akik szeretik az almát, de nem szeretik a körtét?
```SQL
SELECT nev FROM szeret WHERE gyumolcs='alma' 
MINUS
SELECT nev FROM szeret WHERE gyumolcs='körte';
```
- Kik azok a dolgozók, akiknek a fizetése nagyobb, mint 2800?

`SELECT dnev FROM dolgozo WHERE fizetes>2800;`

- Kik azok a dolgozók, akik a 10-es vagy a 20-as osztályon dolgoznak
`SELECT dnev FROM dolgozo WHERE oazon=10 or oazon=20;`

- Kik azok, akiknek a jutaléka nagyobb, mint 600?
`SELECT dnev FROM dolgozo WHERE jutalek>600;`

- Kik azok, akiknek a jutaléka nem nagyobb, mint 600?

`SELECT dnev FROM dolgozo WHERE jutalek<600 OR jutalek IS NULL;`

- Kik azok a dolgozók, akiknek a jutaléka ismeretlen (nincs kitöltve, vagyis NULL)?

`SELECT dnev FROM dolgozo WHERE jutalek IS NULL;`

- Adjuk meg a dolgozók között előforduló foglalkozások neveit
`SELECT DISTINCT foglalkozas FROM dolgozo;`

- Adjuk meg azoknak a nevét és kétszeres fizetését, akik a 10-es osztályon dolgoznak
`SELECT DISTINCT dnev,fizetes*2 FROM dolgozo WHERE oazon = 10;`

- Kik azok a dolgozók, akik 1982.01.01 után léptek be a céghez
`SELECT DISTINCT dnev FROM dolgozo WHERE belepes > TO_DATE('1982.01.01','YYYY.MM.DD');`

- Kik azok, akiknek nincs főnöke?

`SELECT DISTINCT dnev FROM dolgozo WHERE fonoke IS NULL;`

- Kik azok a dolgozók, akiknek a főnöke KING? (egyelőre leolvasva a képernyőről) 

`SELECT DISTINCT dnev FROM dolgozo WHERE fonoke='7839';`
VAGY
`SELECT DISTINCT * FROM dolgozo WHERE fonoke=(SELECT dkod FROM dolgozo WHERE dnev='KING');`
