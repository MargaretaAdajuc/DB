# DB

CREAREA ȘI ÎNTREȚINEREA BAZEI DE DATE

1. Creati o baza de date plasata fizic in mapa MyDocuments\Data, fixand o crestere a fisierului
primar a bazei de 16MB cu limita de crestere de 128 MB si a log-ului de 64 MB cu limita de
crestere de 1024 MB. Pentru fisierele secundare sa se defineasca un Filegroup nou implicit,
setand cresterea fisierelor secundare de 64 MB cu limita de 1024 MB.


2. Creati o baza de date, unde fisierul log sa fie fizic plasat in mapa MyDocuments\Log, numele
fisierului log m mediul sistemului de operare trebuie sa se deosebeasca de cel logic definit in
schema fizica. Este important ca baza de date creata sa fie compatibila cu sistemul MS SQL
Server 2017 si ea sa fie accesibila numai unui singur utiliizator intr-un moment de timp.

3. Creati planul de intretinere a bazei de date, construita m sarcina 1. Spatiul neutilizat de
fisierele bazei de date trebuie indepartat atunci cand el atinge marimea 2000Mb. Spatiul
eliberat trebuie sa fie returnat sistemului de operare. Aceasta operatiune trebuie sa ruleze m
fiecare vineri, la ora 00:00. Raportul executarii planului de intretinere trebuie salvat in dosarul
MyDocuments\SQL_event_logs. lnitializati executarea planului. Dupa executare, verificati
rezultatele in fisierul log.


4. Creati planul de intretinere a bazei de date, construite in exercitiul 2. Numele planului va fi:
,,Reconstruire index". In cadrul acestui plan, sistemul trebuie sa realizeze reconstruirea
indecsilor numai asupra tabelelor de baza (exclusiv viziunilor) din toate schemele care exista
in baza de date in cauza. Spatiul liber pe pagina trebuie sa fie 10%. Sortarea indecsilor trebuie
sa se realizeze in tempdb. Dupa reconstruire, trebuie sa urmeze colectarea statisticilor
complete despre indecsii reconstruiti. Al treilea pas al planului trebuie sa constituie sarcina de
stergere a istoriei despre operatiile de Backup-Restore ce au avut loc pe SQL Server. Trebuie
sters istoricul care este mai vechi de 6 saptamani. Acest plan trebuie sa fie executat in fiecare
prima duminica a lunii. Creati dosarul MyDocuments\SQL_reports. Raportul de executare a
planului trebuie sa fie adaugat in acest fisier. Procesul de mentenanta - sa fie logat in mod
extended. lnitializati executarea planului. Dupa executare, verificati rezultatele in fisierul log
generat.
