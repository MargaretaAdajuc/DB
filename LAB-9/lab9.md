1. Sa se creeze proceduri stocate in baza exercitiilor (2 exercitii) din capitolul 4. 
--Parametrii de intrare trebuie sa corespunda criteriilor din clauzele WHERE ale exercitiilor respective .
``` sql
create procedure lab9_ex1_13
					@nume varchar(20)='Florea',
					@prenume varchar (20)='Ioan'

as 
select distinct d.Disciplina
from discipline d join studenti.studenti_reusita r
on d.Id_Disciplina=r.Id_Disciplina
join studenti s
on r.Id_Student=s.Id_Student
where s.Nume_Student=@nume and s.Prenume_Student=@prenume;

create procedure lab9_ex1_23
				@tip_evaluare varchar(20)='Examen',
				@average_grade int=7
as
select distinct d.Disciplina 
from discipline as d join studenti.studenti_reusita as r
on d.Id_Disciplina=r.Id_Disciplina
join
(select Id_Student, avg(nota) as average_grade from studenti.studenti_reusita where Tip_Evaluare=@tip_evaluare group by id_student) as t
on r.Id_student=t.Id_Student
where t.average_grade>@average_grade
order by d.disciplina desc;
```

2. Sa se creeze o procedura stocata, care nu are niciun parametru de intrare ~i poseda un parametru de ie~ire. 
Parametrul de ie~ire trebuie sa returneze numarul de studenti, care nu au sustinut eel putin o forma de evaluare (nota mai mica de 5 sau valoare NULL). 
```sql
CREATE PROCEDURE lab9_ex2
   @count_students INT = NULL OUTPUT
AS
SELECT @count_students =  COUNT(DISTINCT id_student) 
FROM studenti.studenti_reusita
WHERE Nota < 5 or Nota is NULL

--afisarea rezultatului

DECLARE @count_students INT
EXEC lab9_ex2 @count_students OUTPUT
PRINT 'Nr de studenti ce nu au sustinut cel putin o forma de evaluare = ' + cast(@count_students as VARCHAR(3))
```
3. Sa se creeze o procedura stocata, care ar insera in baza de date informatii despre un student nou. 
In calitate de parametri de intrare sa serveasca datele personale ale studentului nou ~i Cod_ Grupa. 
Sa se genereze toate intrarile-cheie necesare in tabelul studenti_reusita. Notele de evaluare sa fie inserate ca NULL.
```sql
select * from grupe;

CREATE PROCEDURE lab9_ex3 
@nume VARCHAR(50),
@prenume VARCHAR(50),
@data DATE,
@adresa VARCHAR(500),
@cod_grupa CHAR(6)

AS
INSERT INTO studenti 
VALUES (175, @nume, @prenume, @data, @adresa)
INSERT INTO studenti.studenti_reusita
VALUES (175, 100, 100 , (SELECT Id_Grupa FROM grupe WHERE Cod_Grupa = @cod_grupa), 'Testul 1', NULL, '2018-12-05')

Exec lab9_ex3 @nume='Lungu', @prenume='Mariana', @data='1998-12-08', @adresa= 'mun. Chsinau, str. Paris, 38/2', @cod_grupa='TI171';

Select * from studenti;

Select * from profesori;

Select * from studenti.studenti_reusita where Id_Profesor=101;

select * from discipline;
```

