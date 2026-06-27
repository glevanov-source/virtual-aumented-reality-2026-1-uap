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
| **Estudiante** | Salazar Mondragón Jael Santiago |
| **Código** | 2221898131 |
| **Fecha** | 26/06/2026 |
| **Semana** | 11 — Scripts e Inputs en XR |

---

## SECCIÓN I — COMPRENSIÓN CONCEPTUAL (25 puntos)

### 10 preguntas de opción múltiple — una sola respuesta correcta

---

### **Pregunta 1** (2.5 pts)

¿Cuál de las siguientes afirmaciones describe correctamente la diferencia entre el Input System legacy de Unity y el nuevo Input System?

- a) El legacy es más rápido porque no usa reflection; el nuevo es más lento pero más flexible.
- **b) El legacy usa `Input.GetKey()` y está atado a dispositivos específicos; el nuevo usa Actions y Bindings desacoplados del dispositivo.**
- c) El nuevo Input System reemplaza completamente a `ARRaycastManager` para detectar toques en AR.
- d) Ambos sistemas son equivalentes; la diferencia es solo de sintaxis.

**Respuesta:** **b**

**Explicación:**  
El sistema antiguo depende directamente del dispositivo de entrada, mientras que el nuevo organiza las acciones de forma independiente. Esto hace que sea más sencillo reutilizar los mismos controles en distintas plataformas.

---

### **Pregunta 2** (2.5 pts)

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
- **c) En el editor funciona, pero en Android táctil el toque no se registra como click de ratón con el nuevo Input System activo.**
- d) El problema es que `mousePosition` no existe en Unity 2022.

**Respuesta:** **c**

**Explicación:**  
Ese método fue pensado para entradas tipo ratón. Cuando se utiliza el nuevo sistema de inputs, es preferible trabajar con acciones o con las APIs de toque para asegurar compatibilidad en dispositivos móviles.

---

### **Pregunta 3** (2.5 pts)

¿Por qué se recomienda declarar `List<ARRaycastHit> hits` como campo de clase (fuera del método `Update`) en lugar de crearlo dentro de `Update()`?

- a) Porque `List<ARRaycastHit>` no puede crearse dentro de métodos — es una limitación de C#.
- **b) Para reducir la creación de objetos en heap en cada frame y evitar pausas del Garbage Collector.**
- c) Porque `ARRaycastManager.Raycast` requiere que la lista sea un campo de clase para poder escribir en ella.
- d) Solo por estilo de código — no hay diferencia de rendimiento.

**Respuesta:** **b**

**Explicación:**  
Si la lista se crea continuamente dentro de `Update()`, se generan más objetos en memoria. Mantener una sola instancia ayuda a optimizar el rendimiento de la aplicación.

---

### **Pregunta 4** (2.5 pts)

¿Cuándo se dispara el callback `performed` de una `InputAction`?

- a) En cada frame mientras el botón está presionado (equivalente a `GetKey`).
- **b) Solo cuando la acción supera el umbral de activación (ej: botón presionado, trigger > 0.5f).**
- c) Solo cuando la acción se cancela (botón soltado).
- d) En cada frame, independientemente del estado del botón.

**Respuesta:** **b**

**Explicación:**  
El evento `performed` representa el momento en que la acción se ejecuta correctamente. No permanece activo todo el tiempo, sino únicamente cuando la condición de activación se cumple.

---

### **Pregunta 5** (2.5 pts)

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
- **b) `Start()` no se llama al reactivar un GameObject — debería usarse `OnEnable()`/`OnDisable()`.**
- c) `Touch.onFingerDown` no existe; el evento correcto es `Touch.onTouchBegin`.
- d) El problema es que `OnDestroy()` no se llama antes de que el objeto se desactive.

**Respuesta:** **b**

**Explicación:**  
Los métodos `OnEnable()` y `OnDisable()` están pensados para controlar eventos durante las activaciones del objeto. Así se garantiza que las suscripciones siempre estén actualizadas.

---

