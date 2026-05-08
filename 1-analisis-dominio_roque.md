# Parte 1. Análisis del dominio.
***
## 1. ¿Qué tablas de tu BBDD participan en la funcionalidad CRUD de reservas?

### Las tablas principales que usaré en la API son:

- RESERVA (es la base de nuestra BD).
- RECURSO (unida por una FK a RESERVA por id_recurso).
- USUARIONORMAL (unida por una FK a RESERVA por id_usuario).

***

## 2. ¿Qué campos de la entidad Reserva existen en tu BBDD y cuáles expondrás en la API?

|Campo|Tipo|¿Se muestra en API?|Motivo|
|:---:|:----:|:-------------------:|:------:|
|id_recurso|INT (FK)|Sí|Saber el recurso que se utiliza|
|id_reserva_local|INT|Sí (solo en la respuesta)|Saber el ID de la reserva asociado a un recurso|
|id_usuario|INT (FK)|Sí (solo en la respuesta)|Saber quién hace la reserva|
|fecha|DATE|Sí|Saber cuándo es la reserva|
|hora_inicio|TIME|Sí|Saber cuándo empieza una reserva|
|hora_fin|TIME|Sí|Saber cuándo acaba una reserva|
|coste|DECIMAL(10,2)|Sí|Saber el coste de una reserva|
|numero_plazas|INT|Sí|Saber cuántas plazas se van a utilizar|
|motivo|TEXT|Sí|Saber por qué se realiza la reserva|
|observaciones|TEXT|Sí (menos en la respuesta)|Saber información que aporta el usuario|

### Información adicional

Además, añadiré 2 campos para las respuestas de la API, `nombre_recurso` y `nombre_usuario`, para facilitar la lectura y que sea más cómodo para el usuario (con un JOIN a las tablas RECURSO y USUARIONORMAL) y también añadiré un campo que podrá ser modificado, que controla si se llega a anular la reserva en algún momento llamado `disponible`.

***

## 3. ¿Qué validaciones o reglas de negocio aplicarás?

### Restricciones

- PK, VNN: (id_recurso, id_reserva_local).
- FK, VNN:
    - id_recurso que referencia a la tabla RECURSO.
    - id_usuario que referencia a la tabla USUARIONORMAL.    

Estos campos se generarán en base a lo que realice el usuario:

- id_recurso (obtendrá el ID del recurso en función de lo que introduzca el usuario).    
- id_usuario (obtendrá el ID en función del usuario que realice la reserva).

### Lógica

- En la tabla RECURSO ha de existir el ID del recurso de la reserva (id_recurso).
- En la tabla USUARIONORMAL ha de existir el ID del usuario que realiza la reserva (id_usuario).
- hora_inicio < hora_fin

***

## 4. Ejemplos de JSON.

### Request (un usuario crea una reserva)
```
{
    "id_recurso": 1,
    "fecha": "2026-06-30",
    "hora_inicio": "15:00:00",
    "hora_fin": "20:00:00",
    "coste": 200.00,
    "numero_plazas": 10,
    "motivo": "Para pasarlo bien",
    "observaciones": "Voy con unos cuantos"
}
```

### Response (la reserva ha sido creada)
```
{
    "id_recurso": 1,
    "nombre_recurso": "Estudio de Podcast",
    "id_reserva_local": 2,
    "id_usuario": 3,
    "nombre_usuario": "Lucía Méndez",
    "fecha": "2026-06-30",
    "hora_inicio": "15:00:00",
    "hora_fin": "20:00:00",
    "coste": 70.00,
    "numero_plazas": 10,
    "motivo": "Para pasarlo bien",
    "disponible": true
}
```

### Conclusión

Necesito en total 10 campos de la tabla reservas, más 2 añadidos con un JOIN y uno añadido para comprobar el estado de la reserva (en total, 13 campos). Aplico un par de restricciones básicas para asegurarme de que se crea correctamente.

***

## Documentación Swagger UI (Reservas)

### Mis endpoints
[Ver endpoints](./img/endpoints.png)

### Mis schemas

[Ver schemas](./img/schemas.png)

### Responses y ejemplos de los endpoints
- [GET](./img/get_response.png)
- [POST](./img/post_response.png)
- [PATCH](./img/patch_response.png)
- [DELETE](./img/delete_response.png)
