# LAB11 — LABORATORIO EN CLASE
## Curso: Realidad Virtual y Aumentada | PSISP08075
### Semana 11 — Scripts e Inputs: Sistema de Interacción AR Completo

---

> **Laboratorio en clase — autónomo**
> **Tiempo:** 35-40 minutos
> **Modalidad:** individual
> **Entrega:** commit de GitHub antes de terminar la clase

---

## OBJETIVO

Implementar un sistema completo de inputs AR que detecte **3 gestos distintos** (tap, swipe, pinch) y responda con acciones visuales sobre un objeto 3D en AR.

Al terminar este lab, tu escena AR debe:
- Detectar tap → colocar/mover objeto en plano AR
- Detectar swipe horizontal → rotar el objeto
- Detectar pinch → escalar el objeto (con límites)
- Mostrar en pantalla el tipo de gesto detectado (texto UI)

---

## PRERREQUISITOS

Verifica antes de empezar:

```
☐ Unity 2022.3.x abierto
☐ Proyecto con AR Foundation 5.x instalado (Package Manager)
☐ Google ARCore XR Plugin instalado
☐ Build Target: Android (o configurado para XR Device Simulator)
☐ XR Device Simulator importado (Window → Package Manager → XR Interaction Toolkit → Samples)
```

---

## PASO 1 — Crear el proyecto (3 min)

1. Unity Hub → **New Project**
2. Template: **3D (URP)**
3. Nombre: `lab_11_inputs`
4. Crear

---

## PASO 2 — Configurar AR Foundation (5 min)

1. **Window → Package Manager → Unity Registry:**
   - Instalar: `AR Foundation 5.x`
   - Instalar: `Google ARCore XR Plugin 5.x`
   - Instalar: `XR Interaction Toolkit 2.5.x` (incluye Device Simulator)

2. **Edit → Project Settings → XR Plug-in Management:**
   - Android: activar ☑ ARCore
   - Editor (PC): activar ☑ XR Simulation (o dejar vacío para usar Device Simulator)

3. **Edit → Project Settings → Player → Other Settings:**
   - Active Input Handling: **Both** (para compatibilidad en este lab)

4. **Hierarchy → eliminar** Main Camera y Directional Light

5. **Hierarchy → XR → AR Session**

6. **Hierarchy → XR → XR Origin (Mobile AR)**

7. Verificar: en Hierarchy tienes `AR Session` y `XR Origin`

---

## PASO 3 — Crear la escena base (3 min)

1. Seleccionar `XR Origin` en Hierarchy
2. **Add Component → AR Plane Manager**
   - Plane Prefab: `AR Default Plane` (de Assets/Samples o de AR Foundation)
   - Detection Mode: **Horizontal**
3. **Add Component → AR Raycast Manager**

4. **Hierarchy → UI → Canvas:**
   - Render Mode: **Screen Space — Overlay**
   - Agregar hijo: **UI → Text - TextMeshPro**
   - Nombre del Text: `TextGesto`
   - Posición: arriba centrado (Anchor: top-center)
   - Font Size: 36, Color: blanco

5. **Hierarchy → 3D Object → Cube:**
   - Nombre: `CuboPrefab`
   - Escala: 0.1 x 0.1 x 0.1
   - Arrastrarlo a `Assets/Prefabs/` para crear el prefab
   - Eliminar de Hierarchy (solo dejar el prefab)

---

## PASO 4 — Escribir el script InputAR.cs (15 min)

**Assets → Create → C# Script → `InputAR`**

Escribe el siguiente script completo:

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System.Collections.Generic;
using TMPro;

[RequireComponent(typeof(ARRaycastManager))]
public class InputAR : MonoBehaviour
{
    [Header("Configuración")]
    public GameObject prefabObjeto;       // arrastrar CuboPrefab aquí
    public TextMeshProUGUI textoGesto;    // arrastrar TextGesto aquí
    public float velocidadRotacion = 0.4f;
    public float sensibilidadPinch = 1.0f;

    private ARRaycastManager raycastManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();
    private GameObject objetoAR;

    // Estado del pinch
    private float distanciaInicialPinch;
    private float escalaInicialPinch;

    void Awake() => raycastManager = GetComponent<ARRaycastManager>();

    void Update()
    {
        if (Input.touchCount == 0) return;

        if (Input.touchCount == 2)
        {
            // ── GESTO: PINCH ──────────────────────────────────────────
            MostrarGesto("PINCH — escalar");
            ManejarPinch();
        }
        else if (Input.touchCount == 1)
        {
            Touch t = Input.GetTouch(0);

            if (t.phase == TouchPhase.Began)
            {
                // ── GESTO: TAP ────────────────────────────────────────
                MostrarGesto($"TAP en {t.position}");
                ManejarTap(t.position);
            }
            else if (t.phase == TouchPhase.Moved && objetoAR != null)
            {
                // ── GESTO: SWIPE HORIZONTAL → ROTAR ──────────────────
                if (Mathf.Abs(t.deltaPosition.x) > 5f)
                {
                    string dir = t.deltaPosition.x > 0 ? "SWIPE →" : "SWIPE ←";
                    MostrarGesto(dir);
                    ManejarRotacion(t);
                }
            }
        }
    }

