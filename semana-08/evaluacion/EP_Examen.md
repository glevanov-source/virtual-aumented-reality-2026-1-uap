# Examen Parcial — EP
## Realidad Virtual y Aumentada | PSISP08075 | Semana 8

**Estudiante:** ________________________________  **Código:** ____________
**Fecha:** _________________ | **Puntuación total:** ____/20 | **Tiempo:** 120 min

⚠️ Examen individual. Permitido: apuntes propios en hoja física. No permitido: internet, comunicación con compañeros.

---

## SECCIÓN A — Conceptos fundamentales (5 pts)

**A1.** (1 pt) Define la diferencia entre RA (Realidad Aumentada) y VR (Realidad Virtual). Da un ejemplo de cada una en el contexto de Ingeniería de Sistemas.

> _________________________________________________________________
> _________________________________________________________________

**A2.** (1 pt) ¿Qué es el Continuum de Milgram-Kishino? Dibuja el diagrama.

> _________________________________________________________________

**A3.** (1 pt) Explica la arquitectura GameObject-Component de Unity en 3 líneas.

> _________________________________________________________________

**A4.** (2 pt) Completa la tabla comparativa:

| Aspecto | ARCore / AR Foundation | Vuforia Engine |
|---------|----------------------|----------------|
| Tipo de tracking principal | | |
| Model Targets | | |
| Costo base | | |
| Mejor caso de uso | | |

---

## SECCIÓN B — Configuración y código (8 pts)

**B1.** (3 pts) Escribe el código completo del método `Update()` de un script que:
- Detecta un toque en la pantalla (primer frame)
- Hace un ARRaycast al punto de toque
- Si golpea un plano, instancia un prefab (o lo reposiciona si ya existe)

```csharp
// Variables ya declaradas:
// private ARRaycastManager raycastManager;
// private List<ARRaycastHit> hits = new List<ARRaycastHit>();
// private GameObject instancia;
// public GameObject prefab;

void Update()
{
    // Tu código aquí:




}
```

**B2.** (2 pts) Escribe el código de `OnChanged()` para ARTrackedImageManager que:
- Para cada imagen agregada: instancia el prefab correspondiente del diccionario `prefabs`
- Para cada imagen actualizada: actualiza la posición si está en Tracking
- Para cada imagen removida: desactiva el objeto

```csharp
// Dictionary<string, GameObject> prefabs; — mapea nombre → prefab
// Dictionary<string, GameObject> instancias; — mapea nombre → instancia

void OnChanged(ARTrackedImagesChangedEventArgs args)
{
    // Tu código:



}
```

**B3.** (3 pts) Lista los pasos para configurar un proyecto Unity 2022.3 para AR en Android, en orden:

| # | Paso | Dónde se configura |
|---|------|--------------------|
| 1 | | |
| 2 | | |
| 3 | | |
| 4 | | |
| 5 | | |

---

## SECCIÓN C — Diseño y aplicación (7 pts)

**C1.** (4 pts) Una empresa minera de Arequipa quiere una app que ayude a los ingenieros en campo a:
1. Escanear una pieza de maquinaria y ver su ficha técnica en AR
2. Navegar a pie por la planta minera con guía AR

Para cada requerimiento:
a) ¿Qué tecnología AR usarías? (ARCore planos, ARCore imágenes, Vuforia Image Target, Vuforia Model Target)
b) ¿Por qué?
c) ¿Qué componentes Unity necesitarías?

| Requerimiento | Tecnología | Por qué | Componentes |
|--------------|-----------|---------|-------------|
| Escanear pieza → ficha técnica | | | |
| Navegación AR en planta | | | |

**C2.** (3 pts) Diseña en pseudo-código (no C# exacto) el flujo de una app VR de laboratorio de química donde:
- El usuario puede agarrar probetas
- Al mezclar 2 reactivos, el color cambia
- Puede teleportarse entre estaciones

```
// Pseudo-código del flujo:



```

---

*PSISP08075 | Universidad Autónoma del Perú | 2026-1 | EP*
