# LAB6

1. Să se scrie o instrucțiune T-SQL, care ar popula coloana Adresa_Postala_Profesor din tabelul profesori cu valoarea 'mun. Chisinau', unde adresa este necunoscută.

```
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

```
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
