# Laboratorio 06 — App Vuforia con Image Targets
## ⚠️ VERSIÓN ESTUDIANTE

**Tiempo estimado:** 90 minutos

---

## Objetivo

Crear una app AR con Vuforia donde 2 Image Targets activan objetos con animación y sonido.

---

## PASO 1 — Registrar cuenta y obtener License Key (10 min)

1. Ir a https://developer.vuforia.com → "Register"
2. Crear cuenta gratuita
3. En "License Manager" → "Get Development Key"
4. Copiar la License Key

---

## PASO 2 — Crear Database con Image Targets (10 min)

1. "Target Manager" → "Add Database" → nombre: "DB_RVA_S06"
2. Type: Device (base de datos local)
3. "Add Target" → Type: Image → subir 2 imágenes de alta textura
4. Nombrar: "target01" y "target02"
5. Width: 0.1 (10 cm)
6. Descargar la DB como Unity Package

---

## PASO 3 — Configurar Unity (15 min)

1. Abrir proyecto Unity AR
2. Importar Vuforia Package (descargado en paso 2)
3. `Edit → Project Settings → Vuforia Engine` → pegar License Key
4. `Edit → Project Settings → Player → Other Settings` → Camera Usage Description

---

## PASO 4 — Crear la escena Vuforia (20 min)

1. Nueva escena vacía
2. Borrar Main Camera
3. Hierarchy → `Vuforia Engine → AR Camera`
4. Hierarchy → `Vuforia Engine → Image Target` (x2)
5. Asignar Database y target a cada ImageTarget en Inspector
6. Crear objeto 3D como hijo de cada ImageTarget

---

## PASO 5 — Script personalizado de tracking (25 min)

```csharp
using UnityEngine;
using Vuforia;

public class MiTrackableHandler : DefaultTrackableEventHandler
{
    public AudioClip sonidoDeteccion;
    public string nombreMostrar = "Objeto AR";

    private AudioSource audioSource;

    protected override void Start()
    {
        base.Start();
        audioSource = GetComponent<AudioSource>();
        if (audioSource == null)
            audioSource = gameObject.AddComponent<AudioSource>();
    }

    protected override void OnTrackingFound()
    {
        base.OnTrackingFound();
        Debug.Log($"Encontrado: {nombreMostrar}");
        if (sonidoDeteccion != null)
            audioSource.PlayOneShot(sonidoDeteccion);
    }

    protected override void OnTrackingLost()
    {
        base.OnTrackingLost();
        Debug.Log($"Perdido: {nombreMostrar}");
    }
}
```

Asignar este script a cada ImageTarget (reemplaza al DefaultTrackableEventHandler).

---

## Evidencias

```
□ 1. Captura de Vuforia Target Manager con 2 targets
□ 2. Captura de Project Settings con License Key (oscurecer parte del key)
□ 3. Captura de Hierarchy con AR Camera + 2 ImageTargets
□ 4. Video de la app funcionando: 2 marcadores → 2 objetos distintos
□ 5. Captura de la consola con mensajes "Encontrado/Perdido"
```

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
