# ADR-03: Transición a Arquitectura Hexagonal para Grind Core

| Campo  | Valor |
|--------|-------|
| Autor  | Joshua Cruz |
| Fecha  | 12/06/2026 |
| Estado | `Propuesto` |

---

## Contexto

En el ADR anterior (ADR-02) definimos el uso de .NET 10 con el patrón MVC tradicional y PostgreSQL. Sin embargo, a medida que el proyecto Grind Core crece, la lógica matemática del Powerlifting (como calcular el RPE, estimar el 1RM y controlar la fatiga) se está mezclando con los controladores de la web y los modelos de la base de datos. 

Para evitar que el núcleo de la aplicación dependa de herramientas externas y asegurar que el código sea fácil de probar sin necesidad de conectar la base de datos todo el tiempo, necesitamos cambiar la forma en que organizamos la estructura del proyecto.

---

## Decisión

Se decide cambiar el estilo arquitectónico de Grind Core a una **Arquitectura Hexagonal (Puertos y Adaptadores)**. 

El proyecto se reestructurará en tres áreas independientes:
1. **Dominio (El Núcleo):** Contendrá únicamente las reglas de negocio puras del Powerlifting (Atletas, Series, Bloques de entrenamiento y fórmulas de fuerza) en clases limpias de C#, sin frameworks ni anotaciones de base de datos.
2. **Puertos:** Interfaces que definen cómo se comunica el núcleo con el exterior (por ejemplo, cómo guardar un entrenamiento o cómo recibir datos).
3. **Adaptadores:** La tecnología concreta. Aquí se ubicarán los controladores de ASP.NET Core para recibir las peticiones web (adaptadores de entrada) y Entity Framework / PostgreSQL para guardar los datos (adaptadores de salida).

---

## Justificación

Elegimos la Arquitectura Hexagonal porque resuelve los problemas principales de nuestra app:

- **Independencia de la lógica deportiva:** El cálculo del 1RM o el control del RPE no deben cambiar si decidimos modificar la base de datos o si dejamos de usar MVC en el futuro.
- **Pruebas más fáciles:** Al separar el dominio, podemos hacer pruebas unitarias rápidas a las fórmulas de entrenamiento sin tener que levantar PostgreSQL o Supabase.
- **Aislamiento de errores:** Si algo falla en la conexión de la base de datos o en la interfaz web, el núcleo del sistema de entrenamiento permanece intacto y protegido.
