## QUERY GROUP BY EX

1. Contare quanti iscritti ci sono stati ogni anno

```sql
SELECT YEAR(`enrolment_date`) AS "anno",COUNT(*) AS "iscritti"
FROM `students`
GROUP BY anno;
```

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```sql
SELECT `office_address` AS "edificio", COUNT(*) AS "ufficio_insegnanti"
FROM `teachers`
GROUP BY edificio;
```

3. Calcolare la media dei voti di ogni appello d'esame

```sql
SELECT `exam_student`.`exam_id`, ROUND((AVG(`exam_student`.`vote`)),2) AS "media_voto"
FROM `exams`
INNER JOIN `exam_student`
ON `exams`.`id` = `exam_student`.`exam_id`
GROUP BY `exam_student`.`exam_id`;
```

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

```sql
SELECT `departments`.`name` AS "nome_dipartimento", COUNT(`degrees`.`department_id`) AS "n_corsi_di_laurea"
FROM `degrees`
INNER JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
GROUP BY `degrees`.`department_id`;
```

## QUERY JOIN EX

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql
SELECT `degrees`.`name` AS "nome_corso_laurea", `students`.`name` AS "nome",`students`.`surname` AS "cognome"
FROM `degrees`
INNER JOIN `students`
ON `degrees`.`id`=`students`.`degree_id`
WHERE `degrees`.`name` = "Corso di Laurea in Economia";
```

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

```sql
SELECT `departments`.`name` AS "nome_dipartimento" , `degrees`.`level` AS "livellolaurea" , `degrees`.`name` AS "nome_corso"
FROM `degrees`
INNER JOIN `departments`
ON `degrees`.`department_id`=`departments`.`id`
WHERE `degrees`.`level` = "magistrale" AND `departments`.`name`="Dipartimento di Neuroscienze";
```

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```sql
SELECT `teachers`.`name`,`teachers`.`surname`,`courses`.`name` AS "nome_corso"
FROM `teachers`

INNER JOIN `course_teacher`
ON `teachers`.`id`=`course_teacher`.`teacher_id`

INNER JOIN `courses`
ON `course_teacher`.`course_id`=`courses`.`id`

WHERE `teachers`.`name`="Fulvio" AND `teachers`.`surname`="Amato";
```

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

```sql
SELECT `students`.`name`,`students`.`surname`,`degrees`.`name` AS "corso_di_laurea",`departments`.`name` AS "nome_dipartimento"
FROM `students`

INNER JOIN `degrees`
ON `students`.`degree_id`=`degrees`.`id`

INNER JOIN `departments`
ON `degrees`.`department_id`=`departments`.`id`

ORDER BY `students`.`surname`, `students`.`name` ASC;
```

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql
SELECT `degrees`.`name` AS "nome_corso", `courses`.`name` AS "nome_esame", `teachers`.`name` AS "nome_insegnante",  `teachers`.`surname` AS "cognome_insegnante"
FROM `degrees`

INNER JOIN `courses`
ON `degrees`.`id` = `courses`.`degree_id`

INNER JOIN `course_teacher`
ON `courses`.`id`=`course_teacher`.`course_id`

INNER JOIN `teachers`
ON `course_teacher`.`teacher_id`= `teachers`.`id`
ORDER BY `nome_corso` ASC;
```

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

```sql
SELECT DISTINCT `teachers`.`name` AS "nome_prof",  `teachers`.`surname` AS "cognome_prof"
FROM `teachers`

INNER JOIN `course_teacher`
ON `teachers`.`id`=`course_teacher`.`teacher_id`

INNER JOIN `courses`
ON `course_teacher`.`course_id`=`courses`.`id`

INNER JOIN `degrees`
ON `courses`.`degree_id`=`degrees`.`id`

INNER JOIN `departments`
ON `degrees`.`department_id`=`departments`.`id`

WHERE `departments`.`name`="Dipartimento di Matematica";
```

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
   per ogni esame, stampando anche il voto massimo. Successivamente,
   filtrare i tentativi con voto minimo 18.

```sql
SELECT `exam_student`.`student_id`, `students`.`name` AS "nome", `students`.`surname` AS "cognome", `courses`.`name` AS "nome_corso", COUNT(*) AS "tentativi", MAX(`exam_student`.`vote`) as "voto_max"
FROM `students`

INNER JOIN `exam_student`
ON `students`.`id`= `exam_student`.`student_id`

INNER JOIN `exams`
ON `exam_student`.`exam_id` = `exams`.`id`

INNER JOIN `courses`
ON `exams`.`course_id`=`courses`.`id`

GROUP BY `exam_student`.`student_id`, `exams`.`course_id`
HAVING MAX(`exam_student`.`vote`) >= 18;
```