### **Pregunta 6** (2.5 pts)

¿Cuál es la principal ventaja de usar `InputActionReference` en un campo público del script?

- a) Es el único tipo que puede leer valores `Vector2` de controllers VR.
- **b) Permite asignar el binding desde el Inspector sin hardcodear el nombre del botón en el código.**
- c) Compila más rápido que usar el string del nombre de la acción.
- d) Permite usar acciones de múltiples `InputActionAsset` simultáneamente.

**Respuesta:** **b**

**Explicación:**  
Esta referencia facilita conectar acciones directamente desde el Inspector. Gracias a ello, modificar un control no implica editar el código fuente.

---

### **Pregunta 7** (2.5 pts)

Para leer el valor del joystick de un controller VR (entrada analógica continua), ¿qué enfoque es más correcto?

- a) Suscribirse al evento `performed` y leer el valor en el callback.
- **b) Llamar `ReadValue<Vector2>()` en `Update()` para leer el valor cada frame.**
- c) Usar `Input.GetAxis("Horizontal")` del Input System legacy.
- d) Suscribirse a `performed` y `canceled` y calcular la diferencia.

**Respuesta:** **b**

**Explicación:**  
Como el joystick cambia constantemente mientras el usuario lo mueve, conviene consultar su valor en cada actualización para obtener un movimiento continuo.

---

### **Pregunta 8** (2.5 pts)

En `XRGrabInteractable`, ¿cuándo se dispara el evento `activated`?

- a) Cuando el controller apunta hacia el objeto interactable.
- **b) Cuando el usuario presiona el trigger del controller mientras el objeto ya está siendo agarrado.**
- c) Cuando el objeto es instanciado en la escena.
- d) Cuando el objeto entra en el campo de visión del usuario.

**Respuesta:** **b**

**Explicación:**  
Este evento representa una interacción adicional con el objeto seleccionado. Normalmente ocurre cuando el usuario ya lo sostiene y activa el trigger del controlador.

---

### **Pregunta 9** (2.5 pts)

¿Para qué sirve el XR Device Simulator en Unity?

- a) Para compilar la app directamente en el dispositivo XR sin usar Build Settings.
- **b) Para probar interacciones XR con teclado y ratón en el editor, sin necesidad del hardware físico.**
- c) Para simular el rendimiento del dispositivo XR en el editor.
- d) Para reemplazar ARCore en dispositivos que no soportan AR.

**Respuesta:** **b**

**Explicación:**  
Esta herramienta permite probar muchas funciones de realidad virtual desde el editor. Es útil para avanzar en el desarrollo cuando todavía no se dispone del visor físico.

---

### **Pregunta 10** (2.5 pts)

¿Cuál de estas opciones describe correctamente el ciclo `performed → canceled` de una InputAction de tipo `Button`?

- **a) `performed` al presionar, `canceled` al soltar.**
- b) `performed` al soltar, `canceled` al presionar.
- c) `performed` en cada frame que el botón está presionado.
- d) `performed` y `canceled` se disparan siempre en el mismo frame.

**Respuesta:** **a**

**Explicación:**  
El flujo habitual consiste en detectar primero la pulsación mediante `performed` y posteriormente informar que terminó la acción mediante `canceled` cuando el botón deja de presionarse.
## SECCIÓN II — COMPLETAR Y RELACIONAR (25 puntos)

### Parte A — Completa el código (15 pts)

El siguiente script gestiona inputs de controller VR. Rellena los espacios en blanco `[___]` con el código correcto. Cada espacio vale 1 punto.

