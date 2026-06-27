# S12 — GUÍA DE TRABAJO DEL ESTUDIANTE
## Curso: Realidad Virtual y Aumentada | PSISP08075
### Semana 12 — Optimización y Rendimiento en XR

---

> **Instrucciones:** Resuelve todas las secciones en orden. Tiempo sugerido: 90 minutos.
> Entrega en el repositorio del curso como archivo `.md` con tu rama `u-codigo-apellido-nombre-gt12`.
> **No está permitido** buscar respuestas en internet para la Sección A y B.

| Campo | Detalle |
|-------|---------|
| **Estudiante** | __________________ |
| **Código** | __________________ |
| **Fecha** | __________________ |
| **Semana** | 12 — Optimización y Rendimiento en XR |

---

## SECCIÓN A — PREGUNTAS DE OPCIÓN MÚLTIPLE (20 puntos — 2 pts c/u)

**Pregunta 1** *(básica)*

¿Cuántos FPS mínimos exige Meta para aprobar una app en el Quest Store?

- a) 30 FPS
- b) 60 FPS
- c) 72 FPS
- d) 120 FPS

**Respuesta:** ___

---

**Pregunta 2** *(básica)*

¿Cuál es la herramienta principal de Unity para diagnosticar problemas de rendimiento en tiempo real?

- a) Inspector
- b) Unity Profiler
- c) Package Manager
- d) Scene View Stats

**Respuesta:** ___

---

**Pregunta 3** *(básica)*

¿Qué es un "Draw Call" en el contexto del rendering de Unity?

- a) Una función de C# que dibuja texto en pantalla
- b) Una instrucción que la CPU envía a la GPU para renderizar un objeto
- c) Una llamada al sistema operativo para mostrar la ventana de Unity
- d) El proceso de importar modelos 3D al proyecto

**Respuesta:** ___

---

**Pregunta 4** *(básica)*

¿Qué condición debe cumplir un objeto para poder aplicarle Static Batching?

- a) Debe tener un Rigidbody
- b) Debe estar marcado como "Static" y no moverse en runtime
- c) Debe tener exactamente el mismo mesh que otro objeto
- d) Debe estar en la misma capa (Layer) que los otros objetos

**Respuesta:** ___

---

**Pregunta 5** *(intermedia)*

Un modelo 3D tiene 50,000 triángulos en LOD 0. Si el usuario está a 30 metros de distancia y el objeto ocupa el 10% de la pantalla, ¿qué debería mostrar un LOD Group bien configurado?

- a) LOD 0 (50,000 triángulos) porque el objeto está visible
- b) LOD 1 o LOD 2 con menos triángulos, porque el usuario no nota la diferencia a esa distancia
- c) "Culled" (no renderiza) porque el objeto es pequeño en pantalla
- d) LOD 0 siempre, independientemente de la distancia

**Respuesta:** ___

---

**Pregunta 6** *(intermedia)*

Un desarrollador tiene 300 árboles idénticos en su escena con el mismo material. ¿Qué técnica produce mayor reducción de Draw Calls?

- a) Static Batching, porque los árboles no se mueven
- b) GPU Instancing, porque son instancias del mismo mesh con el mismo material
- c) LOD Groups, porque reduce la cantidad de triángulos
- d) Occlusion Culling, porque no renderiza los que están ocultos

**Respuesta:** ___

---

**Pregunta 7** *(intermedia)*

¿Por qué crear `new List<>()` dentro de `void Update()` es un problema de rendimiento?

- a) Porque List no se puede crear dentro de métodos de Unity
- b) Porque genera un objeto nuevo en el heap en cada frame, aumentando la presión sobre el Garbage Collector
- c) Porque Unity no soporta listas genéricas en tiempo de ejecución
- d) Porque List.Add() es más lento que los arrays simples

**Respuesta:** ___

---

**Pregunta 8** *(avanzada)*

En el Unity Profiler, observas que `Camera.Render` toma 45ms y `Scripts Update` toma 2ms por frame. ¿Qué tipo de problema tiene la aplicación?

- a) CPU bound — los scripts son el cuello de botella
- b) GPU bound — el problema está en el rendering
- c) Memory bound — hay demasiada memoria usada
- d) Physics bound — las colisiones son el problema

