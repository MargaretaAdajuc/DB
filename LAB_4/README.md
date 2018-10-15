# Query-uri pentru lab 4


##### 1: nr de ordine

8. Obtineti identificatorii si numele studentilor, ale examenelor sustinute in anul 2018
```
select s.Id_Student, s.Nume_Student, sr.Data_Evaluare
from studenti as s inner join studenti_reusita as sr
on s.Id_Student=sr.Id_Student
where Data_Evaluare like '2018-%-%';
```
![Query #1](https://user-images.githubusercontent.com/34598688/46934389-40f47300-d060-11e8-83fd-b7d8917eec3d.png)

##### 2: nr de ordine + 16

24. Sa se afiseze lista disciplinelor (Disciplina) predate de cel putin doi profesori.
```
select d.Disciplina, count (sr.Id_Profesor)
from discipline as d inner join studenti_reusita as sr
on d.Id_Disciplina=sr.Id_Disciplina
inner join profesori as p 
on p.Id_Profesor=sr.Id_Profesor
group by d.Disciplina
having (count (distinct sr.Id_Profesor)>2);
```
![Query #2](https://user-images.githubusercontent.com/34598688/46936758-10b0d280-d068-11e8-9360-eae3e66cb70a.png)

##### 3: random de la 28 la 39

35. Gasiti denumirile disciplinelor si media notelor pe disciplina. Afisati numai disciplinele cu medii mai mari de 7.0.
```
select d.Disciplina, AVG(sr.Nota)
from discipline as d inner join studenti_reusita as sr
on d.Id_Disciplina=sr.Id_Disciplina
group by Disciplina
having (AVG(sr.Nota)>7);
```
![Query #3](https://user-images.githubusercontent.com/34598688/46937382-cf212700-d069-11e8-9747-9b1d5dcc9002.png)
