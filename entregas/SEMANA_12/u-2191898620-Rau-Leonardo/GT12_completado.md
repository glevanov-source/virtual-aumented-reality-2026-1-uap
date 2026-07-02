# S12 — GUÍA DE TRABAJO DEL ESTUDIANTE
## Curso: Realidad Virtual y Aumentada | PSISP08075
### Semana 12 — Optimización y Rendimiento en XR

---

> **Instrucciones:** Resuelve todas las secciones en orden. Tiempo sugerido: 90 minutos.
> Entrega en el repositorio del curso como archivo `.md` con tu rama `u-codigo-apellido-nombre-gt12`.
> **No está permitido** buscar respuestas en internet para la Sección A y B.

| Campo | Detalle |
|-------|---------|
| **Estudiante** | Rau Bravo Leonardo Cesar |
| **Código** | 2191898620 |
| **Fecha** | 1/07/2026 |
| **Semana** | 12 — Optimización y Rendimiento en XR |

---

## SECCIÓN A — PREGUNTAS DE OPCIÓN MÚLTIPLE (20 puntos — 2 pts c/u)

**Pregunta 1** *(básica)*

¿Cuántos FPS mínimos exige Meta para aprobar una app en el Quest Store?

- a) 30 FPS
- b) 60 FPS
- c) 72 FPS
- d) 120 FPS

**Respuesta:** c

---

**Pregunta 2** *(básica)*

¿Cuál es la herramienta principal de Unity para diagnosticar problemas de rendimiento en tiempo real?

- a) Inspector
- b) Unity Profiler
- c) Package Manager
- d) Scene View Stats

**Respuesta:** b

---

**Pregunta 3** *(básica)*

¿Qué es un "Draw Call" en el contexto del rendering de Unity?

- a) Una función de C# que dibuja texto en pantalla
- b) Una instrucción que la CPU envía a la GPU para renderizar un objeto
- c) Una llamada al sistema operativo para mostrar la ventana de Unity
- d) El proceso de importar modelos 3D al proyecto

**Respuesta:** b

---

**Pregunta 4** *(básica)*

¿Qué condición debe cumplir un objeto para poder aplicarle Static Batching?

- a) Debe tener un Rigidbody
- b) Debe estar marcado como "Static" y no moverse en runtime
- c) Debe tener exactamente el mismo mesh que otro objeto
- d) Debe estar en la misma capa (Layer) que los otros objetos

**Respuesta:** b

---

**Pregunta 5** *(intermedia)*

Un modelo 3D tiene 50,000 triángulos en LOD 0. Si el usuario está a 30 metros de distancia y el objeto ocupa el 10% de la pantalla, ¿qué debería mostrar un LOD Group bien configurado?

- a) LOD 0 (50,000 triángulos) porque el objeto está visible
- b) LOD 1 o LOD 2 con menos triángulos, porque el usuario no nota la diferencia a esa distancia
- c) "Culled" (no renderiza) porque el objeto es pequeño en pantalla
- d) LOD 0 siempre, independientemente de la distancia

**Respuesta:** b

---

**Pregunta 6** *(intermedia)*

Un desarrollador tiene 300 árboles idénticos en su escena con el mismo material. ¿Qué técnica produce mayor reducción de Draw Calls?

- a) Static Batching, porque los árboles no se mueven
- b) GPU Instancing, porque son instancias del mismo mesh con el mismo material
- c) LOD Groups, porque reduce la cantidad de triángulos
- d) Occlusion Culling, porque no renderiza los que están ocultos

**Respuesta:** b

---

**Pregunta 7** *(intermedia)*

¿Por qué crear `new List<>()` dentro de `void Update()` es un problema de rendimiento?

- a) Porque List no se puede crear dentro de métodos de Unity
- b) Porque genera un objeto nuevo en el heap en cada frame, aumentando la presión sobre el Garbage Collector
- c) Porque Unity no soporta listas genéricas en tiempo de ejecución
- d) Porque List.Add() es más lento que los arrays simples

**Respuesta:** b

---

**Pregunta 8** *(avanzada)*

En el Unity Profiler, observas que `Camera.Render` toma 45ms y `Scripts Update` toma 2ms por frame. ¿Qué tipo de problema tiene la aplicación?

- a) CPU bound — los scripts son el cuello de botella
- b) GPU bound — el problema está en el rendering
- c) Memory bound — hay demasiada memoria usada
- d) Physics bound — las colisiones son el problema

**Respuesta:** b

---

**Pregunta 9** *(avanzada)*

