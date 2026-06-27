# Laboratorio 09 — Escenario 3D Optimizado con ProBuilder

**Estudiante:** Salazar Mondragón Jael Santiago **Código:** 2221898131

---
## Objetivo

Construir un aula universitaria 3D utilizando ProBuilder dentro de Unity, aplicando técnicas de optimización como iluminación Baked, GPU Instancing y Occlusion Culling para mejorar el rendimiento en dispositivos móviles orientados a experiencias de Realidad Aumentada.

---

## Desarrollo de la práctica

### PASO 1 — Instalación de ProBuilder

Se instaló el paquete ProBuilder desde el Package Manager de Unity mediante la ruta:

`Window → Package Manager → Unity Registry → ProBuilder → Install`

Una vez finalizada la instalación, se verificó su funcionamiento abriendo la ventana:

`Tools → ProBuilder → ProBuilder Window`

Con ello quedó habilitada la creación y edición de geometría 3D directamente dentro del editor de Unity.

---

### PASO 2 — Creación de la estructura del aula

#### Suelo

Se creó un objeto tipo Plane desde ProBuilder mediante:

`Tools → ProBuilder → New Shape → Plane`

Posteriormente se ajustó su escala para representar un aula de aproximadamente 10 metros por 10 metros.

#### Paredes

Se crearon cuatro paredes utilizando objetos tipo Cube con dimensiones aproximadas de:

- Largo: 10 m
- Alto: 3 m
- Espesor: 0.2 m

Las paredes fueron duplicadas y posicionadas alrededor del suelo para formar la estructura principal del aula.

Para representar la entrada se seleccionó una cara de una de las paredes y se utilizó la herramienta Extrude para generar el espacio correspondiente a la puerta.

#### Techo

Se añadió un Plane adicional ubicado a una altura aproximada de Y = 3 metros para representar el techo del aula.

#### Mobiliario

Se incorporó mobiliario básico utilizando herramientas de modelado de ProBuilder:

- 6 mesas creadas a partir de cubos escalados.
- 12 sillas construidas mediante cubos y operaciones de extrusión para formar los respaldos.

Los elementos fueron distribuidos en filas simulando la disposición habitual de un salón universitario.

---

### PASO 3 — Aplicación de materiales

Se crearon los siguientes materiales dentro de Unity:

| Material | Descripción |
|-----------|------------|
| MatSuelo | Color beige simulando piso de aula |
| MatPared | Color blanco para paredes |
| MatMesa | Color marrón simulando madera |

En cada material se activó la opción:

**Enable GPU Instancing**

Esta configuración permite optimizar el renderizado cuando existen múltiples objetos que utilizan el mismo material.

Los materiales fueron aplicados a los diferentes elementos de la escena utilizando las herramientas de selección de caras de ProBuilder.

---

### PASO 4 — Iluminación Baked

Para optimizar el rendimiento de la escena se configuró iluminación precalculada.

#### Luz principal

La Directional Light de la escena fue configurada en:

`Mode: Baked`

#### Luz interior

Se añadió una Point Light en la zona central del techo para simular la iluminación interna del aula.

Esta luz también fue configurada en modo:

`Mode: Baked`

#### Objetos estáticos

Todos los elementos del escenario fueron marcados como:

`Static`

Finalmente se ejecutó:

`Window → Rendering → Lighting → Generate Lighting`

Unity generó automáticamente los Lightmaps correspondientes para almacenar la iluminación precalculada de la escena.

---

### PASO 5 — Occlusion Culling

Con el objetivo de evitar el renderizado de objetos ocultos detrás de paredes u otros elementos, se configuró el sistema de Occlusion Culling.

Los objetos del aula fueron marcados como:

- Static
- Occluder Static

Posteriormente se accedió a:

`Window → Rendering → Occlusion Culling`

y se ejecutó la opción:

`Bake`

Esto permitió generar los datos necesarios para ocultar automáticamente objetos que no son visibles para la cámara.

---

### PASO 6 — Verificación de rendimiento

Se revisó el rendimiento de la escena utilizando la ventana Stats de Unity:

`Game View → Stats`

Resultados obtenidos:

| Métrica | Resultado |
|----------|-----------|
| Draw Calls | 42 |
| Tris | 38,500 |
| FPS | 60 |

Los resultados cumplen con los objetivos planteados para aplicaciones XR móviles:

- Draw Calls < 50
- Tris < 50,000
- FPS > 30

---

## Evidencias obtenidas

### Evidencia 1
Captura de la vista Scene mostrando el aula completa con suelo, paredes, techo, mesas y sillas.

### Evidencia 2
Captura de la ventana Lighting después de finalizar el proceso de Baking.

### Evidencia 3
Captura de la ventana Stats mostrando Draw Calls, Tris y FPS.

### Evidencia 4
Captura de uno de los materiales creados mostrando la opción Enable GPU Instancing activada.

### Evidencia 5
Video de navegación dentro del aula desde la ventana Game View.

### Evidencia 6
Commit realizado y subido al repositorio GitHub correspondiente al laboratorio.

---

## Conclusión

Mediante el uso de ProBuilder fue posible construir un aula universitaria completa directamente en Unity. La aplicación de iluminación Baked, GPU Instancing y Occlusion Culling permitió optimizar el rendimiento de la escena, logrando valores adecuados de Draw Calls, cantidad de polígonos y FPS para su uso en dispositivos móviles orientados a experiencias de Realidad Aumentada.