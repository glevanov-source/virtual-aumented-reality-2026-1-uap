# Laboratorio 13 — Audio Espacial en Escena XR
## ⚠️ VERSIÓN ESTUDIANTE

**Tiempo estimado:** 90 minutos

---

## Objetivo

Agregar audio espacial 3D, Audio Mixer y vibración háptica a la escena XR del proyecto.

---

## PASO 1 — Crear Audio Mixer (15 min)

1. `Project → Create → Audio Mixer` → nombre: `MixerXR`
2. En Audio Mixer window: crear grupos:
   - **Master** (ya existe)
   - **Ambiente** (hijo de Master)
   - **Efectos** (hijo de Master)
3. En Ambiente: agregar efecto Reverb → Reverb Preset: Medium Room
4. En Efectos: agregar efecto Compressor

---

## PASO 2 — Audio fuentes espaciales (25 min)

En la escena, seleccionar 3 objetos. Para cada uno:
1. Add Component → AudioSource
2. Asignar clip de audio (buscar en Assets/XR Sounds o importar)
3. Configurar:
   - Spatial Blend: **1** (3D)
   - Loop: ✅
   - Min Distance: **0.5**
   - Max Distance: **10**
   - Rolloff: Logarithmic
   - Output: asignar al grupo "Efectos" del mixer

---

## PASO 3 — Script AmbienteAudio.cs (15 min)

```csharp
using UnityEngine;

public class AmbienteAudio : MonoBehaviour
{
    public AudioSource[] fuentesAmbiente;

    void Start()
    {
        foreach (var fuente in fuentesAmbiente)
        {
            fuente.spatialBlend = 1f;
            fuente.Play();
        }
    }
}
```

---

## PASO 4 — Audio al agarrar objetos (en XRGrabInteractable) (20 min)

En el script FeedbackAgarre.cs del lab 07, agregar:

```csharp
// Sonido de agarre ya configurado en el Inspector
// Agregar también vibración:

void AlAgarrar()
{
    // ... código existente
    if (sonidoAgarre != null)
        audioSource.PlayOneShot(sonidoAgarre);

    // Vibración en el controller derecho
    List<InputDevice> controllers = new List<InputDevice>();
    InputDevices.GetDevicesWithCharacteristics(
        InputDeviceCharacteristics.Controller | InputDeviceCharacteristics.Right,
        controllers
    );
    if (controllers.Count > 0)
        controllers[0].SendHapticImpulse(0, 0.5f, 0.1f);
}
```

---

## PASO 5 — Audio Reverb Zone (5 min)

1. Hierarchy → Audio → Audio Reverb Zone
2. Posicionar en el centro de la escena
3. Min Distance: 2, Max Distance: 15
4. Reverb Preset: Medium Room (o uno apropiado para el proyecto)

---

## Evidencias

```
□ 1. Captura del Audio Mixer con grupos y efectos configurados
□ 2. Captura del Inspector de un AudioSource con Spatial Blend = 1
□ 3. Captura del AudioReverbZone configurado
□ 4. Video: moverse en la escena y escuchar cómo el sonido varía con la distancia
□ 5. Video: agarrar objeto → sonido + (si es VR) vibración
□ 6. Link al commit de GitHub
```

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
