# Entregable: Práctica Spring Boot JPA (Relación 1:N)

## Respuestas a las Preguntas de Análisis

### 1. ¿En qué tabla queda la llave foránea y qué propiedad de Java la controla?
- **Tabla con la llave foránea:** Queda en la tabla `comentarios` (columna `producto_id`).
- **Propiedad de Java que la controla:** La propiedad `Comment.producto` (el lado propietario marcado con `@ManyToOne`). La propiedad `Product.comentarios` usa `mappedBy = "producto"`, indicando que es el lado inverso.

### 2. ¿Cuál es la diferencia entre List y Set?
- **`List`:** Colección ordenada que permite elementos duplicados.
- **`Set`:** Colección no ordenada que prohíbe duplicados (basado en `equals` y `hashCode`). En JPA evita duplicados en las asociaciones y se adapta mejor a relaciones de tipo conjunto.

### 3. ¿Por qué los identificadores son String y qué tipo de valor guardan?
- **Tipo de dato:** Son de tipo `String` en Java porque utilizan la estrategia `@GeneratedValue(strategy = GenerationType.UUID)`.
- **Valor que guardan:** Guardan un **UUID** (Identificador Único Universal) de 36 caracteres en formato de texto (ej. `fe251760-e9e7-478c-8159-8f6a8de8340e`), garantizando unicidad sin colisiones.

### 4. ¿Qué ocurre al eliminar un producto con comentarios asociados? ¿Qué cambia si se quita CascadeType.ALL?
- **Con `CascadeType.ALL`:** Al eliminar un `Product`, JPA propaga automáticamente la eliminación a todos sus objetos `Comment` asociados, eliminando ambos registros.
- **Si se quita `CascadeType.ALL`:** JPA intentará eliminar solo la fila en `producto`. Esto provocará una violación de clave foránea en PostgreSQL (`FK Constraint Violation`), porque la tabla `comentarios` aún hará referencia al `producto_id` eliminado.

### 5. ¿Qué diferencia práctica hay entre los FetchType usados en Product y Comment?
- **En `Product.comentarios` (`FetchType.EAGER`):** Al consultar un producto, JPA carga inmediatamente todos sus comentarios asociados en la misma consulta o petición.
- **En `Comment.producto` (`FetchType.LAZY`):** Al consultar un comentario, JPA no trae la entidad `Product` completa a menos que se acceda explícitamente a la propiedad `producto` dentro de una sesión activa.
