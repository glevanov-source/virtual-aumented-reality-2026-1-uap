# LAB10 — SOLUCIÓN COMPLETA DOCENTE
# App AR con Interacción Avanzada: Pinch · Rotate · UI Flotante · Eliminar
## PSISP08075 — Realidad Virtual y Aumentada | Semana 10

> ⚠️ **DOCUMENTO CONFIDENCIAL — Solo para uso del docente**
> Esta guía cubre todo desde cero: crear el proyecto Unity hasta el APK final.

---

## RESUMEN DE LO QUE SE CONSTRUYE

```
App AR "lab_10" — Funcionalidades:
├── 1. Tap en plano AR      → coloca objeto 3D (cápsula naranja)
├── 2. Tap en otro plano    → mueve el objeto a esa posición
├── 3. Swipe horizontal     → rota el objeto en eje Y
├── 4. Pinch (2 dedos)      → escala el objeto (0.05x - 5x)
├── 5. Tap sobre el objeto  → muestra/oculta panel UI flotante
└── 6. Botón "Eliminar"     → destruye el objeto y el panel
```

---

## TIEMPO ESTIMADO POR PASO

| Paso | Actividad | Tiempo |
|------|-----------|--------|
| 0 | Crear proyecto Unity | 5 min |
| 1 | Configurar AR | 8 min |
| 2 | Crear escena base | 5 min |
| 3 | Crear prefab objeto 3D | 7 min |
| 4 | Script GestosAR.cs | 10 min |
| 5 | Asignar script al XR Origin | 5 min |
| 6 | Crear Panel UI flotante | 15 min |
| 7 | Conectar todo en Inspector | 5 min |
| 8 | Compilar y probar | 10 min |
| 9 | Evidencias y commit | 5 min |
| **TOTAL** | | **~75 min** |

---

## PASO 0 — CREAR EL PROYECTO UNITY (5 min)

### 0.1 Crear desde Unity Hub

1. Abrir **Unity Hub**
2. Clic **"New project"** (botón azul arriba a la derecha)
3. Seleccionar template: **3D (URP)** — Universal Render Pipeline
4. Configurar:
   - **Project name:** `lab_10`
   - **Location:** `Documentos/Unity/` (o donde prefieras)
   - **Editor Version:** 2022.3.62f3 (la que ya tienes instalada)
5. Clic **"Create project"** — esperar que cargue (~2 min)

> ✅ **Resultado esperado:** Unity se abre con la escena `SampleScene` vacía.

---

## PASO 1 — CONFIGURAR EL PROYECTO PARA AR (8 min)

### 1.1 Instalar paquetes AR

`Window → Package Manager`:

1. Dropdown arriba izquierda → **"Unity Registry"**
2. Buscar e instalar en este orden:

| Paquete | Versión |
|---------|---------|
| **AR Foundation** | 5.1.x |
| **Google ARCore XR Plugin** | 5.1.x |

> Esperar que cada uno instale antes de instalar el siguiente.

### 1.2 Build Settings — Cambiar a Android

`File → Build Settings`:
1. Seleccionar **Android**
2. Clic **"Switch Platform"** — esperar (puede tomar 1-2 min)

### 1.3 Player Settings — Configurar para ARCore

`Edit → Project Settings → Player → Other Settings`:

| Campo | Valor |
|-------|-------|
| Scripting Backend | **IL2CPP** |
| Target Architectures | ✅ **ARM64** (desmarcar ARM si está) |
| Minimum API Level | **Android 7.0 (API level 24)** |
| Camera Usage Description | `App AR — usa la cámara para Realidad Aumentada` |

### 1.4 Activar ARCore

`Edit → Project Settings → XR Plug-in Management`:
1. Si aparece botón "Install XR Plugin Management" → clic para instalar
2. Tab **Android** (ícono de robot verde)
3. Marcar ✅ **ARCore**

> ✅ **Verificación:** en Player Settings → Other Settings, el campo "Scripting Backend" debe mostrar IL2CPP.

---

