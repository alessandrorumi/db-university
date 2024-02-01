## Repo
`db-university`

## DB
[[Database/2024-01-31 - DB University|DB University]]

## Todo
Dopo aver testato le vostre query con `phpMyAdmin`, riportatele in un file `txt` o `md` e caricatelo nella vostra repo.

### Query
#### Group by
1. Contare quanti iscritti ci sono stati ogni anno
```sql
SELECT YEAR(enrolment_date), COUNT(*) 'student_number'
FROM students
GROUP BY YEAR(enrolment_date);
```

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
```sql
SELECT office_number, COUNT(*) 'teachers'
FROM teachers
GROUP BY office_number;

-- Oppure
SELECT office_address, COUNT(*) 'teachers'
FROM teachers
GROUP BY office_address;
```

3. Calcolare la media dei voti di ogni appello d'esame (dell'esame vogliamo solo l'`id`)
```sql
SELECT exam_id, FLOOR(AVG(vote)) 'average_vote'
FROM exam_student
GROUP BY exam_id;
```

4. Contare quanti corsi di laurea ci sono per ogni dipartimento
```sql
SELECT department_id, COUNT(*) 'degrees_number'
FROM degrees
GROUP BY department_id;
```


#### Join
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
```sql
SELECT students.name, students.surname, students.enrolment_date, degrees.name
FROM students
  JOIN degrees
    ON students.degree_id = degrees.id
WHERE degrees.name LIKE 'Corso di Laurea in Economia';
```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze
```sql
SELECT *
FROM degrees
  JOIN departments
    ON degrees.department_id = departments.id
WHERE departments.name LIKE 'Dipartimento di Neuroscienze'
AND degrees.level LIKE 'magistrale';

-- Oppure
SELECT degrees.*
FROM degrees
  JOIN departments
    ON degrees.department_id = departments.id
WHERE departments.name LIKE 'Dipartimento di Neuroscienze'
AND degrees.level = 'magistrale';
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
```sql
SELECT *
FROM courses
  LEFT JOIN course_teacher
    ON course_teacher.course_id = courses.id
WHERE course_teacher.teacher_id = 44;
```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
```sql
SELECT students.surname, students.name, degrees.name 'degree_name', departments.name 'department_name'
FROM students
  JOIN degrees
    ON students.degree_id = degrees.id
  JOIN departments
    ON degrees.department_id = departments.id
ORDER BY students.surname;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
```sql
SELECT degrees.name 'degree_name', courses.name 'course_name', teachers.surname 'teacher_surname', teachers.name 'teacher_name',
FROM degrees
  JOIN courses
    ON degrees.id = courses.degree_id
  JOIN course_teacher
    ON courses.id = course_teacher.course_id
  JOIN teachers
    ON course_teacher.teacher_id = teachers.id;
```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)
```sql
SELECT teachers.surname 'teacher_surname', teachers.name 'teacher_name', departments.name 'department_name'
FROM teachers
  JOIN course_teacher
    ON teachers.id = course_teacher.teacher_id
  JOIN courses
    ON course_teacher.course_id = courses.id
  JOIN degrees
    ON courses.degree_id = degrees.id
  JOIN departments
    ON degrees.department_id = departments.id
WHERE departments.name LIKE 'Dipartimento di Matematica';
```


##### Bonus
7. Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.
```sql

```