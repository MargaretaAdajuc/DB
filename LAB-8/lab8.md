1. Sa se creeze doua viziuni in baza interogarilor formulate in doua exercitii indicate din capitolul 4.
Prima viziune sa fie construita in Editorul de interogari, iar a doua, utilizand View Designer.

``` sql
CREATE VIEW ViewEX18_fromLAB4_forLAB8
AS
SELECT DISTINCT p.Nume_Profesor, p.Prenume_Profesor
FROM profesori p JOIN studenti.studenti_reusita r
ON p.Id_Profesor=r.Id_Profesor
JOIN discipline d
ON r.Id_Disciplina=d.Id_Disciplina
WHERE d.Nr_ore_plan_disciplina<60;


Select * from ViewEX18_fromLAB4_forLAB8;

Select * from ViewEX6_fromLAB4_forLAB8;
```

2. Sa se scrie cate un exemplu de instructiuni INSERT, UPDATE, DELETE asupra viziunilor create. 
 Sa se adauge comentariile respective referitoare la rezultatele executarii acestor instructiuni. 

``` sql
INSERT INTO dbo.ViewEX6_fromLAB4_forLAB8 (Nume_Student, Prenume_Student) 
VALUES ('Diacenco', 'Margareta'); --Viziunea este construita pe baza cheii primare id_student, care nu permite valori nule, de aceea obtinem eroare.

UPDATE dbo.ViewEX6_fromLAB4_forLAB8 
SET Prenume_Student='Ion'
WHERE Prenume_Student='Vasile';

DELETE FROM dbo.ViewEX6_fromLAB4_forLAB8
WHERE Prenume_Student='Ivan'; --Stergerea unui tuplu din viziune, necesita stergerea acestuia din tabelul studenti, pe baza caruia acesta este construit, ceea ce nu este posibil.


INSERT INTO dbo.ViewEX18_fromLAB4_forLAB8 (Nume_Profesor, Prenume_Profesor)
VALUES ('Cojanu', 'Irina'); --Nu poate fi executata intrucat contine clauza distinct 

UPDATE dbo.ViewEX18_fromLAB4_forLAB8
SET Prenume_Profesor='Elena'
WHERE Prenume_Profesor='Alexandru';

DELETE FROM dbo.ViewEX18_fromLAB4_forLAB8
WHERE Prenume_Profesor='Diana'; 
```

3. Sa se scrie instructiunile SQL care ar modifica viziunile create (in exercitiul 1) in ~a fel, incat sa nu fie posibila modificarea sau ~tergerea
tabelelor pe care acestea sunt definite ~i viziunile sa nu accepte operatiuni DML, daca conditiile clauzei WHERE nu sunt satis:facute.

``` sql
ALTER VIEW ViewEX6_fromLAB4_forLAB8
WITH SCHEMABINDING
AS
SELECT TOP(5) WITH TIES studenti.studenti.Nume_Student, studenti.studenti.Prenume_Student
FROM  plan_studii.discipline INNER JOIN studenti.studenti_reusita 
ON plan_studii.discipline.Id_Disciplina = studenti.studenti_reusita.Id_Disciplina INNER JOIN studenti.studenti 
ON studenti.studenti_reusita.Id_Student = studenti.studenti.Id_Student
WHERE        (studenti.studenti_reusita.Tip_Evaluare = 'Testul 2') AND (plan_studii.discipline.Disciplina = 'Baze de date')
ORDER BY studenti.studenti_reusita.Nota DESC


ALTER VIEW ViewEX18_fromLAB4_forLAB8
WITH SCHEMABINDING
AS
SELECT DISTINCT cadre_didactice.profesori.Nume_Profesor, cadre_didactice.profesori.Prenume_Profesor
FROM cadre_didactice.profesori  JOIN studenti.studenti_reusita 
ON cadre_didactice.profesori.Id_Profesor=studenti.studenti_reusita.Id_Profesor
JOIN plan_studii.discipline
ON studenti.studenti_reusita.Id_Disciplina=plan_studii.discipline.Id_Disciplina
WHERE plan_studii.discipline.Nr_ore_plan_disciplina<60
WITH CHECK OPTION; 
```
4. Sa se scrie instructiunile de testare a proprietatilor noi definite.

``` sql
SELECT * FROM ViewEX6_fromLAB4_forLAB8;

SELECT * FROM ViewEX18_fromLAB4_forLAB8;

INSERT INTO ViewEX6_fromLAB4_forLAB8
VALUES ('Popescu', 'Dumitru');

INSERT INTO ViewEX18_fromLAB4_forLAB8
VALUES ('Popescu', 'Dumitru');

UPDATE ViewEX6_fromLAB4_forLAB8 SET Nume_Student='Mocanu'
WHERE Prenume_Student = 'Alexandru';

UPDATE ViewEX18_fromLAB4_forLAB8 SET Nume_Profesor='Mocanu'
WHERE Prenume_Profesor = 'Alexandru';

DELETE FROM ViewEX6_fromLAB4_forLAB8 WHERE Prenume_Student='Maria';

DELETE FROM ViewEX18_fromLAB4_forLAB8 WHERE Prenume_Profesor='Maria';
```

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

