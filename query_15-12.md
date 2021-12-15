# Query con Group by
1. Contare quanti iscritti ci sono stati ogni anno

    SELECT COUNT(*) 
    AS `number_student` , YEAR(`enrolment_date`) 
    FROM `students` 
    GROUP BY YEAR(`enrolment_date`);

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

    SELECT COUNT(*) 
    AS `number_teacher`, `office_address` 
    FROM `teachers` 
    GROUP BY `office_address`;

3. Calcolare la media dei voti di ogni appello d'esame

    SELECT CAST(AVG(`vote`) AS DECIMAL(10,2)) 
    AS `average_grades`, `exam_id` FROM `exam_student` 
    GROUP BY `exam_id`;

4. Contare quanti corsi di laurea ci sono per ogni dipartimento

    SELECT COUNT(*) 
    AS `number_degree`, `department_id` 
    FROM `degrees` 
    GROUP BY `department_id`;

## Query con Join
1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

    SELECT *
    FROM `students`
    JOIN `degrees` 
    ON `students`.`degree_id` = `degrees`.`id`
    WHERE `degrees`.`name` = 'Corso di Laurea in Economia'; 

<!-- 
    SELECT `students`.`id`, `students`.`degree_id`,`students`.`name`,`students`.`surname`, `students`.`date_of_birth`, `students`.`fiscal_code`, `students`.`enrolment_date`
    FROM `students`
    JOIN `degrees` 
    ON `students`.`degree_id` = `degrees`.`id`
    WHERE `degrees`.`name` = 'Corso di Laurea in Economia'; 
-->

2. Selezionare tutti i Corsi di Laurea del Dipartimento di Neuroscienze
    SELECT * 
    FROM `degrees` 
    JOIN `departments` 
    ON `degrees`.`department_id` = `departments`.`id` 
    WHERE `departments`.`name` = 'Dipartimento di Neuroscienze';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
    
    SELECT * 
    FROM `courses` 
    JOIN `course_teacher` 
    ON `courses`.`id` = `course_teacher`.`course_id`
     WHERE `course_teacher`.`teacher_id` = 44;

    <!-- 
        SELECT * 
        FROM `courses` 
        JOIN `course_teacher` 
        ON `courses`.`id` = `course_teacher`.`course_id` 
        JOIN `teachers` 
        ON `teachers`.`id` = `course_teacher`.`teacher_id`
        WHERE `teachers`.`name` = 'Fulvio' 
        AND `teachers`.`surname` = 'Amato';
    -->

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome
    SELECT * 
    FROM `students` 
    JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`  JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` 
    ORDER BY `students`.`name`, `students`.`surname`;

    <!-- 
    SELECT  `students`.`id`, `students`.`degree_id`, `students`.`name`, `students`.`surname`, `degrees`.`name`, `degrees`.`level`, `degrees`.`email`, `degrees`.`website`
    FROM `students` 
    JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`  
    JOIN `departments` ON `degrees`.`department_id` = `departments`.`id` 
    ORDER BY `students`.`name`, `students`.`surname`;
    -->

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
    SELECT * 
    FROM `courses` 
    JOIN `course_teacher` 
    ON `courses`.`id` = `course_teacher`.`course_id` 
    JOIN `teachers` 
    ON `course_teacher`.`teacher_id` = `teachers`.`id`;

    <!-- 
    SELECT `courses`.`id`, `courses`.`name`, `courses`.`description`,  `courses`.`year`,  `courses`.`website`, `teachers`.`id`, `teachers`.`name`, `teachers`.`surname`,`teachers`.`phone`, `teachers`.`email`, `teachers`.`office_address`
    FROM `courses`
    JOIN `course_teacher` 
    ON `courses`.`id` = `course_teacher`.`course_id` 
    JOIN `teachers` 
    ON `course_teacher`.`teacher_id` = `teachers`.`id` 
    -->

6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

    SELECT DISTINCT `teachers`.* 
    FROM `teachers` 
    JOIN `course_teacher` 
    ON `teachers`.`id` = `course_teacher`.`teacher_id` 
    JOIN `courses` 
    ON `courses`.`id` = `course_teacher`.`course_id`
    JOIN `degrees` 
    ON `degrees`.`id` = `courses`.`degree_id` 
    JOIN `departments` 
    ON `departments`.`id` = `degrees`.`department_id` 
    WHERE `departments`.`name` = 'Dipartimento di Matematica';

7. BONUS: Selezionare per ogni studente quanti tentativi d’esame ha sostenuto per superare ciascuno dei suoi esami

    SELECT COUNT(`exams`.`id`) AS `number_exam_courses`, `courses`.`name`, `students`.`name`, `students`.`surname`
    FROM `students`
    JOIN `exam_student`
    ON `students`.`id` = `exam_student`.`student_id`
    JOIN `exams`
    ON `exam_student`.`exam_id` = `exams`.`id`
    JOIN `courses`
    ON `exams`.`course_id` = `courses`.`id`
    GROUP BY `exams`.`course_id`, `students`.`id`
    ORDER BY `students`.`surname` ASC, `students`.`name` ASC