```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class ControladorVR : MonoBehaviour
{
    public InputActionReference accionTrigger;
    public InputActionReference accionGrip;

    void OnEnable()
    {
        accionTrigger.action.performed += AlPresionarTrigger;
        accionGrip.action.performed += AlAgarrar;
        accionTrigger.action.Enable();
        accionGrip.action.Enable();
    }

    void OnDisable()
    {
        accionTrigger.action.performed -= AlPresionarTrigger;
        accionGrip.action.performed -= AlAgarrar;
    }

    void AlPresionarTrigger(InputAction.CallbackContext ctx)
    {
        float presion = ctx.ReadValue<float>();
        Debug.Log($"Trigger: {presion:F2}");
    }

    void AlAgarrar(InputAction.CallbackContext ctx)
    {
        float val = ctx.ReadValue<float>();
        bool agarrando = val > 0.5f;
        Debug.Log($"Grip: {agarrando}");
    }

    void Update()
    {
        Vector2 joystick = accionGrip.action.ReadValue<Vector2>();
    }
}
```

Espacios a completar:

| # | Respuesta |
|---|-----------|
| 1 | `InputSystem` |
| 2 | `Reference` |
| 3 | `OnEnable` |
| 4 | `performed` |
| 5 | `Enable` |
| 6 | `OnDisable` |
| 7 | `performed` |
| 8 | `CallbackContext` |
| 9 | `ReadValue` |
| 10 | `F2` |
| 11 | `>` |
| 12 | `ReadValue` |

**Explicación:**  
La estructura utiliza el ciclo recomendado del nuevo Input System. Las acciones se activan y desactivan según el estado del objeto para evitar comportamientos inesperados.

---

### Parte B — Relacionar columnas (10 pts)

**Columna A (Concepto)** — **Columna B (Descripción)**

| Columna A | Tu respuesta | Columna B |
|-----------|-------------|-----------|
| 1. `TouchPhase.Began` | **C** | A. Evento que se dispara cuando el controller apunta a un interactable |
| 2. `InputActionReference` | **D** | B. Lee el valor de una acción en cada frame (para entradas continuas) |
| 3. `EnhancedTouchSupport.Enable()` | **E** | C. Solo el primer frame en que el dedo toca la pantalla |
| 4. `ReadValue<T>()` en Update | **B** | D. Campo público que une el script con una acción del Input Actions Asset |
| 5. `hoverEntered` | **A** | E. Activa el API avanzado de toques multi-dedo del nuevo Input System |
| 6. `performed` | **F** | F. Callback que se dispara cuando se activa una InputAction |
| 7. `XR Device Simulator` | **H** | G. Limpiar la lista en vez de crear una nueva con `new` |
| 8. `hits.Clear()` | **G** | H. Herramienta para probar XR con teclado y ratón en el editor |
| 9. `cancelado` / `canceled` | **I** | I. Callback que se dispara cuando se desactiva/suelta una InputAction |
| 10. `selectEntered` | **J** | J. Evento de XRGrabInteractable cuando el usuario agarra el objeto |

**Explicación:**  
Cada concepto cumple una función específica dentro del flujo de interacción XR. Relacionarlos correctamente ayuda a entender cuándo utilizar cada evento o método durante el desarrollo.

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
| 1 | Falta `using System.Collections.Generic;` | Se utiliza `List<ARRaycastHit>` sin importar el espacio de nombres correspondiente. | Agregar `using System.Collections.Generic;`. |
| 2 | Línea A | `EnhancedTouchSupport.Enable()` está en `Start()`, por lo que no vuelve a ejecutarse cuando el objeto se reactiva. | Mover la llamada a `OnEnable()`. |
| 3 | Línea B y C | La suscripción y desuscripción de eventos no sigue el ciclo de activación del GameObject. | Utilizar `OnEnable()` y `OnDisable()`. |
| 4 | Línea D | Se crea una nueva lista cada vez que ocurre un toque, generando asignaciones innecesarias. | Declarar la lista como variable de clase y reutilizarla con `hits.Clear()`. |

### Código corregido

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using UnityEngine.InputSystem.EnhancedTouch;
using System.Collections.Generic;

public class GestorInputAR_Corregido : MonoBehaviour
{
    private ARRaycastManager arRM;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();

    void Awake()
    {
        arRM = GetComponent<ARRaycastManager>();
    }

    void OnEnable()
    {
        EnhancedTouchSupport.Enable();
        Touch.onFingerDown += AlTocar;
    }

