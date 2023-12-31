1. Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.

    ```sql
      SELECT concat(apellido1,' ',apellido2,' ',nombre) as nombre FROM persona WHERE tipo='alumno' ORDER BY apellido1 ASC;
    ```

2. Averigua el nombre y los dos apellidos de los alumnos que **no** han dado de alta su número de teléfono en la base de datos.

    ```sql
      SELECT concat(nombre,' ',apellido1,' ',apellido2) as nombre FROM persona WHERE telefono is NULL and tipo='alumno';
    ```

3. Devuelve el listado de los alumnos que nacieron en `1999`.

    ```sql
      SELECT concat(nombre,' ',apellido1,' ',apellido2) as nombre,fecha_nacimiento FROM persona WHERE YEAR(fecha_nacimiento)=1999 and tipo='alumno';
    ```

4. Devuelve el listado de `profesores` que **no** han dado de alta su número de teléfono en la base de datos y además su nif termina en `K`.

    ```sql
      SELECT concat(nombre,' ',apellido1,' ',apellido2) as nombre,telefono,nif FROM persona WHERE telefono is NULL and tipo='profesor' and nif like '%k';
    ```

5. Devuelve el listado de las asignaturas que se imparten en el primer cuatrimestre, en el tercer curso del grado que tiene el identificador `7`.

    ```sql
      SELECT  nombre,cuatrimestre,id_grado FROM asignatura WHERE cuatrimestre=1 and curso=3 and id_grado=7;
    ```

6. Devuelve un listado con los datos de todas las **alumnas** que se han matriculado alguna vez en el `Grado en Ingeniería Informática (Plan 2015)`.

```sql
  SELECT  CONCAT(P.nombre,' ',P.apellido1,' ',P.apellido2) as nombre,P.sexo, G.nombre FROM persona P 
INNER JOIN alumno_se_matricula_asignatura AA on P.id = AA.id_alumno
INNER JOIN asignatura A on AA.id_asignatura = A.id
INNER JOIN grado G on A.id_grado = G.id WHERE G.nombre='Grado en Ingeniería Informática (Plan 2015)' AND P.sexo=2;
```

7. Devuelve un listado con todas las asignaturas ofertadas en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
      SELECT A.nombre as Asignatura, G.nombre as grado FROM asignatura A INNER JOIN grado G on G.id=A.id_grado WHERE G.nombre='Grado en Ingeniería Informática (Plan 2015)'
    ```

8. Devuelve un listado de los `profesores` junto con el nombre del `departamento` al que están vinculados. El listado debe devolver cuatro columnas, `primer apellido, segundo apellido, nombre y nombre del departamento.` El resultado estará ordenado alfabéticamente de menor a mayor por los `apellidos y el nombre.`

```sql
    SELECT  P.apellido1 as primer_apellido,P.apellido2 as segundo_apellido,P.nombre,D.nombre as departamento FROM persona P
INNER JOIN profesor PR on P.id=PR.id_profesor
INNER JOIN departamento D on PR.id_departamento=D.id
ORDER BY P.apellido1,P.nombre ASC;
```

9. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con nif `26902806M`.

```sql
SELECT CONCAT(P.apellido1,' ',P.apellido2,' ',P.nombre)as nombre,A.nombre as asignatura,CE.anyo_inicio as año_de_inicio,CE.anyo_fin as año_fin from persona P
INNER JOIN alumno_se_matricula_asignatura AM on P.id=AM.id_alumno
INNER JOIN asignatura A on A.id=AM.id_asignatura
INNER JOIN curso_escolar CE on CE.id=AM.id_curso_escolar
WHERE P.nif='26902806M';

```

10. Devuelve un listado con el nombre de todos los departamentos que tienen profesores que imparten alguna asignatura en el `Grado en Ingeniería Informática (Plan 2015)`.

```sql
SELECT DISTINCT D.nombre as departamento FROM departamento D
INNER JOIN profesor PR on D.id=PR.id_departamento
INNER JOIN asignatura A on A.id_profesor=PR.id_profesor
INNER JOIN grado G on G.id=A.id_grado
WHERE G.nombre='Grado en Ingeniería Informática (Plan 2015)'

