# Laboratorio 11 — Tour VR con Google Cardboard
## ⚠️ VERSIÓN ESTUDIANTE

**Tiempo estimado:** 100 minutos

---

## Objetivo

Crear un tour VR de 3 habitaciones con navegación por gaze.

---

## PASO 1 — Configurar proyecto para Cardboard (15 min)

1. `Window → Package Manager` → instalar **Google Cardboard XR Plugin**
2. `Edit → Project Settings → XR Plug-in Management → Android` → ✅ Cardboard
3. Player Settings → Other Settings:
   - Graphics API: **OpenGLES3** (eliminar Vulkan)
   - Scripting Backend: IL2CPP, ARM64 ✅
   - Min API: 26

---

## PASO 2 — Crear las 3 salas (20 min)

1. Sala 1: primitivas simples (cubo aplanado = suelo, cubos = paredes) — color azul
2. Sala 2: color verde
3. Sala 3: color rojo
4. Entre salas: plano vertical con transparencia = "portal"

---

## PASO 3 — Script GazeCursor (25 min)

Implementar el GazeCursor de la sesión.
Agregar a cada portal un script `Portal.cs`:

```csharp
public class Portal : MonoBehaviour
{
    public Transform destino;

    public void AlSerMirado()
    {
        Camera.main.transform.parent.position = destino.position;
    }
}
```

El GazeCursor llama `SendMessage("AlSerMirado")` al objeto mirado.

---

## PASO 4 — Objetos con info (15 min)

En cada sala, agregar 1 objeto con un script `ObjetoInfo.cs`:

```csharp
public class ObjetoInfo : MonoBehaviour
{
    public GameObject panelInfo;

    public void AlSerMirado()
    {
        panelInfo.SetActive(!panelInfo.activeSelf);
    }
}
```

Panel = Canvas World Space con TextMeshPro.

---

## PASO 5 — Skybox (5 min)

`Window → Rendering → Lighting → Environment → Skybox Material`
Usar Material con Shader "Skybox/6 Sided" o "Skybox/Panoramic".

---

## Evidencias

```
□ 1. Captura de Project Settings con Cardboard activado
□ 2. Captura de las 3 salas en Scene View
□ 3. Captura del GazeCursor script (con campos asignados)
□ 4. Video del tour VR: navegar entre salas mirando portales
□ 5. Video mostrando un objeto con info al ser mirado
□ 6. Link al commit de GitHub
```

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
