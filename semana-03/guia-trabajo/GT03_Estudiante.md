# Guía de Trabajo 03 — Detección de Planos y Objetos AR
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** ________________________________  **Código:** ____________
**Fecha:** _________________ | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (6 puntos)

**1.** El ARRaycastManager.Raycast() en AR Foundation:
- [ ] a) Detecta colisiones con Colliders de Unity
- [ ] b) Golpea trackables AR como planos o puntos de la nube
- [ ] c) Usa la cámara secundaria del dispositivo
- [ ] d) Funciona igual que Physics.Raycast de Unity

**2.** TrackableType.PlaneWithinPolygon significa:
- [ ] a) Detectar planos que tengan forma poligonal
- [ ] b) El rayo debe impactar dentro del área exacta del polígono del plano
- [ ] c) Crear un polígono al detectar un plano
- [ ] d) Solo detectar planos con más de 4 lados

**3.** `Input.GetTouch(0).phase == TouchPhase.Began` se usa para:
- [ ] a) Detectar cuando el dedo está moviéndose
- [ ] b) Detectar el primer frame en que el dedo toca la pantalla
- [ ] c) Detectar múltiples dedos simultáneamente
- [ ] d) Detectar cuando el dedo se levanta

**4.** El `hits[0].pose` del ARRaycastHit contiene:
- [ ] a) Solo la posición del impacto en coordenadas de pantalla
- [ ] b) La posición y rotación del punto de impacto en el mundo 3D
- [ ] c) La ID del plano golpeado
- [ ] d) El vector normal del plano en coordenadas de cámara

**5.** ¿Por qué se comprueba `if (Input.touchCount == 0) return;` antes del raycast?
- [ ] a) Para evitar que el raycast se llame 60 veces por segundo cuando no hay toque
- [ ] b) Porque ARRaycastManager solo funciona con toque, no con ratón
- [ ] c) Es obligatorio para que funcione en iOS
- [ ] d) Para mejorar el rendimiento del renderizado

**6.** Al hacer `Instantiate(prefab, pose.position, pose.rotation)`:
- [ ] a) El objeto aparece en la posición de la cámara
- [ ] b) El objeto aparece en el punto de impacto del raycast, orientado según el plano
- [ ] c) El objeto aparece en el origen de la escena (0,0,0)
- [ ] d) La rotación siempre es (0,0,0)

---

## PARTE II — Completar (6 puntos)

1. ARCore detecta dos tipos principales de planos: __________ y __________.

2. La propiedad que devuelve cuántos dedos están tocando la pantalla en Unity es __________.

3. Un **trackable** en AR Foundation es cualquier objeto del mundo real que ARCore puede __________ en el tiempo.

4. Para que ARRaycastManager funcione, debe estar en el mismo GameObject que __________ (componente de Unity XR).

5. `TouchPhase.Began` corresponde al __________ frame del toque; `TouchPhase.Ended` corresponde al frame en que el dedo __________.

6. El método `SetPositionAndRotation(pos, rot)` en un Transform equivale a asignar __________ y __________ por separado.

---

## PARTE III — Análisis de código (5 puntos)

Analizar este fragmento de código:

```csharp
void Update()
{
    if (Input.touchCount > 0)
    {
        Touch toque = Input.GetTouch(0);
        if (toque.phase == TouchPhase.Began)
        {
            List<ARRaycastHit> hits = new List<ARRaycastHit>();
            if (raycastManager.Raycast(toque.position, hits, TrackableType.FeaturePoint))
            {
                Instantiate(prefab, hits[0].pose.position, Quaternion.identity);
            }
        }
    }
}
```

**3.1** (1 pt) ¿Qué tipo de superficie detecta este código? ¿Cuándo sería útil?

> _________________________________________________________________

**3.2** (2 pt) Identifica 2 problemas o limitaciones de este código para una app de producción.

| Problema | Solución propuesta |
|----------|-------------------|
| | |
| | |

**3.3** (2 pt) Modifica el código para que use `TrackableType.PlaneWithinPolygon` y que solo instancie 1 objeto (si ya existe, lo reposiciona en vez de crear otro).

```csharp
// Escribe aquí tu versión mejorada:




```

---

## PARTE IV — Reflexión (3 puntos)

**4.1** (1 pt) ¿Por qué `Quaternion.identity` (rotación cero) no es la mejor elección para colocar un objeto sobre un plano AR?

> _________________________________________________________________

**4.2** (2 pt) Diseña brevemente una app AR donde la detección de planos sea la tecnología principal. Describe: qué detecta, qué muestra, para qué sirve.

> _________________________________________________________________
> _________________________________________________________________
> _________________________________________________________________

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
