# Query-uri pentru lab 4


##### 1: nr de ordine

8. Obțineti identificatorii și numele studenților ai examenelor susținute în anul 2018.
```
select s.Id_Student, s.Nume_Student, sr.Data_Evaluare
from studenti as s inner join studenti_reusita as sr
on s.Id_Student=sr.Id_Student
where Data_Evaluare like '2018-%-%';
```
![Query #1](https://user-images.githubusercontent.com/34598688/46934389-40f47300-d060-11e8-83fd-b7d8917eec3d.png)

##### 2: nr de ordine + 16

24. Să se afișeze lista disciplinelor (Disciplina) predate de cel puțin doi profesori.
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

30. Câți studenți au studiat, deja, disciplina Baze de date în 2018 și 2019 și care este media lor la reușita curentă?
```
select COUNT (sr.Id_Student), avg(Nota)
from  studenti_reusita as sr inner join studenti as s
on sr.Id_Student=s.Id_Student
inner join discipline as d
on d.Id_Disciplina = sr.Id_Disciplina
where Data_Evaluare like '201[89]-%-%';
```
![Query #3](https://user-images.githubusercontent.com/34598688/47269453-6bfe2b80-d566-11e8-912c-4ad6dcde88f2.png)

