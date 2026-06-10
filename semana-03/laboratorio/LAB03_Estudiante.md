# Laboratorio 03 — App AR de Colocación de Objetos con Touch
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** ________________________________  **Código:** ____________
**Tiempo estimado:** 100 minutos

---

## Objetivo

Construir una app AR completa donde:
- ARCore detecta planos horizontales
- Al tocar la pantalla, se coloca un objeto 3D sobre el plano
- El objeto puede reposicionarse tocando en otro lugar
- UI indica cuando un plano ha sido detectado

---

## PASO 1 — Preparar el proyecto (10 min)

Continúa con tu proyecto Unity AR del Lab 02 (con AR Foundation + ARCore configurado).

Verificar que tienes:
- AR Session en escena
- XR Origin con Main Camera
- ARPlaneManager en XR Origin (Detection Mode: Horizontal)

---

## PASO 2 — Crear el prefab del objeto a colocar (15 min)

1. Crear un `Empty GameObject` en Hierarchy → renombrar a `PrefabMesa`
2. Agregar Child: 3D Cube (escala 0.3, 0.05, 0.3) → "Tablero"
3. Agregar 4 Child Capsules como patas (escala 0.02, 0.1, 0.02 c/u) → posicionar en las esquinas
4. Crear material `MatMadera` (color marrón)
5. Arrastrar a `Assets/Prefabs/` para convertir en Prefab

---

## PASO 3 — Script ColocarObjeto.cs (30 min)

Crear `Assets/Scripts/ColocarObjeto.cs`:

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System.Collections.Generic;
using TMPro;

[RequireComponent(typeof(ARRaycastManager))]
public class ColocarObjeto : MonoBehaviour
{
    [Header("Configuración")]
    public GameObject prefabObjeto;
    public TextMeshProUGUI textoEstado;

    private ARRaycastManager raycastManager;
    private ARPlaneManager planeManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();
    private GameObject objetoActual;
    private bool planoDetectado = false;

    void Awake()
    {
        raycastManager = GetComponent<ARRaycastManager>();
        planeManager   = GetComponent<ARPlaneManager>();
    }

    void OnEnable()  => planeManager.planesChanged += OnPlanesChanged;
    void OnDisable() => planeManager.planesChanged -= OnPlanesChanged;

    void OnPlanesChanged(ARPlanesChangedEventArgs args)
    {
        if (args.added.Count > 0 && !planoDetectado)
        {
            planoDetectado = true;
            if (textoEstado != null)
                textoEstado.text = "✓ Plano detectado. Toca para colocar objeto.";
        }
    }

    void Update()
    {
        if (!planoDetectado) return;
        if (Input.touchCount == 0) return;

        Touch toque = Input.GetTouch(0);
        if (toque.phase != TouchPhase.Began) return;

        hits.Clear();
        if (raycastManager.Raycast(toque.position, hits, TrackableType.PlaneWithinPolygon))
        {
            Pose pose = hits[0].pose;
            if (objetoActual == null)
            {
                objetoActual = Instantiate(prefabObjeto, pose.position, pose.rotation);
                if (textoEstado != null)
                    textoEstado.text = "Objeto colocado. Toca para mover.";
            }
            else
            {
                objetoActual.transform.SetPositionAndRotation(pose.position, pose.rotation);
            }
        }
    }
}
```

Asignar al XR Origin (que ya tiene ARRaycastManager y ARPlaneManager).

---

## PASO 4 — Crear la UI (20 min)

1. En Hierarchy → clic derecho → UI → Canvas
2. Canvas → Render Mode: Screen Space - Overlay
3. Agregar TextMeshPro: `UI → Text - TextMeshProUGUI`
   - Texto inicial: "Mueve la cámara para detectar una superficie"
   - Posición: arriba centrado
   - Font size: 24
4. Arrastrar el texto al campo "Texto Estado" del script ColocarObjeto
5. Agregar un panel de fondo semitransparente (Image, Alpha 0.5)

---

## PASO 5 — Funcionalidad extra: Escalar el objeto (15 min)

Agregar a ColocarObjeto.cs la función de escalar con pinch (dos dedos):

```csharp
// Agregar en la clase, como campo:
private float escalaInicial;
private float distanciaInicialDedos;

// Agregar en Update(), después del bloque de un toque:
if (objetoActual != null && Input.touchCount == 2)
{
    Touch t0 = Input.GetTouch(0);
    Touch t1 = Input.GetTouch(1);

    if (t0.phase == TouchPhase.Began || t1.phase == TouchPhase.Began)
    {
        distanciaInicialDedos = Vector2.Distance(t0.position, t1.position);
        escalaInicial = objetoActual.transform.localScale.x;
    }
    else
    {
        float distanciaActual = Vector2.Distance(t0.position, t1.position);
        float factor = distanciaActual / distanciaInicialDedos;
        float nuevaEscala = Mathf.Clamp(escalaInicial * factor, 0.05f, 2f);
        objetoActual.transform.localScale = Vector3.one * nuevaEscala;
    }
}
```

---

## PASO 6 — Evidencias

Guardar en `entregas/SEMANA-03/u-TU-CODIGO/`:

```
□ 1. Captura de la escena en Unity (Hierarchy + Inspector de XR Origin)
□ 2. Captura del script ColocarObjeto.cs en el editor
□ 3. Video/GIF mostrando: detección de plano → texto cambia → toque → objeto aparece
□ 4. Video/GIF mostrando: pinch para escalar el objeto
□ 5. Link al commit de GitHub con los cambios de esta semana
```

---

## Preguntas de reflexión

1. ¿Qué ocurre si tocas la pantalla antes de que se detecte un plano? ¿Cómo lo manejaste?

2. ¿Por qué `TrackableType.PlaneWithinPolygon` es mejor que `PlaneEstimated` para colocar objetos físicos?

3. Si quisieras rotar el objeto con un dedo, ¿qué `TouchPhase` usarías y cómo calcularías el ángulo de rotación?

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