¿Cuál es la ventaja del formato de textura ASTC sobre una textura sin comprimir (RGBA32) en Android?

- a) ASTC es más rápido de cargar desde el disco pero ocupa más memoria en GPU
- b) ASTC reduce el tamaño en memoria de GPU hasta en un 75-80%, mejorando el rendimiento
- c) ASTC solo funciona en dispositivos con Android 10 o superior
- d) ASTC mejora la calidad visual pero no tiene impacto en rendimiento

**Respuesta:** b

---

**Pregunta 10** *(avanzada)*

¿Qué es el "GC.Collect" que aparece en el Unity Profiler y por qué causa stutters?

- a) Un método que recolecta datos de juego para análisis estadístico
- b) El proceso del Garbage Collector que libera memoria de objetos no usados, pausando temporalmente todos los demás procesos
- c) Una llamada automática que guarda el estado del juego cada cierto tiempo
- d) El proceso de Unity para compilar los scripts en tiempo de ejecución

**Respuesta:** b

---

## SECCIÓN B — COMPLETAR Y RELACIONAR (20 puntos)

### Parte B1 — Completar espacios en blanco (10 pts — 2 pts c/u)

1. El valor de tiempo por frame para alcanzar 90 FPS es __11.11 ms__ milisegundos. Si algún proceso supera ese valor, la app baja de 90 FPS.

2. Para aplicar GPU Instancing, el material del objeto debe tener activada la opción __Enable GPU Instancing__ en el Inspector del material.

3. El comando de Unity para acceder al Profiler es: __Window__ → Analysis → Profiler.

4. Cuando un objeto 3D tiene LOD configurado y la cámara se aleja lo suficiente, el sistema lo marca como __Culled__, es decir, deja de renderizarlo completamente.

5. `GetComponent<Renderer>()` llamado en `void Start()` (método de inicio, una sola vez) es la práctica correcta para cachear la referencia.

---

### Parte B2 — Relacionar columnas (10 pts — 1 pt c/u)

| Columna A — Técnica / Concepto | Tu respuesta | Columna B — Descripción |
|-------------------------------|-------------|------------------------|
| 1. Static Batching | C | A. No renderiza objetos que están detrás de otros |
| 2. GPU Instancing | D | B. Reduce triángulos según la distancia a la cámara |
| 3. LOD Group | B | C. Agrupa objetos estáticos con el mismo material en 1 Draw Call |
| 4. Occlusion Culling | A | D. Renderiza muchas copias del mismo mesh en 1 Draw Call |
| 5. Baked Lighting | E | E. Pre-calcula la iluminación y la guarda en texturas |
| 6. Single Pass Stereo | F | F. Renderiza ambos ojos en una sola pasada (VR) |
| 7. GC Alloc | G | G. Memoria en heap generada por objetos temporales |
| 8. MaterialPropertyBlock | H | H. Permite propiedades únicas por instancia sin romper batching |
| 9. Draw Call | I | I. Instrucción CPU → GPU para renderizar un objeto |
| 10. Fixed Foveated Rendering | J | J. Menor resolución en bordes del campo visual VR |

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

1. Uso de `GameObject.Find()` dentro de `Update()`
El primer problema es el uso de `GameObject.Find("CuboAR")` dentro del método `Update()`. Esta función obliga a Unity a buscar el objeto por nombre en toda la jerarquía de la escena cada frame. En escenas pequeñas puede no notarse, pero en proyectos AR o VR con muchos objetos esta búsqueda consume CPU innecesariamente y puede afectar el rendimiento.

2. Uso repetido de `GetComponent<Renderer>()`
El segundo problema es llamar a `GetComponent<Renderer>()` en cada frame. Aunque no es tan costoso como `GameObject.Find`, Unity igualmente debe buscar el componente dentro del objeto cada vez que se ejecuta `Update()`. Repetir esta operación continuamente genera trabajo extra para la CPU, cuando lo ideal sería obtener la referencia una sola vez y reutilizarla.

3. Uso de `Debug.Log()` en cada frame
Otro problema importante es el uso de `Debug.Log()` dentro de `Update()`. Como `Update()` se ejecuta decenas de veces por segundo, se generan demasiados mensajes en la consola. Esto aumenta el consumo de CPU, ralentiza el editor y además puede generar asignaciones temporales de memoria debido a la concatenación de strings, provocando pausas del Garbage Collector.

b) Reescribe el código optimizado manteniendo la funcionalidad (el color sigue cambiando con el tiempo). (4 pts)

