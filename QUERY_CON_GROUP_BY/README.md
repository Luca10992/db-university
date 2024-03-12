### Dopo aver creato un nuovo database nel vostro phpMyAdmin e aver importato lo schema allegato, eseguite le query del file allegato.

#### 1- Contare quanti iscritti ci sono stati ogni anno

```sql
SELECT COUNT(`id`), `enrolment_date`
FROM `students`
GROUP BY `enrolment_date`;

```

### 2- Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```sql
SELECT COUNT(`id`), `office_address`
FROM `teachers`
GROUP BY `office_address`;
```

### 3- Calcolare la media dei voti di ogni appello d'esame

```sql
SELECT `exam_id`, AVG(`vote`)
FROM `exam_student`
GROUP BY `exam_id`;
```

### 4- Contare quanti corsi di laurea ci sono per ogni dipartimento

```sql
SELECT COUNT(*) as `number_degrees`, `departments`.`id`, `departments`.`name`
FROM `degrees`
INNER JOIN `departments`
ON `departments`.`id` = `department_id`
GROUP BY `department_id`;
```