```

11. Devuelve un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.

```sql
    SELECT DISTINCT P.id as id,CONCAT(P.nombre,' ',P.apellido1,' ',P.apellido2) as nombre,CE.anyo_inicio as año_inicio,CE.anyo_fin as añofinal FROM persona P
    INNER JOIN alumno_se_matricula_asignatura MA on P.id=MA.id_alumno
    INNER JOIN curso_escolar CE on CE.id=MA.id_curso_escolar
    WHERE CE.anyo_inicio=2018 and CE.anyo_fin=2019;
```

12. Devuelve un listado con los nombres de **todos** los profesores y los departamentos que tienen vinculados. El listado también debe mostrar aquellos profesores que no tienen ningún departamento asociado. El listado debe devolver cuatro columnas, nombre del departamento, primer apellido, segundo apellido y nombre del profesor. El resultado estará ordenado alfabéticamente de menor a mayor por el nombre del departamento, apellidos y el nombre.

```sql
SELECT D.nombre as departamento,P.apellido1,P.apellido2,P.nombre FROM persona P
INNER JOIN profesor PR on P.id=PR.id_profesor
INNER JOIN departamento D on PR.id_departamento=D.id
ORDER BY D.nombre,P.apellido1,P.apellido2,P.nombre ASC;
```

13. Devuelve un listado con los profesores que no están asociados a un departamento.Devuelve un listado con los departamentos que no tienen profesores asociados.

```sql

SELECT CONCAT(P.nombre,' ',P.apellido1,' ',P.apellido2) as nombre FROM persona P where P.tipo='profesor' and P.id not IN(SELECT id_profesor FROM profesor)