```csharp
using UnityEngine;

public class CambioColorAR : MonoBehaviour
{
    private Renderer cuboRenderer;

    void Start()
    {
        GameObject obj = GameObject.Find("CuboAR");
        cuboRenderer = obj.GetComponent<Renderer>();
    }

    void Update()
    {
        cuboRenderer.material.color = new Color(
            Mathf.Sin(Time.time),
            0.5f,
            0.5f
        );
    }
}
```

### Pregunta C2 (8 pts)

Compara **Baked Lighting** y **Real-time Lighting** en el contexto de una app AR móvil:

a) Explica cuál es más apropiada para una escena AR estática (objetos que no se mueven) y justifica tu respuesta en términos de rendimiento. (4 pts)

Para una escena AR estática, Baked Lighting es más apropiada porque la iluminación se precalcula y se guarda en lightmaps, evitando cálculos de luces y sombras en cada frame. Esto reduce el uso de CPU y GPU, mejorando el rendimiento y manteniendo FPS estables en dispositivos móviles. En cambio, Real-time Lighting consume más recursos porque calcula la iluminación en tiempo real.

b) ¿En qué escenario específico de AR sería NECESARIO usar iluminación en tiempo real en lugar de baked? (4 pts)

La iluminación en tiempo real sería necesaria en una app AR cuando los objetos virtuales o las fuentes de luz cambian constantemente, por ejemplo en una experiencia AR donde el usuario mueve una linterna virtual o interactúa con objetos que cambian de posición. En ese caso, las luces y sombras deben actualizarse dinámicamente para mantener una iluminación realista, algo que Baked Lighting no puede hacer.

---

### Pregunta C3 — Mini caso de análisis (14 pts)

**Situación:** Tu equipo desarrolló la siguiente experiencia AR para el proyecto grupal: una sala de exhibición virtual con 150 cuadros 3D (cada uno con textura única de 2048×2048 sin comprimir), 20 luces en tiempo real, un script `ExhibicionManager.cs` que en `Update()` busca todos los cuadros con `FindObjectsOfType<CuadroAR>()` y calcula la distancia a la cámara, y sombras activadas en todos los objetos. El resultado: 8 FPS en un celular Snapdragon 720G.

**Pregunta C3a** (7 pts): Identifica los 4 problemas principales de rendimiento en orden de gravedad. Para cada uno: nombra el problema, explica por qué es grave y propón la técnica de optimización específica.

**1. Texturas 2048×2048 sin comprimir**
El problema más grave es el uso de 150 texturas de 2048×2048 sin compresión, ya que consumen una enorme cantidad de memoria GPU y ancho de banda. Esto puede causar saturación de VRAM, stutters y caídas severas de FPS. La optimización recomendada es comprimir las texturas con ASTC y reducir resolución (por ejemplo a 1024×1024 o 512×512 cuando sea posible).

**2. 20 luces en tiempo real**
Tener 20 luces dinámicas en una app AR móvil es extremadamente costoso porque Unity debe recalcular iluminación y sombras continuamente. Esto sobrecarga especialmente la GPU. La solución es reemplazar la mayoría por Baked Lighting y dejar solo las luces dinámicas realmente necesarias.

**3. Sombras activadas en todos los objetos**
Las sombras en todos los cuadros incrementan mucho el costo de rendering, ya que generan múltiples cálculos adicionales por frame. En móviles esto afecta bastante el rendimiento. La optimización consiste en desactivar sombras innecesarias, usar sombras baked o limitar sombras solo a objetos importantes.

**4. `FindObjectsOfType<CuadroAR>()` dentro de `Update()`**
El script ejecuta búsquedas globales en cada frame, lo cual genera una carga innecesaria en CPU. Unity debe recorrer todos los objetos constantemente para encontrarlos. La solución es cachear las referencias una sola vez en `Start()` o `Awake()` y reutilizarlas.

**Pregunta C3b** (7 pts): El docente dice: "Primero midan, después optimicen". Describe el proceso exacto de 5 pasos que seguirías en Unity para diagnosticar correctamente el problema antes de tocar una sola línea de código o un solo ajuste de material.

Antes de realizar cualquier optimización, primero abriría el Unity Profiler desde Window → Analysis → Profiler, ya que esta herramienta permite medir con precisión el rendimiento de la aplicación en tiempo real. Luego ejecutaría la escena directamente en el celular Snapdragon 720G, porque el rendimiento dentro del editor de Unity puede ser muy diferente al de un dispositivo móvil real. Una vez obtenidos los datos, analizaría qué componente está consumiendo más tiempo por frame, identificando si el principal cuello de botella proviene de la CPU, la GPU, la memoria o el sistema de físicas. Después, revisaría métricas más específicas como Draw Calls, uso de memoria, cantidad de triángulos, GC Alloc y el tiempo de Camera.Render, ya que estos indicadores permiten localizar con mayor exactitud la causa del bajo rendimiento. Finalmente, con toda esta información, aplicaría las optimizaciones de manera progresiva y volvería a medir después de cada cambio, asegurándome de comprobar objetivamente qué mejoras realmente aumentan el rendimiento antes de modificar más elementos del proyecto.

