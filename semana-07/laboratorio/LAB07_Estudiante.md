# Laboratorio 07 — Escena VR Interactiva con XR Interaction Toolkit
## ⚠️ VERSIÓN ESTUDIANTE

**Tiempo estimado:** 90 minutos

---

## Objetivo

Crear una escena VR donde el usuario puede:
- Moverse por teleportación
- Agarrar y lanzar objetos
- Recibir feedback al interactuar

---

## PASO 1 — Instalar XR Interaction Toolkit (10 min)

1. `Window → Package Manager → Unity Registry`
2. Buscar "XR Interaction Toolkit" → Install versión 2.5.x
3. En la pestaña "Samples" del paquete, importar:
   - ✅ Starter Assets
   - ✅ XR Device Simulator

---

## PASO 2 — Configurar la escena VR (20 min)

1. Nueva escena → borrar Main Camera
2. Buscar en Project el prefab `XR Origin (XR Rig)` (de Starter Assets)
3. Arrastrar a la Hierarchy
4. Crear suelo: Plane → escala 5,1,5 → Add Component → Teleportation Area
5. Agregar iluminación: Directional Light
6. Arrastrar `XR Device Simulator` prefab a la Hierarchy

---

## PASO 3 — Objetos agarrables (25 min)

Crear 4 objetos en la escena:
1. Cubo rojo → Add: Rigidbody + Box Collider + XRGrabInteractable
2. Esfera azul → Add: Rigidbody + Sphere Collider + XRGrabInteractable (Throw = true)
3. Cápsula verde → Add: Rigidbody + Capsule Collider + XRGrabInteractable
4. Cilindro naranja → Add: Rigidbody + Cylinder Collider + XRGrabInteractable

Crear materiales de colores y aplicarlos.

---

## PASO 4 — Script de feedback al agarrar (25 min)

`Assets/Scripts/FeedbackAgarre.cs`:

```csharp
using UnityEngine;
using UnityEngine.XR.Interaction.Toolkit;

[RequireComponent(typeof(XRGrabInteractable))]
public class FeedbackAgarre : MonoBehaviour
{
    public Color colorAgarrado = Color.yellow;
    public AudioClip sonidoAgarre;
    public AudioClip sonidoSuelta;

    private Color colorOriginal;
    private Renderer rend;
    private AudioSource audioSource;
    private XRGrabInteractable grab;

    void Awake()
    {
        rend = GetComponent<Renderer>();
        colorOriginal = rend.material.color;
        audioSource = gameObject.AddComponent<AudioSource>();
        grab = GetComponent<XRGrabInteractable>();

        grab.selectEntered.AddListener((_) => AlAgarrar());
        grab.selectExited.AddListener((_) => AlSoltar());
    }

    void AlAgarrar()
    {
        rend.material.color = colorAgarrado;
        if (sonidoAgarre != null)
            audioSource.PlayOneShot(sonidoAgarre);
    }

    void AlSoltar()
    {
        rend.material.color = colorOriginal;
        if (sonidoSuelta != null)
            audioSource.PlayOneShot(sonidoSuelta);
    }
}
```

Asignar a los 4 objetos. Buscar clips de audio en `Assets/XR/Sounds` (de Starter Assets).

---

## Evidencias

```
□ 1. Captura de Hierarchy completa (XR Rig + objetos + Teleportation Area)
□ 2. Captura del Inspector de uno de los objetos (mostrando los 4 componentes)
□ 3. Captura del script FeedbackAgarre en Inspector (con campos llenos)
□ 4. Video usando el XR Device Simulator: teleportar + agarrar + lanzar objeto
□ 5. Link al commit de GitHub
```

## Reto extra (opcional +2 puntos)

Crear un "contenedor virtual" (caja sin tapa) donde al soltar el objeto dentro, se active una animación o se cambie su color. Usar OnTriggerEnter del Rigidbody.

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