SELECT D.nombre as departamento from departamento D WHERE D.id not IN(SELECT id_departamento FROM profesor)
```

14. Devuelve un listado con los profesores que no imparten ninguna asignatura.

```sql
SELECT CONCAT(P.nombre,' ', P.apellido1,' ', P.apellido2) AS nombre
FROM persona P
INNER JOIN profesor PR ON PR.id_profesor = P.id
LEFT JOIN asignatura A ON A.id_profesor = PR.id_profesor
WHERE A.id_profesor IS NULL
```

15. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

```sql
SELECT A.nombre
FROM asignatura A
LEFT JOIN profesor P ON P.id_profesor = A.id_profesor
WHERE A.id_profesor IS NULL
```

16. Devuelve un listado con todos los departamentos que tienen alguna asignatura que no se haya impartido en ningún curso escolar. El resultado debe mostrar el nombre del departamento y el nombre de la asignatura que no se haya impartido nunca.

```sql
SELECT D.nombre AS nombre_departamento, A.nombre AS nombre_asignatura
FROM departamento D
INNER JOIN asignatura A ON A.id = D.id
LEFT JOIN curso_escolar C ON C.id = A.id
WHERE C.id IS NULL
ORDER BY D.nombre, A.nombre;
```

17. Devuelve el número total de **alumnas** que hay.

```sql
SELECT COUNT() AS alumnas
FROM persona P
WHERE P.tipo = 'alumno' and P.sexo='M';
```

18. Calcula cuántos alumnos nacieron en `1999`.

```sql
SELECT COUNT() AS '1999'
FROM persona P
WHERE YEAR(P.fecha_nacimiento) = 1999;
```

19. Calcula cuántos profesores hay en cada departamento. El resultado sólo debe mostrar dos columnas, una con el nombre del departamento y otra con el número de profesores que hay en ese departamento. El resultado sólo debe incluir los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.

```sql
SELECT D.nombre, COUNT(*) AS numero_profesores
FROM departamento D
INNER JOIN profesor P ON P.id_departamento = D.id
GROUP BY D.nombre
ORDER BY numero_profesores DESC;
```

20. Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.

     ```sql
       # Consulta Aqui
     ```

21. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta que pueden existir grados que no tienen asignaturas asociadas. Estos grados también tienen que aparecer en el listado. El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.

     ```sql
         sql
    SELECT G.nombre, COUNT(A.id_grado) AS numero_asignaturas
    FROM grado G
    LEFT JOIN asignatura A ON A.id_grado = G.id
    GROUP BY G.nombre
    ORDER BY numero_asignaturas DESC;
     ```

22. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de `40` asignaturas asociadas.

     ```sql
       sql
    SELECT G.nombre, COUNT(A.id) AS numero_asignaturas
    FROM grado G
    JOIN asignatura A ON A.id_grado = G.id
    GROUP BY G.nombre
    HAVING COUNT(A.id) > 40
    ORDER BY numero_asignaturas DESC; 
     ```

23. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de crédidos.

     ```sql
      sql
    SELECT G.nombre, A.tipo, SUM(A.creditos) AS numero_creditos
    FROM grado G
    INNER JOIN asignatura A ON A.id_grado = G.id
    GROUP BY G.nombre, A.tipo
    ORDER BY numero_creditos DESC;
     ```

24. Devuelve un listado que muestre cuántos alumnos se han matriculado de alguna asignatura en cada uno de los cursos escolares. El resultado deberá mostrar dos columnas, una columna con el año de inicio del curso escolar y otra con el número de alumnos matriculados.

     ```sql
       sql
    SELECT C.anyo_inicio, COUNT(*) AS numero_alumnos
    FROM alumno_se_matricula_asignatura MA
    INNER JOIN curso_escolar C ON C.id = MA.id_curso_escolar
    GROUP BY C.anyo_inicio;
     ```

25. Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.

     ```sql
       sql
    SELECT CONCAT(P.nombre,' ', P.apellido1,' ', P.apellido2), COUNT(A.id) AS numero_asignaturas
    FROM persona P
    INNER JOIN profesor PR ON PR.id_profesor = P.id
    LEFT JOIN asignatura A ON A.id_profesor = PR.id_profesor
    GROUP BY P.id
    ORDER BY numero_asignaturas DESC;
     ```

26. Devuelve todos los datos del alumno más joven.

     ```sql
       sql
    SELECT *
    FROM persona P
    WHERE tipo = 'alumno'
    ORDER BY P.fecha_nacimiento ASC
    LIMIT 1;
     ```

27. Devuelve un listado con los profesores que no están asociados a un departamento.

     ```sql
            sql
    SELECT P.id, CONCAT(P.nombre,' ', P.apellido1,' ', P.apellido2) AS nombre
    FROM persona P
    LEFT JOIN profesor PR ON PR.id_profesor = P.id
    WHERE PR.id_departamento IS NULL AND P.tipo='profesor';
     ```

28. Devuelve un listado con los departamentos que no tienen profesores asociados.

     ```sql
         sql
    SELECT d.*
    FROM departamento d
    LEFT JOIN profesor pr ON d.id = pr.id_departamento
    WHERE pr.id_departamento IS NULL;
     ```

29. Devuelve un listado con los profesores que tienen un departamento asociado y que no imparten ninguna asignatura.

     ```sql
        sql
    SELECT  P.id, CONCAT(P.nombre,' ', P.apellido1,' ', P.apellido2) AS nombre
    FROM persona p
    JOIN profesor pr ON p.id = pr.id_profesor
    LEFT JOIN asignatura a ON pr.id_profesor = a.id_profesor
    WHERE pr.id_departamento IS NOT NULL
    AND a.id IS NULL
    AND p.tipo = 'profesor';
     ```

30. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

     ```sql
      sql
    SELECT *
    FROM asignatura
    WHERE id_profesor IS NULL;
     ```

31. Devuelve un listado con todos los departamentos que no han impartido asignaturas en ningún curso escolar.

     ```sql
       sql
    SELECT D.*
    FROM departamento D
    LEFT JOIN profesor P ON D.id = P.id_departamento
    LEFT JOIN asignatura A ON P.id_profesor = A.id_profesor
    LEFT JOIN alumno_se_matricula_asignatura MA ON A.id = MA.id_asignatura
    LEFT JOIN curso_escolar C ON MA.id_curso_escolar = C.id
    GROUP BY D.id
    HAVING COUNT(DISTINCT C.id) = 0;
     ```