## PASO 2 — CREAR LA ESCENA AR BASE (5 min)

### 2.1 Limpiar la escena por defecto

En **Hierarchy**, eliminar todo lo que hay por defecto:
- Clic derecho → Delete en: `Main Camera`, `Directional Light`, `Global Volume` (si existe)

### 2.2 Agregar componentes AR

En **Hierarchy → clic derecho**:

1. **XR → AR Session**
2. **XR → XR Origin** (en versiones anteriores puede llamarse "AR Session Origin")

La Hierarchy debe quedar así:
```
SampleScene
├── AR Session
└── XR Origin
    └── Camera Offset
        └── Main Camera
```

### 2.3 Verificar la Main Camera

Seleccionar `Main Camera` en Hierarchy. En Inspector debe tener:
- ✅ `Camera` component
- ✅ `ARCameraManager`
- ✅ `ARCameraBackground`
- ✅ `TrackedPoseDriver`

Si alguno falta: **Add Component** y buscarlo.

### 2.4 Agregar ARPlaneManager al XR Origin

1. Seleccionar **XR Origin** en Hierarchy
2. Inspector → **Add Component** → buscar `AR Plane Manager`
3. Configurar:
   - **Detection Mode: Horizontal**
   - **Plane Prefab:** dejar vacío (usará visualización por defecto)

### 2.5 Agregar ARRaycastManager al XR Origin

Con **XR Origin** aún seleccionado:
- **Add Component** → `AR Raycast Manager`

> No necesita configuración adicional.

### 2.6 Guardar la escena

`File → Save As` → nombre: `EscenaLAB10` → guardar en `Assets/Scenes/`

---

## PASO 3 — CREAR EL PREFAB DEL OBJETO 3D (7 min)

### 3.1 Crear la cápsula (objeto que aparecerá en AR)

1. **Hierarchy → clic derecho → 3D Object → Capsule**
2. Renombrar a `ObjetoAR`
3. En Inspector → **Transform**:
   - Scale: X=`0.3`, Y=`0.3`, Z=`0.3`

### 3.2 Crear material naranja

1. **Project → Assets → clic derecho → Create → Folder** → nombre: `Materials`
2. Dentro de Materials: **clic derecho → Create → Material** → nombre: `MatNaranja`
3. Seleccionar `MatNaranja` → Inspector → **Albedo** → elegir color naranja `#F19100`
4. Arrastrar `MatNaranja` sobre el `ObjetoAR` en la Hierarchy

### 3.3 Verificar el Collider

Seleccionar `ObjetoAR` → Inspector debe tener `Capsule Collider` ✅
(La cápsula lo trae por defecto — es necesario para detectar el tap sobre el objeto)

### 3.4 Convertir en Prefab

1. En **Project → Assets**: crear carpeta `Prefabs`
2. **Arrastrar `ObjetoAR`** desde la **Hierarchy** hacia la carpeta `Assets/Prefabs/`
3. El ícono del objeto en Hierarchy se vuelve **azul** → es un prefab ✅
4. **Eliminar `ObjetoAR`** de la Hierarchy (Delete) — ya está guardado como prefab

> ✅ **Resultado:** `Assets/Prefabs/ObjetoAR.prefab` existe en el Project.

---

## PASO 4 — CREAR EL SCRIPT GestosAR.cs (10 min)

### 4.1 Crear el archivo

1. **Project → Assets → clic derecho → Create → Folder** → `Scripts`
2. Dentro de Scripts: **clic derecho → Create → C# Script** → `GestosAR`
3. **Doble clic** en `GestosAR` → se abre VS Code

### 4.2 Reemplazar todo el contenido con este código

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System.Collections.Generic;

/// <summary>
/// Gestiona todos los gestos táctiles AR:
/// - 1 tap en plano: colocar/mover objeto
/// - 1 tap en objeto: mostrar/ocultar panel UI
/// - 1 dedo swipe: rotar objeto en Y
/// - 2 dedos pinch: escalar objeto
/// - Botón UI: eliminar objeto
/// </summary>
[RequireComponent(typeof(ARRaycastManager))]
public class GestosAR : MonoBehaviour
{
    [Header("Prefabs")]
    public GameObject prefabObjeto;       // prefab de la cápsula naranja
    public GameObject panelInfoPrefab;    // prefab del panel UI flotante

