1. Sa se modifice declan~atorul inregistrare_ noua, in a§a fel, incat in cazul actualizarii
auditoriului sa apara mesajul de informare, care, in afara de disciplina ~i ora, va afi~a codul
grupei afectate, ziua, blocul, auditoriul vechi ~i auditoriul nou.
``` sql
use universitatea

DROP TRIGGER IF EXISTS Inregistrare_noua 
GO
CREATE TRIGGER Inregistrare_noua ON orarul
AFTER UPDATE
AS SET NOCOUNT ON
IF UPDATE(Auditoriu)
SELECT 'Disciplina ' + UPPER(discipline.Disciplina)+ 
		'Grupa: ' + grupe.Cod_Grupa +
		'Ziua: ' + CAST(inserted.Zi as VARCHAR(5)) + 
		'Ora ' + CAST(inserted.Ora as VARCHAR(5)) + 
		'Auditoriul nou: ' + CAST(inserted.Auditoriu as VARCHAR(5)) + CAST(inserted.Bloc as VARCHAR(5)) +
		'Auditoriul vechi: ' + CAST(deleted.Auditoriu as VARCHAR(5))
FROM inserted,deleted, discipline, grupe
WHERE deleted.Id_Disciplina = discipline.Id_Disciplina
AND inserted.Id_Grupa = grupe.Id_Grupa


GO
UPDATE orarul
SET Auditoriu = 200
WHERE Id_Grupa = 1 AND Id_Profesor = 117;
```
![image](https://user-images.githubusercontent.com/34598688/50133088-c380f500-0292-11e9-8e38-a2c706fb2374.png)
2. Sa se creeze declan~atorul, care ar asigura popularea corecta (consecutiva) a tabelelor studenti
~i studenti_reusita, ~i ar permite evitarea erorilor la nivelul cheilor exteme.
``` sql

CREATE TRIGGER EX ON studenti_reusita
INSTEAD OF INSERT
AS SET NOCOUNT ON
   
  INSERT INTO studenti_reusita 
  SELECT * FROM inserted
  WHERE Id_Student in (SELECT Id_Student FROM studenti)
  

  INSERT INTO studenti values (667,'Nume', 'Prenume', '1998-06-07', 'MD-2022')
  INSERT INTO studenti_reusita values (667, 101, 101, 1, 'Examen', null, null)

 
  select * from studenti where Id_Student= 667
  select * from studenti_reusita where Id_Student = 667
```
![image](https://user-images.githubusercontent.com/34598688/50133174-1f4b7e00-0293-11e9-85d9-eaf54d4498a4.png)
3. Sa se creeze un declan~ator, care ar interzice mic~orarea notelor in tabelul studenti_reusita ~i
modificarea valorilor campului Data_Evaluare, unde valorile acestui camp sunt nenule. Declan~atorul trebuie sa se lanseze, numai daca sunt afectate datele studentilor din grupa
,,CIB171". Se va afi~a un mesaj de avertizare in cazul tentativei de a incalca constrangerea.
``` sql
CREATE TRIGGER EX3 ON studenti_reusita
AFTER UPDATE
AS
SET NOCOUNT ON

IF UPDATE (Nota)
DECLARE @CIB_ID INT = (SELECT Id_Grupa  FROM grupe WHERE Cod_Grupa = 'CIB171')

DECLARE @count int = (SELECT count(*) FROM deleted, inserted 
			WHERE deleted.Id_Disciplina = inserted.Id_Disciplina AND deleted.Id_Grupa = inserted.Id_Grupa AND deleted.Id_Profesor = inserted.Id_Profesor AND deleted.Tip_Evaluare = inserted.Tip_Evaluare AND deleted.Id_Student = inserted.Id_Student
			AND inserted.Nota < deleted.Nota 
			AND inserted.Id_Grupa = @CIB_ID)
	
BEGIN
	IF (@count > 0 )
	PRINT ('Cant Lower Grades For CIB 171')
	ROLLBACK TRANSACTION
END

IF UPDATE(Data_Evaluare)
		SET @count = (SELECT count(*) FROM deleted 
		WHERE Data_Evaluare IS NOT NULL AND Id_Grupa = @CIB_ID)

		IF @count > 0
		BEGIN
			PRINT ('Cant Modify Data Evaluare')
			ROLLBACK TRANSACTION
		END


GO

DECLARE @CIB_ID INT = (SELECT Id_Grupa FROM grupe WHERE Cod_Grupa = 'CIB171')

UPDATE studenti_reusita
SET Data_Evaluare = '2018-01-01'
WHERE Id_Grupa = @CIB_ID; 

GO

DECLARE @CIB_ID INT = (SELECT Id_Grupa FROM grupe WHERE Cod_Grupa = 'CIB171')

UPDATE studenti_reusita
SET Nota = 8
WHERE Nota = 9 AND Id_Grupa = @CIB_ID;

```

4. Sa se creeze un declan~ator DDL care ar interzice modificarea coloanei ld_Disciplina in
tabelele bazei de date universitatea cu afi~area mesajului respectiv.
``` sql
IF OBJECT_ID('EX4', 'TR') is not null
   DROP TRIGGER EX4
GO
CREATE TRIGGER EX4 ON DATABASE
FOR ALTER_TABLE
AS 
SET NOCOUNT ON
DECLARE @ID_DISCPLINA varchar(60)
SET @ID_DISCPLINA = EVENTDATA().value('(/EVENT_INSTANCE/AlterTableActionList/*/Columns/Name)[1]', 'nvarchar(100)') 
IF @ID_DISCPLINA ='Id_Disciplina'
BEGIN 
PRINT ('Cant ALTER Id_Disciplina')
ROLLBACK;
END
```

5. Sa se creeze un declan~ator DDL care ar interzice modificarea schemei bazei de date in afara
orelor de lucru. sql
``` sql
DROP TRIGGER IF EXISTS EX5

CREATE TRIGGER EX5
ON DATABASE

FOR ALTER_TABLE
AS
SET NOCOUNT ON
DECLARE @START TIME
DECLARE @END TIME
DECLARE @CURRENT_TIME TIME

SET @CURRENT_TIME = CONVERT(Time, GETDATE())
SET @START = '8:00:00'
SET @END = '17:00:00'

IF (@CURRENT_TIME < @START) OR (@CURRENT_TIME > @END)
BEGIN	
PRINT ' STILL AT WORK, CANT ALTER DATABASE '
ROLLBACK
END


CREATE TABLE test_ex_5 (
    column1 INT,
    column2 INT,
); 


ALTER TABLE test_ex_5 ALTER COLUMN column1 varchar(1230);
```
6. Sa se creeze un declan~ator DDL care, la modificarea proprietatilor coloanei ld_Profesor
dintr-un tabel, ar face schimbari asemanatoare in mod automat in restul tabelelor.
``` sql
CREATE TRIGGER e6
ON DATABASE
FOR ALTER_TABLE
AS
SET NOCOUNT ON

DECLARE @COMANDA varchar(500)
DECLARE @ID_PROFESOR varchar (20)
DECLARE @TABELUL varchar (50)
DECLARE @COMANDA_NOUA varchar(500)

SELECT @ID_PROFESOR = EVENTDATA().value('(/EVENT_INSTANCE/AlterTableActionList/*/Columns/Name)[1]', 'nvarchar(max)')
IF @ID_PROFESOR = 'Id_Profesor'
BEGIN

SELECT @COMANDA = EVENTDATA().value ('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]', 'nvarchar(max)')
SELECT @TABELUL = EVENTDATA().value ('(/EVENT_INSTANCE/ObjectName)[1]','nvarchar(max)')
SELECT @COMANDA_NOUA = REPLACE(@ID_PROFESOR, @TABELUL, 'studenti_reusita');
EXECUTE (@COMANDA_NOUA)
SELECT @COMANDA_NOUA = REPLACE(@ID_PROFESOR, @TABELUL, 'profesori');
EXECUTE (@COMANDA_NOUA)
SELECT @COMANDA_NOUA = REPLACE(@ID_PROFESOR, @TABELUL, 'orarul');
EXECUTE (@COMANDA_NOUA)

PRINT 'Data Modified Across All Occurences'
END

ALTER TABLE cadre_didactice.profesori ALTER COLUMN Id_Profesor SMALLINT
```