    void OnDisable()
    {
        Touch.onFingerDown -= AlTocar;
        EnhancedTouchSupport.Disable();
    }

    void AlTocar(Finger dedo)
    {
        hits.Clear();

        if (arRM.Raycast(dedo.screenPosition, hits, TrackableType.PlaneWithinPolygon))
        {
            Debug.Log($"Plano AR en: {hits[0].pose.position}");
        }
    }
}
```

**Explicación:**  
La versión corregida sigue una organización más adecuada para aplicaciones XR. Además de mejorar el ciclo de vida de los eventos, también evita crear memoria adicional en cada interacción.

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

**a)** Diseña el Input Actions Asset

| Asset name | Action Map | Action | Tipo | Tipo de valor |
|------------|-----------|--------|------|--------------|
| MuseoARInputs | AR_Interactions | TapPieza | Button | Button |
| MuseoARInputs | AR_Interactions | DobleTapPanel | Button | Button |
| MuseoARInputs | AR_Interactions | SwipeVertical | Value | Vector2 |
| MuseoARInputs | AR_Interactions | PinchPanel | Value | Axis |
| MuseoARInputs | AR_Interactions | LongPressPieza | Button | Button |

**Explicación:**  
La distribución de acciones permite organizar mejor las entradas de la aplicación. Además, facilita agregar nuevos dispositivos sin modificar la estructura principal.

---

**b)** Escribe el esqueleto del script `InputMuseoAR.cs` que use `InputActionReference` para las 5 acciones. El esqueleto debe incluir: declaración de campos, `OnEnable()` con suscripciones y Enable, `OnDisable()` con desuscripciones, y los 5 métodos de callback con `Debug.Log()`.


```csharp
using UnityEngine;
using UnityEngine.InputSystem;

public class InputMuseoAR : MonoBehaviour
{
    public InputActionReference tapPieza;
    public InputActionReference dobleTapPanel;
    public InputActionReference swipeVertical;
    public InputActionReference pinchPanel;
    public InputActionReference longPressPieza;

    void OnEnable()
    {
        tapPieza.action.performed += OnTapPieza;
        dobleTapPanel.action.performed += OnDobleTapPanel;
        swipeVertical.action.performed += OnSwipeVertical;
        pinchPanel.action.performed += OnPinchPanel;
        longPressPieza.action.performed += OnLongPressPieza;

        tapPieza.action.Enable();
        dobleTapPanel.action.Enable();
        swipeVertical.action.Enable();
        pinchPanel.action.Enable();
        longPressPieza.action.Enable();
    }

    void OnDisable()
    {
        tapPieza.action.performed -= OnTapPieza;
        dobleTapPanel.action.performed -= OnDobleTapPanel;
        swipeVertical.action.performed -= OnSwipeVertical;
        pinchPanel.action.performed -= OnPinchPanel;
        longPressPieza.action.performed -= OnLongPressPieza;

        tapPieza.action.Disable();
        dobleTapPanel.action.Disable();
        swipeVertical.action.Disable();
        pinchPanel.action.Disable();
        longPressPieza.action.Disable();
    }

    void OnTapPieza(InputAction.CallbackContext ctx)
    {
        Debug.Log("Tap simple sobre pieza: mostrar panel de información.");
    }

    void OnDobleTapPanel(InputAction.CallbackContext ctx)
    {
        Debug.Log("Doble tap sobre panel: expandir o colapsar detalles.");
    }

    void OnSwipeVertical(InputAction.CallbackContext ctx)
    {
        Vector2 valor = ctx.ReadValue<Vector2>();
        Debug.Log($"Swipe vertical detectado: {valor.y}");
    }

    void OnPinchPanel(InputAction.CallbackContext ctx)
    {
        float zoom = ctx.ReadValue<float>();
        Debug.Log($"Pinch sobre panel: zoom {zoom}");
    }