    [Header("Sensibilidad de gestos")]
    public float velocidadRotacion = 0.3f;   // grados por pixel de swipe
    public float sensibilidadPinch = 1f;     // multiplicador del pinch

    // Referencias internas
    private ARRaycastManager raycastManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();
    private GameObject objeto;            // el objeto AR instanciado
    private GameObject panelInfo;         // el panel UI flotante

    // Variables para el gesto de pinch
    private float distanciaInicialPinch;
    private float escalaInicialPinch;

    // ─────────────────────────────────────────────
    // INICIALIZACIÓN
    // ─────────────────────────────────────────────

    void Awake()
    {
        raycastManager = GetComponent<ARRaycastManager>();
    }

    // ─────────────────────────────────────────────
    // LOOP PRINCIPAL
    // ─────────────────────────────────────────────

    void Update()
    {
        // Sin toques: no hacer nada
        if (Input.touchCount == 0) return;

        if (Input.touchCount == 2)
        {
            // DOS DEDOS → gesto de pinch (escalar)
            ManejarPinch();
        }
        else if (Input.touchCount == 1)
        {
            Touch toque = Input.GetTouch(0);

            if (toque.phase == TouchPhase.Began)
            {
                // PRIMER FRAME DEL TAP → colocar/mover o toggle panel
                ManejarToque(toque.position);
            }
            else if (toque.phase == TouchPhase.Moved && objeto != null)
            {
                // DEDO MOVIÉNDOSE → rotar objeto
                ManejarRotacion(toque);
            }
        }
    }

    // ─────────────────────────────────────────────
    // MÉTODOS DE GESTOS
    // ─────────────────────────────────────────────

    void ManejarToque(Vector2 posicion)
    {
        // PRIORIDAD 1: ¿tocó el objeto existente?
        if (objeto != null)
        {
            Ray ray = Camera.main.ScreenPointToRay(posicion);
            if (Physics.Raycast(ray, out RaycastHit hit))
            {
                bool tocaObjeto = hit.collider.gameObject == objeto
                               || hit.collider.transform.IsChildOf(objeto.transform);
                if (tocaObjeto)
                {
                    TogglePanel();
                    return; // no continuar al AR raycast
                }
            }
        }

        // PRIORIDAD 2: ARRaycast al mundo real para colocar/mover
        hits.Clear();
        if (raycastManager.Raycast(posicion, hits, TrackableType.PlaneWithinPolygon))
        {
            Pose pose = hits[0].pose;

            if (objeto == null)
            {
                // Primera vez: instanciar el objeto
                objeto = Instantiate(prefabObjeto, pose.position, pose.rotation);
            }
            else
            {
                // Ya existe: mover a la nueva posición
                objeto.transform.SetPositionAndRotation(pose.position, pose.rotation);
            }

            // Si el panel está visible, moverlo con el objeto
            if (panelInfo != null)
                panelInfo.transform.position = objeto.transform.position + Vector3.up * 0.2f;
        }
    }

    void ManejarRotacion(Touch toque)
    {
        // deltaPosition.x = cuánto se movió el dedo horizontalmente
        float rotacion = -toque.deltaPosition.x * velocidadRotacion;
        objeto.transform.Rotate(Vector3.up, rotacion, Space.World);
    }

