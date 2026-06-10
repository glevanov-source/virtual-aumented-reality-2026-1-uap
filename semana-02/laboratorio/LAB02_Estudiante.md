# Laboratorio 02 — Configuración Proyecto AR y Primera Detección de Planos
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** ________________________________  **Código:** ____________
**Tiempo estimado:** 90 minutos

---

## Objetivo

Al finalizar:
- ✅ Proyecto Unity configurado para AR (AR Foundation + ARCore)
- ✅ Escena con AR Session + XR Origin
- ✅ ARPlaneManager detectando planos horizontales
- ✅ Prefab de plano personalizado
- ✅ APK compilado e instalado en celular (si tienes Android)

---

## PASO 1 — Configurar proyecto AR (20 min)

Abre tu proyecto Unity 2022.3 del lab anterior.

### 1.1 Instalar paquetes

`Window → Package Manager → Unity Registry`:
1. **AR Foundation** 5.1.x → Install
2. **Google ARCore XR Plugin** 5.1.x → Install
3. Cerrar Package Manager

### 1.2 Configurar Build Settings

`File → Build Settings`:
- Platform: **Android** → Switch Platform

`Edit → Project Settings → Player → Other Settings`:
- Scripting Backend: **IL2CPP**
- Target Architectures: ✅ **ARM64**
- Minimum API Level: **Android 7.0 (API 24)**
- Camera Usage Description: "Esta app usa la cámara para Realidad Aumentada"

`Edit → Project Settings → XR Plug-in Management → Android`:
- ✅ **ARCore**

---

## PASO 2 — Crear la escena AR (15 min)

1. `File → New Scene → Basic (URP)` o vacía
2. Eliminar la Main Camera existente
3. En Hierarchy → clic derecho:
   - **XR → AR Session** (gestiona ARCore)
   - **XR → XR Origin** (o "AR Session Origin" en versiones anteriores)
4. Guardar como: `Assets/Scenes/EscenaAR.unity`

Verificar en Hierarchy:
```
├── AR Session
└── XR Origin
    └── Camera Offset
        └── Main Camera
            (con ARCameraManager y ARCameraBackground)
```

---

## PASO 3 — Agregar detección de planos (25 min)

### 3.1 ARPlaneManager

1. Seleccionar **XR Origin** en Hierarchy
2. En Inspector → Add Component → buscar **AR Plane Manager**
3. Configurar:
   - Detection Mode: **Horizontal**
   - Plane Prefab: (dejar vacío por ahora, usará visualización por defecto)

### 3.2 Crear Prefab de plano personalizado

En Project:
1. `Assets` → clic derecho → Create → Material → nombre: `MatPlanoAR`
2. Shader: Standard, Albedo: color azul semitransparente, Alpha: 0.5
3. En Hierarchy → clic derecho → XR → **AR Default Plane** (si existe) O:
4. Crear Plane (3D Object), asignar MatPlanoAR
5. Arrastrar a Project (crear Prefab): `Assets/Prefabs/PrefabPlanoAR.prefab`
6. Asignar al Plane Prefab del ARPlaneManager

### 3.3 Script PlanoDetectado.cs

Crear script en `Assets/Scripts/PlanoDetectado.cs`:

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;

// Muestra en consola cuando se detecta un plano nuevo
[RequireComponent(typeof(ARPlaneManager))]
public class PlanoDetectado : MonoBehaviour
{
    private ARPlaneManager planeManager;

    void Awake()
    {
        planeManager = GetComponent<ARPlaneManager>();
    }

    void OnEnable()
    {
        planeManager.planesChanged += OnPlanesChanged;
    }

    void OnDisable()
    {
        planeManager.planesChanged -= OnPlanesChanged;
    }

    void OnPlanesChanged(ARPlanesChangedEventArgs args)
    {
        foreach (var plane in args.added)
        {
            Debug.Log($"Plano detectado: {plane.trackableId} — Tamaño: {plane.size}");
        }
    }
}
```

Asignar el script al **XR Origin**.

---

## PASO 4 — Compilar APK (20 min)

Si tienes celular Android:
1. Activar **Modo Desarrollador** en tu celular: Configuración → Acerca del teléfono → Número de compilación (toca 7 veces)
2. Activar **Depuración USB** en Opciones de desarrollador
3. Conectar celular por USB
4. En Unity: `File → Build Settings → Build And Run`
5. Unity compilará e instalará en tu celular

Si no tienes celular Android: usar el XR Device Simulator:
1. Package Manager → instalar **XR Interaction Toolkit**
2. En Hierarchy → agregar **XR Device Simulator** prefab

---

## PASO 5 — Evidencias

Guardar en `entregas/SEMANA-02/u-TU-CODIGO/`:

```
□ 1. Captura de XR Plugin Management con ARCore activado
□ 2. Captura de Player Settings (API level 24, ARM64, IL2CPP)
□ 3. Captura de Hierarchy con AR Session y XR Origin
□ 4. Captura del Inspector de XR Origin con ARPlaneManager
□ 5. Video o captura de app corriendo (en celular o simulador)
□ 6. Captura de la consola con el mensaje de plano detectado
```

---

## Preguntas de reflexión

1. ¿Qué diferencia hay entre `planesChanged.added` y `planesChanged.updated`?

2. ¿Por qué se usa `[RequireComponent(typeof(ARPlaneManager))]` en el script?

3. ¿Qué pasaría si cambias Detection Mode a "Vertical" — qué tipos de superficies detectaría?

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
