# SESIÓN 05 — Marcadores AR e Image Tracking
## Realidad Virtual y Aumentada | PSISP08075 | Semana 5

---

## Información General

| Campo | Detalle |
|-------|---------|
| **Tema** | ARTrackedImageManager y XRReferenceImageLibrary |
| **Valor** | Innovación tecnológica en soluciones de realidad aumentada |
| **Duración** | 3h 45min (receso 20 min) |
| **Logro** | Implementa AR basada en marcadores usando ARTrackedImageManager |

---

## 1. INICIO (30 min)

### Demo impactante (10 min)

Docente imprime una imagen (logo de la universidad o tarjeta de visita) y la apunta con la cámara — aparece un modelo 3D encima.

> "Este es el tipo de AR que se usa en revistas, packaging, marketing. Hoy aprenderán a construirlo."

### Pregunta de activación (10 min)

> "¿Cuál es la diferencia fundamental entre AR por marcadores (como esta demo) y AR por detección de planos (como la semana pasada)?"

Respuesta esperada:
- **Marcadores:** anclan a una imagen específica. El objeto solo aparece si la cámara ve ESA imagen.
- **Planos:** anclan a cualquier superficie plana. No necesitan imagen predefinida.

### Preview del logro (10 min)

> "Usarán ARTrackedImageManager para que la cámara reconozca imágenes específicas y active objetos 3D. Al final, tendrán una app donde cada marcador activa un objeto diferente."

---

## 2. UTILIDAD (15 min)

### ¿Dónde se usa image tracking?

| Industria | Caso de uso | Ejemplo concreto |
|-----------|-----------|-----------------|
| Marketing | Packaging AR | Cereales con personaje animado |
| Educación | Libros AR | Libros de anatomía con órganos 3D |
| Industria | Manuales de servicio | Escanear pieza → ver instrucciones 3D |
| Turismo | Mapas AR | Mapa impreso → tour virtual |
| Salud | Medicamentos AR | Caja del medicamento → dosificación 3D |

---

## 3. TRANSFORMACIÓN (75 min)

### Concepto 1: XRReferenceImageLibrary (20 min)

Una **Image Library** es una base de datos de imágenes de referencia que ARCore usará para reconocer en el mundo real.

**Crear la biblioteca:**
1. `Assets → Create → XR → Reference Image Library`
2. Nombrar: `BibliotecaMarcadores`
3. Clic en "+" para agregar imágenes:
   - Arrastrar imagen PNG/JPG con alta textura (no logos lisos)
   - Establecer tamaño físico real (ej: 0.15 m = 15 cm)
   - Habilitar "Keep Texture at Runtime"

**Criterios para buenos marcadores:**
```
✅ Alta textura/contraste    ✅ Asimétrico (no rotacionalmente simétrico)
✅ Sin zonas homogéneas      ✅ Resolución ≥ 300x300 px
❌ Logo con fondo blanco     ❌ Cuadrícula uniforme
❌ Código QR simple          ❌ Imagen borrosa
```

**Rating de calificación** (Vuforia/ARCore muestran estrellas):
- ⭐⭐⭐⭐⭐ = reconocimiento excelente
- ⭐⭐⭐ = aceptable para demos
- ⭐ = evitar en producción

### Concepto 2: ARTrackedImageManager (20 min)

```csharp
using UnityEngine.XR.ARFoundation;

// ARTrackedImageManager detecta imágenes de la biblioteca
// y crea/actualiza ARTrackedImage por cada imagen encontrada

void OnEnable()  => trackedImageManager.trackedImagesChanged += OnChanged;
void OnDisable() => trackedImageManager.trackedImagesChanged -= OnChanged;

void OnChanged(ARTrackedImagesChangedEventArgs args)
{
    foreach (var imagen in args.added)
        ActualizarContenidoAR(imagen);  // imagen encontrada por primera vez

    foreach (var imagen in args.updated)
        ActualizarContenidoAR(imagen);  // imagen ya vista, actualizada

    foreach (var imagen in args.removed)
        DesactivarContenidoAR(imagen);  // imagen fuera de cámara
}

void ActualizarContenidoAR(ARTrackedImage imagen)
{
    string nombreMarcador = imagen.referenceImage.name;
    bool activo = imagen.trackingState == TrackingState.Tracking;
    // Activar/desactivar objetos según marcador y estado
}
```