    void ManejarPinch()
    {
        if (objeto == null) return;

        Touch t0 = Input.GetTouch(0);
        Touch t1 = Input.GetTouch(1);

        // Primer frame del pinch: guardar distancia y escala iniciales
        if (t0.phase == TouchPhase.Began || t1.phase == TouchPhase.Began)
        {
            distanciaInicialPinch = Vector2.Distance(t0.position, t1.position);
            escalaInicialPinch    = objeto.transform.localScale.x;
        }
        else
        {
            // Calcular ratio de cambio de distancia
            float distanciaActual = Vector2.Distance(t0.position, t1.position);
            float factor          = distanciaActual / distanciaInicialPinch;
            float nuevaEscala     = Mathf.Clamp(
                                        escalaInicialPinch * factor * sensibilidadPinch,
                                        0.05f,  // mínimo (muy pequeño)
                                        5f      // máximo (muy grande)
                                    );
            objeto.transform.localScale = Vector3.one * nuevaEscala;
        }
    }

    // ─────────────────────────────────────────────
    // PANEL UI
    // ─────────────────────────────────────────────

    void TogglePanel()
    {
        if (panelInfo == null && panelInfoPrefab != null)
        {
            // Primera vez: instanciar el panel encima del objeto
            panelInfo = Instantiate(panelInfoPrefab);
            panelInfo.transform.position = objeto.transform.position + Vector3.up * 0.2f;
        }
        else if (panelInfo != null)
        {
            // Ya existe: toggle visible/invisible
            panelInfo.SetActive(!panelInfo.activeSelf);
        }
    }

    /// <summary>
    /// Llamado por el botón "Eliminar" del panel UI.
    /// Destruye el objeto AR y el panel.
    /// </summary>
    public void EliminarObjeto()
    {
        if (objeto   != null) Destroy(objeto);
        if (panelInfo != null) Destroy(panelInfo);
        objeto    = null;
        panelInfo = null;
    }

    // ─────────────────────────────────────────────
    // LATE UPDATE — Panel siempre mirando a la cámara
    // ─────────────────────────────────────────────

    void LateUpdate()
    {
        if (panelInfo != null && panelInfo.activeSelf)
        {
            // Billboard: el panel rota para siempre mirar a la cámara
            panelInfo.transform.LookAt(Camera.main.transform);
            panelInfo.transform.Rotate(0, 180f, 0); // invertir (LookAt mira al revés)
        }
    }
}
```

### 4.3 Guardar y volver a Unity

- **Cmd+S** (Mac) o **Ctrl+S** (Windows) para guardar
- Volver a Unity — esperar la barra de compilación azul abajo
- Si la barra termina sin rojo en Console: ✅ compiló correctamente

---

## PASO 5 — ASIGNAR EL SCRIPT AL XR ORIGIN (5 min)

1. En **Hierarchy → seleccionar XR Origin**
2. Inspector → **Add Component** → escribir `GestosAR` → seleccionar

El Inspector del XR Origin debe verse así al terminar:

```
XR Origin (Script)          ✅
AR Plane Manager            ✅  Detection Mode: Horizontal
AR Raycast Manager          ✅
Gestos AR (Script)          ✅
  ├── Prefab Objeto:         [vacío — se conecta en Paso 7]
  ├── Panel Info Prefab:     [vacío — se conecta en Paso 7]
  ├── Velocidad Rotacion:    0.3
  └── Sensibilidad Pinch:    1
```

> ⚠️ Si aparece error "can't add GestosAR because it requires ARRaycastManager": 
> ya tiene ARRaycastManager → Unity lo detectó automáticamente gracias a `[RequireComponent]` ✅

---

## PASO 6 — CREAR EL PANEL UI FLOTANTE (15 min)

El panel UI es un Canvas en modo **World Space** que flota en el aire junto al objeto AR.

### 6.1 Crear el Canvas base

1. **Hierarchy → clic derecho → UI → Canvas**
2. Renombrar a `PanelInfoAR`
3. En Inspector del **Canvas**:
   - **Render Mode: World Space** ← cambiar de "Screen Space Overlay"
4. Seleccionar el **Rect Transform** del Canvas:
   - Width: `200`
   - Height: `120`
   - Scale: X=`0.001`, Y=`0.001`, Z=`0.001`

> El scale 0.001 hace que el canvas tenga 20cm × 12cm en el mundo 3D — tamaño correcto para AR.

### 6.2 Agregar fondo oscuro semitransparente

1. Seleccionar `PanelInfoAR` → **clic derecho → UI → Image**
2. Renombrar a `Fondo`
3. Inspector del Image:
   - **Color:** negro `#000000`, **Alpha: 180** (semitransparente)
   - En Rect Transform: **Anchor Presets → stretch/stretch** (Alt+clic en el preset de esquinas)

