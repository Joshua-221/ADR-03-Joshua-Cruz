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
