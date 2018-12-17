1. Sa se creeze doua viziuni in baza interogarilor formulate in doua exercitii indicate din capitolul 4.
Prima viziune sa fie construita in Editorul de interogari, iar a doua, utilizand View Designer.

``` sql
CREATE VIEW ViewEX18_fromLAB4_forLAB8
AS
SELECT DISTINCT p.Nume_Profesor, p.Prenume_Profesor
FROM profesori p JOIN studenti_reusita r
ON p.Id_Profesor=r.Id_Profesor
JOIN discipline d
ON r.Id_Disciplina=d.Id_Disciplina
WHERE d.Nr_ore_plan_disciplina<60;

Select * from ViewEX18_fromLAB4_forLAB8;
```
![image](https://user-images.githubusercontent.com/34598688/50075524-dab6d880-01e7-11e9-8b85-8534270cfda0.png)

2. Sa se scrie cate un exemplu de instructiuni INSERT, UPDATE, DELETE asupra viziunilor create. 
 Sa se adauge comentariile respective referitoare la rezultatele executarii acestor instructiuni. 

``` sql
INSERT INTO dbo.ViewEX18_fromLAB4_forLAB8 (Nume_Profesor, Prenume_Profesor)
VALUES ('Cojanu', 'Irina'); --Nu poate fi executata intrucat contine clauza distinct 

UPDATE dbo.ViewEX18_fromLAB4_forLAB8
SET Prenume_Profesor='Elena'
WHERE Prenume_Profesor='Alexandru';

DELETE FROM dbo.ViewEX18_fromLAB4_forLAB8
WHERE Prenume_Profesor='Diana'; 
```
![image](https://user-images.githubusercontent.com/34598688/50075607-15b90c00-01e8-11e9-937b-30c6a19205b3.png)
![image](https://user-images.githubusercontent.com/34598688/50075654-2e292680-01e8-11e9-9a97-649f8f1823d7.png)
![image](https://user-images.githubusercontent.com/34598688/50075686-4305ba00-01e8-11e9-8788-f1f3087fb7ca.png)

3. Sa se scrie instructiunile SQL care ar modifica viziunile create (in exercitiul 1) in ~a fel, incat sa nu fie posibila modificarea sau ~tergerea tabelelor pe care acestea sunt definite ~i viziunile sa nu accepte operatiuni DML, daca conditiile clauzei WHERE nu sunt satis:facute.

``` sql
ALTER VIEW ViewEX18_fromLAB4_forLAB8
WITH SCHEMABINDING
AS
SELECT DISTINCT profesori.Nume_Profesor, profesori.Prenume_Profesor
FROM profesori  JOIN studenti_reusita 
ON profesori.Id_Profesor=studenti_reusita.Id_Profesor
JOIN discipline
ON studenti_reusita.Id_Disciplina=discipline.Id_Disciplina
WHERE discipline.Nr_ore_plan_disciplina<60
WITH CHECK OPTION;
```
![image](https://user-images.githubusercontent.com/34598688/50075995-19995e00-01e9-11e9-8b79-72a596e2d830.png)
4. Sa se scrie instructiunile de testare a proprietatilor noi definite.

``` sql
SELECT * FROM ViewEX18_fromLAB4_forLAB8;
INSERT INTO ViewEX18_fromLAB4_forLAB8
VALUES ('Popescu', 'Dumitru');
UPDATE ViewEX18_fromLAB4_forLAB8 SET Nume_Profesor='Mocanu'
WHERE Prenume_Profesor = 'Alexandru';
DELETE FROM ViewEX18_fromLAB4_forLAB8 WHERE Prenume_Profesor='Maria';
```
![image](https://user-images.githubusercontent.com/34598688/50076089-5cf3cc80-01e9-11e9-893e-11bc94ba79d3.png)

5. Sa se rescrie 2 interogari formulate in exercitiile din capitolul 4, in ~a fel. incat interogarile imbricate sa fie redate sub forma expresiilor CTE.
``` sql
WITH CTE1 
AS
(select s.Nume_Student, s.Prenume_Student,r.Id_Disciplina --utilizand exemplul 31 din laboratorul 4
from studenti.studenti_reusita r 
join studenti.studenti s on r.Id_Student = s.Id_Student
where r.Nota <= 4
group by Nume_Student,Prenume_Student,Id_Disciplina
having count(r.nota)>2)
SELECT * FROM CTE1;
WITH CTE2
AS
(select s.Nume_Student, s.Prenume_Student, avg(r.Nota) as Media, r.Id_Grupa --utilizand exemplul 32 din laboratorul 4
from studenti s join studenti.studenti_reusita r
on s.Id_Student=r.Id_Student
group by s.Nume_Student, s.Prenume_Student, r.Id_Grupa)
SELECT * FROM CTE2;
```
![image](https://user-images.githubusercontent.com/34598688/50076671-01c2d980-01eb-11e9-89d4-9a3970ca3b40.png)
![image](https://user-images.githubusercontent.com/34598688/50076717-1b642100-01eb-11e9-90db-2492f31abbe5.png)

6. Se considera un graf orientat, precum cel din figura de mai jos ~i fie se dore~te parcursa calea de la nodul id = 3 la nodul unde id = 0. 
Sa se faca reprezentarea grafului orientat in forma de expresie-tabel recursiv. Sa se observe instructiunea de dupa UNION ALL a membrului recursiv,  precum ~i partea de pana la UNION ALL reprezentata de membrul-ancora. 
``` sql
DECLARE @Table1 TABLE
(
ID INT,
pred_ID INT
)

INSERT  @Table1
SELECT 5, NULL UNION ALL
SELECT 4, NULL UNION ALL
SELECT 3, NULL UNION ALL
SELECT 0, 5 UNION ALL
SELECT 2, 4 UNION ALL
SELECT 2, 3 UNION ALL
SELECT 1, 2 UNION ALL
SELECT 0, 1;

WITH recursion
AS 
(
SELECT *, pred_ID as pred_Nod, 0 AS generatie 
FROM @Table1
WHERE pred_ID IS NULL
AND ID=3
UNION ALL
SELECT graph.*, recursion.ID AS predecesor, generatie +1
FROM @Table1 AS graph
INNER JOIN recursion
ON graph.pred_ID=recursion.ID
)
SELECT * FROM recursion
``` 
![image](https://user-images.githubusercontent.com/34598688/50076792-4babbf80-01eb-11e9-9662-6f7226433425.png)