4. Fie ca un profesor se elibereaza din functie la mijlocul semestrului. 
Sa se creeze o procedura stocata care ar reatribui inregistrarile din tabelul studenti_reusita unui alt profesor. 
Parametri de intrare: numele ~i prenumele profesorului vechi, numele ~i prenumele profesorului nou, disciplina. 
In cazul in care datele inserate sunt incorecte sau incomplete, sa se afi~eze un mesaj de avertizare. 
```sql
CREATE PROCEDURE lab9_ex4
@nume_vechi VARCHAR(20),
@prenume_vechi VARCHAR(20),
@nume_nou VARCHAR(20),
@prenume_nou VARCHAR(20),
@disciplina VARCHAR(50)

AS


IF(
	( SELECT discipline.Id_Disciplina 
	  FROM discipline 
	  WHERE Disciplina = @disciplina) IN ( SELECT DISTINCT studenti.studenti_reusita.Id_Disciplina 
										   FROM studenti.studenti_reusita
										   WHERE Id_Profesor = (SELECT cadre_didactice.profesori.Id_Profesor 
																FROM cadre_didactice.profesori 
																WHERE Nume_Profesor = @nume_vechi AND Prenume_Profesor = @prenume_vechi)
										  )
	)
BEGIN

UPDATE studenti.studenti_reusita
SET Id_Profesor =  (SELECT Id_Profesor
			      FROM cadre_didactice.profesori
				  WHERE Nume_Profesor = @nume_nou AND   Prenume_Profesor = @prenume_nou)

WHERE Id_Profesor = (SELECT Id_profesor
					 FROM cadre_didactice.profesori
     			     WHERE Nume_Profesor = @nume_vechi AND Prenume_Profesor = @prenume_vechi)

END

ELSE

BEGIN
  
  PRINT 'The values you entered do not meet the data type requirements'

END

EXEC lab9_ex4 'Micu', 'Elena', 'Micu', 'George', 'Baze de date';
```
5. Sa se creeze o procedura stocata care ar forma o lista cu primii 3 cei mai buni studenti la o disciplina, 
si acestor studenti sa le fie marita nota la examenul final cu un punct (nota maximala posibila este 10). 
In calitate de parametru de intrare, va servi denumirea disciplinei. 
Procedura sa returneze urmatoarele campuri: Cod_ Grupa, Nume_Prenume_Student, Disciplina, Nota _ Veche, Nota _ Noua. 
```sql
CREATE PROCEDURE lab9_ex5
@discipline VARCHAR(50)
AS
DECLARE @best TABLE (Id_Student int, Media float)
INSERT INTO @best
	SELECT TOP (3) studenti.studenti_reusita.Id_Student, AVG(cast (Nota as float)) as Media
	FROM studenti.studenti_reusita, discipline
	WHERE discipline.Id_Disciplina = studenti.studenti_reusita.Id_Disciplina
	AND discipline.Disciplina = @discipline
	GROUP BY studenti.studenti_reusita.Id_Student
	ORDER BY Media desc		

SELECT cod_grupa, nume_student+' '+Prenume_Student as Nume, disciplina, nota AS Nota_Veche, iif(nota > 9, 10, nota + 1) AS Nota_Noua 
    FROM studenti.studenti_reusita, discipline, grupe, studenti
	WHERE discipline.id_disciplina = studenti.studenti_reusita.id_disciplina
	AND grupe.Id_Grupa = studenti.studenti_reusita.Id_Grupa
	AND  studenti.Id_Student = studenti.studenti_reusita.Id_Student
	AND studenti.Id_Student in (select Id_Student from @best)
	AND Disciplina = @discipline
	AND Tip_Evaluare = 'Examen'

UPDATE studenti.studenti_reusita
SET studenti.studenti_reusita.Nota = (CASE WHEN nota >= 9 THEN 10 ELSE nota + 1 END)
WHERE Tip_Evaluare = 'Examen'
AND Id_Disciplina = (Select Id_Disciplina from discipline where disciplina=@discipline)
AND Id_Student in (select Id_Student from @best)

exec lab9_ex5 @discipline = 'Baze de date'
```
6. Sa se creeze functii definite de utilizator in baza exercitiilor (2 exercitii) din capitolul 4. 
Parametrii de intrare trebuie sa corespunda criteriilor din clauzele WHERE ale exercitiilor respective. 

