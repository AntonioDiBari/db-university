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
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
   Neuroscienze
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
   sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
   nome
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
6. Selezionare tutti i docenti che insegnano nel Dipartimento di
   Matematica (54)
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
   per ogni esame, stampando anche il voto massimo. Successivamente,
   filtrare i tentativi con voto minimo 18.
