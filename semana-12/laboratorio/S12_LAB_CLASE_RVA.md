# LAB 12 — LABORATORIO EN CLASE
## Curso: Realidad Virtual y Aumentada | PSISP08075
### Semana 12 — Diagnóstico y Optimización con Unity Profiler

---

> **Laboratorio en clase — autónomo y autoexplicativo**
> **Tiempo:** 35–40 minutos
> **Modalidad:** Individual
> **Entrega:** Screenshot + commit antes de salir de clase

---

## OBJETIVO

Usar el Unity Profiler para **diagnosticar** un proyecto XR con problemas de rendimiento, identificar el cuello de botella, y aplicar **al menos 2 técnicas de optimización** medibles.

---

## ANTES DE EMPEZAR — VERIFICAR ENTORNO (2 min)

```
☐ Unity 2022.3.x abierto con tu proyecto AR (lab_11_inputs u otro)
☐ El proyecto tiene al menos 10-15 objetos en la escena
☐ Puedes hacer Play sin errores rojos en Console
```

Si no tienes un proyecto con suficientes objetos, crea rápidamente una escena con 50 cubos:
```
Hierarchy → clic derecho → 3D Object → Cube (repetir 50 veces)
O: seleccionar 1 Cube → Ctrl+D × 50 veces (duplicar)
```

---

## PASO 1 — MEDIR EL ESTADO INICIAL (8 min)

### 1.1 — Activar el panel Stats

1. Presiona **▶️ Play**
2. En la ventana Game, haz clic en el botón **"Stats"** (arriba a la derecha)
3. Aparece un panel superpuesto con métricas en tiempo real

4. **Anota los valores** en esta tabla (copiarla en tu archivo de entrega):

```
MEDICIÓN INICIAL
═══════════════════════════════════════
FPS: _____________
Draw Calls: _____________
Tris: _____________
Verts: _____________
Used Memory: _____________ MB
═══════════════════════════════════════
```

5. Presiona **▶️** para salir del Play

---

### 1.2 — Abrir el Profiler

1. En el menú de Unity: **Window → Analysis → Profiler** (o Ctrl+7)
2. Se abre la ventana del Profiler (puede estar en una pestaña junto a Project)

3. Presiona **▶️ Play** de nuevo
4. El Profiler empieza a grabar automáticamente (verás barras creciendo)
5. Deja correr por **15 segundos**
6. Presiona **▶️** para parar

---

### 1.3 — Identificar el frame más lento

1. En la gráfica del Profiler, busca el **pico más alto** (la barra más tall)
2. Haz **clic sobre ese pico** — se resalta en azul
3. En la sección inferior del Profiler verás la jerarquía de ese frame:

```
Ejemplo de lo que podrías ver:
PlayerLoop                          16.7ms
├── Update.ScriptRunBehaviourUpdate  X.Xms  ← SCRIPTS
│   ├── TuScript.Update()            X.Xms
│   └── ...
├── PhysicsUpdate                    X.Xms  ← FÍSICA
└── Camera.Render                    X.Xms  ← RENDERING
    ├── Drawing                      X.Xms
    └── PostProcessing               X.Xms
```

4. **Anota** qué proceso toma más tiempo (en ms):

```
ANÁLISIS DEL FRAME MÁS LENTO
═══════════════════════════════════════
Proceso más lento: _____________
Tiempo que toma: _____________ ms
Segundo proceso: _____________
Tiempo: _____________ ms
¿Es CPU bound o GPU bound?: _____________
═══════════════════════════════════════
```

**¿Cómo determinar CPU o GPU bound?**
- Si `Camera.Render` > 10ms → **GPU bound** (problema de rendering)
- Si `Scripts Update` > 5ms → **CPU bound** (problema de scripts)
- Ambos pueden estar altos al mismo tiempo

---

## PASO 2 — APLICAR TÉCNICA DE OPTIMIZACIÓN #1: STATIC BATCHING (10 min)

### 2.1 — Ver los Draw Calls actuales

Antes de aplicar Static Batching, en el Stats panel anota:
```
Draw Calls antes de Static Batching: _____________
```

### 2.2 — Marcar objetos como Static

1. En la **Hierarchy**, selecciona UN objeto estático (un cubo que no se mueva)
2. En el **Inspector**, arriba a la derecha verás la casilla **"Static"**
3. Haz clic para activarla ☑

4. Unity puede preguntar: "Do you want to enable the static flags for all child objects too?"
   → Haz clic en **"Yes, change children"**

5. **Seleccionar múltiples objetos a la vez:**
   - En la Hierarchy, haz clic en el primer objeto estático
   - Mantén **Shift** y haz clic en el último
   - Con todos seleccionados, activa la casilla **"Static"** en el Inspector
   - Aplica a todos