### 6.3 Agregar texto — Nombre del objeto

1. Seleccionar `PanelInfoAR` → **clic derecho → UI → Text - TextMeshPro**
   - Si aparece ventana "TMP Importer": clic **"Import TMP Essentials"**
2. Renombrar el texto a `TxtNombre`
3. Inspector:
   - **Text:** `Objeto AR`
   - **Font Size:** `18`
   - **Color:** blanco `#FFFFFF`
   - **Alignment:** Center / Middle
   - **Rect Transform:** Pos Y=`35`, Height=`30`, Width=`180`

### 6.4 Agregar texto — Descripción/instrucciones

1. Seleccionar `PanelInfoAR` → **clic derecho → UI → Text - TextMeshPro**
2. Renombrar a `TxtInstrucciones`
3. Inspector:
   - **Text:** `Swipe: rotar | Pinch: escalar`
   - **Font Size:** `10`
   - **Color:** gris claro `#CCCCCC`
   - **Alignment:** Center / Middle
   - **Rect Transform:** Pos Y=`5`, Height=`25`, Width=`180`

### 6.5 Agregar botón Eliminar

1. Seleccionar `PanelInfoAR` → **clic derecho → UI → Button - TextMeshPro**
2. Renombrar el Button a `BtnEliminar`
3. Expandir el Button → seleccionar el **Text (TMP)** hijo:
   - **Text:** `Eliminar`
   - **Font Size:** `12`
4. Seleccionar el `BtnEliminar` (el Button en sí):
   - Inspector → **Image** → Color: rojo `#CC2200`
   - **Rect Transform:** Pos Y=`-38`, Height=`25`, Width=`100`

### 6.6 Conectar el botón al método EliminarObjeto()

> ⚠️ Este paso es crítico — sin él el botón no hará nada.

1. Seleccionar `BtnEliminar` en Hierarchy
2. Inspector → sección **On Click ()** → clic el botón **"+"**
3. Aparece una fila con campo de objeto vacío:
   - Arrastrar **XR Origin** (desde Hierarchy) al **campo del objeto** (donde dice "None (Object)")
4. En el dropdown de la derecha (donde dice "No Function"):
   - Seleccionar **GestosAR → EliminarObjeto ()**

Debe verse así:
```
On Click ()
└── [XR Origin]  GestosAR.EliminarObjeto ()
```

### 6.7 Agregar Canvas Scaler y Event System (si no existen)

Unity los agrega automáticamente al crear el Canvas. Verificar en Hierarchy:
- Debe existir un `EventSystem` ✅ (si no: Hierarchy → clic derecho → UI → Event System)

### 6.8 Convertir en Prefab

1. En **Project**: entrar a carpeta `Assets/Prefabs/`
2. **Arrastrar `PanelInfoAR`** desde la **Hierarchy** hacia esa carpeta
3. El ícono se vuelve azul → prefab creado ✅
4. **Eliminar `PanelInfoAR`** de la Hierarchy (clic derecho → Delete)

Estructura final del Project:
```
Assets/
├── Materials/
│   └── MatNaranja.mat
├── Prefabs/
│   ├── ObjetoAR.prefab       ← la cápsula naranja
│   └── PanelInfoAR.prefab    ← el panel UI flotante
├── Scenes/
│   └── EscenaLAB10.unity
└── Scripts/
    └── GestosAR.cs
```

---

## PASO 7 — CONECTAR TODO EN EL INSPECTOR (5 min)

1. Seleccionar **XR Origin** en Hierarchy
2. En Inspector → script **Gestos AR**:

| Campo | Qué arrastrar | Desde |
|-------|--------------|-------|
| **Prefab Objeto** | `ObjetoAR` | `Assets/Prefabs/` |
| **Panel Info Prefab** | `PanelInfoAR` | `Assets/Prefabs/` |