**Respuesta:** ___

---

**Pregunta 9** *(avanzada)*

¿Cuál es la ventaja del formato de textura ASTC sobre una textura sin comprimir (RGBA32) en Android?

- a) ASTC es más rápido de cargar desde el disco pero ocupa más memoria en GPU
- b) ASTC reduce el tamaño en memoria de GPU hasta en un 75-80%, mejorando el rendimiento
- c) ASTC solo funciona en dispositivos con Android 10 o superior
- d) ASTC mejora la calidad visual pero no tiene impacto en rendimiento

**Respuesta:** ___

---

**Pregunta 10** *(avanzada)*

¿Qué es el "GC.Collect" que aparece en el Unity Profiler y por qué causa stutters?

- a) Un método que recolecta datos de juego para análisis estadístico
- b) El proceso del Garbage Collector que libera memoria de objetos no usados, pausando temporalmente todos los demás procesos
- c) Una llamada automática que guarda el estado del juego cada cierto tiempo
- d) El proceso de Unity para compilar los scripts en tiempo de ejecución

**Respuesta:** ___

---

## SECCIÓN B — COMPLETAR Y RELACIONAR (20 puntos)

### Parte B1 — Completar espacios en blanco (10 pts — 2 pts c/u)

1. El valor de tiempo por frame para alcanzar 90 FPS es _____________ milisegundos. Si algún proceso supera ese valor, la app baja de 90 FPS.

2. Para aplicar GPU Instancing, el material del objeto debe tener activada la opción _____________ en el Inspector del material.

3. El comando de Unity para acceder al Profiler es: _____________ → Analysis → Profiler.

4. Cuando un objeto 3D tiene LOD configurado y la cámara se aleja lo suficiente, el sistema lo marca como _____________, es decir, deja de renderizarlo completamente.

5. `GetComponent<Renderer>()` llamado en `void _____________()` (método de inicio, una sola vez) es la práctica correcta para cachear la referencia.

---

### Parte B2 — Relacionar columnas (10 pts — 1 pt c/u)

| Columna A — Técnica / Concepto | Tu respuesta | Columna B — Descripción |
|-------------------------------|-------------|------------------------|
| 1. Static Batching | ___ | A. No renderiza objetos que están detrás de otros |
| 2. GPU Instancing | ___ | B. Reduce triángulos según la distancia a la cámara |
| 3. LOD Group | ___ | C. Agrupa objetos estáticos con el mismo material en 1 Draw Call |
| 4. Occlusion Culling | ___ | D. Renderiza muchas copias del mismo mesh en 1 Draw Call |
| 5. Baked Lighting | ___ | E. Pre-calcula la iluminación y la guarda en texturas |
| 6. Single Pass Stereo | ___ | F. Renderiza ambos ojos en una sola pasada (VR) |
| 7. GC Alloc | ___ | G. Memoria en heap generada por objetos temporales |
| 8. MaterialPropertyBlock | ___ | H. Permite propiedades únicas por instancia sin romper batching |
| 9. Draw Call | ___ | I. Instrucción CPU → GPU para renderizar un objeto |
| 10. Fixed Foveated Rendering | ___ | J. Menor resolución en bordes del campo visual VR |

---

## SECCIÓN C — ANÁLISIS Y REFLEXIÓN (30 puntos)

### Pregunta C1 (8 pts)

Un estudiante tiene este código en su proyecto AR:

```csharp
void Update()
{
    GameObject obj = GameObject.Find("CuboAR");
    obj.GetComponent<Renderer>().material.color = new Color(
        Mathf.Sin(Time.time), 0.5f, 0.5f);
    Debug.Log("Color actualizado: " + obj.name);
}
```

a) Identifica **todos** los problemas de rendimiento en este código (mínimo 3). (4 pts)

b) Reescribe el código optimizado manteniendo la funcionalidad (el color sigue cambiando con el tiempo). (4 pts)

---

### Pregunta C2 (8 pts)

Compara **Baked Lighting** y **Real-time Lighting** en el contexto de una app AR móvil:

a) Explica cuál es más apropiada para una escena AR estática (objetos que no se mueven) y justifica tu respuesta en términos de rendimiento. (4 pts)

b) ¿En qué escenario específico de AR sería NECESARIO usar iluminación en tiempo real en lugar de baked? (4 pts)