---

## SECCIÓN D — PREGUNTAS AVANZADAS Y DE CASO (30 puntos)

### Caso profesional (15 pts)

**Contexto:** La empresa *Lima XR Studios* ganó un contrato para desarrollar una app de entrenamiento VR para mineros de la empresa Antamina (Perú). La simulación muestra un túnel minero de 500m de largo con: 2,000 objetos 3D (rocas, equipos, señales de seguridad), 8 tipos de materiales distintos (pero muchos objetos comparten el mismo material), explosiones de partículas dinámicas, física activa para simular caída de rocas, y 15 scripts de lógica de entrenamiento corriendo en Update. El headset objetivo es Meta Quest 2 (GPU: Adreno 650, 6GB RAM).

**Pregunta D1** (5 pts): Diseña una **estrategia de optimización completa** para este proyecto. Organiza tu respuesta en una tabla con: técnica aplicada, por qué aplica a este caso específico, impacto esperado en FPS, y si requiere modificar los assets 3D o solo configuración de Unity.

| Técnica aplicada | Por qué aplica en este caso | Impacto esperado en FPS | ¿Assets o configuración? |
|------------------|-----------------------------|--------------------------|---------------------------|
| GPU Instancing | Hay muchos objetos repetidos (rocas, señales) con el mismo material | Alto (reduce significativamente los Draw Calls) | Configuración + materiales |
| Static Batching | Objetos del túnel que no se mueven (paredes, rocas fijas) | Medio-Alto | Configuración |
| Occlusion Culling | El túnel es largo y muchos objetos no son visibles al mismo tiempo | Alto | Configuración |
| LOD Groups | Objetos lejanos no necesitan alto detalle en un túnel de 500m | Alto | Assets 3D |
| Baked Lighting | La iluminación del túnel es mayormente estática | Muy alto | Configuración |
| Optimización de partículas | Explosiones dinámicas son costosas en GPU | Medio-Alto | Configuración |
| Optimización de scripts (Update) | Múltiples scripts en Update generan carga constante en CPU | Medio | Código |
| Reducción de materiales | 8 materiales pueden unificarse para reducir draw calls | Medio | Assets + materiales |

**Pregunta D2** (5 pts): El físico de "caída de rocas" usa Rigidbody en 500 objetos simultáneamente. Propón una solución que mantenga el realismo visual de la simulación sin comprometer el rendimiento. (Pista: no todas las rocas necesitan física activa al mismo tiempo.)

Para mantener realismo sin destruir el rendimiento, implementaría un sistema de física por proximidad y activación dinámica. En lugar de tener los 500 Rigidbody activos siempre, solo activaría la física en las rocas que estén cerca del jugador o dentro del campo de interacción, mientras que las demás permanecerían como objetos estáticos o incluso desactivados. Además, se podría usar un sistema de simulación por eventos, donde las rocas lejanas no usan física real sino animaciones precomputadas o “fake physics” (animaciones de caída). De esta forma, la simulación mantiene el realismo visual, pero se reduce drásticamente la carga de CPU y el costo de física en el Unity.

**Pregunta D3** (5 pts): "¿Cómo implementarías un sistema de LOD automático en runtime para el túnel, donde el nivel de detalle cambia dinámicamente según la posición del usuario y el FPS actual?"

Implementaría un sistema híbrido que combine LOD basado en distancia y adaptación dinámica por rendimiento. Primero, cada objeto del túnel tendría un LOD Group predefinido con varios niveles de detalle. Luego, un script central monitorearía la posición del usuario en el túnel y ajustaría el LOD según la distancia. Sin embargo, para hacerlo más avanzado, integraría un sistema que también mida el FPS en tiempo real usando el Unity Profiler o un contador interno, de modo que si el FPS baja de cierto umbral (por ejemplo 60 o 72 FPS en VR), el sistema automáticamente reduce el LOD global bajando la calidad de todos los objetos lejanos. Esto permite que la aplicación se adapte dinámicamente al rendimiento del dispositivo, priorizando estabilidad de FPS sobre calidad visual. En resumen, se combina LOD por distancia + ajuste dinámico por FPS, lo cual garantiza una experiencia estable incluso en hardware limitado como Meta Quest 2.

