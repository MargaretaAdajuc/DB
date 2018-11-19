# LAB7

1. Creați o diagramă a bazei de date, folosind forma de vizualizare standard, structura căreia este descrisă la începutul sarcinilor practice din capitolul 4.

![task1](https://user-images.githubusercontent.com/34598688/48687415-f8355a00-ebc9-11e8-8075-de82018f1a42.png)

2. Să se adauge constrângeri referențiale (legate cu tabelele studenti și profesori) necesare coloanelor Sef_grupa și Prof_Indrumator (sarcina3, capitolul 6) din tabelul grupe.



3 La diagrama construitii, sa se adauge ~i tabelul orarul definit in capitolul 6 al acestei lucrari:
tabelul orarul contine identificatorul disciplinei (ld_Disciplina), identificatorul profesorului
(Id_Profesor) ~i blocul de studii (Bloc). Cheia tabelului este constituita din trei cfunpuri:
identificatorul grupei (Id_ Grupa), ziua lectiei (Z1), ora de inceput a lectiei (Ora), sala unde
are loc lectia (Auditoriu).



4. Tabelul orarul trebuie sa contina ~i 2 chei secundare: (Zi, Ora, Id_Grupa, Id_Profesor) ~i
(Zi, Ora, ld_Grupa, ld_Disciplina).


5. in diagrama, de asemenea, trebuie sa se defineasca constrangerile referentiale (FK-PK) ale
atributelor ld_Disciplina, ld_Profesor, Id_ Grupa din tabelului orarul cu atributele tabelelor
respective.


6. Creati, in baza de date universitatea, trei scheme noi: cadre_didactice, plan_studii ~i studenti.
Transferati tabelul profesori din schema dbo in schema cadre didactice, tinand cont de
dependentelor definite asupra tabelului mentionat. in acela~i mod ~ se trateze tabelele orarul,
discipline care apartin schemei plan_studii ~i tabelele studenti, studenti_reusita, care apartin
schemei studenti. Se scrie instructiunile SQL respective.


7. Modificati 2-3 interogari asupra bazei de date universitatea prezentate in capitolul 4 astfel ca
numele tabelelor accesate sa fie descrise in mod explicit, tinand cont de faptul ca tabelele au
fost mutate in scheme noi.



8. Creati sinonimele respective pentru a simplifica interogarile construite in exercitiul precedent
~i reformulati interogarile, folosind sinonimele create.
