# S12 — SESIÓN DE CLASE COMPLETA
## Curso: Realidad Virtual y Aumentada | PSISP08075
### Semana 12 — Optimización y Rendimiento en XR

---

| Campo | Detalle |
|-------|---------|
| **Curso** | Realidad Virtual y Aumentada — PSISP08075 |
| **Semana** | 12 |
| **Tema** | Optimización y rendimiento en XR |
| **Valor transversal** | Compromiso en la identificación de problemas de rendimiento y la aplicación de técnicas de optimización |
| **Logro de aprendizaje** | Al finalizar la sesión, el estudiante optimiza el rendimiento de la experiencia XR, demostrando compromiso con la mejora continua del proyecto |
| **Duración total** | 3 horas (180 min) + 20 min receso |
| **Modalidad** | Presencial con práctica en equipo |

---

## TABLA DE CONTENIDOS

1. [Cronograma](#cronograma)
2. [INICIO](#inicio)
3. [UTILIDAD](#utilidad)
4. [TRANSFORMACIÓN](#transformacion)
   - 4.1 ¿Por qué el rendimiento es crítico en XR?
   - 4.2 Unity Profiler — el diagnóstico profesional
   - 4.3 Draw Calls y Batching
   - 4.4 LOD Groups — niveles de detalle
   - 4.5 Occlusion Culling
   - 4.6 Gestión de texturas y memoria
   - 4.7 Optimización de scripts (C# y GC)
   - 4.8 Optimizaciones específicas para AR y VR
5. [RECESO](#receso)
6. [PRÁCTICA](#practica)
7. [CIERRE](#cierre)
8. [Guion verbal sugerido](#guion)
9. [Casos reales recomendados](#casos)
10. [Evaluación formativa](#eval)
11. [Referencias APA 7](#referencias)

---

## CRONOGRAMA {#cronograma}

| # | Bloque | Actividad | Duración | Responsable |
|---|--------|-----------|----------|-------------|
| 1 | INICIO | Rompe-hielo + Logro + Revisión S11 + Diagnóstico | 20 min | Docente |
| 2 | UTILIDAD | Impacto profesional del rendimiento XR | 10 min | Docente |
| 3 | TRANSFORMACIÓN | Profiler, Draw Calls, LOD, Culling, Memoria, Scripts, AR/VR | 70 min | Docente activo |
| — | **RECESO** | — | **20 min** | — |
| 4 | PRÁCTICA | Caso grupal + ejercicio individual | 40 min | Estudiantes |
| 5 | CIERRE | Síntesis + metacognición + puente S13 | 10 min | Docente |

---

## 1. INICIO (20 min) {#inicio}

### a) ROMPE-HIELO — "El precio del lag" (5 min)

**Instrucción verbal al docente:**

> "Cierren los ojos por 5 segundos. Imaginen que están usando unas gafas de VR en una experiencia de entrenamiento médico — están aprendiendo a hacer una cirugía virtual. De repente, la imagen se traba por un segundo. ¿Qué sintieron? ¿Mareo? ¿Frustración? ¿Desconfianza en el sistema? Abran los ojos."

Esperar respuestas espontáneas. Luego:

> "Lo que acaban de sentir en su imaginación es lo que el 47% de los usuarios de VR reportan en sus primeras sesiones cuando el frame rate cae por debajo de 60 FPS. Ese 'lag' no es solo molesto: en entornos médicos, militares o industriales puede causar decisiones incorrectas. Hoy aprenden a eliminarlo."

**Propósito:** conectar emocionalmente el problema de rendimiento con consecuencias reales antes de abordar la solución técnica.

---

### b) LOGRO DE APRENDIZAJE (3 min)

**Guion verbal textual:**

> "El logro de hoy es concreto: al finalizar esta sesión van a poder tomar un proyecto XR con mal rendimiento, identificar EXACTAMENTE dónde está el problema usando el Unity Profiler, y aplicar al menos 3 técnicas de optimización para mejorar los FPS. No es teoría — es un proceso de diagnóstico y corrección que van a repetir semana a semana en sus proyectos grupales."
>
> "El valor que trabajamos hoy es el compromiso. Optimizar no es glamoroso — no produce nuevas funcionalidades visibles. Pero un proyecto que corre a 12 FPS no tiene futuro, sin importar qué tan buenas sean sus ideas. El compromiso con el rendimiento es lo que distingue a los desarrolladores XR profesionales de los amateurs."

---

### c) REVISIÓN SESIÓN ANTERIOR — S11: Scripts e Inputs (7 min)

**Pregunta 1:**

> "La semana pasada trabajaron InputActionReference. ¿Por qué es mejor que hardcodear el binding en el código? Den un ejemplo concreto del contexto XR."

**Respuesta esperada:**
InputActionReference desacopla el código del dispositivo físico. Si hardcodeas `Keyboard.current.spaceKey`, el código solo funciona con teclado. Con InputActionReference, el mismo script funciona con teclado, controller Quest, air-tap de HoloLens — solo cambia el binding en el Input Actions Asset. En XR esto es crítico porque las apps se despliegan en múltiples plataformas simultáneamente.

**Si responde mal:** "Piensen en la palabra 'acoplamiento'. ¿Qué tan difícil sería cambiar de Quest a HoloLens si el nombre del botón está escrito directamente en el código?"

---

**Pregunta 2:**

> "¿Por qué se declara `List<ARRaycastHit>` como campo de clase y no se crea con `new List<>()` dentro de `Update()`?"

**Respuesta esperada:**
Crear `new List<>()` dentro de `Update()` genera un objeto en heap en cada frame (60+ veces por segundo). El Garbage Collector de C# debe limpiar estos objetos periódicamente, causando micro-pausas (GC spikes) que en VR se perciben como stutters que rompen la inmersión. Declararlo como campo y llamar `.Clear()` reutiliza el mismo objeto sin presión al GC.

**Si responde mal:** "Este concepto está directamente conectado con lo de hoy. Crear objetos innecesariamente en Update es uno de los errores de optimización más comunes. Sigan ese hilo mental."

---

**Pregunta 3:**

> "¿Qué diferencia hay entre el callback `performed` de una InputAction y leer `ReadValue<T>()` en Update? ¿Cuándo usa cada uno?"

**Respuesta esperada:**
`performed` (evento) se dispara una sola vez cuando la acción se activa — ideal para acciones discretas (presionar botón, tap, doble tap). `ReadValue<T>()` en Update lee el valor en cada frame — ideal para entradas analógicas continuas como joystick o trigger como acelerador. Error común: usar `performed` para joystick (solo se activa al cruzar zonas de dead band, no continuamente).

---

### d) DIAGNÓSTICO INICIAL (5 min)

**Pregunta 1:** "¿Qué es el 'frame rate' en una aplicación XR y qué valor mínimo se considera aceptable?"

**Respuesta esperada:** Frame rate (FPS = Frames Per Second) es cuántas imágenes renderiza la app por segundo. En AR móvil: mínimo 30 FPS para experiencia fluida. En VR (Quest, PICO): mínimo 72 FPS (Meta recomienda 90 FPS). Por debajo de estos valores el usuario siente latencia, malestar (motion sickness en VR) o simplemente una experiencia deficiente. Valores aceptables varían: juegos: 60 FPS; VR: 72-120 FPS; AR móvil: 30-60 FPS.

**Pregunta 2:** "¿Qué herramienta de Unity usarían para medir el rendimiento de una aplicación mientras corre?"

**Respuesta esperada:** El **Unity Profiler** (Window → Analysis → Profiler). Muestra en tiempo real: CPU, GPU, memoria, física, scripts, rendering — y permite identificar exactamente qué código o qué objeto consume más tiempo. Respuesta alternativa válida: el panel **Stats** en la ventana Game (muestra FPS, draw calls, triángulos en tiempo real durante el Play Mode).

**Pregunta 3:** "Si una escena AR tiene 500 objetos 3D activos, ¿qué problemas de rendimiento anticipan?"

**Respuesta esperada (lo que se espera que intuyan, no que sepan exactamente):** muchos draw calls (una llamada de renderizado por objeto = 500 draw calls), mucho procesamiento de física si tienen colliders, memoria de texturas alta. En móvil, esto puede bajar el frame rate a 10-15 FPS. No se espera que sepan la solución exacta — el objetivo es medir intuición antes de enseñar las técnicas.

---

## 2. UTILIDAD (10 min) {#utilidad}

### El costo real del mal rendimiento en XR

**Dato de impacto:**
Un estudio de Oculus/Meta (2021) encontró que el **47% de los usuarios abandona una app VR** si experimenta motion sickness en la primera sesión. El motion sickness está directamente causado por frame rates bajos o inconsistentes — específicamente cuando el tiempo de renderizado supera 11ms (equivalente a 90 FPS). No hay segunda oportunidad: la primera experiencia de náusea hace que el usuario no regrese.

**Problema real en la industria:**
La app de entrenamiento quirúrgico de Fundamental Surgery (UK) tardó 8 meses en optimización antes de poder lanzarse en Quest 2. El problema no era el contenido — era que las manos virtuales del cirujano tenían latencia de 40ms por mal uso del sistema de físicas. Con latencia, el cerebro del cirujano no sincroniza la mano virtual con la real → confusión → náusea.

**Impacto económico:**
Un frame rate de 30 FPS vs 90 FPS en VR no es solo calidad visual — es literalmente la diferencia entre una app que se puede publicar en el Quest Store y una que Meta rechaza automáticamente (Meta requiere rendimiento mínimo de 72 FPS para aprobar apps).

### Aplicación en empresas reales

- **Meta:** rechaza apps del Quest Store con frame rate < 72 FPS en pruebas de QA.
- **Microsoft HoloLens 2:** MRTK incluye un "Performance Monitoring Tool" obligatorio durante el desarrollo.
- **Niantic (Pokémon GO):** equipo de 12 ingenieros dedicados exclusivamente a optimización AR en Android/iOS.
- **Unity Technologies:** el Unity Profiler es una herramienta estándar de pipeline en todos los estudios AAA.
- **Varjo (VR industrial):** sus headsets de resolución ultra-alta (35 megapíxeles/ojo) requieren GPUs de servidor — la optimización no es opcional, es arquitectural.

### Pregunta retadora de apertura

> "Si una app XR corre bien en tu PC de desarrollo (GPU RTX 3070, 32 GB RAM) pero a 12 FPS en el celular del usuario final (Snapdragon 720G, 6 GB RAM), ¿cuál es el problema real y cómo lo resolverías sin bajar la calidad visual?"

**Respuesta esperada:** el problema es que el desarrollador optimizó para su hardware de desarrollo, no para el hardware objetivo. Las soluciones incluyen: LOD Groups (modelos más simples a distancia), texture compression (formato ETC2 o ASTC para móvil), reducción de draw calls (batching, GPU Instancing), baked lighting (en lugar de lighting dinámico), culling agresivo (no renderizar lo que no se ve). La idea clave: optimizar para el hardware MÁS DÉBIL en tu público objetivo, no para el tuyo.

---

## 3. TRANSFORMACIÓN (70 min) {#transformacion}

### 3.1 ¿Por qué el rendimiento es crítico en XR? (8 min)

**Explicación conceptual:**

En aplicaciones convencionales (web, escritorio), un frame de 100ms se percibe como "lentitud". En XR, las consecuencias son fisiológicas:

```
RELACIÓN FPS ↔ EXPERIENCIA XR

FPS     | Latencia/frame | Efecto en VR            | Efecto en AR
--------|----------------|-------------------------|------------------
120 FPS | 8.3 ms         | Excelente, sin malestar | Fluidez perfecta
90 FPS  | 11.1 ms        | Mínimo recomendado VR   | Muy fluido
72 FPS  | 13.9 ms        | Límite Quest Store      | Fluido
60 FPS  | 16.7 ms        | AR móvil aceptable      | Aceptable
30 FPS  | 33.3 ms        | Motion sickness leve    | Perceptible
< 20    | > 50 ms        | Motion sickness severo  | Inutilizable
```

**El pipeline de renderizado en Unity:**

```
Por cada frame Unity ejecuta:
1. Update() de todos los scripts          → CPU
2. Physics (FixedUpdate, colisiones)      → CPU
3. Frustum Culling (¿qué está en vista?)  → CPU
4. Draw Calls al GPU                      → CPU → GPU
5. Vertex Shader (posición de vértices)   → GPU
6. Fragment Shader (color de píxeles)     → GPU
7. Post-processing (bloom, AA, etc.)      → GPU
8. Display (envío al buffer de pantalla)  → GPU

Si cualquier paso tarda más de 11ms → baja de 90 FPS
```

**PREGUNTA AL GRUPO 1:**

> "¿Por qué en VR el límite de tiempo por frame es tan estricto (11ms para 90 FPS) comparado con una app web donde 100ms se considera rápido?"

**Respuesta esperada:**
En VR el cerebro humano tiene mecanismos de detección de desincronización muy precisos. El vestíbulo (oído interno) detecta movimiento físico de la cabeza en ~1ms. Si la imagen tarda 80ms en actualizar su posición después de que la cabeza giró, el cerebro recibe señales contradictorias: el oído dice "me moví" pero los ojos dicen "todo sigue igual". Esta discrepancia entre señales vestibulares y visuales causa literalmente náuseas (motion sickness). En apps web, la latencia solo causa percepción de "lentitud" — no tiene consecuencia fisiológica. Por eso el estándar es completamente diferente.

---

### 3.2 Unity Profiler — el diagnóstico profesional (15 min)

**Explicación conceptual:**

El Profiler es la herramienta de diagnóstico central. Sin él, optimizar es adivinar. Con él, es ciencia.

```
Cómo acceder:
Window → Analysis → Profiler   (Ctrl + 7)

Vistas principales:
┌─────────────────────────────────────────────────────────────────┐
│ CPU Usage    │ Rendering │ Memory │ Physics │ Audio │ GC Alloc  │
│ (ms por       │ (draw     │ (MB    │ (fisica │       │ (basura   │
│  frame)       │  calls)   │  usados)│ activa)│       │  generada)│
└─────────────────────────────────────────────────────────────────┘

La gráfica principal:
- Línea verde = 60 FPS (16.7ms)
- Línea amarilla = 30 FPS (33.3ms)
- Picos que superan la línea verde = frames con problema
```

**Cómo leer el Profiler paso a paso:**

```
1. Presionar Play en Unity
2. Abrir Profiler (Window → Analysis → Profiler)
3. En la gráfica, hacer clic en el frame más lento (el pico más alto)
4. En la sección inferior: ver la jerarquía de llamadas
   PlayerLoop
   ├── Update.ScriptRunBehaviourUpdate     ← scripts
   │   ├── MiScript.Update()    5.2ms      ← ¡AQUÍ está el problema!
   │   └── OtroScript.Update()  0.1ms
   ├── PhysicsUpdate            2.3ms
   └── Camera.Render            8.7ms      ← rendering
       ├── Drawing              7.1ms
       └── PostProcessing       1.6ms
5. El número en ms indica cuánto tardó cada parte
6. Buscar lo que ocupa más ms → eso es el cuello de botella
```

**Panel Stats (alternativa rápida):**

```
Activar: ventana Game → botón "Stats" (arriba a la derecha)
Muestra en tiempo real:
- FPS: frames por segundo
- Draw Calls: cuántas llamadas de renderizado por frame
- Tris: triángulos renderizados
- Verts: vértices renderizados
- Used Memory: memoria usada por la app

Referencia para AR móvil:
- Draw Calls < 100 (ideal < 50)
- Tris < 100,000
- Memory < 1.5 GB
```

**PREGUNTA AL GRUPO 2:**

> "En el Profiler ven esto: Camera.Render toma 45ms por frame. ¿Cuál es la causa más probable y qué técnicas aplicarían primero?"

**Respuesta esperada:**
45ms en Camera.Render indica que el cuello de botella está en el rendering (GPU/draw calls), no en los scripts. Las causas más probables son: demasiados draw calls (muchos objetos sin batching), shaders complejos (Fragment Shaders pesados), sombras en tiempo real (shadow maps son muy costosos en móvil), post-processing (bloom, depth of field — muy pesados). Las técnicas a aplicar en orden de impacto: 1) Activar Static Batching para objetos que no se mueven, 2) Desactivar sombras en tiempo real (usar baked lighting), 3) Desactivar post-processing en móvil, 4) Reducir resolución de rendering (render scale < 1.0).

---

**Mini actividad (3 min):**
Cada estudiante abre su proyecto de grupo en Unity, activa el panel Stats durante Play, y anota: FPS actual, Draw Calls, Tris. Compartir con el grupo — ¿cuál tiene más draw calls?

---

### 3.3 Draw Calls y Batching (15 min)

**Explicación conceptual:**

Un **Draw Call** es una instrucción que la CPU envía a la GPU diciéndole "renderiza este objeto". El problema: cada Draw Call tiene un overhead de comunicación CPU→GPU.

```
Sin optimización:
100 objetos = 100 Draw Calls = 100 × overhead = problema

Con batching:
100 objetos del mismo material = 1 Draw Call = mucho mejor
```

**Tipos de Batching en Unity:**

```
┌─────────────────────────────────────────────────────────────────────┐
│ TIPO              │ CÓMO ACTIVAR        │ CONDICIÓN              │
├─────────────────────────────────────────────────────────────────────┤
│ Static Batching   │ Inspector → Static ☑│ Objeto NO se mueve     │
│                   │ Project Settings →  │ Mismo material          │
│                   │ Player → Static     │ Máx. 64,000 verts      │
├───────────────────┼─────────────────────┼────────────────────────┤
│ Dynamic Batching  │ Project Settings →  │ Objeto SÍ se mueve     │
│                   │ Graphics → Dynamic  │ Mismo material          │
│                   │ Batching ☑          │ < 300 vértices/objeto  │
├───────────────────┼─────────────────────┼────────────────────────┤
│ GPU Instancing    │ Material → Enable   │ Mismo mesh             │
│                   │ GPU Instancing ☑    │ Mismo material         │
│                   │                     │ Muchas instancias      │
└─────────────────────────────────────────────────────────────────────┘
```

**Ejemplo práctico:**

```
Escena: 200 árboles idénticos con el mismo material

Sin optimización: 200 Draw Calls
Con GPU Instancing: 1 Draw Call (200 instancias)
Mejora: 199 Draw Calls eliminados → +40 FPS en móvil típico

Cómo activar GPU Instancing:
1. Seleccionar el Material del árbol
2. Inspector → "Enable GPU Instancing" ☑
3. Todos los objetos con ese material → 1 Draw Call
```

**Código para instanciar con GPU Instancing:**

```csharp
using UnityEngine;

public class SpawnInstanciado : MonoBehaviour
{
    public Mesh meshObjeto;            // el mesh que se instanciará
    public Material materialInstancia; // material con GPU Instancing activado
    public int cantidad = 200;

    // MaterialPropertyBlock: permite pasar propiedades únicas por instancia
    // (ej: color diferente) sin romper el batching
    private MaterialPropertyBlock propBlock;
    private Matrix4x4[] matrices; // posición/rotación/escala de cada instancia

    void Start()
    {
        propBlock = new MaterialPropertyBlock();
        matrices  = new Matrix4x4[cantidad];

        for (int i = 0; i < cantidad; i++)
        {
            // Posición aleatoria en un área de 100x100m
            Vector3 pos = new Vector3(
                Random.Range(-50f, 50f),
                0f,
                Random.Range(-50f, 50f));

            // Rotación aleatoria en Y
            Quaternion rot = Quaternion.Euler(0, Random.Range(0f, 360f), 0);

            // Escala ligeramente variable para naturalidad
            Vector3 escala = Vector3.one * Random.Range(0.8f, 1.2f);

            matrices[i] = Matrix4x4.TRS(pos, rot, escala);
        }
    }

    void Update()
    {
        // Renderizar todas las instancias en 1 Draw Call
        Graphics.DrawMeshInstanced(meshObjeto, 0, materialInstancia, matrices);
    }
}
```

**PREGUNTA AL GRUPO 3:**

> "¿Por qué GPU Instancing requiere que todos los objetos usen exactamente el mismo material, y cómo solucionarían el caso donde necesitan que cada árbol tenga un color ligeramente diferente?"

**Respuesta esperada:**
GPU Instancing envía toda la información de las instancias al GPU en una sola llamada. Para que el GPU pueda procesar todas a la vez, deben compartir el mismo shader y material base. Si cada árbol tiene un material diferente, el GPU necesita cambiar de estado entre cada uno → múltiples draw calls de nuevo. La solución para colores diferentes es `MaterialPropertyBlock`: permite pasar propiedades por instancia (como color) sin crear un material nuevo por objeto. El shader debe estar configurado para leer propiedades de instancia con `UNITY_INSTANCING_BUFFER`. Esto mantiene el 1 Draw Call mientras permite variación por instancia.

---

**Mini actividad (4 min):**
En el proyecto de grupo: contar cuántos objetos comparten el mismo material. Proponer cuáles serían candidatos para GPU Instancing o Static Batching.

---

### 3.4 LOD Groups — Niveles de Detalle (10 min)

**Explicación conceptual:**

Level of Detail (LOD) es el principio de mostrar modelos 3D más simples cuando están lejos de la cámara. El usuario no nota la diferencia en objetos pequeños o lejanos, pero el GPU sí: menos triángulos = menos trabajo.

```
Ejemplo — Un árbol con LOD:

Distancia 0-5m:   LOD 0 → Modelo HIGH  (10,000 triángulos) ← el usuario lo ve de cerca
Distancia 5-20m:  LOD 1 → Modelo MED   (3,000 triángulos)  ← lejos, no nota diferencia
Distancia 20-50m: LOD 2 → Modelo LOW   (500 triángulos)    ← muy lejos
Distancia > 50m:  Culled              (0 triángulos)       ← no se renderiza
```

**Cómo configurar LOD Groups en Unity:**

```
1. Seleccionar el GameObject del objeto
2. Add Component → LOD Group
3. En el componente LOD Group:
   - LOD 0: arrastrar el GameObject con el modelo HIGH
   - LOD 1: arrastrar el GameObject con el modelo MEDIUM
   - LOD 2: arrastrar el GameObject con el modelo LOW
   - Culled: el objeto desaparece (0% de pantalla ocupado)
4. Ajustar los porcentajes de pantalla:
   - LOD 0: cuando el objeto ocupa > 60% de la pantalla
   - LOD 1: 20-60% de pantalla
   - LOD 2: 5-20% de pantalla
   - Culled: < 5% de pantalla
```

**PREGUNTA AL GRUPO 4:**

> "¿Tiene sentido aplicar LOD Groups a una escena AR donde el usuario puede acercarse físicamente a los objetos virtuales? Justifiquen con un ejemplo concreto."

**Respuesta esperada:**
Sí, tiene sentido pero con configuración diferente. En AR, el usuario puede acercarse al objeto real (caminar físicamente hacia él), por lo que el LOD debe basarse en la distancia real entre la cámara y el objeto virtual. La diferencia con VR: en VR la "distancia" está controlada por el joystick; en AR puede cambiar rápidamente por movimiento físico. Ejemplo: un modelo de motor industrial colocado en AR en una fábrica — los técnicos cercanos ven el LOD 0 (alta resolución para inspección), mientras los que están al otro lado del taller ven el LOD 2 (silueta básica). Si todos ven LOD 0 todo el tiempo, el GPU del celular se satura aunque nadie esté cerca.

---

### 3.5 Occlusion Culling (7 min)

**Explicación conceptual:**

Occlusion Culling es el proceso de no renderizar objetos que están detrás de otros objetos (ocultos). Si hay una pared delante de 50 objetos, ¿para qué renderizarlos?

```
Sin Occlusion Culling:
┌─────────────────────┐
│  Cámara             │   Renderiza: Pared + 50 objetos detrás = 51 Draw Calls
│  [==] ─→ [Pared] [obj1][obj2]...[obj50]
└─────────────────────┘

Con Occlusion Culling:
┌─────────────────────┐
│  Cámara             │   Renderiza: solo Pared = 1 Draw Call
│  [==] ─→ [Pared]   │   Los objetos detrás: CULLED (no se envían al GPU)
└─────────────────────┘
```

**Cómo activar en Unity:**

```
1. Window → Rendering → Occlusion Culling
2. En la ventana que abre: pestaña "Object"
   - Marcar todos los objetos estáticos (Static → Occluder Static o Occludee Static)
   - Occluder: el objeto que bloquea (paredes, suelo)
   - Occludee: el objeto que se puede ocultar (muebles, props)
3. Pestaña "Bake" → clic en "Bake"
   Unity calcula qué ve la cámara desde cada posible posición
4. En Play: los objetos ocultos no se renderizarán automáticamente
```

**PREGUNTA AL GRUPO 5:**

> "¿Occlusion Culling tiene utilidad en una escena AR donde el ambiente real es dinámico (personas que se mueven, puertas que abren)? ¿Por qué sí o no?"

**Respuesta esperada:**
En AR estándar (sin oclusión real del ambiente físico), los objetos virtuales no pueden ser "tapados" por objetos físicos del mundo real — el celular no conoce la geometría del mundo real en tiempo real (solo detecta planos). Por eso el Occlusion Culling de Unity (que usa geometría 3D del mundo virtual) tiene utilidad limitada en AR pura. Sin embargo: 1) Si la escena AR tiene objetos virtuales que se tapan entre sí (ej: un edificio virtual tapa a otros edificios virtuales), el culling sí aplica. 2) En VR o MR (Mixed Reality), donde sí hay geometría del mundo real mapeada (HoloLens Spatial Mapping), el culling cobra mucha importancia. En AR móvil básica, el impacto es menor — más útil son el batching y los LODs.

---

### 3.6 Gestión de texturas y memoria (8 min)

**Explicación conceptual:**

Las texturas son el mayor consumo de memoria en apps 3D. Una textura de 4K sin comprimir:
```
4096 × 4096 píxeles × 4 bytes (RGBA) = 64 MB por textura
Si tienes 20 texturas así = 1.28 GB de memoria de GPU
Celular típico tiene 2-4 GB de RAM total → problema crítico
```

**Formatos de compresión para móvil:**

```
┌─────────────────────────────────────────────────────────────────┐
│ FORMATO  │ PLATAFORMA      │ RATIO │ CALIDAD   │ CUÁNDO USAR   │
├─────────────────────────────────────────────────────────────────┤
│ ETC2     │ Android (OpenGL)│ 6:1   │ Buena     │ Android básico│
│ ASTC     │ Android moderno │ 4-25:1│ Excelente │ Android 2019+ │
│ PVRTC    │ iOS/A-series    │ 4-8:1 │ Buena     │ iOS           │
│ BC/DXT   │ PC/Desktop      │ 4-8:1 │ Excelente │ PC/Mac        │
└─────────────────────────────────────────────────────────────────┘

En Unity:
Inspector → Texture → Format: Automatic (deja que Unity elija por plataforma)
O forzar ASTC 6x6 para Android moderno
```

**Configuración óptima de texturas:**

```
Inspector de cada textura:
- Max Size: 512 o 1024 (no 4096 en móvil)
- Generate Mipmaps: ☑ (reduce aliasing y mejora rendimiento)
- Compression: Normal Quality
- Filter Mode: Bilinear (no Trilinear en móvil — más costoso)
```

**PREGUNTA AL GRUPO 6:**

> "Un estudiante tiene una escena AR con una textura de logotipo de empresa (256×256 con un ícono simple). ¿Qué tamaño de textura recomendarían y por qué?"

**Respuesta esperada:**
Para un logotipo simple de 256×256, esa resolución podría ser adecuada si el objeto no ocupa mucho espacio en pantalla. Pero lo importante es: 1) Asegurarse de que Generate Mipmaps esté activo (Unity genera versiones más pequeñas automáticamente para distancias), 2) Usar formato comprimido (ETC2 o ASTC), 3) Si el logotipo se ve de cerca (objeto grande en AR), una textura de 512×512 puede mejorar la nitidez. Error común: usar texturas de 4096 para logos simples porque "se ve mejor en Photoshop" — en el celular esa diferencia es imperceptible pero el costo de memoria es 256x mayor que una de 512.

---

### 3.7 Optimización de scripts — GC y Update (7 min)

**Explicación conceptual:**

Los scripts mal escritos son culpables de muchos problemas de rendimiento. Los principales patrones de error:

**Patrón 1 — Crear objetos en Update (GC pressure):**
```csharp
// ❌ MAL — crea una nueva cadena cada frame (60 GC allocations/seg)
void Update()
{
    string texto = "Puntos: " + puntos.ToString();
    textoUI.text = texto;
}

// ✅ BIEN — actualiza solo cuando cambia el valor
void AgregarPuntos(int valor)
{
    puntos += valor;
    textoUI.text = "Puntos: " + puntos.ToString();  // solo cuando cambia
}
```

**Patrón 2 — Buscar componentes en Update:**
```csharp
// ❌ MAL — GetComponent es lento, llamarlo 60 veces/seg es costoso
void Update()
{
    GetComponent<Renderer>().material.color = Color.red; // busca cada frame
}

// ✅ BIEN — cachear la referencia en Awake/Start
private Renderer cachedRenderer;
void Awake() => cachedRenderer = GetComponent<Renderer>(); // una sola vez
void Update() => cachedRenderer.material.color = Color.red;  // rápido
```

**Patrón 3 — Find/FindObjectOfType en Update:**
```csharp
// ❌ CRÍTICO — busca entre TODOS los objetos de la escena cada frame
void Update()
{
    GameObject.Find("Player").transform.position;   // ¡muy lento!
}

// ✅ BIEN — serializar la referencia o cachear en Start
[SerializeField] private Transform player;  // arrastrar desde Inspector
```

**Patrón 4 — Corrutinas vs Update para timers:**
```csharp
// ❌ MAL — timer en Update crea carga cada frame
private float timer = 0f;
void Update()
{
    timer += Time.deltaTime;
    if (timer > 5f) { HacerAlgo(); timer = 0f; }
}

// ✅ MEJOR — corrutina solo corre cuando es necesaria
IEnumerator TimerCorrutina()
{
    yield return new WaitForSeconds(5f);
    HacerAlgo();
    // No hay Update corriendo en los 5 segundos de espera
}
```

**PREGUNTA AL GRUPO 7:**

> "Un estudiante tiene un juego a 45 FPS en el editor pero a 12 FPS en el celular. El Profiler muestra que 'GC.Collect' toma 40ms cada ~2 segundos. ¿Cuál es la causa y cómo la encontrarían?"

**Respuesta esperada:**
El Garbage Collector (GC) se activa cuando hay suficiente basura acumulada en el heap. 40ms de GC.Collect significa que hay muchos objetos temporales creados en cada frame. Para encontrar la causa: en el Profiler, cambiar a la vista "Memory" y buscar la columna "GC Alloc" — muestra cuántos bytes de basura crea cada función en cada frame. Las causas más comunes: strings concatenados con "+" en Update, new List<>() dentro de Update, LINQ queries (que crean IEnumerables temporales), boxing/unboxing de value types. Solución: buscar todas las funciones que tienen GC Alloc > 0 por frame y refactorizarlas usando los patrones de caching.

---

### 3.8 Optimizaciones específicas AR y VR (7 min)

**Para AR móvil (ARCore/ARFoundation):**

```
1. ARPlaneManager: desactivar cuando no es necesario
   arPlaneManager.enabled = false; // después de colocar el objeto
   // Los planes seguirán visibles pero dejan de detectarse → ahorra CPU

2. Resolución de cámara AR:
   // En lugar de la resolución máxima del celular, usar 720p
   // ARCameraManager configura la resolución de captura
   // Resolución menor → procesamiento ARCore más rápido

3. Lighting Estimation:
   // ARCore estima la luz del ambiente (útil para realismo)
   // Pero es costoso en CPU → desactivar si no es crítico para el proyecto

4. Anchors: limpiar anchors que ya no se usan
   // Cada ARAnchor consume CPU para tracking
   anchorManager.RemoveAnchor(anchorViejo);
```

**Para VR (Quest/PICO):**

```
1. Fixed Foveated Rendering (FFR):
   // Renderiza menor resolución en los bordes del campo visual
   // Los bordes del ojo humano tienen menor acuidad → el usuario no lo nota
   // Ahorra ~20% de carga GPU
   OVRManager.fixedFoveatedRenderingLevel = OVRManager.FixedFoveatedRenderingLevel.High;

2. Single Pass Stereo Rendering:
   // En lugar de renderizar 2 veces (un ojo izquierdo, uno derecho)
   // Unity renderiza 1 sola vez y copia al segundo ojo con ajuste de parallax
   // Project Settings → XR Plug-in Management → Stereo Rendering Mode → Single Pass Instanced

3. Eliminar post-processing en VR:
   // Bloom, vignette, color grading — muy costosos y causan ghosting en VR
   // Usar solo FXAA (anti-aliasing ligero) o MSAA x2

4. Target Frame Rate:
   Application.targetFrameRate = 90;  // Quest 2
   // O 72 si la escena es pesada
```

**PREGUNTA AL GRUPO 8:**

> "¿Por qué el Single Pass Stereo Rendering puede no ser compatible con algunos shaders custom y qué tendrían que cambiar en el shader para que funcione?"

**Respuesta esperada:**
Single Pass Stereo Rendering usa un texture array para los dos ojos y una sola llamada de renderizado. Los shaders custom que no están escritos para esto no saben en qué índice del array están renderizando. Unity ofrece macros como `UNITY_STEREO_EYE_INDEX` que deben incluirse en los shaders para que funcionen. Los shaders del Unity Standard y URP ya incluyen soporte. El problema ocurre con shaders escritos manualmente sin usar estas macros — el resultado puede ser que un ojo vea la imagen correcta y el otro una imagen incorrecta o negra. La solución: reescribir los shaders usando las macros del XR SDK de Unity.

---

## RECESO (20 min) {#receso}

---

## 4. PRÁCTICA (40 min) {#practica}

### a) CASO PRÁCTICO GRUPAL (25 min)

**Escenario:**

> *PeruTech XR* desarrolló una app de entrenamiento VR para técnicos de la empresa de distribución eléctrica ENEL Perú. La app muestra una subestación eléctrica virtual de tamaño real donde los técnicos practican procedimientos de mantenimiento. La escena tiene: 850 objetos 3D (cables, interruptores, tableros, postes), 45 materiales diferentes, sombras en tiempo real, post-processing con bloom, y 12 scripts corriendo en Update(). El problema: corre a 18 FPS en Meta Quest 2 (Meta requiere 72 FPS mínimo para publicar). La empresa tiene 3 semanas para optimizar o pierde el contrato.

**Datos del Profiler en el estado actual:**

| Métrica | Valor actual | Valor objetivo |
|---------|-------------|---------------|
| FPS | 18 | 72 |
| Draw Calls | 847 | < 100 |
| Triángulos | 2.8M | < 500K |
| GC Alloc/frame | 4.2 KB | 0 KB |
| Script Update ms | 12ms | < 3ms |
| Camera.Render ms | 42ms | < 8ms |

**Tarea grupal (grupos de 3-4):**

1. Identificar los 5 problemas de rendimiento más críticos basándose en los datos del Profiler
2. Proponer la técnica de optimización específica para cada problema
3. Estimar qué impacto tendría cada técnica en los FPS (razonar, no calcular exactamente)
4. Definir el orden de implementación (de mayor a menor impacto)
5. ¿Qué problema NO se puede resolver sin cambiar el contenido 3D (assets)?

**Producto esperado:** una tabla de plan de optimización con: problema identificado → técnica → impacto estimado → prioridad.

**Preguntas de andamiaje que el docente hace mientras circula:**
- "El problema de 847 Draw Calls — ¿cuántos de los 850 objetos podrían ser estáticos (no se mueven durante el entrenamiento)?"
- "¿Qué pasa con los triángulos? ¿LOD Groups o reducción manual de polígonos?"
- "El 4.2KB de GC Alloc por frame — ¿en qué contexto se vuelve crítico: 60 FPS normal vs spike de GC cada 2 segundos?"
- "¿El post-processing con bloom es necesario en una app de entrenamiento industrial? ¿Qué función cumple estéticamente?"
- "Si los 12 scripts en Update podrían convertirse en corrutinas o eventos, ¿cuáles serían candidatos?"

**Respuesta modelo (puesta en común):**

| # | Problema | Técnica | Impacto estimado | Prioridad |
|---|----------|---------|-----------------|-----------|
| 1 | 847 Draw Calls | Static Batching (objetos fijos) + GPU Instancing (cables repetidos) | -60% Draw Calls → +25 FPS | 1ª |
| 2 | 2.8M triángulos | LOD Groups en todos los objetos (3 niveles) | -70% tris en distancia → +20 FPS | 2ª |
| 3 | Sombras tiempo real | Baked Lighting (lightmaps) — subestación es estática | -15ms render → +30 FPS | 1ª (empate) |
| 4 | Post-processing bloom | Desactivar bloom (innecesario en entrenamiento industrial) | -5ms → +10 FPS | 3ª |
| 5 | GC Alloc + Scripts | Cachear GetComponent, eliminar strings en Update | -4ms CPU → +8 FPS | 4ª |

Problema que NO se resuelve sin cambiar assets: los 2.8M de triángulos requieren retocar o recrear los modelos 3D en menor densidad de polígonos. El LOD ayuda a distancia, pero el LOD 0 (de cerca) seguirá siendo pesado.

---

### b) EJERCICIO INDIVIDUAL (15 min)

**Tarea:** analizar el siguiente script y encontrar todos los problemas de rendimiento. Luego reescribir la versión optimizada.

```csharp
// ❌ VERSIÓN CON PROBLEMAS — encontrar y corregir TODOS los errores
using UnityEngine;

public class ControladorAR_Lento : MonoBehaviour
{
    public int puntos = 0;

    void Update()
    {
        // Problema 1: ??
        GameObject jugador = GameObject.Find("Jugador");

        // Problema 2: ??
        Renderer r = GetComponent<Renderer>();
        r.material.color = Color.red;

        // Problema 3: ??
        string textoUI = "Puntos: " + puntos.ToString() + " | Tiempo: " + Time.time.ToString("F1");
        FindObjectOfType<UnityEngine.UI.Text>().text = textoUI;

        // Problema 4: ??
        if (Input.GetKeyDown(KeyCode.Space))
            puntos++;
    }
}
```

**Criterio de éxito:** la versión optimizada no tiene `new` ni operaciones costosas en Update, usa caching, y actualiza la UI solo cuando es necesario.

**Solución esperada:**

```csharp
// ✅ VERSIÓN OPTIMIZADA
using UnityEngine;
using UnityEngine.UI;

public class ControladorAR_Optimizado : MonoBehaviour
{
    public int puntos = 0;

    private Transform jugador;     // cacheado en Awake
    private Renderer cachedRend;   // cacheado en Awake
    private Text textoUI;          // cacheado en Awake
    private int ultimosPuntos = -1; // para detectar cambio

    void Awake()
    {
        // Cachear todas las referencias una sola vez
        jugador    = GameObject.Find("Jugador").transform;
        cachedRend = GetComponent<Renderer>();
        textoUI    = FindObjectOfType<Text>(); // válido en Awake (una vez)
        cachedRend.material.color = Color.red; // setear una vez, no cada frame
    }

    void Update()
    {
        // Actualizar UI solo cuando cambia el puntaje
        if (puntos != ultimosPuntos)
        {
            ultimosPuntos = puntos;
            // Usar StringBuilder en proyectos reales; aquí simplificado:
            textoUI.text = "Puntos: " + puntos.ToString();
        }
    }

    // La detección de tecla en Update está bien — es el uso correcto de Update
    // Mover al contexto correcto:
    void Update2() // (nota: esto es solo para el ejercicio — en código real sería el Update)
    {
        if (Input.GetKeyDown(KeyCode.Space))
            puntos++;
    }
}
```

Los 4 problemas identificados:
1. `GameObject.Find` en Update — O(n) búsqueda en toda la escena cada frame
2. `GetComponent<Renderer>()` en Update — búsqueda de componente cada frame
3. String concatenado con "+" en Update — GC allocation cada frame (nuevos strings)
4. `FindObjectOfType<Text>()` en Update — búsqueda en toda la escena cada frame (peor que Find)

---

## 5. CIERRE (10 min) {#cierre}

### a) SÍNTESIS COLABORATIVA (4 min)

**Pregunta 1:**
> "Nombren las 5 técnicas de optimización más importantes que vimos hoy, de mayor a menor impacto en una escena AR típica."

**Respuesta esperada:**
1. Baked Lighting (mayor impacto en rendering — elimina cálculo dinámico de sombras)
2. Static Batching + GPU Instancing (reduce draw calls radicalmente)
3. LOD Groups (reduce triángulos en distancia)
4. Optimización de scripts (elimina GC pressure y CPU spikes)
5. Texture Compression y tamaño correcto (reduce memoria y swap de GPU)

**Pregunta 2:**
> "¿Cuál es la diferencia entre un problema de CPU y un problema de GPU en el Profiler? ¿Cómo los distinguen?"

**Respuesta esperada:**
En el Profiler: problema de CPU se ve como tiempo alto en "Scripts", "Physics", "Update" — el CPU está haciendo demasiado trabajo antes de enviar al GPU. Problema de GPU se ve como tiempo alto en "Camera.Render", "Drawing", "GPU" — el GPU tarda en renderizar. Para distinguirlos: si el tiempo total del frame cae cuando reduces scripts pero no al reducir objetos 3D → CPU bound. Si cae al reducir draw calls/triángulos pero no con menos scripts → GPU bound. La solución es diferente: CPU bound → optimizar scripts/física; GPU bound → batching/LODs/shaders/sombras.

**Pregunta 3:**
> "¿Por qué optimizar debe hacerse con el Profiler y no 'a ojo'?"

**Respuesta esperada:**
La optimización prematura o "a ojo" es el error más costoso en desarrollo. Es común que el cuello de botella esté en un lugar inesperado: un shader aparentemente simple puede ser 10x más lento que uno visual complejo; una función que "parece inofensiva" puede ejecutarse 10,000 veces por frame. El Profiler muestra los números reales — sin datos, es imposible saber qué optimización tendrá impacto. Donald Knuth dijo: "Premature optimization is the root of all evil". La regla: medir primero, optimizar lo que el Profiler señala, medir de nuevo para confirmar el impacto.

---

### b) METACOGNICIÓN (3 min)

> "Respondan mentalmente: ¿qué técnica de hoy aplicarían ESTA SEMANA en su proyecto grupal? ¿Cuál sería el primer paso concreto? Si no saben por dónde empezar en su proyecto, ¿qué les impide abrir el Profiler ahora mismo?"

---

### c) TAREA / PUENTE (3 min)

**Tarea concreta:**

Abrir el proyecto grupal esta semana y:
1. Anotar los FPS actuales y Draw Calls desde el panel Stats
2. Activar el Profiler durante 30 segundos en Play
3. Identificar los 3 procesos que más tiempo toman
4. Aplicar al menos 1 técnica de las vistas hoy
5. Registrar los FPS antes y después

Subir un archivo `optimizacion_s12.md` al repositorio del proyecto con:
```markdown
# Optimización Semana 12
## Estado inicial
- FPS: X | Draw Calls: X | Tris: X
## Técnica aplicada: [nombre]
## Estado final
- FPS: X | Draw Calls: X | Tris: X
## Análisis: ¿qué cambió y por qué?
```

**Conexión con Semana 13:**
> "La semana 13 vemos audio y efectos especiales. Un audio 3D mal configurado puede costar 5ms de CPU por fuente. Todo lo que aprendieron hoy aplica: optimización de Audio Mixer, Audio LOD, e interacción entre efectos de partículas y rendimiento."

---

## GUION VERBAL SUGERIDO {#guion}

**Al abrir el tema del Profiler:**
> "El Profiler de Unity es como un médico que puede ver exactamente dónde duele. Sin él, optimizar es como operar a ciegas. Antes de tocar una sola línea de código, los profesionales siempre miden. Siempre. 'Optimicé a ojo y funcionó' no es una estrategia — es suerte, y la suerte se acaba en producción."

**Al mostrar el código con problemas:**
> "Este código lo escribieron estudiantes de semestres anteriores. ¿Ven los problemas? Spoiler: hay cuatro. Y este tipo de código en un proyecto VR es la diferencia entre 72 FPS y 18 FPS — literalmente la diferencia entre publicar en el Quest Store o no poder."

**Al cerrar la clase:**
> "Hoy no les enseñé a hacer magia. Les enseñé a leer el termómetro del sistema y a conocer los antibióticos. El diagnóstico primero, el tratamiento después. Eso es la optimización profesional."

---

## CASOS REALES RECOMENDADOS {#casos}

1. **Meta Quest Store Performance Requirements (2024):** Meta rechaza automáticamente apps con < 72 FPS. Publicado en Meta Developer Documentation.

2. **Beat Saber optimización (2019):** el equipo de Beat Games (hoy de Meta) documentó cómo pasaron de 45 a 90 FPS en Quest usando principalmente Static Batching y Baked Lighting. (GDC 2019: "Lessons learned from Beat Saber")

3. **Fundamental Surgery VR (UK, 2021):** 8 meses de optimización para pasar de 15 a 90 FPS en Quest 2, documentado en Unity Case Studies 2021.

4. **Niantic (Pokémon GO AR):** equipo dedicado de 12 ingenieros de performance para mantener 30 FPS en la gama más amplia de Android (desde gama baja con Snapdragon 450). Unity Blog, 2022.

5. **Varjo XR-3 (VR industrial, Finlandia):** headset de 35 megapíxeles/ojo requiere técnicas de foveated rendering y occlusion culling en tiempo real — imposible sin ellas. Varjo Developer Docs.

---

## EVALUACIÓN FORMATIVA {#eval}

| Momento | Señal de comprensión | Señal de dificultad |
|---------|---------------------|---------------------|
| Diagnóstico inicial | Da valor de FPS objetivo para VR (72/90) | No sabe qué son los FPS |
| Transformación 3.2 | Lee el Profiler e identifica el cuello de botella | Confunde CPU y GPU bound |
| Transformación 3.3 | Identifica candidatos a batching/instancing | "¿Para qué reducir draw calls?" |
| Práctica grupal | Tabla de plan completa con prioridad correcta | Propone optimizar todo al mismo tiempo |
| Ejercicio individual | Encuentra los 4 problemas en el código | Encuentra solo 1-2 |
| Cierre | Nombra 5 técnicas con impacto | Solo recuerda 1-2 |

---

## REFERENCIAS APA 7 {#referencias}

Unity Technologies. (2024). *Unity Profiler documentation*. https://docs.unity3d.com/Manual/Profiler.html

Unity Technologies. (2024). *Draw call batching*. https://docs.unity3d.com/Manual/DrawCallBatching.html

Unity Technologies. (2024). *Level of Detail (LOD) for meshes*. https://docs.unity3d.com/Manual/LevelOfDetail.html

Meta. (2024). *Quest performance optimization guide*. https://developer.oculus.com/documentation/unity/unity-perf-guidelines/

Knoll, A., & Wahl, T. (2022). *Real-time rendering for XR applications*. ACM SIGGRAPH Asia. https://doi.org/10.1145/3550454.3555501

Sherrod, A., & Jones, W. (2011). *Beginning DirectX 11 game programming*. Course Technology.

Akenine-Möller, T., Haines, E., & Hoffman, N. (2018). *Real-time rendering* (4th ed.). A K Peters/CRC Press.

Luban, P. (2020). *Practical game design: Learn the art and science of game design through applicable skills and cutting-edge insights*. Packt Publishing.

*PSISP08075 | Universidad Autónoma del Perú | Ingeniería de Sistemas | 2026-1*