---

### Pregunta de diseño (8 pts)

> "¿Cómo implementarías un sistema de 'Adaptive Quality' para tu proyecto XR que automáticamente reduce la calidad visual cuando el FPS cae por debajo de 60, y la restaura cuando el FPS sube? Describe la lógica del script en pseudocódigo o C# real."

En un proyecto XR como Museum Guide AR, un sistema de Adaptive Quality se implementaría con la idea de monitorear constantemente el rendimiento en tiempo real y ajustar automáticamente los parámetros gráficos para mantener una experiencia fluida. Básicamente, el sistema mediría el FPS promedio cada cierto intervalo de tiempo y, si detecta que el rendimiento cae por debajo de 60 FPS, reduciría automáticamente la carga gráfica disminuyendo elementos como la distancia de sombras, el nivel de detalle (LOD bias), la resolución de render o la calidad de partículas. Por otro lado, si el rendimiento se mantiene estable por encima del umbral, el sistema iría restaurando progresivamente la calidad para aprovechar mejor el hardware sin afectar la fluidez. Esto es especialmente importante en aplicaciones como museos AR, donde el usuario debe mantener una experiencia visual estable sin interrupciones, incluso en dispositivos móviles con recursos limitados.

```csharp
using UnityEngine;

public class AdaptiveQualityManager : MonoBehaviour
{
    public float updateInterval = 1f;
    private float timer;
    private int frames;

    void Update()
    {
        frames++;
        timer += Time.deltaTime;

        if (timer >= updateInterval)
        {
            float fps = frames / timer;
            ApplyQuality(fps);

            frames = 0;
            timer = 0;
        }
    }

    void ApplyQuality(float fps)
    {
        if (fps < 60f)
        {
            QualitySettings.DecreaseLevel(true);
            QualitySettings.shadowDistance = 20f;
            QualitySettings.lodBias = 0.7f;
        }
        else if (fps > 65f)
        {
            QualitySettings.IncreaseLevel(true);
            QualitySettings.shadowDistance = 50f;
            QualitySettings.lodBias = 1f;
        }
    }
}
```


---

### Pregunta de pensamiento crítico (7 pts)

> "Un compañero propone: 'Para optimizar nuestra app AR, vamos a reducir la resolución de pantalla del celular al 50% — así el GPU renderiza menos píxeles y ganamos FPS'. ¿Cuál es el riesgo de esta estrategia y en qué contextos XR específicos podría ser aceptable vs. inaceptable?"

La idea de reducir la resolución al 50% puede efectivamente mejorar el rendimiento porque disminuye la carga del GPU al renderizar menos píxeles, sin embargo el riesgo principal es que también reduce la claridad visual y la calidad de la experiencia inmersiva, lo cual en XR puede ser crítico. En aplicaciones como un museo AR o VR educativo, donde el usuario necesita leer información, observar detalles de modelos 3D o mantener una sensación de realismo, esta reducción puede afectar negativamente la legibilidad, la percepción de profundidad y la comodidad visual, generando una experiencia borrosa o poco profesional.

Por otro lado, esta estrategia podría ser aceptable en contextos donde el rendimiento es más importante que la calidad visual, por ejemplo en simulaciones de entrenamiento en tiempo real, aplicaciones industriales o escenarios VR muy pesados donde mantener 72–90 FPS es prioritario para evitar mareos o *motion sickness*, como en dispositivos tipo Meta Quest 2. En cambio, sería inaceptable en experiencias XR de exhibición, museos o educación visual detallada, donde la fidelidad gráfica es parte fundamental del objetivo de la aplicación.

---

## RÚBRICA DE AUTOEVALUACIÓN

| Criterio | Cumplido |
|----------|---------|
| Respondí las 10 preguntas de opción múltiple | x |
| Completé los 5 espacios en blanco de B1 | x |
| Relacioné todas las 10 columnas de B2 | x |
| En C1 identifiqué mínimo 3 problemas y escribí código optimizado | x |
| En C2 expliqué la diferencia con ejemplo concreto | x |
| En C3 identifiqué 4 problemas con técnicas específicas | x |
| En D1 mi tabla de estrategia tiene mínimo 5 técnicas | x |
| En D2 propuse solución de física que no requiere 500 Rigidbodies activos | x |
| Entregué como `.md` en la rama correcta de GitHub | x |

---

*PSISP08075 | Universidad Autónoma del Perú | Ingeniería de Sistemas | Semana 12 | 2026-1*