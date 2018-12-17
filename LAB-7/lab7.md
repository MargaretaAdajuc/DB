# LAB7

1. Creați o diagramă a bazei de date, folosind forma de vizualizare standard, structura căreia este descrisă la începutul sarcinilor practice din capitolul 4.

![task1](https://user-images.githubusercontent.com/34598688/48687415-f8355a00-ebc9-11e8-8075-de82018f1a42.png)

2. Să se adauge constrângeri referențiale (legate cu tabelele studenti și profesori) necesare coloanelor Sef_grupa și Prof_Indrumator (sarcina3, capitolul 6) din tabelul grupe.
![image](https://user-images.githubusercontent.com/34598688/50074466-b60d3180-01e4-11e9-8e83-521f32e2cde6.png)
3 La diagrama construitii, sa se adauge ~i tabelul orarul definit in capitolul 6 al acestei lucrari:
tabelul orarul contine identificatorul disciplinei (ld_Disciplina), identificatorul profesorului
(Id_Profesor) ~i blocul de studii (Bloc). Cheia tabelului este constituita din trei cfunpuri:
identificatorul grupei (Id_ Grupa), ziua lectiei (Z1), ora de inceput a lectiei (Ora), sala unde
are loc lectia (Auditoriu).
![image](https://user-images.githubusercontent.com/34598688/50074845-cffb4400-01e5-11e9-8ed0-44f0bd587759.png)


4. Tabelul orarul trebuie sa contina ~i 2 chei secundare: (Zi, Ora, Id_Grupa, Id_Profesor) ~i
(Zi, Ora, ld_Grupa, ld_Disciplina).
![image](https://user-images.githubusercontent.com/34598688/50074983-48620500-01e6-11e9-9f15-566489668c35.png)
![image](https://user-images.githubusercontent.com/34598688/50075038-734c5900-01e6-11e9-8167-2ed660829b3c.png)
5. in diagrama, de asemenea, trebuie sa se defineasca constrangerile referentiale (FK-PK) ale
atributelor ld_Disciplina, ld_Profesor, Id_ Grupa din tabelului orarul cu atributele tabelelor
respective.
![image](https://user-images.githubusercontent.com/34598688/50075107-a42c8e00-01e6-11e9-9efe-f36f74ac1a3a.png)
6. Creati, in baza de date universitatea, trei scheme noi: cadre_didactice, plan_studii ~i studenti.
Transferati tabelul profesori din schema dbo in schema cadre didactice, tinand cont de
dependentelor definite asupra tabelului mentionat. in acela~i mod ~ se trateze tabelele orarul,
discipline care apartin schemei plan_studii ~i tabelele studenti, studenti_reusita, care apartin
schemei studenti. Se scrie instructiunile SQL respective.

```sql
USE universitatea;
GO
ALTER SCHEMA cadre_didactice TRANSFER dbo.profesori; 
GO

USE universitatea; 
GO 
ALTER SCHEMA plan_studii TRANSFER dbo.discipline; 
GO 

USE universitatea; 
GO 
ALTER SCHEMA studenti TRANSFER dbo.studenti; 
GO  

USE universitatea; 
GO 
ALTER SCHEMA studenti TRANSFER dbo.studenti_reusita; 
GO 

SELECT * from cadre_didactice.profesori;
SELECT * from plan_studii.discipline;
SELECT * from studenti.studenti;
SELECT * from studenti.studenti_reusita;
```
<!---nu imi da permisiune--->
7. Modificati 2-3 interogari asupra bazei de date universitatea prezentate in capitolul 4 astfel ca
numele tabelelor accesate sa fie descrise in mod explicit, tinand cont de faptul ca tabelele au
fost mutate in scheme noi.

<!---nu imi da permisiune--->

8. Creati sinonimele respective pentru a simplifica interogarile construite in exercitiul precedent
~i reformulati interogarile, folosind sinonimele create.
``` sql
CREATE SYNONYM discipline1  
FOR discipline; 
GO  
CREATE SYNONYM reusita  
FOR studenti_reusita; 
GO

CREATE SYNONYM studenti1  
FOR studenti; 
GO  

CREATE SYNONYM profesori1 
FOR profesori; 
GO  

select distinct d.Disciplina, p.Nume_Profesor, p.Prenume_Profesor
from discipline1 as d join reusita as r
on d.Id_Disciplina=r.Id_Disciplina
join profesori1 as p
on r.Id_Profesor=p.Id_Profesor
order by p.Nume_Profesor desc, p.Prenume_Profesor desc;

select disciplina
from discipline1
where len(disciplina)>20;

select nume_student, prenume_student
from studenti1
where Nume_Student like '%u';
``` 
![image](https://user-images.githubusercontent.com/34598688/50075367-6c721600-01e7-11e9-81b6-4620f81ca083.png)
![image](https://user-images.githubusercontent.com/34598688/50075395-86abf400-01e7-11e9-9ae6-ee96ae50b575.png)
![image](https://user-images.githubusercontent.com/34598688/50075426-a3e0c280-01e7-11e9-8ac0-5a5a1f1480c5.png)