3. Dejar los demás valores:
   - Velocidad Rotacion: `0.3`
   - Sensibilidad Pinch: `1`

### Verificación final del Inspector del XR Origin:

```
✅ XR Origin (Script)
✅ AR Plane Manager       — Detection Mode: Horizontal
✅ AR Raycast Manager
✅ Gestos AR (Script)
     Prefab Objeto:       [ObjetoAR]     ✅
     Panel Info Prefab:   [PanelInfoAR]  ✅
     Velocidad Rotacion:  0.3
     Sensibilidad Pinch:  1
```

### Verificación de la Hierarchy:

```
SampleScene (o EscenaLAB10)
├── AR Session
├── XR Origin              ← tiene: ARPlaneManager, ARRaycastManager, GestosAR
│   └── Camera Offset
│       └── Main Camera    ← tiene: ARCameraManager, ARCameraBackground
└── EventSystem
```

---

## PASO 8 — COMPILAR Y PROBAR (10 min)

### Opción A: Probar en celular Android (recomendado)

**Requisitos previos en el celular:**
- Modo Desarrollador activado
- Depuración USB activada
- Celular conectado por USB

**En Unity:**
1. `File → Build Settings`
2. Verificar que la escena `EscenaLAB10` está en la lista (si no: clic "Add Open Scenes")
3. Clic **"Build and Run"**
4. Seleccionar carpeta destino para el APK (ej: Desktop)
5. Esperar compilación (~5 min primera vez, ~2 min las siguientes)

### Opción B: Probar con XR Device Simulator (sin celular)

1. `Window → Package Manager → XR Interaction Toolkit`
2. Pestaña **Samples** → **XR Device Simulator** → **Import**
3. En `Assets/Samples/XR Interaction Toolkit/*/XR Device Simulator/`:
   - Arrastrar prefab `XR Device Simulator` a la Hierarchy
4. Clic **Play ▶️**

**Controles del simulador:**
| Tecla/Acción | Función |
|-------------|---------|
| `WASD` | Mover la cámara |
| Click izquierdo (mantener) | Simular toque |
| `G` | Grip (agarrar) |
| `T` | Toggle controller activo |

---

## PASO 9 — PROBAR LOS 5 GESTOS (check de calidad)

Verificar cada gesto en orden:

### ✅ Gesto 1 — Tap en plano: colocar objeto

- Mover el celular despacio sobre una mesa/piso
- Esperar que aparezca la visualización del plano (malla verde/blanca)
- Tocar la pantalla sobre el plano
- **Resultado esperado:** aparece la cápsula naranja sobre la superficie real

### ✅ Gesto 2 — Tap en otro plano: mover objeto

- Tocar en otro punto del mismo plano (o de otro plano)
- **Resultado esperado:** el objeto se mueve a la nueva posición

### ✅ Gesto 3 — Swipe horizontal: rotar

- Con 1 dedo, deslizar horizontalmente sobre la pantalla
- **Resultado esperado:** el objeto rota en su eje Y
- Deslizar a la derecha = rota en un sentido, izquierda = otro sentido

### ✅ Gesto 4 — Pinch: escalar

- Poner 2 dedos en la pantalla
- Separar los dedos = agrandar objeto
- Juntar los dedos = achicar objeto
- **Resultado esperado:** el objeto crece/encoge suavemente

### ✅ Gesto 5 — Tap sobre el objeto: panel UI

- Tocar directamente sobre la cápsula naranja
- **Resultado esperado:** aparece panel flotante con "Objeto AR", instrucciones y botón rojo "Eliminar"
- El panel rota para mirar siempre a la cámara (efecto billboard)
- Tocar nuevamente el objeto: el panel desaparece

### ✅ Gesto 6 — Botón Eliminar

- Cuando el panel está visible: tocar el botón rojo "Eliminar"
- **Resultado esperado:** objeto Y panel desaparecen completamente
- Siguiente tap en plano: puede colocar uno nuevo