``` sql
CREATE FUNCTION lab9_ex6_11 (@disciplina VARCHAR(20), @data_like varchar(20), @nota SMALLINT)
RETURNS TABLE
AS
RETURN
(SELECT distinct profesori.Nume_Profesor ,profesori.Prenume_Profesor
FROM studenti.studenti_reusita join profesori
on studenti.studenti_reusita.Id_Profesor=profesori.Id_Profesor
 join discipline
on studenti.studenti_reusita.Id_Disciplina=discipline.Id_Disciplina
where discipline.Disciplina=@disciplina and studenti.studenti_reusita.Data_Evaluare like @data_like and studenti.studenti_reusita.Nota<@nota)

create function lab9_ex6_30 (@disciplina varchar(20), @tip_evaluare varchar(20), @data1 varchar(20), @data2 varchar(20))
returns table
as
return
(select count(x.Id_Student) "No of students", avg(x.reusita_curenta) "Media curenta totala"
from
(select distinct r.Id_Student, t.reusita_curenta
from studenti.studenti_reusita r join discipline d
on r.Id_Disciplina=d.Id_Disciplina
join (select r.id_student, avg(r.nota) as reusita_curenta
	  from studenti.studenti_reusita r
	  where r.Tip_Evaluare=@tip_evaluare
	  group by r.Id_Student) as t
on r.Id_Student=t.Id_Student
where d.Disciplina=@disciplina and r.Data_Evaluare like @data1 and r.Data_Evaluare like @data2) as x)
```
7. Sa se scrie functia care ar calcula varsta studentului. Sa se defineasca urmatorul format al functiei: <nume_functie>(<Data_Nastere_Student>).
```sql
CREATE FUNCTION lab9_ex7 (@data_nasterii DATE )
RETURNS INT
 BEGIN
 DECLARE @varsta INT
 SELECT @varsta = (SELECT (YEAR(GETDATE()) -
							 YEAR(@data_nasterii) - 
								CASE WHEN (MONTH(@data_nasterii) > MONTH(GETDATE())) OR
											 (MONTH(@data_nasterii) = MONTH(GETDATE()) AND DAY(@data_nasterii)> DAY(GETDATE()))
								THEN  1
								ELSE  0
								 END))
 RETURN @varsta
 END
```
8. Sa se creeze o functie definita de utilizator, care ar returna datele referitoare la reusita unui student. 
Se define~te urmatorul format al functiei: <nume_functie> (<Nume_Prenume_Student>). 
Sa fie afi~at tabelul cu urmatoarele campuri: Nume_Prenume_Student, Disciplina, Nota, Data_Evaluare.
``` sql
CREATE FUNCTION lab9_ex8 (@nume_prenume_s VARCHAR(50))
RETURNS TABLE 
AS
RETURN
(SELECT Nume_Student + ' ' + Prenume_Student as Student, Disciplina, Nota, Data_Evaluare
 FROM studenti, discipline, studenti.studenti_reusita
 WHERE studenti.Id_Student = studenti.studenti_reusita.Id_Student
 AND discipline.Id_Disciplina = studenti.studenti_reusita.Id_Disciplina 
 AND Nume_Student + ' ' + Prenume_Student = @nume_prenume_s )
```
9. Se cere realizarea unei functii definite de utilizator, care ar gasi cel mai sarguincios sau cel mai slab student dintr-o grupa. 
Se define~te urmatorul format al functiei: <nume_functie> (<Cod_ Grupa>, <is_good>). 
Parametrul <is_good> poate accepta valorile "sarguincios" sau "slab", respectiv. 
Functia sa returneze un tabel cu urmatoarele campuri Grupa, Nume_Prenume_Student, Nota Medie , is_good. Nota Medie sa fie cu precizie de 2 zecimale.
``` sql
CREATE FUNCTION lab9_ex9 (@cod_grupa VARCHAR(10), @is_good VARCHAR(20))
RETURNS @Test Table (Cod_Grupa varchar(10), Student varchar (100), Media decimal(4,2), Reusita varchar(20))
AS
begin

if @is_good = 'sarguincios'
begin
insert into @Test

SELECT top (1) Cod_Grupa, Nume_Student + ' ' + Prenume_Student as Student,
		 CAST(AVG( Nota * 1.0) as decimal (4,2)) as Media, @is_good
 FROM grupe,studenti, studenti.studenti_reusita
 WHERE grupe.Id_Grupa = studenti.studenti_reusita.Id_Grupa
 AND studenti.Id_Student = studenti.studenti_reusita.Id_Student
 AND Cod_Grupa = @cod_grupa
 GROUP BY Cod_Grupa, Nume_Student, Prenume_Student
 Order by Media desc
 end

 else

 begin 
 insert into @Test
SELECT top (1) Cod_Grupa, Nume_Student + ' ' + Prenume_Student as Student,
		 CAST(AVG( Nota * 1.0) as decimal (4,2)) as Media, @is_good
 FROM grupe,studenti, studenti.studenti_reusita
 WHERE grupe.Id_Grupa = studenti.studenti_reusita.Id_Grupa
 AND studenti.Id_Student = studenti.studenti_reusita.Id_Student
 AND Cod_Grupa = @cod_grupa
 GROUP BY Cod_Grupa, Nume_Student, Prenume_Student
 Order by Media 
 end

 RETURN 
 end
 ```
