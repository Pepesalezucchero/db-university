### Query

#### Group by

1. Contare quanti iscritti ci sono stati ogni anno

```
SELECT YEAR(enrolment_date), COUNT(*) 'students_sign_in' FROM students GROUP BY YEAR(enrolment_date);

```

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```
SELECT office_address , COUNT(*) 'number_of_teachers_inside' FROM teachers GROUP BY office_address;

```

3. Calcolare la media dei voti di ogni appello d'esame (dell'esame vogliamo solo l'`id`)

```
SELECT exam_id, FLOOR(AVG(vote)) AS 'average_vote' FROM exam_student GROUP BY exam_id;

```

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

```
SELECT department_id, COUNT(*) 'nbr_of_courses' FROM degrees GROUP BY department_id;

```

#### Join

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```
SELECT students.name AS 'signed_in_Corso di Laurea in Economia' FROM students JOIN degrees ON students.degree_id = degrees.id WHERE degrees.name LIKE 'Corso di Laurea in Economia';

```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

```
SELECT degrees.* FROM degrees JOIN departments ON degrees.department_id = departments.id WHERE degrees.level LIKE 'magistrale' AND departments.name LIKE 'Dipartimento di Neuroscienze';

```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```
SELECT * FROM courses JOIN course_teacher ON course_teacher.course_id = courses.id WHERE course_teacher.teacher_id = 44;

```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

```
SELECT students.*, degrees.name 'degree_name', departments.name 'department_name' FROM students JOIN degrees ON students.degree_id = degrees.id JOIN departments ON degrees.department_id = departments.id ORDER BY students.surname, students.name;

(al contrario)

SELECT students.*, degrees.name 'degree_name', departments.name 'department_name' FROM students JOIN degrees ON students.degree_id = degrees.id JOIN departments ON degrees.department_id = departments.id ORDER BY students.name, students.surname;

```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```
SELECT degrees.name 'degree_name', courses.name 'course_name', teachers.surname 'teacher_surname', teachers.name 'teacher_name' FROM degrees JOIN courses ON degrees.id = courses.degree_id JOIN course_teacher ON courses.id = course_teacher.course_id JOIN teachers ON course_teacher.teacher_id = teachers.id;

```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

```
SELECT DISTINCT teachers.*, departments.name 'department_name' FROM teachers JOIN course_teacher ON teachers.id = course_teacher.teacher_id JOIN courses ON course_teacher.course_id = courses.id JOIN degrees ON courses.degree_id = degrees.id JOIN departments ON degrees.department_id = departments.id WHERE departments.name LIKE 'Dipartimento di Matematica';

```

##### Bonus

7. Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.

```
SELECT students.*, courses.id 'course_id', courses.name 'course', COUNT(courses.id) 'exam_attempts', MAX(exam_student.vote) FROM students JOIN exam_student ON students.id = exam_student.student_id JOIN exams ON exams.id = exam_student.exam_id JOIN courses ON exams.course_id = courses.id GROUP BY students.surname, students.name, courses.id, exam_student.student_id;

```