---

## PASO 10 — EVIDENCIAS Y COMMIT (5 min)

### Capturas requeridas:

```
□ 1. Captura del Inspector del XR Origin
      (mostrando GestosAR con prefabs asignados)

□ 2. Captura del PanelInfoAR abierto en el editor
      (mostrando el Canvas, textos y botón Eliminar)

□ 3. Captura del On Click() del botón
      (mostrando XR Origin → GestosAR.EliminarObjeto)

□ 4. Video 1: tap en plano → objeto aparece → swipe (rota) → pinch (escala)

□ 5. Video 2: tap sobre objeto → panel aparece → botón Eliminar → objeto desaparece
```

### Commit a GitHub:

```bash
# Navegar a la carpeta del proyecto Unity
cd ~/Documentos/Unity/lab_10

# Inicializar git (si no lo hiciste antes)
git init
curl -o .gitignore https://raw.githubusercontent.com/github/gitignore/main/Unity.gitignore

# Agregar y commitear
git add Assets/ ProjectSettings/ Packages/
git commit -m "feat(s10): app AR gestos avanzados pinch rotate panel UI"

# Subir a tu rama
git push origin u-TU-CODIGO
```

---

## SOLUCIÓN A ERRORES FRECUENTES

| Error | Causa probable | Solución |
|-------|---------------|---------|
| **Script no aparece en Add Component** | Error de compilación | Console (Ctrl+Shift+C) → leer el error rojo → corregir |
| **"can't add because requires ARRaycastManager"** | Ya está agregado | Buscar ARRaycastManager en el Inspector — está ahí |
| **Panel aparece gigante en pantalla** | Canvas en Screen Space | Canvas → Render Mode → **World Space** |
| **Panel se ve pero no rota con la cámara** | LateUpdate no está | Verificar que el script tiene el método `LateUpdate()` |
| **Tap no detecta el objeto** | Falta Collider en el prefab | Abrir ObjetoAR prefab → Add Component → Capsule Collider |
| **`Camera.main` is null** | Tag de cámara faltante | Main Camera → Inspector → Tag: **MainCamera** |
| **Botón Eliminar no hace nada** | On Click() mal configurado | Repetir Paso 6.6 — verificar que selecciona `GestosAR.EliminarObjeto()` |
| **ARRaycast nunca golpea** | ARPlaneManager no está | XR Origin → Add Component → AR Plane Manager |
| **Objeto aparece pero no se ve** | Material sin asignar | Arrastrar MatNaranja al prefab ObjetoAR |
| **Compilación falla: ARM64** | Target Architecture mal | Player Settings → Other Settings → ARM64 ✅ |
| **No detecta planos** | API level bajo | Player Settings → Min API Level → Android 7.0 (24) |

---

## CÓDIGO COMPLETO FINAL — GestosAR.cs

Para referencia rápida durante clase:

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System.Collections.Generic;

[RequireComponent(typeof(ARRaycastManager))]
public class GestosAR : MonoBehaviour
{
    [Header("Prefabs")]
    public GameObject prefabObjeto;
    public GameObject panelInfoPrefab;

    [Header("Sensibilidad de gestos")]
    public float velocidadRotacion = 0.3f;
    public float sensibilidadPinch = 1f;

    private ARRaycastManager raycastManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();
    private GameObject objeto;
    private GameObject panelInfo;
    private float distanciaInicialPinch;
    private float escalaInicialPinch;

    void Awake() => raycastManager = GetComponent<ARRaycastManager>();

    void Update()
    {
        if (Input.touchCount == 0) return;

        if (Input.touchCount == 2)
            ManejarPinch();
        else if (Input.touchCount == 1)
        {
            Touch toque = Input.GetTouch(0);
            if (toque.phase == TouchPhase.Began)
                ManejarToque(toque.position);
            else if (toque.phase == TouchPhase.Moved && objeto != null)
                ManejarRotacion(toque);
        }
    }

