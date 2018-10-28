# LAB 5

1. Completați următorul cod pentru a afișa cel mai mare număr dintre cele trei numere prezentate:
```
DECLARE @N1 INT, @N2 INT, @N3 INT;
DECLARE @MAI_MARE INT;
SET @N1 = 60 * RAND();
SET @N2 = 60 * RAND();
SET @N3 = 60 * RAND();
PRINT @N1;
PRINT @N2;
PRINT @N3;
if @N1>@N2 and @N1>@N3 print 'N1 este cel mai mare număr';
else 
	begin
	if @N2>@N1 and @N2>@N3 print 'N2 este cel mai mare număr';
	else print 'N3 este cel mai mare număr';
	end;
	go
```
![Task1](https://user-images.githubusercontent.com/34598688/47621711-7a1eef80-db04-11e8-9c85-3d8f5ad70013.png)

2. Afișati primele zece date (numele, prenumele studentului) în funcție de valoarea notei (cu exceptia notelor 6 și 8) a studentului la primul test al disciplinei Baze de date, folosind structura de altemativă IF...ELSE. Să se folosească variabilele.
```

```


3. Rezolvați aceeși sarcină, 1, apelând la structura selectivă CASE.
```

```


4. Modificați exercițiile din sarcinile 1 și 2 pentru a include procesarea erorilor cu TRY și CATCH și RAISERRROR.
```

```