---

### Pregunta C3 — Mini caso de análisis (14 pts)

**Situación:** Tu equipo desarrolló la siguiente experiencia AR para el proyecto grupal: una sala de exhibición virtual con 150 cuadros 3D (cada uno con textura única de 2048×2048 sin comprimir), 20 luces en tiempo real, un script `ExhibicionManager.cs` que en `Update()` busca todos los cuadros con `FindObjectsOfType<CuadroAR>()` y calcula la distancia a la cámara, y sombras activadas en todos los objetos. El resultado: 8 FPS en un celular Snapdragon 720G.

**Pregunta C3a** (7 pts): Identifica los 4 problemas principales de rendimiento en orden de gravedad. Para cada uno: nombra el problema, explica por qué es grave y propón la técnica de optimización específica.

**Pregunta C3b** (7 pts): El docente dice: "Primero midan, después optimicen". Describe el proceso exacto de 5 pasos que seguirías en Unity para diagnosticar correctamente el problema antes de tocar una sola línea de código o un solo ajuste de material.

---

## SECCIÓN D — PREGUNTAS AVANZADAS Y DE CASO (30 puntos)

### Caso profesional (15 pts)

**Contexto:** La empresa *Lima XR Studios* ganó un contrato para desarrollar una app de entrenamiento VR para mineros de la empresa Antamina (Perú). La simulación muestra un túnel minero de 500m de largo con: 2,000 objetos 3D (rocas, equipos, señales de seguridad), 8 tipos de materiales distintos (pero muchos objetos comparten el mismo material), explosiones de partículas dinámicas, física activa para simular caída de rocas, y 15 scripts de lógica de entrenamiento corriendo en Update. El headset objetivo es Meta Quest 2 (GPU: Adreno 650, 6GB RAM).

**Pregunta D1** (5 pts): Diseña una **estrategia de optimización completa** para este proyecto. Organiza tu respuesta en una tabla con: técnica aplicada, por qué aplica a este caso específico, impacto esperado en FPS, y si requiere modificar los assets 3D o solo configuración de Unity.

**Pregunta D2** (5 pts): El físico de "caída de rocas" usa Rigidbody en 500 objetos simultáneamente. Propón una solución que mantenga el realismo visual de la simulación sin comprometer el rendimiento. (Pista: no todas las rocas necesitan física activa al mismo tiempo.)

**Pregunta D3** (5 pts): "¿Cómo implementarías un sistema de LOD automático en runtime para el túnel, donde el nivel de detalle cambia dinámicamente según la posición del usuario y el FPS actual?"

---

### Pregunta de diseño (8 pts)

> "¿Cómo implementarías un sistema de 'Adaptive Quality' para tu proyecto XR que automáticamente reduce la calidad visual cuando el FPS cae por debajo de 60, y la restaura cuando el FPS sube? Describe la lógica del script en pseudocódigo o C# real."

---

### Pregunta de pensamiento crítico (7 pts)

> "Un compañero propone: 'Para optimizar nuestra app AR, vamos a reducir la resolución de pantalla del celular al 50% — así el GPU renderiza menos píxeles y ganamos FPS'. ¿Cuál es el riesgo de esta estrategia y en qué contextos XR específicos podría ser aceptable vs. inaceptable?"

---

## RÚBRICA DE AUTOEVALUACIÓN

| Criterio | Cumplido |
|----------|---------|
| Respondí las 10 preguntas de opción múltiple | ☐ |
| Completé los 5 espacios en blanco de B1 | ☐ |
| Relacioné todas las 10 columnas de B2 | ☐ |
| En C1 identifiqué mínimo 3 problemas y escribí código optimizado | ☐ |
| En C2 expliqué la diferencia con ejemplo concreto | ☐ |
| En C3 identifiqué 4 problemas con técnicas específicas | ☐ |
| En D1 mi tabla de estrategia tiene mínimo 5 técnicas | ☐ |
| En D2 propuse solución de física que no requiere 500 Rigidbodies activos | ☐ |
| Entregué como `.md` en la rama correcta de GitHub | ☐ |

---

*PSISP08075 | Universidad Autónoma del Perú | Ingeniería de Sistemas | Semana 12 | 2026-1*