**TrackingState:**
- `Tracking`: imagen visible y siendo rastreada
- `Limited`: imagen parcialmente visible o perdiendo tracking
- `None`: imagen no detectada

### Concepto 3: Múltiples marcadores → múltiples objetos (15 min)

**Patrón Dictionary para manejar múltiples marcadores:**

```csharp
// Asociar nombre de marcador a prefab/objeto
private Dictionary<string, GameObject> objetosPorMarcador = new Dictionary<string, GameObject>()
{
    { "marcador_planeta_tierra", prefabTierra },
    { "marcador_planeta_marte",  prefabMarte },
    { "marcador_planeta_venus",  prefabVenus }
};
```

### Demo: Script completo (20 min)

```csharp
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System.Collections.Generic;

[RequireComponent(typeof(ARTrackedImageManager))]
public class GestorMarcadores : MonoBehaviour
{
    [System.Serializable]
    public struct MarcadorObjeto
    {
        public string nombreMarcador;
        public GameObject prefab;
    }

    public MarcadorObjeto[] marcadores;

    private ARTrackedImageManager manager;
    private Dictionary<string, GameObject> instancias = new Dictionary<string, GameObject>();

    void Awake() => manager = GetComponent<ARTrackedImageManager>();
    void OnEnable()  => manager.trackedImagesChanged += OnImagesChanged;
    void OnDisable() => manager.trackedImagesChanged -= OnImagesChanged;

    void OnImagesChanged(ARTrackedImagesChangedEventArgs args)
    {
        foreach (var img in args.added)   ProcesarImagen(img);
        foreach (var img in args.updated) ProcesarImagen(img);
        foreach (var img in args.removed)
        {
            if (instancias.TryGetValue(img.referenceImage.name, out GameObject obj))
                obj.SetActive(false);
        }
    }

    void ProcesarImagen(ARTrackedImage img)
    {
        string nombre = img.referenceImage.name;
        bool tracking = img.trackingState == TrackingState.Tracking;

        // Crear instancia si no existe
        if (!instancias.ContainsKey(nombre))
        {
            foreach (var m in marcadores)
            {
                if (m.nombreMarcador == nombre)
                {
                    var obj = Instantiate(m.prefab, img.transform.position, img.transform.rotation);
                    obj.transform.parent = img.transform;
                    instancias[nombre] = obj;
                    break;
                }
            }
        }

        // Actualizar visibilidad y posición
        if (instancias.TryGetValue(nombre, out GameObject instancia))
        {
            instancia.SetActive(tracking);
            if (tracking)
                instancia.transform.SetPositionAndRotation(img.transform.position, img.transform.rotation);
        }
    }
}
```

---

## 4. PRÁCTICA (60 min)

### Actividad: App de tarjetas AR (60 min)

**Escenario:** Crear una app donde 2-3 tarjetas imprestas activan diferentes modelos 3D.

1. Diseñar/descargar 2-3 imágenes de alta textura
2. Crear XRReferenceImageLibrary con esas imágenes
3. Crear 3 prefabs simples (planeta Tierra, cubo giratorio, cápsula)
4. Implementar GestorMarcadores
5. Probar con imágenes impresas o en pantalla de otro celular

---

## 5. CIERRE (25 min)

### Showcase (10 min)

3-4 grupos muestran su app de marcadores funcionando.

### Limitaciones del image tracking (5 min)

- **Iluminación:** ambientes oscuros degradan el tracking
- **Angulación:** ángulos muy oblicuos (>60°) pierden el tracking
- **Movimiento:** marcadores en movimiento pueden "temblar"
- **Escala:** el tamaño físico debe ser correcto para anclar correctamente

### Tarea S06 (5 min)

> Preparar los marcadores para el proyecto del grupo (imágenes a usar en su proyecto).

---

## Referencias APA 7

Google LLC. (2024). *AR Foundation image tracking*. https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@5.1/manual/features/image-tracking.html

Schmalstieg, D., & Höllerer, T. (2016). *Augmented reality: Principles and practice*. Addison-Wesley.
