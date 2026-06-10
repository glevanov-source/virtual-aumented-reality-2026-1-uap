# Evaluación Continua 1 — EC1
## Realidad Virtual y Aumentada | PSISP08075 | Semana 4

**Estudiante:** ________________________________  **Código:** ____________
**Fecha:** _________________ | **Puntuación total:** ____/20 | **Tiempo:** 90 min

⚠️ Examen individual. No está permitido el uso de internet ni comunicación con compañeros.

---

## SECCIÓN A — Opción Múltiple (5 pts — 1 pt c/u)

**1.** ¿Qué describe el Continuum de Milgram-Kishino?

a) La velocidad de procesamiento de dispositivos XR
b) El espectro entre la realidad pura y la virtualidad pura
c) Los niveles de fidelidad gráfica en RV
d) La taxonomía de dispositivos de entrada XR

**2.** En Unity, el componente `Transform`:

a) Puede eliminarse si el objeto no necesita moverse
b) Solo existe en objetos 3D que tienen geometría
c) Siempre está presente en todo GameObject y no puede eliminarse
d) Es idéntico al componente RectTransform

**3.** Para que ARCore funcione en un dispositivo Android, el API level mínimo es:

a) 19 (Android 4.4)
b) 21 (Android 5.0)
c) 24 (Android 7.0)
d) 28 (Android 9.0)

**4.** `TrackableType.PlaneWithinPolygon` en un ARRaycast indica:

a) Detectar planos de forma poligonal
b) El hit debe estar dentro del área observada del plano
c) Crear un polígono en el plano detectado
d) Solo funciona con planos verticales

**5.** `Input.GetTouch(0).phase == TouchPhase.Began` se ejecuta:

a) Cada frame mientras el dedo toca la pantalla
b) Solo el primer frame en que el dedo hace contacto
c) Cuando el dedo se levanta
d) Cuando el dedo se mueve más de 5 píxeles

---

## SECCIÓN B — Completar (5 pts — 1 pt c/u)

1. AR Foundation es una API ____________ que abstrae ARCore (Android) y ARKit (iOS).

2. Para compilar apps AR de Android en Unity, el Scripting Backend debe ser ____________.

3. Un ____________ en AR Foundation es cualquier elemento del mundo real que ARCore puede rastrear en el tiempo (planos, imágenes, etc.).

4. La propiedad `hits[0].pose` de un ARRaycastHit contiene la ____________ y ____________ del punto de impacto.

5. ARCore puede detectar planos ____________ (como mesas) y planos ____________ (como paredes).

---

## SECCIÓN C — Desarrollo de código (6 pts)

### C.1 — Analizar y completar (3 pts)

Completa el script para que funcione correctamente:

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System.Collections.Generic;

[RequireComponent(typeof(____________))]   // (1) Componente requerido
public class ColocarObjetoEC : MonoBehaviour
{
    public GameObject prefabObjeto;
    private ARRaycastManager raycastManager;
    private List<ARRaycastHit> hits = new List<ARRaycastHit>();
    private GameObject instancia;

    void Awake()
    {
        raycastManager = GetComponent<____________>();  // (2)
    }

    void Update()
    {
        if (Input.touchCount == 0) ____________;        // (3) salir del método

        Touch toque = Input.GetTouch(0);
        if (toque.phase != ____________.Began) return;  // (4) fase del toque

        hits.____________;                               // (5) limpiar la lista
        if (raycastManager.Raycast(toque.position, hits, TrackableType.PlaneWithinPolygon))
        {
            Pose pose = ____________[0].pose;            // (6) variable de resultados
            if (instancia == null)
                instancia = Instantiate(prefabObjeto, pose.____________, pose.____________);  // (7)(8)
            else
                instancia.transform.SetPositionAndRotation(pose.position, pose.____________);  // (9)
        }
    }
}
```

**Llenar los 9 espacios en blanco:**

| # | Respuesta |
|---|-----------|
| 1 | |
| 2 | |
| 3 | |
| 4 | |
| 5 | |
| 6 | |
| 7 | |
| 8 | |
| 9 | |

### C.2 — Preguntas de código (3 pts)

**a)** ¿Por qué se declara `hits` como campo de clase y no dentro de `Update()`? (1 pt)

> _________________________________________________________________

**b)** ¿Qué pasa si no se agrega `[RequireComponent(typeof(ARRaycastManager))]`? ¿Cuándo se rompería el código? (1 pt)

> _________________________________________________________________

**c)** Escribe la línea de código para hacer el objeto visible/invisible (toggle) cuando se presiona el botón B del teclado: (1 pt)

```csharp
// En Update():

```

---

## SECCIÓN D — Caso práctico (4 pts)

Una empresa de retail limeña quiere una app AR para que sus clientes vean cómo quedarían los muebles en su sala antes de comprarlos.

**D.1** (1 pt) ¿Qué tipo de tracking AR usarías (marker-based o marker-less) y por qué?

> _________________________________________________________________

**D.2** (2 pt) Describe los componentes Unity necesarios para implementar esta app:

| Componente | Función en esta app |
|-----------|-------------------|
| | |
| | |
| | |

**D.3** (1 pt) Menciona 1 limitación técnica importante de esta app y cómo la comunicarías al cliente.

> _________________________________________________________________

---

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
