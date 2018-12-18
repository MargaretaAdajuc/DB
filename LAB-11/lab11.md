1. Să se creeze un dosar Backup_labll. Să se execute un backup complet al bazei de date universitatea în acest dosar. Fișierul copiei de rezervă să se numească exercitiull.bak. Să se scrie instrucțiunea SQL respectivă.
``` sql
BACKUP DATABASE universitatea  
TO DISK = 'D:\Backup_lab11\exercitiul1.Bak'  
WITH FORMAT 
BACKUP LOG universitatea TO DISK = 'D:\Backup_lab11\exercitiul1.Bak'
```
![image](https://user-images.githubusercontent.com/34598688/50133811-de089d80-0295-11e9-9501-834fefe7c268.png)

2. Să se scrie instrucțiunea unui backup diferențiat al bazei de date universitatea. Fișierul copiei de rezervă să se numească exercitiul2.bak.
``` sql
BACKUP DATABASE universitatea  
TO DISK = 'D:\Backup_lab11\exercitiul2.Bak'  
WITH DIFFERENTIAL 
BACKUP LOG universitatea TO DISK = 'D:\Backup_lab11\exercitiul2.Bak'
``` 
![image](https://user-images.githubusercontent.com/34598688/50133846-05f80100-0296-11e9-9a25-47ece29bc310.png)
3. Să se scrie instrucțiunea unui backup al jurnalului de tranzacții al bazei de date universitatea. Fișierul copiei de rezervă să se numească exercitiul3.bak
``` sql
BACKUP LOG universitatea  
   TO DISK = 'D:\Backup_lab11\exercitiul3.Bak'
``` 
![image](https://user-images.githubusercontent.com/34598688/50133876-38096300-0296-11e9-8d4f-8450c492d383.png)
4. Să se execute restaurarea consecutivă a tuturor copiilor de rezervă create. Recuperarea trebuie să fie realizată într-o bază de date nouă universitatea_lab11. Fișierele bazei de date noi se află în dosarul BD_lab11. Să se scrie instrucțiunile SQL respective.
``` sql
use universitatea1
RESTORE FILELISTONLY  
   FROM DISK = 'D:\Backup_lab11\exercitiul1.Bak';  
RESTORE DATABASE universitatea1  
   FROM DISK = 'D:\Backup_lab11\exercitiul1.Bak'; 
``` 
