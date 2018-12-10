# LAB6

1. Să se scrie o instrucțiune T-SQL, care ar popula coloana Adresa_Postala_Profesor din tabelul profesori cu valoarea 'mun. Chisinau', unde adresa este necunoscută.

```sql
update profesori 
set Adresa_Postala_Profesor = 'mun.Chisinau'
where Adresa_Postala_Profesor IS NULL;

SELECT Nume_Profesor, Prenume_Profesor, Adresa_Postala_Profesor
FROM profesori;
```
![Ex_1](https://user-images.githubusercontent.com/34598688/49708970-c3517b80-fc3a-11e8-8673-fcb7682299fe.png)

2. Să se modifice schema tabelului grupe, ca să corespunda următoarelor cerințe:  
a) Câmpul Cod_ Grupa să accepte numai valorile unice și să nu accepte valori necunoscute.  
b) Să se țină cont că cheia primară, deja, este definită asupra coloanei Id_Grupa.  

```sql
alter table grupe 
add unique (Cod_Grupa)
alter table grupe 
alter column Cod_Grupa char(6) NOT NULL;

select Cod_Grupa
from grupe
```
![Ex_2](https://user-images.githubusercontent.com/34598688/49709012-f267ed00-fc3a-11e8-8141-ea291a99f4a2.png)

3. La tabelul grupe, să se adauge 2 coloane noi Sef_grupa și Prof_Indrumator, ambele de tip INT. Să se populeze câmpurile nou-create cu cele mai potrivite candidaturi în baza criteriilor de mai jos:  
a) Șeful grupei trebuie să aiba cea mai bună reușită (medie) din grupă la toate formele de evaluare și la toate disciplinele. Un student nu poate fi șef de grupă la mai multe grupe.  
b) Profesorul îndrumător trebuie să predea un număr maximal posibil de discipline la grupa dată. Dacă nu există o singură candidatură, care corespunde primei cerințe, atunci este ales din grupul de candidați acel cu identificatorul (Id_Profesor) minimal. Un profesor nu
poate fi îndrumator la mai multe grupe.  
c) Să se scrie instrucțiunile ALTER, SELECT, UPDATE necesare pentru crearea coloanelor în tabelul grupe, pentru selectarea candidatilor și inserarea datelor.  

```sql
ALTER TABLE grupe
ADD Sef_Grupa INT

DECLARE @nr_de_grupe INT = (select count (Id_Grupa) from grupe)
DECLARE @index INT = 1
DECLARE @sef_grupa INT

WHILE (@index <= @nr_de_grupe)
BEGIN 
UPDATE grupe

set Sef_Grupa = (select top(1) Id_Student from
(select Id_Student, avg(Nota) as nrNota        
from studenti_reusita where Id_Grupa = @index group by Id_Student) as q1
order by q1.nrNota desc),

Prof_Indrumator = (select top(1) Id_Profesor from
(select Id_Profesor, count (distinct Id_Disciplina) as nr_Disciplina
from studenti_reusita where Id_Grupa = @index group by Id_Profesor) as q2
order by q2.nr_Disciplina desc) where Id_Grupa = @index;

set @index += 1;
END

select *
from grupe
```

![Ex_3](https://user-images.githubusercontent.com/34598688/49717237-09b8d180-fc5f-11e8-8ade-67bd7f8956c5.png)

4. Să se scrie o instrucțiune T-SQL, care ar mări toate notele de evaluare șefilor de grupe cu un punct. Nota maximală (10) nu poate fi mărită. 

```sql

```

