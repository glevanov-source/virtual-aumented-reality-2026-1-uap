# S11 — SESIÓN DE CLASE COMPLETA
## Curso: Realidad Virtual y Aumentada | PSISP08075
### Semana 11 — Scripts e Inputs en XR

---

| Campo | Detalle |
|-------|---------|
| **Curso** | Realidad Virtual y Aumentada — PSISP08075 |
| **Semana** | 11 |
| **Tema** | Scripts e Inputs en XR: programación de interacciones |
| **Valor transversal** | Creatividad y perseverancia en la programación de funcionalidades e interacciones XR |
| **Logro de aprendizaje** | Al finalizar la sesión, el estudiante implementa interacciones básicas en la experiencia XR, demostrando perseverancia en la programación de funcionalidades |
| **Duración total** | 3 horas (180 min) + 20 min receso |
| **Modalidad** | Presencial con práctica en equipo |

---

## TABLA DE CONTENIDOS

1. [Cronograma de la sesión](#cronograma)
2. [INICIO](#inicio)
3. [UTILIDAD](#utilidad)
4. [TRANSFORMACIÓN](#transformacion)
   - 4.1 El Input System de Unity: old vs new
   - 4.2 Touch Input en AR móvil
   - 4.3 Controller Input en VR
   - 4.4 InputActionReference — el patrón profesional
   - 4.5 XR Interaction Events
   - 4.6 Pruebas con XR Device Simulator
5. [RECESO](#receso)
6. [PRÁCTICA](#practica)
7. [CIERRE](#cierre)
8. [Guion verbal sugerido](#guion)
9. [Casos reales recomendados](#casos)
10. [Referencias APA 7](#referencias)

---

## CRONOGRAMA DE LA SESIÓN {#cronograma}

| # | Bloque | Actividad | Duración | Responsable |
|---|--------|-----------|----------|-------------|
| 1 | INICIO | Rompe-hielo + Logro + Revisión S10 + Diagnóstico | 20 min | Docente |
| 2 | UTILIDAD | Por qué importa / contexto profesional | 10 min | Docente + Estudiantes |
| 3 | TRANSFORMACIÓN | Input System, Touch AR, Controller VR, InputActionReference | 70 min | Docente (activo) |
| — | **RECESO** | — | **20 min** | — |
| 4 | PRÁCTICA | Caso grupal + ejercicio individual | 40 min | Estudiantes |
| 5 | CIERRE | Síntesis + metacognición + tarea | 10 min | Docente + Estudiantes |
| — | **TOTAL** | — | **170 min + 20** | — |

---

## 1. INICIO (20 min) {#inicio}

### a) ROMPE-HIELO — "El gesto secreto" (5 min)

**Instrucción verbal al docente:**

> "Buenos días. Antes de empezar, una pregunta rápida — no hace falta pensar mucho. Muestren con sus manos el gesto que más usan en su celular: puede ser swipe, pinch, tap, long press, lo que sea. Háganlo en el aire, ahora."

Esperar que todos hagan el gesto. Luego:

> "Ahora la pregunta real: ¿saben cómo Unity detecta ESE gesto que acaban de hacer? ¿Qué pasa dentro del código cuando el sistema operativo registra que pusieron dos dedos en la pantalla y los alejaron? Hoy van a entenderlo — y van a programarlo."

**Propósito:** activar la conexión entre lo cotidiano (gestos táctiles) y el tema técnico (Input System). El movimiento físico genera atención.

---

### b) LOGRO DE APRENDIZAJE (3 min)

**Guion verbal textual:**

> "El logro de hoy tiene tres partes. Primero: van a entender cómo Unity maneja los inputs — tanto el Input System antiguo como el nuevo. Segundo: van a escribir scripts que respondan a toques táctiles en AR y a botones de controller en VR. Tercero: van a probar esas interacciones con el XR Device Simulator, sin necesidad de tener el celular o las gafas en mano.
>
> Al final de esta sesión, deberían poder abrir un proyecto Unity en blanco y escribir desde cero un script que detecte un tap, un pinch y un botón de controller. Eso es lo que evaluaré en el lab."

---

### c) REVISIÓN SESIÓN ANTERIOR (7 min)

**Pregunta 1:**

> "En la semana 10 implementaron gestos táctiles. Explíquenme: ¿cuál es la diferencia entre `Physics.Raycast` y `ARRaycastManager.Raycast`? ¿A qué golpea cada uno?"

**Respuesta esperada:**
- `Physics.Raycast`: detecta Colliders de GameObjects de Unity — sirve para saber si el usuario tocó un objeto 3D ya instanciado en la escena.
- `ARRaycastManager.Raycast`: detecta trackables AR (planos, feature points detectados por ARCore) — sirve para encontrar una posición en el mundo real donde colocar un objeto nuevo.
- Son complementarios: primero se comprueba si el toque fue sobre un objeto existente (`Physics.Raycast`), y si no, se hace el ARRaycast para colocar uno nuevo.

**Si un estudiante responde mal:** "Casi. Recuerden que ARRaycastManager trabaja con el mundo real detectado por ARCore — no con los Colliders de Unity. Son dos capas diferentes: la capa de Unity 3D y la capa de AR del dispositivo. ¿Alguien quiere completar la respuesta?"

---

**Pregunta 2:**

> "¿Por qué se declara `List<ARRaycastHit> hits` como campo de clase (privado fuera de Update) y no dentro del método Update()?"

**Respuesta esperada:**
- Si se crea `new List<>()` dentro de `Update()`, se crea una nueva instancia en heap en cada frame (60 veces por segundo).
- Esto genera garbage (basura de memoria) que el Garbage Collector (GC) de Unity debe limpiar periódicamente.
- Las pausas del GC causan micro-stutters visibles en AR/VR — especialmente problemáticas porque rompen la inmersión.
- Al declararla como campo de clase y llamar `.Clear()` en lugar de `new`, se reutiliza el mismo objeto de lista.

**Si responde mal:** "Piensen en el GC de C#. Cada vez que crean un objeto con `new`, ese objeto vive en el heap y el GC tiene que limpiarlo eventualmente. En AR/VR, ese micro-parone se nota. La solución es reutilizar."

---

**Pregunta 3:**

> "En el gesto de pinch, usaron `Mathf.Clamp`. ¿Qué hace y por qué es necesario en ese contexto?"

**Respuesta esperada:**
- `Mathf.Clamp(valor, min, max)` limita un valor a un rango definido.
- Sin Clamp, si el usuario hace pinch muy agresivo, el objeto podría escalar a 0 (invisible) o a valores enormes (llena toda la pantalla).
- Los valores `0.05f` y `5f` definen el rango seguro de escala para la UX.

---

### d) DIAGNÓSTICO INICIAL (5 min)

**Instrucción al docente:** estas preguntas son para medir qué saben de Input System — no corregir, solo escuchar y tomar nota mental para calibrar la velocidad de la Transformación.

**Pregunta 1:** "¿Alguien sabe qué es el 'Input System' de Unity? ¿Es lo mismo que `Input.GetKeyDown()`?"

**Respuesta esperada:** `Input.GetKeyDown()` es el Input System antiguo (legacy). El nuevo Input System de Unity es un paquete separado (`com.unity.inputsystem`) que usa un modelo basado en acciones (Actions) y bindings. La diferencia principal es que el nuevo es más flexible y soporta múltiples dispositivos (teclado, gamepad, XR controllers) con la misma acción.

**Pregunta 2:** "¿Qué es un 'binding' en el contexto del Input System?"

**Respuesta esperada:** un binding es la asignación entre una acción abstracta (ej: "Disparo") y una entrada física concreta (ej: tecla espacio, botón A del gamepad, trigger del controller VR). Permite cambiar el dispositivo sin cambiar el código.

**Pregunta 3:** "¿Cómo detectarían el botón 'Trigger' de un controller VR de Meta Quest en Unity?"

**Respuesta esperada (aceptable sin saber la respuesta exacta):** se puede usar el nuevo Input System con bindings específicos para XR controllers, o el componente `XRController` del XR Interaction Toolkit. Si no saben: es exactamente lo que van a aprender hoy.

---

## 2. UTILIDAD (10 min) {#utilidad}

### Por qué los inputs XR son críticos

El 78% de los usuarios abandona una app XR en los primeros 90 segundos si la interacción no responde como espera (Nielsen Norman Group, 2022). No importa qué tan bueno sea el arte 3D o la detección AR — si el script de input no captura correctamente el gesto del usuario, la experiencia falla.

**Problema concreto que resuelve este tema:**

> Un desarrollador junior en una empresa de simulación industrial escribe `Input.GetMouseButtonDown(0)` para detectar el click en su app AR. Funciona perfectamente en el editor de Unity, pero cuando compila para Android, no detecta nada. El toque del celular no es un click de ratón. El junior pierde 3 días buscando el error.

Este es el error más común que cometen los desarrolladores que vienen de videojuegos tradicionales al llegar a XR móvil. Hoy aprenden por qué ocurre y cómo evitarlo.

### Aplicación en empresas reales

- **Meta Quest Platform:** las apps del Quest Store deben implementar interacciones con el nuevo Input System — Meta rechaza apps que usen el legacy Input System en 2024.
- **Microsoft HoloLens 2:** usa Mixed Reality Toolkit (MRTK) que está construido sobre el nuevo Input System de Unity.
- **Niantic (Pokémon GO):** el equipo de AR de Niantic usa ARCore + gestión de inputs táctiles optimizada para minimizar latencia en la detección de toques.
- **TOYOTA Manufacturing:** usa HoloLens con scripts de input personalizados para guiar a trabajadores en líneas de ensamblaje — cada gesto incorrecto detiene la línea.

### Pregunta retadora de apertura

> "Un controller VR de Meta Quest 3 tiene 12 entradas diferentes: 2 triggers, 2 grips, 4 botones frontales, 2 joysticks, 2 joystick press. ¿Cómo organizarían el código para manejar todas esas entradas sin escribir 12 bloques if-else separados? ¿Existe una arquitectura mejor?"

**Respuesta esperada:** el sistema de Input Actions de Unity resuelve exactamente esto — cada acción (Agarrar, Disparar, Menú, Moverse) se define una sola vez, y sus bindings pueden apuntar a cualquier entrada física. El código solo escucha la acción, no el botón específico.

---

## 3. TRANSFORMACIÓN (70 min) {#transformacion}

### 3.1 El Input System de Unity: Antiguo vs Nuevo (12 min)

**Explicación conceptual:**

Unity tiene dos sistemas de input que coexisten:

```
Sistema Legacy (antiguo)           Sistema Nuevo (Input System package)
────────────────────────           ──────────────────────────────────
Input.GetKeyDown(KeyCode.Space)    InputAction disparar; disparar.performed += ...
Input.GetAxis("Horizontal")        InputActionAsset con Actions y Bindings
Input.GetMouseButtonDown(0)        InputActionReference en el Inspector
Input.touchCount                   EnhancedTouchSupport.Enable() + Touch API
```

**¿Por qué el nuevo es mejor para XR?**

```
LEGACY                              NUEVO (Input System)
──────                              ──────────────────
Escribe: Input.GetKey(KeyCode.A)   Escribe: accionMover.ReadValue<Vector2>()
Problema: depende del teclado       Ventaja: funciona con cualquier dispositivo
No funciona en: Quest, HoloLens,   Funciona en: todos los dispositivos XR,
  celulares (touch ≠ mouse)          gamepads, teclado, ratón, XR controllers
```

**Instalación del nuevo Input System:**

```
Window → Package Manager → Unity Registry → Input System → Install
Edit → Project Settings → Player → Other Settings → 
  Active Input Handling → Input System Package (New)
```

---

**PREGUNTA AL GRUPO 1:**

> "¿Si instalan el nuevo Input System pero en el código usan `Input.GetKey()` del legacy, qué creen que pasa?"

**Respuesta esperada:** en modo "Input System Package (New)", el legacy `Input` class puede arrojar excepciones o simplemente devolver valores vacíos/falsos porque el backend de input ya no está activo. Unity da la opción de usar "Both" (ambos sistemas a la vez) para compatibilidad, pero no es recomendable para proyectos XR nuevos porque agrega overhead.

---

**Mini actividad (3 min):** todos abren Unity → Package Manager → verificar si tienen el Input System instalado. Anotar la versión que tienen.

---

### 3.2 Touch Input en AR Móvil — La forma correcta (15 min)

**Explicación conceptual:**

En AR móvil (Android con ARCore), los inputs táctiles se gestionan de dos formas:

**Forma 1 — Legacy (solo funciona en editor con ratón):**
```csharp
// ❌ NO usar en AR móvil
if (Input.GetMouseButtonDown(0))
{
    // Esto funciona en editor pero NO en Android táctil
}
```

**Forma 2 — Touch API (funciona en Android, RECOMENDADA para AR):**
```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System.Collections.Generic;

public class InputARMovil : MonoBehaviour
{
    private ARRaycastManager arRaycastManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>(); // campo de clase, no dentro de Update

    void Awake() => arRaycastManager = GetComponent<ARRaycastManager>();

    void Update()
    {
        // Input.touchCount: número de dedos activos en pantalla
        if (Input.touchCount == 0) return; // sin toques → salir

        Touch toque = Input.GetTouch(0);   // primer dedo

        // TouchPhase.Began = solo el PRIMER frame del toque
        // Evita que el raycast se ejecute múltiples frames
        if (toque.phase != TouchPhase.Began) return;

        // toque.position = coordenadas de pantalla (píxeles)
        hits.Clear();
        if (arRaycastManager.Raycast(toque.position, hits, TrackableType.PlaneWithinPolygon))
        {
            // hits[0].pose.position = posición 3D en el mundo real
            Debug.Log($"Toque en plano AR: {hits[0].pose.position}");
        }
    }
}
```

**Forma 3 — Nuevo Input System con Enhanced Touch (AR + gestos avanzados):**
```csharp
using UnityEngine;
using UnityEngine.InputSystem.EnhancedTouch;
using Touch = UnityEngine.InputSystem.EnhancedTouch.Touch;

public class InputARNuevo : MonoBehaviour
{
    void OnEnable()
    {
        EnhancedTouchSupport.Enable();         // activar en OnEnable
        Touch.onFingerDown += AlTocar;
        Touch.onFingerMove += AlMover;
    }

    void OnDisable()
    {
        Touch.onFingerDown -= AlTocar;         // siempre desuscribirse
        Touch.onFingerMove -= AlMover;
        EnhancedTouchSupport.Disable();
    }

    void AlTocar(Finger dedo)
    {
        // dedo.screenPosition = posición en pantalla
        // dedo.index = índice del dedo (0 = primero, 1 = segundo)
        Debug.Log($"Dedo {dedo.index} tocó en: {dedo.screenPosition}");
    }

    void AlMover(Finger dedo)
    {
        // dedo.currentTouch.delta = cuánto se movió desde el último frame
        Vector2 delta = dedo.currentTouch.delta;
        Debug.Log($"Dedo {dedo.index} se movió: {delta}");
    }
}
```

---

**PREGUNTA AL GRUPO 2:**

> "¿Por qué es más seguro usar `Touch.onFingerDown` con evento (+=) que verificar `Input.touchCount > 0` en Update()?"

**Respuesta esperada:** el modelo de eventos (callbacks) desacopla la detección del input de la lógica de negocio. `Input.touchCount` se evalúa 60+ veces por segundo aunque no haya toques — es un polling continuo. El evento se dispara solo cuando ocurre el toque. Además, con el modelo de eventos es más fácil tener múltiples suscriptores al mismo evento sin modificar el código de detección.

---

**PREGUNTA AL GRUPO 3:**

> "Tienen un script AR donde `EnhancedTouchSupport.Enable()` está en `Start()`. Al volver de otra escena, el touch deja de funcionar. ¿Cuál es el error y cómo lo corrigen?"

**Respuesta esperada:** `Enable()` debe estar en `OnEnable()` y `Disable()` en `OnDisable()` — no en `Start()`/`OnDestroy()`. Cuando la escena se recarga o el GameObject se desactiva y reactiva, `Start()` no se llama de nuevo pero `OnEnable()` sí. Si se pone en `Start()`, al reactivar el objeto el soporte no se reactiva y los eventos no se suscriben correctamente.

---

**Error común de desarrolladores:**

```csharp
// ❌ ERROR FRECUENTE: llamar Enable en Start y Disable en OnDestroy
void Start() => EnhancedTouchSupport.Enable();
void OnDestroy() => EnhancedTouchSupport.Disable();
// Problema: al pausar/reanudar el juego, OnDisable/OnEnable se llaman
// pero el soporte ya no está alineado con el estado del objeto

// ✅ CORRECTO: usar OnEnable/OnDisable para sincronizar con el ciclo de Unity
void OnEnable()  { EnhancedTouchSupport.Enable();  Touch.onFingerDown += Handler; }
void OnDisable() { Touch.onFingerDown -= Handler;  EnhancedTouchSupport.Disable(); }
```

---

### 3.3 Controller Input en VR con XR Interaction Toolkit (15 min)

**Explicación conceptual:**

En VR (Quest, Pico, Index), los inputs vienen de controllers físicos. El XR Interaction Toolkit (XRI) abstrae las diferencias entre fabricantes:

```
Quest 3 Controller           Pico 4 Controller         Valve Index
───────────────────          ─────────────────         ─────────────
Primary Button (A/X)    →   Primary Button        →   Primary Button
Secondary Button (B/Y)  →   Secondary Button      →   Secondary Button  
Trigger (Índice)        →   Trigger               →   Trigger
Grip (Medio)            →   Grip                  →   Grip
Thumbstick              →   Thumbstick            →   Touchpad

Todo mapeado a InputActions del Input System ✅
```

**Leer entradas de controller con InputActionReference:**

```csharp
using UnityEngine;
using UnityEngine.InputSystem;
using UnityEngine.XR.Interaction.Toolkit;

public class ControllerVR : MonoBehaviour
{
    [Header("Acciones de Input — asignar desde Inspector")]
    // InputActionReference permite asignar el binding desde el Inspector
    // sin hardcodear el nombre del botón en el código
    public InputActionReference accionTrigger;
    public InputActionReference accionGrip;
    public InputActionReference accionMover;
    public InputActionReference accionBotonA;

    void OnEnable()
    {
        // Suscribirse a los eventos de las acciones
        accionTrigger.action.performed += AlPresionarTrigger;
        accionTrigger.action.canceled  += AlSoltarTrigger;
        accionGrip.action.performed    += AlAgarrar;
        accionBotonA.action.performed  += AlPresionarA;

        // Activar las acciones (importante si no están en un ActionMap activo)
        accionTrigger.action.Enable();
        accionGrip.action.Enable();
        accionMover.action.Enable();
        accionBotonA.action.Enable();
    }

    void OnDisable()
    {
        // Siempre desuscribirse para evitar memory leaks
        accionTrigger.action.performed -= AlPresionarTrigger;
        accionTrigger.action.canceled  -= AlSoltarTrigger;
        accionGrip.action.performed    -= AlAgarrar;
        accionBotonA.action.performed  -= AlPresionarA;
    }

    void AlPresionarTrigger(InputAction.CallbackContext ctx)
    {
        // ctx.ReadValue<float>() = valor del trigger (0.0 a 1.0)
        float presion = ctx.ReadValue<float>();
        Debug.Log($"Trigger presionado: {presion:F2}");
    }

    void AlSoltarTrigger(InputAction.CallbackContext ctx)
    {
        Debug.Log("Trigger soltado");
    }

    void AlAgarrar(InputAction.CallbackContext ctx)
    {
        // Grip: también float 0-1, pero típicamente se trata como bool
        bool agarrando = ctx.ReadValue<float>() > 0.5f;
        Debug.Log($"Grip: {(agarrando ? "Agarrando" : "Soltando")}");
    }

    void AlPresionarA(InputAction.CallbackContext ctx)
    {
        Debug.Log("Botón A presionado");
    }

    void Update()
    {
        // Para valores continuos como el joystick: ReadValue en Update
        // No usar eventos para entradas continuas
        if (accionMover.action.enabled)
        {
            Vector2 movimiento = accionMover.action.ReadValue<Vector2>();
            if (movimiento.magnitude > 0.1f) // dead zone
                Debug.Log($"Joystick: {movimiento}");
        }
    }
}
```

---

**PREGUNTA AL GRUPO 4:**

> "¿Cuál es la diferencia entre usar el callback `performed` vs leer `ReadValue()` en `Update()`? ¿Cuándo es mejor cada uno?"

**Respuesta esperada:**
- `performed` (evento): se dispara **una vez** cuando la acción se activa (ej: al presionar un botón). Ideal para acciones discretas: disparar, saltar, abrir menú.
- `ReadValue<T>()` en Update: lee el valor **cada frame**. Ideal para entradas analógicas continuas: joystick de movimiento, trigger como palanca de aceleración, posición del puntero.
- Error común: usar `performed` para joystick (solo se activa al cambiar de zona) → el movimiento se siente entrecortado.

---

### 3.4 InputActionReference — El patrón profesional (12 min)

**¿Por qué InputActionReference es el patrón recomendado en 2024?**

```
Enfoque A — Hardcoded (evitar)
─────────────────────────────
void Update()
{
    if (Keyboard.current.spaceKey.wasPressedThisFrame)
        Saltar();
}
// Problema: el código está atado al teclado
// Si cambian a gamepad → reescribir el código

Enfoque B — InputActionAsset en código (mejor, pero acoplado)
─────────────────────────────────────────────────────────────
public InputActionAsset actions;
void Start() => actions["Gameplay/Saltar"].performed += ctx => Saltar();
// Problema: el nombre "Gameplay/Saltar" es un string → typos en runtime

Enfoque C — InputActionReference (RECOMENDADO)
──────────────────────────────────────────────
public InputActionReference accionSaltar;  // se asigna desde Inspector
void OnEnable() => accionSaltar.action.performed += ctx => Saltar();
// Ventaja: el binding se configura en el Inspector
// El código no sabe NI le importa qué tecla/botón está asignado
// Cambiar de teclado a VR controller: solo cambiar el binding en el Inspector
```

**Configuración visual del InputActionReference:**

```
1. Project → Create → Input Actions → nombre: XRInputActions
2. Agregar Action Map: "XR Gameplay"
3. Agregar Actions:
   - Colocar (Button): binding → <XRController>/triggerButton
   - Mover (Value → Vector2): binding → <XRController>/thumbstick
   - Seleccionar (Button): binding → <XRController>/primaryButton
4. En el script: public InputActionReference accionColocar;
5. En Inspector: arrastrar la acción "Colocar" al campo
```

---

**PREGUNTA AL GRUPO 5:**

> "Un compañero dice: 'No entiendo para qué sirve InputActionReference — es más fácil poner directamente `Keyboard.current.spaceKey` en el código'. ¿Cómo le explicarían el problema con ese enfoque en el contexto XR?"

**Respuesta esperada:** en XR, la app puede correr en dispositivos completamente diferentes: hoy en Android táctil, mañana en Meta Quest con controllers, pasado en HoloLens con air-tap. Si el código hardcodea `Keyboard.current`, funciona solo en un dispositivo. Con InputActionReference, el mismo script funciona en todos los dispositivos — solo se cambian los bindings en el Input Actions Asset, sin tocar el código. Es el principio de Open/Closed: abierto a extensión (nuevos dispositivos), cerrado a modificación (el código no cambia).

---

### 3.5 XR Interaction Events — El sistema de eventos de XRI (8 min)

**Los componentes XRI emiten eventos que se pueden escuchar:**

```csharp
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit;

// XRGrabInteractable emite eventos que cualquier script puede escuchar
[RequireComponent(typeof(XRGrabInteractable))]
public class ObjetoInteractivo : MonoBehaviour
{
    private XRGrabInteractable interactable;

    void Awake()
    {
        interactable = GetComponent<XRGrabInteractable>();

        // Suscribirse a los eventos del interactable
        interactable.selectEntered.AddListener(AlAgarrar);
        interactable.selectExited.AddListener(AlSoltar);
        interactable.hoverEntered.AddListener(AlApuntar);   // controller apunta al objeto
        interactable.activated.AddListener(AlActivar);       // trigger mientras se agarra
    }

    void OnDestroy()
    {
        // Limpiar listeners al destruir el objeto
        interactable.selectEntered.RemoveListener(AlAgarrar);
        interactable.selectExited.RemoveListener(AlSoltar);
    }

    void AlAgarrar(SelectEnterEventArgs args)
    {
        // args.interactorObject = el controller que agarró
        Debug.Log($"Agarrado por: {args.interactorObject.transform.name}");
        GetComponent<Renderer>().material.color = Color.yellow;
    }

    void AlSoltar(SelectExitEventArgs args)
    {
        GetComponent<Renderer>().material.color = Color.white;
    }

    void AlApuntar(HoverEnterEventArgs args)
    {
        // Resaltar el objeto cuando el controller lo apunta
        GetComponent<Renderer>().material.color = Color.cyan;
    }

    void AlActivar(ActivateEventArgs args)
    {
        // Trigger presionado mientras se agarra: acción secundaria
        Debug.Log("Objeto activado mientras es agarrado");
    }
}
```

---

**PREGUNTA AL GRUPO 6:**

> "¿Por qué es importante llamar `RemoveListener` en `OnDestroy()` para los eventos de XRGrabInteractable? ¿Qué pasa si no lo hacen?"

**Respuesta esperada:** XRGrabInteractable mantiene referencias a los listeners que se suscriben con `AddListener`. Si el objeto 3D es destruido (`Destroy(gameObject)`) pero los listeners no se removieron, el XRGrabInteractable (que puede seguir existiendo en otro objeto) mantiene una referencia al script destruido. Cuando ocurre el evento, intenta invocar el método en un objeto null → `MissingReferenceException`. Es un memory/reference leak. `RemoveListener` en `OnDestroy` previene esto.

---

### 3.6 Pruebas con XR Device Simulator (8 min)

**El XR Device Simulator permite probar interacciones XR SIN hardware:**

```
Instalación:
Window → Package Manager → XR Interaction Toolkit → Samples → XR Device Simulator → Import

Uso:
1. Arrastrar prefab "XR Device Simulator" a la Hierarchy
2. Play ▶️
3. Controles:
   - WASD: mover la cámara (posición)
   - Ratón (mantener clic derecho): rotar la vista
   - T: alternar control entre cámara y controllers
   - G: Grip del controller
   - Shift (izquierdo): activa el controller izquierdo
   - Espacio: activa el controller derecho
   - Click izquierdo: Trigger del controller activo
```

**PREGUNTA AL GRUPO 7:**

> "¿Para qué sirve el XR Device Simulator si igual van a probar en el celular? ¿No es un paso extra innecesario?"

**Respuesta esperada:** el ciclo Build→Install→Test en Android toma 3-7 minutos por iteración. Con el Simulator, el ciclo es Play→Test→Stop en 2 segundos. Para depurar la lógica de scripts (si la acción se dispara, si el evento llega, si el cálculo es correcto), el Simulator es 20-30x más rápido que compilar. Solo se compila al Android cuando la lógica ya funciona en el Simulator. Es exactamente el mismo principio que probar código en unit tests antes de desplegarlo a producción.

---

**PREGUNTA AL GRUPO 8:**

> "¿Cuál es el riesgo de SOLO probar con el XR Device Simulator y nunca en el hardware real?"

**Respuesta esperada:** el Simulator no emula todas las características del hardware real: latencia de tracking, resolución de pantalla, peso del dispositivo (cybersickness), rendimiento real del GPU/CPU del dispositivo, temperatura del celular bajo carga, comportamiento del ARCore en diferentes condiciones de iluminación. Un app que funciona perfectamente en Simulator puede tener:
- 12 FPS en el celular real (rendimiento)
- Tracking inestable (condiciones de luz)
- Gestos que no se detectan bien (la pantalla física es diferente del ratón)

El Simulator es para la lógica; el hardware real es para la experiencia.

---

## RECESO (20 min) {#receso}

---

## 4. PRÁCTICA (40 min) {#practica}

### a) CASO PRÁCTICO GRUPAL (25 min)

**Escenario:**

> La empresa *PerúTech Inmersivo* está desarrollando una app de capacitación AR para técnicos electricistas de Enel Perú. El técnico apunta el celular a un tablero eléctrico → la app detecta el tablero (Image Target) → aparece un panel AR con instrucciones de seguridad. El técnico puede: (1) navegar por los pasos con swipe vertical, (2) ampliar el diagrama con pinch, (3) marcar un paso como completado con doble tap. El jefe de proyecto pide que el sistema de inputs sea extensible — en 6 meses van a agregar soporte para HoloLens.

**Tarea grupal (grupos de 3-4):**

1. Diseñar el sistema de inputs usando InputActionReference (no hardcoded)
2. Definir las 3 acciones (NavegaciónPasos, ZoomDiagrama, MarcarCompleto) con sus tipos de valor
3. Escribir el esqueleto del script `InputElectricista.cs` con la suscripción correcta a eventos
4. Responder: ¿cómo cambiarían el binding de "touch swipe" a "HoloLens air-gesture" sin modificar el script?

**Producto esperado:** diagrama en papel con el Input Actions Asset diseñado + esqueleto de código.

**Preguntas de andamiaje (el docente hace mientras circula):**
- "¿Qué tipo de valor usarían para el swipe vertical? ¿`float` o `Vector2`?" (Vector2 — tiene dirección X e Y)
- "¿El pinch es una acción `Button` o `Value`?" (Value — retorna la escala del pinch, no solo si/no)
- "¿El doble tap lo detectarían con el Input System o con `TouchPhase`?" (ambos son válidos — discutir tradeoffs)
- "¿Dónde pondrían el `Enable()` de las acciones?" (en `OnEnable()`, no en `Start()`)

**Respuesta modelo (puesta en común):**

```
Input Actions Asset: ElectricistaActions
├── Action Map: "Capacitacion AR"
│   ├── NavegaciónPasos → Value → Vector2
│   │   └── Binding: <Touchscreen>/delta (swipe)
│   │       Futuro HoloLens: <HandInteraction>/airGestureDelta
│   ├── ZoomDiagrama → Value → float
│   │   └── Binding: <Touchscreen>/Pinch/Delta/magnitude
│   └── MarcarCompleto → Button
│       └── Binding: <Touchscreen>/DoubleTap
```

```csharp
public class InputElectricista : MonoBehaviour
{
    [Header("Acciones")]
    public InputActionReference accionNavegar;
    public InputActionReference accionZoom;
    public InputActionReference accionMarcar;

    void OnEnable()
    {
        accionNavegar.action.performed += AlNavegar;
        accionZoom.action.performed    += AlZoom;
        accionMarcar.action.performed  += AlMarcar;
        accionNavegar.action.Enable();
        accionZoom.action.Enable();
        accionMarcar.action.Enable();
    }

    void OnDisable()
    {
        accionNavegar.action.performed -= AlNavegar;
        accionZoom.action.performed    -= AlZoom;
        accionMarcar.action.performed  -= AlMarcar;
    }

    void AlNavegar(InputAction.CallbackContext ctx)
    {
        Vector2 delta = ctx.ReadValue<Vector2>();
        if (Mathf.Abs(delta.y) > Mathf.Abs(delta.x)) // swipe vertical
            NavegarPasos(delta.y > 0 ? 1 : -1);
    }

    void AlZoom(InputAction.CallbackContext ctx)
    {
        float factor = ctx.ReadValue<float>();
        AplicarZoom(factor);
    }

    void AlMarcar(InputAction.CallbackContext ctx) => MarcarPasoCompleto();

    // ← Para agregar HoloLens: solo cambiar el binding en el Inspector
    // El código NO cambia
    void NavegarPasos(int direccion) { /* ... */ }
    void AplicarZoom(float factor)   { /* ... */ }
    void MarcarPasoCompleto()         { /* ... */ }
}
```

**Cambiar de touch a HoloLens:** en el Input Actions Asset, cambiar el binding de `<Touchscreen>/delta` a `<HandInteraction>/airGestureDelta`. El script `InputElectricista.cs` **no se toca**. Eso es el patrón InputActionReference funcionando.

---

### b) EJERCICIO INDIVIDUAL (15 min)

**Tarea:** escribir un script `DetectorGestos.cs` que:

1. Detecte tap simple (1 dedo, `TouchPhase.Began`) → imprima "TAP en [posición]"
2. Detecte swipe horizontal (1 dedo, `TouchPhase.Moved`, `deltaPosition.x > 50`) → imprima "SWIPE DERECHA" o "SWIPE IZQUIERDA"
3. Detecte pinch (2 dedos) → imprima "PINCH — factor: [valor]" donde el factor es la razón entre distancia actual e inicial
4. Use `List<ARRaycastHit>` como campo de clase (no dentro de Update)
5. Se compile sin errores

**Criterio de éxito:** el estudiante abre la Console en Unity y ve los 3 mensajes distintos al hacer los gestos en el simulador.

**Plantilla base (el estudiante completa los espacios `// TODO:`):**

```csharp
using UnityEngine;
using System.Collections.Generic;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;

public class DetectorGestos : MonoBehaviour
{
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();

    // TODO: declarar variables privadas para pinch
    // (distancia inicial, escala inicial)

    void Update()
    {
        if (Input.touchCount == 0) return;

        if (Input.touchCount == 2)
        {
            // TODO: detectar pinch y calcular factor
        }
        else if (Input.touchCount == 1)
        {
            Touch t = Input.GetTouch(0);

            if (t.phase == TouchPhase.Began)
            {
                // TODO: imprimir TAP con la posición
            }
            else if (t.phase == TouchPhase.Moved)
            {
                // TODO: detectar swipe horizontal (umbral: |deltaPosition.x| > 50)
                // e imprimir SWIPE DERECHA o SWIPE IZQUIERDA
            }
        }
    }
}
```

---

## 5. CIERRE (10 min) {#cierre}

### a) SÍNTESIS COLABORATIVA (4 min)

**Pregunta de cierre 1:**

> "¿Cuál es la diferencia fundamental entre el Input System legacy y el nuevo? Den un ejemplo de cuándo el legacy falla en XR."

**Respuesta esperada:** el legacy usa polling con métodos estáticos (`Input.GetKey`, `Input.GetMouseButton`) que están atados a dispositivos específicos. En AR móvil, `GetMouseButtonDown(0)` no detecta toques — el sistema operativo de Android envía toques como `Touch`, no como mouse. El nuevo Input System usa un modelo de acciones + bindings que abstrae el dispositivo: el código escucha la acción "Seleccionar", sin importar si viene de un tap, un controller VR, un teclado o un air-tap de HoloLens.

**Pregunta de cierre 2:**

> "¿Por qué `OnEnable`/`OnDisable` son mejores que `Start`/`OnDestroy` para suscribirse a eventos de input?"

**Respuesta esperada:** `OnEnable`/`OnDisable` se sincronizan con el estado de activación del GameObject. Cuando el objeto se desactiva (por ejemplo, al cambiar de escena o al ocultar un panel), los suscriptores se limpian automáticamente. `Start` solo se llama una vez al iniciar — si el objeto se desactiva y reactiva, los eventos no se re-suscriben y el sistema deja de responder.

**Pregunta de cierre 3:**

> "Nombren 3 ventajas de usar InputActionReference en vez de hardcodear el binding en el código."

**Respuesta esperada:**
1. **Portabilidad:** cambiar de dispositivo (móvil → Quest → HoloLens) sin modificar el código.
2. **Mantenibilidad:** los diseñadores de UX pueden cambiar los bindings desde el Inspector sin saber C#.
3. **Reusar:** el mismo script funciona en múltiples proyectos con distintos bindings.

---

### b) METACOGNICIÓN (3 min)

> "En silencio, respondan mentalmente estas preguntas. No tienen que decirlas en voz alta."
>
> - ¿Qué concepto de hoy fue el más difícil de entender?
> - ¿Qué error de código te imaginas que cometerías al implementar esto por primera vez?
> - ¿Hay algo que el docente explicó y que aún no te quedó claro?

Luego:
> "Si levantaron la mano mental para alguna de estas — ese es su punto de partida para el laboratorio. El laboratorio existe para que cometan ese error y lo corrijan solos."

---

### c) TAREA / PUENTE (3 min)

**Tarea concreta:**

Implementar `DetectorGestos.cs` completamente (completar todos los TODO del ejercicio individual) y probarlo en el XR Device Simulator. Subir al repositorio del curso con el mensaje:

```
feat(s11): implementar DetectorGestos con tap swipe pinch
```

**Conexión con semana 12:**

> "La semana que viene agregamos lo que conecta esos inputs con objetos reales: interacciones avanzadas con la cámara AR, efectos de feedback visual y audio, y cómo el sistema de inputs escala a proyectos con 50+ objetos interactivos. Lo que aprendan hoy es la base — sin esto, la semana 12 no tiene sentido."

---

## GUION VERBAL SUGERIDO {#guion}

**Al abrir el tema del nuevo Input System:**
> "El Input System antiguo de Unity tiene 15 años. Fue diseñado cuando Unity era un motor de videojuegos de escritorio. El XR no existía. Hoy tienen que entender el nuevo sistema no porque Unity lo imponga, sino porque es la única forma de escribir código que funcione en cualquier dispositivo XR sin reescribir todo cada vez."

**Al mostrar el patrón de eventos vs polling:**
> "Imaginen que tienen que vigilar si alguien llama a la puerta. Hay dos formas: quedarse parado en la puerta mirando 60 veces por segundo si alguien llamó — eso es Update() con polling. O poner un timbre y hacer lo que quieran hasta que suene — eso son los eventos. En XR, con 10 objetos interactivos y 5 tipos de input, el polling los va a llevar al infierno."

**Al cerrar el tema:**
> "El código que escribieron hoy — `OnEnable`, `+=`, `-=`, `InputActionReference` — es exactamente el mismo patrón que usan los ingenieros de Meta, de Unity Technologies y de Microsoft en sus SDKs. No es académico: es producción."

---

## CASOS REALES RECOMENDADOS {#casos}

1. **Meta SDK Quest 3 (2024):** el SDK oficial de Meta para Unity usa exclusivamente el nuevo Input System con `InputActionReference` para todos los bindings de controller. Código fuente disponible en GitHub: `meta-xr-sdk-core`.

2. **Microsoft Mixed Reality Toolkit (MRTK3):** construido sobre el nuevo Input System de Unity. Cada interacción de HoloLens (air-tap, hand ray, eye gaze) es una `InputAction`. Repositorio: `microsoft/MixedRealityToolkit-Unity`.

3. **Google ARCore SDK para Unity:** usa `EnhancedTouchSupport` del nuevo Input System para gestión avanzada de toques multi-dedo. Docs: `developers.google.com/ar/develop/unity-arf`.

4. **TOYOTA Production System VR (2023):** Toyota usa simuladores VR (Unity + Quest Pro) para entrenar a trabajadores de manufactura. Los scripts de input fueron refactorizados de legacy a InputActionReference en 2023 para soportar múltiples modelos de gafas simultáneamente (fuente: Unity Technologies Case Studies 2023).

5. **Accenture XR Training Platform:** plataforma de capacitación empresarial en VR/AR que usa Unity. El sistema de input está diseñado con InputActionReference para soportar Quest, PICO y Cardboard desde el mismo codebase.

---

## EVALUACIÓN FORMATIVA

| Momento | Qué evaluar | Señal de comprensión | Señal de no comprensión |
|---------|------------|---------------------|------------------------|
| Revisión S10 | Diferencia Physics vs ARRaycast | Explica las dos capas | Confunde los dos |
| Transformación 3.1 | Legacy vs nuevo Input System | Da ejemplo de fallo del legacy | "Son lo mismo" |
| Transformación 3.2 | TouchPhase.Began | Explica por qué una vez | "Lo uso en cada frame" |
| Transformación 3.4 | InputActionReference | "Cambio el binding en Inspector" | "Cambio el código" |
| Práctica grupal | Diseño del InputActions Asset | Define tipos correctos (Button/Value) | Pone todo como Button |
| Ejercicio individual | Código compila | Console muestra 3 mensajes distintos | Solo muestra TAP |

---

## REFERENCIAS APA 7 {#referencias}

Unity Technologies. (2024). *Input System documentation*. https://docs.unity3d.com/Packages/com.unity.inputsystem@1.8/manual/index.html

Unity Technologies. (2024). *XR Interaction Toolkit documentation 3.0*. https://docs.unity3d.com/Packages/com.unity.xr.interaction.toolkit@3.0/manual/index.html

Microsoft. (2024). *Mixed Reality Toolkit 3 input overview*. https://learn.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/mrtk3-overview/input/input-overview

Google LLC. (2024). *ARCore Unity SDK — Touch input*. https://developers.google.com/ar/develop/unity-arf/touch

LaViola, J. J., Kruijff, E., McMahan, R. P., Bowman, D. A., & Poupyrev, I. (2017). *3D user interfaces: Theory and practice* (2nd ed.). Addison-Wesley Professional.

Bowman, D. A., Kruijff, E., LaViola, J. J., & Poupyrev, I. (2004). *3D user interfaces: Theory and practice*. Addison Wesley Longman.

Nielsen, J., & Budiu, R. (2013). *Mobile usability*. New Riders.

*PSISP08075 | Universidad Autónoma del Perú | Ingeniería de Sistemas | 2026-1*