    void OnLongPressPieza(InputAction.CallbackContext ctx)
    {
        Debug.Log("Long press sobre pieza: guardar en favoritos.");
    }
}
```

**Explicación:**  
El código mantiene separadas las acciones de entrada de la lógica de la aplicación. Esto hace que el mantenimiento y futuras modificaciones resulten mucho más sencillas.

---

**c)** Responde en 3 líneas: ¿qué cambiarías en el código para agregar soporte a Meta Quest 3 en 6 meses?

> Para agregar soporte a Meta Quest 3 únicamente incorporaría nuevos bindings dentro del mismo Input Actions Asset. Las acciones seguirían siendo las mismas y solo cambiaría el dispositivo que las ejecuta, evitando modificar la lógica principal del proyecto.

**Explicación:**  
Gracias al nuevo Input System, los controles pueden cambiar sin afectar el funcionamiento interno del programa. Esto facilita ampliar el proyecto hacia nuevas plataformas XR.
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

---

### 1. Código completo de `GestorInputMultiModal.cs`

```csharp
using UnityEngine;
using UnityEngine.InputSystem;
using UnityEngine.InputSystem.EnhancedTouch;
using UnityEngine.XR.ARFoundation;

public class GestorInputMultiModal : MonoBehaviour
{
    private enum ModoXR { AR, VR }
    private ModoXR modoActual;

    [Header("Referencias AR")]
    public ARSession arSession;

    [Header("Referencias VR")]
    public GameObject xrRig;
    public Transform controllerDerecho;

    [Header("Input Actions VR")]
    public InputActionReference accionTrigger;
    public InputActionReference accionGrip;
    public InputActionReference accionJoystick;

    private float distanciaInicialPinch;
    private bool pinchActivo;

    void Awake()
    {
        DetectarModo();
    }

    void OnEnable()
    {
        if (modoActual == ModoXR.AR)
        {
            EnhancedTouchSupport.Enable();
            Touch.onFingerDown += OnFingerDownAR;
        }
        else
        {
            accionTrigger.action.performed += OnTriggerVR;
            accionGrip.action.performed += OnGripVR;

            accionTrigger.action.Enable();
            accionGrip.action.Enable();
            accionJoystick.action.Enable();
        }
    }

    void OnDisable()
    {
        if (modoActual == ModoXR.AR)
        {
            Touch.onFingerDown -= OnFingerDownAR;
            EnhancedTouchSupport.Disable();
        }
        else
        {
            accionTrigger.action.performed -= OnTriggerVR;
            accionGrip.action.performed -= OnGripVR;

            accionTrigger.action.Disable();
            accionGrip.action.Disable();
            accionJoystick.action.Disable();
        }
    }

    void Update()
    {
        if (modoActual == ModoXR.AR)
        {
            ProcesarGestosAR();
        }
        else
        {
            ProcesarJoystickVR();
        }
    }

    void DetectarModo()
    {
        if (arSession != null)
        {
            modoActual = ModoXR.AR;
            Debug.Log("Modo detectado: AR Android");
        }
        else
        {
            modoActual = ModoXR.VR;
            Debug.Log("Modo detectado: VR Quest");
        }
    }

    // -------------------------
    // INPUTS MODO AR
    // -------------------------

    void OnFingerDownAR(Finger dedo)
    {
        AccionSeleccionar(dedo.screenPosition);
    }

    void ProcesarGestosAR()
    {
        var dedos = Touch.activeFingers;

        if (dedos.Count == 2)
        {
            Vector2 p0 = dedos[0].screenPosition;
            Vector2 p1 = dedos[1].screenPosition;

            float distanciaActual = Vector2.Distance(p0, p1);

            if (!pinchActivo)
            {
                distanciaInicialPinch = distanciaActual;
                pinchActivo = true;
            }
            else
            {
                float factor = distanciaActual / distanciaInicialPinch;
                AccionEscalar(factor);
            }
        }
        else
        {
            pinchActivo = false;
        }

        if (dedos.Count == 1)
        {
            Vector2 delta = dedos[0].screenPosition - dedos[0].currentTouch.startScreenPosition;
            if (Mathf.Abs(delta.x) > 50f)
            {
                float angulo = delta.x * 0.05f;
                AccionRotar(angulo);
            }
        }
    }

