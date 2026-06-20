# S11 — GUÍA DE TRABAJO ESTUDIANTE
## Curso: Realidad Virtual y Aumentada | PSISP08075
### Semana 11 — Scripts e Inputs en XR

---

> **Instrucciones generales:**
> - Resuelve TODAS las secciones en orden.
> - No está permitido buscar respuestas en internet para la Sección I y II — usa solo lo visto en clase.
> - Para la Sección III y IV puedes consultar la documentación de Unity.
> - Entrega como archivo `.md` con tu código en bloques de código fenced (```csharp).
> - Tiempo sugerido: 90 minutos.

| Campo | Detalle |
|-------|---------|
| **Estudiante** | __________________ |
| **Código** | __________________ |
| **Fecha** | __________________ |
| **Semana** | 11 — Scripts e Inputs en XR |

---

## SECCIÓN I — COMPRENSIÓN CONCEPTUAL (25 puntos)
### 10 preguntas de opción múltiple — una sola respuesta correcta

---

**Pregunta 1** (2.5 pts)

¿Cuál de las siguientes afirmaciones describe correctamente la diferencia entre el Input System legacy de Unity y el nuevo Input System?

- a) El legacy es más rápido porque no usa reflection; el nuevo es más lento pero más flexible.
- b) El legacy usa `Input.GetKey()` y está atado a dispositivos específicos; el nuevo usa Actions y Bindings desacoplados del dispositivo.
- c) El nuevo Input System reemplaza completamente a `ARRaycastManager` para detectar toques en AR.
- d) Ambos sistemas son equivalentes; la diferencia es solo de sintaxis.

**Respuesta:** ___

---

**Pregunta 2** (2.5 pts)

En un proyecto AR para Android, un estudiante escribe:

```csharp
void Update()
{
    if (Input.GetMouseButtonDown(0))
        ColocarObjeto(Input.mousePosition);
}
```

¿Qué problema tiene este código al compilar para Android?

- a) Ningún problema — `GetMouseButtonDown` funciona en Android porque Unity mapea los toques a clicks de ratón.
- b) Solo funciona si el dispositivo tiene un ratón Bluetooth conectado al Android.
- c) En el editor funciona, pero en Android táctil el toque no se registra como click de ratón con el nuevo Input System activo.
- d) El problema es que `mousePosition` no existe en Unity 2022.

**Respuesta:** ___

---

**Pregunta 3** (2.5 pts)

¿Por qué se recomienda declarar `List<ARRaycastHit> hits` como campo de clase (fuera del método `Update`) en lugar de crearlo dentro de `Update()`?

- a) Porque `List<ARRaycastHit>` no puede crearse dentro de métodos — es una limitación de C#.
- b) Para reducir la creación de objetos en heap en cada frame y evitar pausas del Garbage Collector.
- c) Porque `ARRaycastManager.Raycast` requiere que la lista sea un campo de clase para poder escribir en ella.
- d) Solo por estilo de código — no hay diferencia de rendimiento.

**Respuesta:** ___

---

**Pregunta 4** (2.5 pts)

¿Cuándo se dispara el callback `performed` de una `InputAction`?

- a) En cada frame mientras el botón está presionado (equivalente a `GetKey`).
- b) Solo cuando la acción supera el umbral de activación (ej: botón presionado, trigger > 0.5f).
- c) Solo cuando la acción se cancela (botón soltado).
- d) En cada frame, independientemente del estado del botón.

**Respuesta:** ___

---

**Pregunta 5** (2.5 pts)

Un desarrollador escribe este código:

```csharp
void Start()
{
    EnhancedTouchSupport.Enable();
    Touch.onFingerDown += AlTocar;
}
void OnDestroy()
{
    Touch.onFingerDown -= AlTocar;
    EnhancedTouchSupport.Disable();
}
```

Al desactivar y reactivar el GameObject en runtime, el touch deja de funcionar. ¿Cuál es el error?

- a) `EnhancedTouchSupport.Enable()` solo puede llamarse una vez en toda la vida de la app.
- b) `Start()` no se llama al reactivar un GameObject — debería usarse `OnEnable()`/`OnDisable()`.
- c) `Touch.onFingerDown` no existe; el evento correcto es `Touch.onTouchBegin`.
- d) El problema es que `OnDestroy()` no se llama antes de que el objeto se desactive.

**Respuesta:** ___

---

**Pregunta 6** (2.5 pts)

¿Cuál es la principal ventaja de usar `InputActionReference` en un campo público del script?

- a) Es el único tipo que puede leer valores `Vector2` de controllers VR.
- b) Permite asignar el binding desde el Inspector sin hardcodear el nombre del botón en el código.
- c) Compila más rápido que usar el string del nombre de la acción.
- d) Permite usar acciones de múltiples `InputActionAsset` simultáneamente.

**Respuesta:** ___

---

**Pregunta 7** (2.5 pts)

Para leer el valor del joystick de un controller VR (entrada analógica continua), ¿qué enfoque es más correcto?

- a) Suscribirse al evento `performed` y leer el valor en el callback.
- b) Llamar `ReadValue<Vector2>()` en `Update()` para leer el valor cada frame.
- c) Usar `Input.GetAxis("Horizontal")` del Input System legacy.
- d) Suscribirse a `performed` y `canceled` y calcular la diferencia.

**Respuesta:** ___

---

**Pregunta 8** (2.5 pts)

En `XRGrabInteractable`, ¿cuándo se dispara el evento `activated`?

- a) Cuando el controller apunta hacia el objeto interactable.
- b) Cuando el usuario presiona el trigger del controller mientras el objeto ya está siendo agarrado.
- c) Cuando el objeto es instanciado en la escena.
- d) Cuando el objeto entra en el campo de visión del usuario.

**Respuesta:** ___

---

**Pregunta 9** (2.5 pts)

¿Para qué sirve el XR Device Simulator en Unity?

- a) Para compilar la app directamente en el dispositivo XR sin usar Build Settings.
- b) Para probar interacciones XR con teclado y ratón en el editor, sin necesidad del hardware físico.
- c) Para simular el rendimiento del dispositivo XR en el editor.
- d) Para reemplazar ARCore en dispositivos que no soportan AR.

**Respuesta:** ___

---

**Pregunta 10** (2.5 pts)

¿Cuál de estas opciones describe correctamente el ciclo `performed → canceled` de una InputAction de tipo `Button`?

- a) `performed` al presionar, `canceled` al soltar.
- b) `performed` al soltar, `canceled` al presionar.
- c) `performed` en cada frame que el botón está presionado.
- d) `performed` y `canceled` se disparan siempre en el mismo frame.

**Respuesta:** ___

---

## SECCIÓN II — COMPLETAR Y RELACIONAR (25 puntos)

### Parte A — Completa el código (15 pts)

El siguiente script gestiona inputs de controller VR. Rellena los espacios en blanco `[___]` con el código correcto. Cada espacio vale 1 punto.

```csharp
using UnityEngine;
using UnityEngine.[1___];         // namespace del Input System

public class ControladorVR : MonoBehaviour
{
    public InputAction[2___] accionTrigger;    // tipo de referencia a usar
    public InputActionReference accionGrip;

    void [3___]()                 // método correcto de Unity para suscribirse
    {
        accionTrigger.action.[4___] += AlPresionarTrigger;   // evento correcto
        accionGrip.action.performed  += AlAgarrar;
        accionTrigger.action.[5___]();    // activar la acción
        accionGrip.action.Enable();
    }

    void [6___]()                 // método correcto de Unity para desuscribirse
    {
        accionTrigger.action.performed -= AlPresionarTrigger;
        accionGrip.action.[7___]   -= AlAgarrar;   // evento correcto para desuscribir
    }

    void AlPresionarTrigger(InputAction.[8___] ctx)  // tipo del parámetro del callback
    {
        float presion = ctx.[9___]<float>();   // método para leer el valor
        Debug.Log($"Trigger: {presion:[10___]}");   // formato con 2 decimales
    }

    void AlAgarrar(InputAction.CallbackContext ctx)
    {
        float val = ctx.ReadValue<float>();
        bool agarrando = val [11___] 0.5f;   // comparación para detectar agarre
        Debug.Log($"Grip: {agarrando}");
    }

    void Update()
    {
        // Para entradas continuas: leer en Update, NO usar evento performed
        Vector2 joystick = accionGrip.action.[12___]<Vector2>();  // método correcto
    }
}
```

Espacios a completar:

| # | Respuesta |
|---|-----------|
| 1 | ___________ |
| 2 | ___________ |
| 3 | ___________ |
| 4 | ___________ |
| 5 | ___________ |
| 6 | ___________ |
| 7 | ___________ |
| 8 | ___________ |
| 9 | ___________ |
| 10 | ___________ |
| 11 | ___________ |
| 12 | ___________ |

---

### Parte B — Relacionar columnas (10 pts — 1 pt c/u)

**Columna A (Concepto)** — **Columna B (Descripción)**

| Columna A | Tu respuesta | Columna B |
|-----------|-------------|-----------|
| 1. `TouchPhase.Began` | ___ | A. Evento que se dispara cuando el controller apunta a un interactable |
| 2. `InputActionReference` | ___ | B. Lee el valor de una acción en cada frame (para entradas continuas) |
| 3. `EnhancedTouchSupport.Enable()` | ___ | C. Solo el primer frame en que el dedo toca la pantalla |
| 4. `ReadValue<T>()` en Update | ___ | D. Campo público que une el script con una acción del Input Actions Asset |
| 5. `hoverEntered` | ___ | E. Activa el API avanzado de toques multi-dedo del nuevo Input System |
| 6. `performed` | ___ | F. Callback que se dispara cuando se activa una InputAction |
| 7. `XR Device Simulator` | ___ | G. Limpiar la lista en vez de crear una nueva con `new` |
| 8. `hits.Clear()` | ___ | H. Herramienta para probar XR con teclado y ratón en el editor |
| 9. `cancelado` / `canceled` | ___ | I. Callback que se dispara cuando se desactiva/suelta una InputAction |
| 10. `selectEntered` | ___ | J. Evento de XRGrabInteractable cuando el usuario agarra el objeto |

---

## SECCIÓN III — ANÁLISIS Y APLICACIÓN (25 puntos)

### Caso 1 — Diagnóstico de código defectuoso (12 pts)

El siguiente script tiene **4 errores** que impiden que funcione correctamente en Android AR. Identifica cada error, explica por qué es un error y escribe la corrección.

```csharp
// Script: GestorInputAR_Defectuoso.cs
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;

public class GestorInputAR_Defectuoso : MonoBehaviour
{
    private ARRaycastManager arRM;

    void Start()
    {
        arRM = GetComponent<ARRaycastManager>();
        UnityEngine.InputSystem.EnhancedTouch.EnhancedTouchSupport.Enable();       // Línea A
        UnityEngine.InputSystem.EnhancedTouch.Touch.onFingerDown += AlTocar;       // Línea B
    }

    void OnDestroy()  // Línea C
    {
        UnityEngine.InputSystem.EnhancedTouch.Touch.onFingerDown -= AlTocar;
        UnityEngine.InputSystem.EnhancedTouch.EnhancedTouchSupport.Disable();
    }

    void AlTocar(UnityEngine.InputSystem.EnhancedTouch.Finger dedo)
    {
        List<ARRaycastHit> hits = new List<ARRaycastHit>();   // Línea D
        if (arRM.Raycast(dedo.screenPosition, hits, TrackableType.PlaneWithinPolygon))
        {
            Debug.Log($"Plano AR en: {hits[0].pose.position}");
        }
    }
}
```

Completa la tabla:

| Error | Línea | ¿Por qué es error? | Corrección |
|-------|-------|-------------------|------------|
| 1 | ___ | | |
| 2 | ___ | | |
| 3 | ___ | | |
| 4 | ___ | | |

---

### Caso 2 — Diseño de arquitectura de inputs (13 pts)

Una startup limeña desarrolla **"GuíaMuseo AR"** — una app AR para el Museo Larco de Lima donde los visitantes apuntan su celular a las piezas arqueológicas y aparece información interactiva en AR. Los inputs requeridos son:

- **Tap simple** sobre la pieza → mostrar panel de información
- **Doble tap** sobre el panel → expandir/colapsar detalles
- **Swipe vertical** sobre el panel → navegar entre secciones (Historia / Materiales / Época)
- **Pinch** sobre el panel → zoom in/out del panel AR
- **Long press** (2 segundos) sobre la pieza → guardar en favoritos

El jefe de producto dice: "En 6 meses vamos a agregar soporte para Meta Quest 3 con controllers — no quiero que tengan que reescribir todos los scripts de input."

**Tarea:**

**a)** Diseña el Input Actions Asset (nombre del asset, nombre del Action Map, nombre y tipo de cada Action) que cumpla los requisitos. Usa el siguiente formato de tabla:

| Asset name | Action Map | Action | Tipo | Tipo de valor |
|------------|-----------|--------|------|--------------|
| | | | | |
| | | | | |

**b)** Escribe el esqueleto del script `InputMuseoAR.cs` que use `InputActionReference` para las 5 acciones. El esqueleto debe incluir: declaración de campos, `OnEnable()` con suscripciones y Enable, `OnDisable()` con desuscripciones, y los 5 métodos de callback con `Debug.Log()`.

**c)** Responde en 3 líneas: ¿qué cambiarías en el código para agregar soporte a Meta Quest 3 en 6 meses?

---

## SECCIÓN IV — CASO AVANZADO (25 puntos)

### Reto: Sistema de Input Multi-Modal para XR

**Contexto:** el equipo de VR de tu grupo de proyecto necesita un sistema de inputs que funcione en dos modos:

- **Modo AR (Android):** usa toques táctiles
- **Modo VR (Quest):** usa controllers

El mismo script debe funcionar en ambos modos. Al iniciar, el sistema detecta automáticamente si está en AR o VR y configura los inputs correspondientes.

**Tarea:** implementa `GestorInputMultiModal.cs` con los siguientes requisitos:

**Requisito 1:** detectar el modo de ejecución:
```csharp
// Si tiene ARSession → modo AR; si tiene XRRig con controllers → modo VR
private enum ModoXR { AR, VR }
private ModoXR modoActual;
```

**Requisito 2:** en modo AR:
- Tap → `AccionSeleccionar(posicion2D)`
- Pinch → `AccionEscalar(factor)`
- Swipe horizontal → `AccionRotar(angulo)`

**Requisito 3:** en modo VR:
- Trigger → `AccionSeleccionar(posicion3D del controller)`
- Grip → `AccionEscalar()` (agarrar para escalar)
- Joystick X → `AccionRotar(angulo)`

**Requisito 4:** las tres acciones (`AccionSeleccionar`, `AccionEscalar`, `AccionRotar`) deben estar definidas como métodos que reciben parámetros — el código de lógica NO debe estar duplicado entre el modo AR y VR.

**Requisito 5:** `InputActionReference` para las acciones VR, `EnhancedTouchSupport` para las acciones AR.

**Entrega:**
1. Código completo de `GestorInputMultiModal.cs`
2. Explicación en comentarios de por qué el diseño evita duplicación de lógica
3. Tabla de decisiones: ¿qué clase de problema resuelve este patrón en proyectos reales?

> **Criterio de evaluación:**
> - Código compila sin errores: 10 pts
> - Detección correcta del modo: 5 pts
> - No duplicación de lógica de negocio: 5 pts
> - Uso correcto de OnEnable/OnDisable: 5 pts

---

## RÚBRICA DE AUTOEVALUACIÓN

Antes de entregar, verifica:

| Criterio | Cumplido |
|----------|---------|
| Respondí todas las preguntas de la Sección I | ☐ |
| Completé todos los espacios de la Sección II-A | ☐ |
| Relacioné todos los items de la Sección II-B | ☐ |
| Identifiqué los 4 errores del Caso 1 con correcciones | ☐ |
| El Input Actions Asset del Caso 2 tiene tipos correctos (Button/Value) | ☐ |
| El código de la Sección IV compila (lo verifiqué en Unity) | ☐ |
| Usé `OnEnable`/`OnDisable` (no `Start`/`OnDestroy`) para los eventos | ☐ |
| El archivo está en formato `.md` con código en bloques ` ```csharp ` | ☐ |

---

*PSISP08075 | Universidad Autónoma del Perú | Ingeniería de Sistemas | Semana 11 | 2026-1*
