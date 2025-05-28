
#  Documentación sobre Triggers en Bases de Datos

## 1. ¿Qué es un Trigger?

Un **Trigger** (disparador) es un objeto de base de datos que se ejecuta **automáticamente** cuando ocurre un determinado evento en una tabla o vista. Estos eventos pueden ser operaciones **INSERT**, **UPDATE** o **DELETE**.

Es una forma de automatizar tareas comunes en la base de datos sin necesidad de intervención directa del usuario o del desarrollador.

## 2. ¿Para qué sirve un Trigger?

Un Trigger sirve para:

- Automatizar tareas de mantenimiento (ej. auditorías, registros de cambios).
- Validar o restringir datos antes o después de una modificación.
- Actualizar otras tablas relacionadas.
- Prevenir errores o acciones no deseadas.

## 3. ¿Cuándo se puede usar un Trigger?

Se puede usar un Trigger en los siguientes casos:

- Cuando se desea mantener un **historial** de cambios.
- Para **sincronizar** datos entre tablas automáticamente.
- Al **validar condiciones** específicas antes de permitir operaciones (como restricciones personalizadas).
- Para **auditoría** y seguridad: registrar quién cambió qué y cuándo.

## 4. Estructura de un Trigger

La estructura básica de un Trigger depende del sistema de gestión de base de datos (DBMS), pero generalmente sigue esta forma:

```sql
CREATE TRIGGER nombre_trigger
[BEFORE | AFTER] [INSERT | UPDATE | DELETE]
ON nombre_tabla
FOR EACH ROW
BEGIN
   -- Instrucciones SQL
END;
```

## 5. Tipos de Trigger

Según el **momento de ejecución**:

- `BEFORE`: se ejecuta **antes** de que ocurra el evento.
- `AFTER`: se ejecuta **después** de que ocurra el evento.
- `INSTEAD OF`: reemplaza el evento, común en vistas.

Según el **evento**:

- `INSERT`
- `UPDATE`
- `DELETE`

## 6. ¿Cuándo ejecuta su acción un Trigger?

Un Trigger ejecuta su acción **automáticamente** al detectarse el evento que lo activa. Por ejemplo:

- `AFTER INSERT`: cuando una fila es insertada.
- `BEFORE DELETE`: antes de eliminarse una fila.

## 7. Ejemplo de un Trigger

###  Ejemplo: Auditoría de inserciones

```sql
CREATE TABLE LogInsert (
    id INT AUTO_INCREMENT PRIMARY KEY,
    tabla_afectada VARCHAR(50),
    fecha DATETIME,
    usuario VARCHAR(50)
);

CREATE TRIGGER trg_log_insert
AFTER INSERT ON empleados
FOR EACH ROW
BEGIN
   INSERT INTO LogInsert (tabla_afectada, fecha, usuario)
   VALUES ('empleados', NOW(), USER());
END;
```

Este trigger registra en la tabla `LogInsert` cada inserción que ocurre en la tabla `empleados`.

## 8. Trigger Marketing

En marketing, un **trigger** (disparador) es un evento o comportamiento del usuario que inicia una acción automatizada, como el envío de un correo.

### Ejemplos:

- Usuario abandona el carrito → se envía un email recordatorio.
- Cliente cumple años → se envía cupón de descuento.
- Primer acceso → se envía bienvenida.

 Estos triggers se configuran en plataformas como Mailchimp, HubSpot, Salesforce, etc.

## 9. ¿Cómo hacer un Trigger?

Pasos básicos:

1. Identificar el evento (INSERT, UPDATE, DELETE).
2. Determinar si debe ser antes o después (`BEFORE` o `AFTER`).
3. Escribir el código SQL que debe ejecutarse.
4. Usar `CREATE TRIGGER` para registrarlo en la base de datos.

## 10. Características de un Trigger

- **Automático**: se ejecuta sin intervención del usuario.
- **Reactivo**: responde a eventos de datos.
- **Invisibles** para el cliente o frontend.
- **Difíciles de depurar** si no se documentan bien.
- Puede acceder a las **variables OLD y NEW** (dependiendo del DBMS).

## 11. Desventajas de un Trigger

- Puede **ocultar lógica de negocio**, haciendo el sistema difícil de entender.
- Puede impactar el **rendimiento**, si hace operaciones costosas.
- **Complica la depuración** (no se ve fácilmente qué lo activó).
- Riesgo de **dependencias cíclicas** entre tablas.

## 12. Sustitutos de Triggers en otras bases de datos

- **Procedimientos almacenados**: se llaman manualmente, pero pueden hacer tareas similares.
- **ORM Hooks (Entity Framework, Django, Sequelize, etc.)**: lógica que se ejecuta antes o después de una operación en la base de datos desde el código.
- **Jobs programados (cronjobs)**: para tareas recurrentes o de mantenimiento.
- **Reglas o restricciones**: en algunos DBMS como PostgreSQL se pueden usar `RULES` como una forma alternativa.

###  Conclusión

Los triggers son herramientas poderosas para automatizar lógica en la base de datos, pero deben usarse con cuidado y estar bien documentados. En sistemas complejos, pueden ser reemplazados o complementados por lógica en la capa de aplicación.
