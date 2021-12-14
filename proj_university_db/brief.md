Modellizzare la struttura di una tabella per memorizzare tutti i dati riguardanti una università:
- sono presenti diversi dipartimenti, ciascuno con i propri corsi di laurea;
- ogni corso di laurea è formato da diversi corsi;
- ogni corso può essere tenuto da diversi insegnanti e prevede più appelli d'esame;
- ogni studente è iscritto ad un corso di laurea;
- per ogni appello d'esame a cui lo studente ha partecipato, è necessario memorizzare il voto ottenuto, anche se non sufficiente. 



- departements
    - id
    - name 
    - address
    - phone
    - email
    - website
    - department_coordinator
- degrees
    - id
    - name
    - address
    - email
    - website
    - duration
    - degree_coordinator
    - department_id | FK
- courses
    - id
    - name
    - description
    - website
    - cfu
    - degree_id | FK
- teachers
    - id
    - name
    - last_name
    - phone
    - email
    - office_address
    - website
- exam_sessions
    - id
    - date
    - hour
    - location
    - address
    - courses_id | FK
- exame_student_vote
    - name_exam
    - student_id
    - exame_id
    - vote
- students
    - id
    - name
    - last_name
    - email
    - date_birth
    - fiscal_code
    - registration_date
    - badge_number
    - degreess_id | FK




Relazioni:
1) dipartimento --> lauree

- Una dipartimento può avere diverse lauree, mentre una laurea può essere assegnato ad un solo dipartimento.
<!-- relazione one to many -->

2) lauree --> corsi

- una laurea è formata da diversi corsi e un corso può avere diverse lauree
<!-- relazione one to many --> 
    o 
<!-- many to many -->

<!-- Ho solo questo dubbio, perchè comunque ci sono dei corsi che fanno parte di diverse lauree, es. Analisi 1 ad ingegneria. Però non      viene specificato, quindi non sapevo cosa mettere. -->

3) corsi --> insegnanti

- un corso può essere tenuto da più insegnanti e diversi insegnanti possono tenere lo stesso corso
<!-- relazione many to many -->

4) corsi --> sessione_esami

- ogni corso prevede diverse sessioni di esame, ma la sessione di esame può essere assegnata ad un solo corso.
<!-- relazione one to many --> 

5) studenti --> corsi
- ogni studente è iscritto ad un solo corso di laurea
<!-- relazione one to many --> 

6) sessione_esami --> studenti
- per un appello di esame registro la votazione degli studenti anche se non è sufficiente. diversi appelli e diversi studenti
<!-- relazione many to many -->