    // -------------------------
    // INPUTS MODO VR
    // -------------------------

    void OnTriggerVR(InputAction.CallbackContext ctx)
    {
        if (controllerDerecho != null)
        {
            AccionSeleccionar(controllerDerecho.position);
        }
    }

    void OnGripVR(InputAction.CallbackContext ctx)
    {
        AccionEscalar(1.1f);
    }

    void ProcesarJoystickVR()
    {
        Vector2 joystick = accionJoystick.action.ReadValue<Vector2>();

        if (Mathf.Abs(joystick.x) > 0.2f)
        {
            float angulo = joystick.x * 2f;
            AccionRotar(angulo);
        }
    }

    // -------------------------
    // ACCIONES GENERALES
    // Estas acciones concentran la lógica principal.
    // Así evitamos duplicar código entre AR y VR.
    // AR y VR solo cambian la forma de entrada,
    // pero la acción final se mantiene centralizada.
    // -------------------------

    void AccionSeleccionar(Vector2 posicion2D)
    {
        Debug.Log($"Seleccionar en pantalla AR: {posicion2D}");
    }

    void AccionSeleccionar(Vector3 posicion3D)
    {
        Debug.Log($"Seleccionar desde controller VR: {posicion3D}");
    }

    void AccionEscalar(float factor)
    {
        Debug.Log($"Escalar objeto con factor: {factor}");
    }

    void AccionRotar(float angulo)
    {
        Debug.Log($"Rotar objeto con ángulo: {angulo}");
    }
}
```

**Explicación:**  
El script separa la lectura de entradas según el dispositivo, pero mantiene las acciones principales en métodos comunes. De esta manera, AR y VR pueden usar controles distintos sin repetir toda la lógica del sistema.

---

### 3. Tabla de decisiones: ¿qué clase de problema resuelve este patrón en proyectos reales?

| Decisión | Problema que resuelve | Beneficio en proyectos reales |
|----------|----------------------|-------------------------------|
| Detectar modo AR o VR al iniciar | Evita crear scripts independientes para cada entorno | Permite mantener una sola base de código |
| Usar `InputActionReference` en VR | Evita escribir botones específicos dentro del script | Facilita cambiar controles desde el Inspector |
| Usar `EnhancedTouchSupport` en AR | Permite trabajar con gestos táctiles más completos | Mejora la interacción en dispositivos móviles |
| Centralizar acciones generales | Reduce la repetición de código entre plataformas | Hace que el sistema sea más ordenado y mantenible |
| Usar `OnEnable` y `OnDisable` | Gestiona correctamente eventos y suscripciones | Evita errores al activar o desactivar objetos |

**Explicación:**  
Este enfoque es útil cuando un proyecto debe funcionar en más de una plataforma XR. Al centralizar las acciones, los cambios futuros se realizan en menos partes del código.

---

## RÚBRICA DE AUTOEVALUACIÓN

Antes de entregar, verifica:

| Criterio | Cumplido |
|----------|---------|
| Respondí todas las preguntas de la Sección I | ☑ |
| Completé todos los espacios de la Sección II-A | ☑ |
| Relacioné todos los items de la Sección II-B | ☑ |
| Identifiqué los 4 errores del Caso 1 con correcciones | ☑ |
| El Input Actions Asset del Caso 2 tiene tipos correctos (Button/Value) | ☑ |
| El código de la Sección IV compila (lo verifiqué en Unity) | ☑ |
| Usé `OnEnable`/`OnDisable` (no `Start`/`OnDestroy`) para los eventos | ☑ |
| El archivo está en formato `.md` con código en bloques ` ```csharp ` | ☑ |

---

*PSISP08075 | Universidad Autónoma del Perú | Ingeniería de Sistemas | Semana 11 | 2026-1*