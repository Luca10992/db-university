### Dopo aver creato un nuovo database nel vostro phpMyAdmin e aver importato lo schema allegato, eseguite le query del file allegato.

#### 1- Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql
SELECT `students`.*
FROM `students`
INNER JOIN `degrees`
ON `degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';

```

### 2- Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

```sql
SELECT `degrees`.*
FROM `degrees`
INNER JOIN `departments`
ON `departments`.`id` = `department_id`
WHERE `degrees`.`level` = 'magistrale'
AND `departments`.`name` = 'Dipartimento di Neuroscienze';
```

### 3- Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```sql
SELECT
	`degrees`.`id` as `degrees_id`,
    `degrees`.`name` as `degrees_name`,
    `degrees`.`level` as `degrees_level`,
    `courses`.`id` as `courses_id`,
    `courses`.`name` as `courses_name`,
    `courses`.`cfu` as `courses_cfu`,
    `teachers`.`id` as `teachers_id`,
    `teachers`.`name` as `teachers_name`,
    `teachers`.`surname` as `teachers_surname`
FROM `degrees`
INNER JOIN `courses`
ON `degrees`.`id` = `courses`.`degree_id`
INNER JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`
INNER JOIN `teachers`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
WHERE `teachers`.`name` = 'Fulvio'
AND `teachers`.`surname` = 'Amato';
```

### 4- Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui

### sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

```sql
SELECT
	`students`.`id` as `students_id`,
	`degrees`.`id` as `degree_id`,
    `students`.`surname` as `students_surname`,
	`students`.`name` as `students_name`,
    `degrees`.`name` as `degree_name`,
    `degrees`.`level` as `degree_level`,
    `departments`.`id` as `department_id`,
    `departments`.`name`as `department_name`,
    `departments`.`head_of_department`
FROM `students`
INNER JOIN `degrees`
ON `degree_id` = `degrees`.`id`
INNER JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
ORDER BY `students_surname`, `students_name`;
```

### 5- Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql
SELECT
	`degrees`.`id` as `degrees_id`,
    `degrees`.`name` as `degrees_name`,
    `degrees`.`level` as `degrees_level`,
    `courses`.`id` as `courses_id`,
    `courses`.`name` as `courses_name`,
    `courses`.`cfu` as `courses_cfu`,
    `teachers`.`id` as `teachers_id`,
    `teachers`.`name` as `teachers_name`,
    `teachers`.`surname` as `teachers_surname`
FROM `degrees`
INNER JOIN `courses`
ON `courses`.`id` = `degree_id`
INNER JOIN `teachers`
ON `teachers`.`id`;
```

### 6- Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

```sql
SELECT DISTINCT
	`teachers`.`id` as `teachers_id`,
    `teachers`.`name` as `teachers_name`,
    `teachers`.`surname` as `teachers_surname`,
    `departments`.`id` as `departments_id`,
    `departments`.`name` as `departments_name`
FROM `teachers`
INNER JOIN `course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
INNER JOIN `courses`
ON `courses`.`id` = `course_teacher`.`course_id`
INNER JOIN `degrees`
ON `degrees`.`id` = `courses`.`degree_id`
INNER JOIN `departments`
ON `departments`.`id` = `department_id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';
```

### 7- BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame,

### stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.

<!-- VOTO PIù ALTO -->

```sql
SELECT
	`students`.`id` as `students_id`,
    COUNT(`students`.`registration_number`) as `number_exam`,
    `students`.`name` as `students_name`,
    `students`.`surname` as `students.surname`,
	MAX(`exam_student`.`vote`) as `vote`
FROM `students`
INNER JOIN `exam_student`
ON `students`.`id` = `exam_student`.`student_id`
INNER JOIN `exams`
ON `exams`.`id` = `exam_student`.`exam_id`
INNER JOIN `courses`
ON `exams`.`course_id` = `courses`.`id`
GROUP BY `students`.`id`, `courses`.`id`;
```

<!-- VOTO PIù BASSO -->

```sql
SELECT
	`students`.`id` as `students_id`,
    COUNT(`students`.`registration_number`) as `number_exam`,
    `students`.`name` as `students_name`,
    `students`.`surname` as `students.surname`,
	MIN(`exam_student`.`vote`) as `vote`
FROM `students`
INNER JOIN `exam_student`
ON `students`.`id` = `exam_student`.`student_id`
INNER JOIN `exams`
ON `exams`.`id` = `exam_student`.`exam_id`
GROUP BY `students`.`id`
HAVING `vote` >= 18;
```