    void ManejarToque(Vector2 posicion)
    {
        if (objeto != null)
        {
            Ray ray = Camera.main.ScreenPointToRay(posicion);
            if (Physics.Raycast(ray, out RaycastHit hit))
            {
                if (hit.collider.gameObject == objeto ||
                    hit.collider.transform.IsChildOf(objeto.transform))
                {
                    TogglePanel();
                    return;
                }
            }
        }

        hits.Clear();
        if (raycastManager.Raycast(posicion, hits, TrackableType.PlaneWithinPolygon))
        {
            Pose pose = hits[0].pose;
            if (objeto == null)
                objeto = Instantiate(prefabObjeto, pose.position, pose.rotation);
            else
                objeto.transform.SetPositionAndRotation(pose.position, pose.rotation);

            if (panelInfo != null)
                panelInfo.transform.position = objeto.transform.position + Vector3.up * 0.2f;
        }
    }

    void ManejarRotacion(Touch toque)
    {
        float rotacion = -toque.deltaPosition.x * velocidadRotacion;
        objeto.transform.Rotate(Vector3.up, rotacion, Space.World);
    }

    void ManejarPinch()
    {
        if (objeto == null) return;
        Touch t0 = Input.GetTouch(0), t1 = Input.GetTouch(1);

        if (t0.phase == TouchPhase.Began || t1.phase == TouchPhase.Began)
        {
            distanciaInicialPinch = Vector2.Distance(t0.position, t1.position);
            escalaInicialPinch    = objeto.transform.localScale.x;
        }
        else
        {
            float factor      = Vector2.Distance(t0.position, t1.position) / distanciaInicialPinch;
            float nuevaEscala = Mathf.Clamp(escalaInicialPinch * factor * sensibilidadPinch, 0.05f, 5f);
            objeto.transform.localScale = Vector3.one * nuevaEscala;
        }
    }

    void TogglePanel()
    {
        if (panelInfo == null && panelInfoPrefab != null)
        {
            panelInfo = Instantiate(panelInfoPrefab);
            panelInfo.transform.position = objeto.transform.position + Vector3.up * 0.2f;
        }
        else if (panelInfo != null)
            panelInfo.SetActive(!panelInfo.activeSelf);
    }

    public void EliminarObjeto()
    {
        if (objeto    != null) Destroy(objeto);
        if (panelInfo != null) Destroy(panelInfo);
        objeto    = null;
        panelInfo = null;
    }

    void LateUpdate()
    {
        if (panelInfo != null && panelInfo.activeSelf)
        {
            panelInfo.transform.LookAt(Camera.main.transform);
            panelInfo.transform.Rotate(0, 180f, 0);
        }
    }
}
```

---

## RÚBRICA DE EVALUACIÓN — LAB 10

| Criterio | Puntos | Cómo verificar |
|---------|--------|----------------|
| Proyecto Unity creado con nombre `lab_10` | 1 | Verificar en Unity Hub |
| AR Foundation + ARCore configurado correctamente | 2 | Player Settings + XR Plugin Management |
| Escena con AR Session + XR Origin + componentes | 2 | Hierarchy correcta |
| Prefab ObjetoAR (con material y Collider) | 2 | Assets/Prefabs/ |
| Script GestosAR.cs sin errores de compilación | 3 | Console sin rojo |
| Gesto 1: tap → colocar objeto | 2 | Video evidencia |
| Gesto 2: swipe → rotar objeto | 2 | Video evidencia |
| Gesto 3: pinch → escalar objeto | 2 | Video evidencia |
| Panel UI World Space con textos y botón | 2 | Captura prefab |
| Tap objeto → toggle panel (billboard) | 1 | Video evidencia |
| Botón Eliminar funcional | 1 | Video evidencia |
| Commit a GitHub con mensaje correcto | 2 | Link al commit |
| **TOTAL** | **20** | |

---

*PSISP08075 — Realidad Virtual y Aumentada | Universidad Autónoma del Perú | 2026-1*
*Semana 10 — Interacción Avanzada en AR*
*⚠️ CONFIDENCIAL — Solo para uso del docente*