6. Si te pide "Set to Static" → aceptar

### 2.3 — Verificar el impacto

1. Presiona **▶️ Play**
2. Mira el panel Stats
3. Anota:

```
DESPUÉS DE STATIC BATCHING
═══════════════════════════════════════
FPS: _____________
Draw Calls: _____________  (¿bajaron? ¿cuánto?)
Tris: _____________
═══════════════════════════════════════
Reducción de Draw Calls: _____________  (antes - después)
Impacto en FPS: _____________  (antes - después)
```

4. Presiona **▶️** para salir

---

## PASO 3 — APLICAR TÉCNICA DE OPTIMIZACIÓN #2: GPU INSTANCING (8 min)

Esta técnica aplica cuando tienes múltiples objetos con el **mismo material**.

### 3.1 — Verificar que los objetos comparten material

1. Selecciona 2-3 cubos en la Hierarchy
2. En el Inspector, verifica que el material es el mismo (mismo nombre en el componente Mesh Renderer)

Si no comparten material:
- Seleccionar todos los cubos
- En Inspector → Mesh Renderer → Materials → arrastrar el mismo material a todos

### 3.2 — Activar GPU Instancing en el material

1. En el **Project** panel, encuentra el material compartido (ej: `Default-Material` o el que uses)
2. Haz clic en el material para seleccionarlo
3. En el **Inspector** del material, busca: **"Enable GPU Instancing"**
4. Activa la casilla ☑

```
Si no ves "Enable GPU Instancing":
El shader del material no soporta instancing.
Crea un material nuevo:
  - Project → Create → Material
  - Shader: Standard (sí soporta instancing)
  - Activa Enable GPU Instancing ☑
  - Asigna este material a todos tus cubos
```

### 3.3 — Medir el impacto

1. Presiona **▶️ Play**
2. Anota en Stats:

```
DESPUÉS DE GPU INSTANCING
═══════════════════════════════════════
FPS: _____________
Draw Calls: _____________
═══════════════════════════════════════
¿Mejoraron los Draw Calls?: _____________
```

> **Nota:** Static Batching y GPU Instancing compiten — Unity elige uno u otro.
> Si activaste Static Batching, desactívalo temporalmente (desmarcar Static) para ver el efecto de GPU Instancing por separado. Luego puedes activar el que dé mejor resultado para tu caso.

---

## PASO 4 — DETECTAR UN PROBLEMA EN SCRIPTS (5 min)

### 4.1 — Ver GC Alloc en el Profiler

1. Abre el **Profiler** (Window → Analysis → Profiler)
2. Presiona ▶️ Play
3. En la gráfica superior, busca la línea **"GC Alloc"** (puede ser verde o naranja)
4. Si hay picos regulares de GC Alloc → hay objetos creados en Update

### 4.2 — Crear un script con problema intencional y medirlo

Para ver el impacto del GC, crea este script y agrégalo a un objeto vacío:

```csharp
// Assets/Scripts/ScriptLento.cs
using UnityEngine;

public class ScriptLento : MonoBehaviour
{
    void Update()
    {
        // ❌ Este código crea una nueva cadena cada frame
        string texto = "Frame: " + Time.frameCount.ToString();
        // No hacemos nada con 'texto' — solo para medir el costo
    }
}
```

1. Crea el script y agrégalo al Main Camera o a un GameObject vacío
2. Presiona Play
3. En Profiler → busca `ScriptLento.Update` en la jerarquía
4. Anota el valor en la columna **"GC Alloc"**:

```
GC Alloc de ScriptLento.Update por frame: _____________ bytes
```

5. Ahora crea la versión optimizada y compara:

```csharp
// Assets/Scripts/ScriptRapido.cs
using UnityEngine;
using System.Text; // para StringBuilder

public class ScriptRapido : MonoBehaviour
{
    private StringBuilder sb = new StringBuilder(); // reutilizable
    private int ultimoFrame = -1;

    void Update()
    {
        if (Time.frameCount == ultimoFrame) return; // evitar trabajo duplicado
        ultimoFrame = Time.frameCount;

        sb.Clear();
        sb.Append("Frame: ");
        sb.Append(Time.frameCount);
        string texto = sb.ToString(); // 0 GC Alloc si reutilizamos sb
    }
}
```

6. Agrega `ScriptRapido` al mismo objeto, desactiva `ScriptLento`, presiona Play
7. Busca `ScriptRapido.Update` en el Profiler y anota GC Alloc

```
GC Alloc de ScriptRapido.Update por frame: _____________ bytes
Reducción: _____________
```

---

## PASO 5 — CREAR EL REPORTE Y HACER COMMIT (5 min)

