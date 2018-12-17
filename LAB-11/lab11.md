1. Sa se creeze un dosar Backup_labll. Sa se execute un backup complet al bazei de date
universitatea in acest dosar. Fi~ierul copiei de rezerva sa se numeasca exercitiull.bak. Sa se
scrie instructiunea SQL respectiva.
``` sql
BACKUP DATABASE universitatea  
TO DISK = 'D:\Backup_lab11\exercitiul1.Bak'  
WITH FORMAT 
BACKUP LOG universitatea TO DISK = 'C:\Users\ritad\Documents\DB\Backup_lab11\exercitiul1.Bak'
```
2 Sa se scrie instructiunea unui backup diferentiat al bazei de date universitatea. Fi~ierul copiei
de rezerva sa se numeasca exercitiul2.bak.
``` sql
BACKUP DATABASE universitatea  
TO DISK = 'D:\Backup_lab11\exercitiul2.Bak'  
WITH DIFFERENTIAL 
BACKUP LOG universitatea TO DISK = 'C:\Users\ritad\Documents\DB\Backup_lab11\exercitiul2.Bak'
``` 
3 Sa se scrie instructiunea unui backup al jurnalului de tranzactii al bazei de date universitatea.
Fi~ierul copiei de rezerva sa se numeasca exercitiul3.bak
``` sql
BACKUP LOG universitatea  
   TO DISK = 'C:\Users\ritad\Documents\DB\Backup_lab11\exercitiul3.Bak'
``` 
4 Sa se execute restaurarea consecutiva a tuturor copiilor de rezerva create. Recuperarea trebuie
sa fie realizata intr-o baza de date noua universitatea_labll. Fi~ierele bazei de date noise afla
in dosarul BD_labll. Sa se scrie instructiunile SQL respective
``` sql
use universitatea_1
RESTORE FILELISTONLY  
   FROM DISK = 'C:\Users\ritad\Documents\DB\Backup_lab11\exercitiul1.Bak';  
RESTORE DATABASE universitatea1  
   FROM DISK = 'C:\Users\ritad\Documents\DB\Backup_lab11\exercitiul1.Bak';  
``` 
