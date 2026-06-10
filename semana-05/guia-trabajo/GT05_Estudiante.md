# Guía de Trabajo 05 — Marcadores AR e Image Tracking
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** ________________________________  **Código:** ____________
**Fecha:** _________________ | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (6 puntos)

**1.** La XRReferenceImageLibrary en Unity es:
- [ ] a) Una carpeta de Assets con imágenes PNG
- [ ] b) Una base de datos de imágenes de referencia que ARCore usará para reconocer marcadores
- [ ] c) Un shader especial para renderizar marcadores en pantalla
- [ ] d) Un plugin de terceros para Vuforia

**2.** Para un buen marcador AR, la imagen debe:
- [ ] a) Ser simétrica rotatoriamente para detectar orientación
- [ ] b) Tener alta textura, contraste y ser asimétrica
- [ ] c) Ser un logo corporativo con fondo blanco
- [ ] d) Tener baja resolución para procesarse más rápido

**3.** `TrackingState.Tracking` en ARTrackedImage significa:
- [ ] a) La imagen se está buscando pero no se ha encontrado
- [ ] b) La imagen es visible y siendo rastreada activamente
- [ ] c) La imagen fue encontrada y luego perdida
- [ ] d) El tracking ha fallado completamente

**4.** ¿Por qué se parent-ea el objeto AR al `img.transform` del ARTrackedImage?
- [ ] a) Para mejorar el rendimiento gráfico
- [ ] b) Para que el objeto 3D siga automáticamente los movimientos del marcador
- [ ] c) Es obligatorio por la API de ARCore
- [ ] d) Para compartir el mismo Collider entre ambos objetos

**5.** El `referenceImage.name` de un ARTrackedImage devuelve:
- [ ] a) El path del archivo de imagen en Assets
- [ ] b) El nombre asignado a la imagen en la XRReferenceImageLibrary
- [ ] c) El ID único del trackable
- [ ] d) El nombre del GameObject del ARTrackedImageManager

**6.** ¿Qué ocurre con el objeto AR cuando el marcador sale del campo de visión?
- [ ] a) Se destruye automáticamente
- [ ] b) Se mantiene en la última posición conocida
- [ ] c) Puede desactivarse con SetActive(false) o moverse según TrackingState
- [ ] d) ARCore lo mueve al centro de la pantalla

---

## PARTE II — Completar (6 puntos)

1. El evento __________ del ARTrackedImageManager se dispara cuando una imagen es detectada, actualizada o eliminada.

2. Para asignar el tamaño físico real del marcador (ej: 15 cm), se configura el campo __________ en la XRReferenceImageLibrary.

3. Un marcador con alta __________ y sin zonas homogéneas será detectado más fácilmente por ARCore.

4. Si `trackingState == TrackingState.__________`, el marcador está fuera de cámara o no se detecta.

5. Para manejar múltiples marcadores, se recomienda usar un __________ que asocie el nombre del marcador con su objeto/prefab.

6. `args.added` contiene imágenes detectadas __________ primera vez; `args.updated` contiene imágenes __________.

---

## PARTE III — Análisis de código (5 puntos)

```csharp
void ProcesarImagen(ARTrackedImage img)
{
    string nombre = img.referenceImage.name;
    bool activo = img.trackingState == TrackingState.Tracking;

    if (!instancias.ContainsKey(nombre))
    {
        var obj = Instantiate(prefabsPorNombre[nombre], img.transform);
        instancias[nombre] = obj;
    }

    instancias[nombre].SetActive(activo);
}
```

**3.1** (2 pt) Explica qué hace este método línea por línea.

> _________________________________________________________________
> _________________________________________________________________

**3.2** (1 pt) ¿Qué problema ocurriría si `nombre` no existe en `prefabsPorNombre`?

> _________________________________________________________________

**3.3** (2 pt) Agrega una verificación para el caso mencionado en 3.2.

```csharp
// Versión mejorada:



```

---

## PARTE IV — Aplicación (3 puntos)

**4.1** (3 pt) Diseña brevemente una app AR con marcadores para el contexto universitario (carné estudiantil, horario, plano del campus). Describe:
- ¿Qué marcador(es) usarías?
- ¿Qué activaría cada marcador?
- ¿Cómo beneficiaría a los estudiantes?

> _________________________________________________________________
> _________________________________________________________________
> _________________________________________________________________

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