### 5.1 — Crear el archivo de reporte

Crea un archivo `optimizacion_lab12.md` en la raíz del proyecto con esta estructura:

```markdown
# Reporte Optimización — Lab 12
**Nombre:** [Tu nombre]
**Código:** [Tu código]
**Fecha:** [fecha]

## Estado Inicial
| Métrica | Valor |
|---------|-------|
| FPS | |
| Draw Calls | |
| Tris | |
| Used Memory | |
| Frame más lento (ms) | |
| Proceso más lento | |

## Técnica 1: Static Batching
- Objetos marcados como Static: [cantidad]
- Draw Calls antes: [valor]
- Draw Calls después: [valor]
- FPS antes: [valor]
- FPS después: [valor]
- Análisis: [¿qué pasó y por qué?]

## Técnica 2: GPU Instancing
- Material modificado: [nombre]
- Draw Calls antes: [valor]
- Draw Calls después: [valor]
- Análisis: [¿qué pasó y por qué?]

## Análisis de Scripts (GC Alloc)
- GC Alloc ScriptLento: [bytes/frame]
- GC Alloc ScriptRapido: [bytes/frame]
- Conclusión: [¿qué aprendiste?]

## Reflexión Final
¿Qué técnica tuvo mayor impacto en tu proyecto? ¿Por qué?
[Escribe 3-5 líneas]
```

### 5.2 — Tomar capturas

Toma 2 screenshots con la tecla `PrtScn` o herramienta de captura:
1. El panel Stats mostrando los valores **iniciales**
2. El panel Stats mostrando los valores **después de optimizar**

Guarda las capturas como `antes.png` y `despues.png`.

### 5.3 — Commit en GitHub

```bash
git add .
git commit -m "feat(s12): diagnosticar y optimizar rendimiento con Profiler"
git push
```

---

## CHECKLIST DE ENTREGA

```
☐ Tabla de medición inicial completada (FPS, Draw Calls, Tris)
☐ Static Batching aplicado y diferencia medida
☐ GPU Instancing aplicado y diferencia medida
☐ ScriptLento y ScriptRapido creados y GC Alloc comparado
☐ Archivo optimizacion_lab12.md con todas las tablas
☐ Capturas "antes.png" y "despues.png"
☐ Commit en GitHub con mensaje "feat(s12): diagnosticar..."
```

---

## GUÍA DE SOLUCIÓN DE PROBLEMAS RÁPIDA

| Síntoma | Causa | Solución |
|---------|-------|----------|
| Draw Calls no bajan con Static Batching | Los objetos tienen materiales diferentes | Asegurarse de que todos usen exactamente el mismo material |
| GPU Instancing no aparece en el material | El shader no lo soporta | Cambiar el shader a "Standard" o "Universal Render Pipeline/Lit" |
| Profiler no muestra datos | Profiler está desconectado | En el Profiler, verificar que dice "Playmode" y no "Editor" |
| GC Alloc siempre en 0 | El script no está activo | Verificar que el script está en un objeto activo en la Hierarchy |
| FPS varía mucho entre frames | Problema de GC spikes | Es normal — el GC se activa intermitentemente |

---

## ROL DEL DOCENTE DURANTE EL LAB

**Qué observar mientras circula:**
- ¿Los estudiantes anotaron los valores ANTES de optimizar? (sin medición previa, no hay comparación)
- ¿Encontraron el Profiler o están buscando el Stats solo?
- ¿El Draw Call bajó con Static Batching? Si no: probablemente los materiales son diferentes

**Preguntas de andamiaje para estudiantes bloqueados:**
- "¿Anotaste los Draw Calls antes de marcar Static? Sin eso no puedes comparar."
- "¿Todos tus cubos tienen el mismo material? Abre el Inspector de dos cubos a la vez y compara."
- "¿Qué parte del Profiler te muestra el GC Alloc? Busca la línea verde en la gráfica."

**Señales de comprensión:**
- El estudiante puede decir "bajé de X a Y Draw Calls con Static Batching"
- El estudiante identifica que ScriptLento genera basura en memoria cada frame

**Señal de no comprensión:**
- Dice "los FPS mejoraron" sin poder decir exactamente cuánto
- No anotó los valores iniciales antes de optimizar

**Cierre del laboratorio (5 min):**
> "¿Quién tuvo la mayor reducción de Draw Calls? ¿Quién tuvo la menor? ¿Por qué creen que hay diferencia entre proyectos? El punto no es el número — es el PROCESO: medir, identificar, actuar, medir de nuevo. Ese es el ciclo de optimización profesional."

---

*PSISP08075 | Universidad Autónoma del Perú | Ingeniería de Sistemas | Semana 12 | 2026-1*