    void ManejarTap(Vector2 posicion)
    {
        hits.Clear();
        if (raycastManager.Raycast(posicion, hits, TrackableType.PlaneWithinPolygon))
        {
            Pose pose = hits[0].pose;
            if (objetoAR == null)
                objetoAR = Instantiate(prefabObjeto, pose.position, pose.rotation);
            else
                objetoAR.transform.SetPositionAndRotation(pose.position, pose.rotation);
        }
    }

    void ManejarRotacion(Touch t)
    {
        float angulo = -t.deltaPosition.x * velocidadRotacion;
        objetoAR.transform.Rotate(Vector3.up, angulo, Space.World);
    }

    void ManejarPinch()
    {
        if (objetoAR == null) return;

        Touch t0 = Input.GetTouch(0);
        Touch t1 = Input.GetTouch(1);

        // Al comenzar el pinch: guardar estado inicial
        if (t0.phase == TouchPhase.Began || t1.phase == TouchPhase.Began)
        {
            distanciaInicialPinch = Vector2.Distance(t0.position, t1.position);
            escalaInicialPinch    = objetoAR.transform.localScale.x;
            return;
        }

        // Durante el pinch: calcular nueva escala
        float distanciaActual = Vector2.Distance(t0.position, t1.position);
        float factor = distanciaActual / distanciaInicialPinch;
        float nuevaEscala = Mathf.Clamp(
            escalaInicialPinch * factor * sensibilidadPinch,
            0.05f, 3f);

        objetoAR.transform.localScale = Vector3.one * nuevaEscala;
    }

    void MostrarGesto(string mensaje)
    {
        if (textoGesto != null)
            textoGesto.text = mensaje;
    }
}
```

---

## PASO 5 — Conectar en el Inspector (4 min)

1. Seleccionar `XR Origin` en Hierarchy
2. **Add Component → Input AR** (buscar el script)
3. Arrastrar referencias:
   - **Prefab Objeto** → `CuboPrefab` (desde Assets/Prefabs)
   - **Texto Gesto** → `TextGesto` (desde Hierarchy/Canvas)
4. Dejar velocidad en 0.4 y sensibilidad en 1.0

---

## PASO 6 — Agregar XR Device Simulator y probar (5 min)

1. **Window → Package Manager → XR Interaction Toolkit → Samples → XR Device Simulator → Import**
2. **Assets/Samples/.../XR Device Simulator → arrastrar el prefab `XR Device Simulator` a Hierarchy**
3. **▶️ Play**

**Controles del simulador para probar toques táctiles:**
- El Simulator emula VR, no touch directo. Para simular toques AR en el editor:
  - Usamos el **fallback de mouse:** en el Inspector del script, el `Input.touchCount` detecta clicks de ratón si está en modo "Both" en Player Settings.
  - Click izquierdo del ratón = tap (1 toque simulado)

4. Click en el suelo (plano simulado) → debe aparecer el cubo
5. Observar el texto en pantalla: "TAP en (x, y)"

---

## PASO 7 — Verificación y commit (5 min)

Verifica en la consola que aparecen mensajes como:

```
TAP en (540.0, 960.0)
SWIPE → 
PINCH — escalar
```

Luego hacer commit:

```bash
cd ~/Desktop/rva-repos/rva-2026-1-uap   # ajustar a tu ruta
git add .
git commit -m "feat(s11): implementar InputAR con tap swipe pinch"
git push
```

---

## CHECKLIST DE ENTREGA

```
☐ 1. Script InputAR.cs creado en Assets/Scripts/
☐ 2. El Cubo aparece al hacer tap (click) en la escena
☐ 3. El texto UI muestra el tipo de gesto detectado
☐ 4. Sin errores en Console (ventana roja)
☐ 5. Commit subido a GitHub con mensaje feat(s11): ...
```

---

## ERRORES COMUNES Y SOLUCIONES RÁPIDAS

| Error | Causa probable | Solución rápida |
|-------|---------------|----------------|
| `NullReferenceException` en `textoGesto.text` | No se arrastró el TextGesto al Inspector | Seleccionar XR Origin → arrastrar TextGesto al campo |
| El cubo no aparece | `prefabObjeto` es null en Inspector | Arrastrar CuboPrefab al campo "Prefab Objeto" |
| No detecta toques | `Active Input Handling` en "New" (no "Both") | Project Settings → Player → Active Input Handling → Both |
| `ARRaycastManager` no encontrado | El componente no está en XR Origin | Add Component → AR Raycast Manager en XR Origin |
| `TextMeshPro not found` | TMP no importado | Window → TextMeshPro → Import TMP Essential Resources |

---

*PSISP08075 | Universidad Autónoma del Perú | Ingeniería de Sistemas | Semana 11 | 2026-1*
