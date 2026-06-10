# Guía de Trabajo 10 — Interacción Avanzada en AR
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** ________________________________  **Código:** ____________
**Fecha:** _________________ | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (4 puntos)

**1.** Para evitar conflicto entre pinch (2 dedos) y rotate (1 dedo), se debe:
- [ ] a) Deshabilitar uno de los dos gestos
- [ ] b) Usar `if (touchCount == 2)` para pinch y `else if (touchCount == 1)` para rotate
- [ ] c) Usar callbacks en vez de Update()
- [ ] d) Cada gesto requiere un script separado

**2.** Un Canvas con Render Mode "World Space" en AR permite:
- [ ] a) Que la UI siempre esté en la misma posición de la pantalla
- [ ] b) Que la UI exista como objeto 3D flotante en el espacio AR
- [ ] c) Que la UI sea transparente
- [ ] d) Optimizar automáticamente el número de draw calls

**3.** `Physics.Raycast` en este contexto (S10) detecta:
- [ ] a) Planos AR detectados por ARCore
- [ ] b) Colliders de GameObjects de Unity
- [ ] c) Feature points del SLAM
- [ ] d) La posición de la luz en la escena

**4.** `LookAt(Camera.main.transform)` aplicado a un panel UI:
- [ ] a) Hace que el panel se mueva hacia la cámara
- [ ] b) Rota el panel para que siempre enfrente a la cámara (billboard)
- [ ] c) Cambia el tamaño del panel según la distancia
- [ ] d) Desactiva el panel cuando la cámara se aleja

---

## PARTE II — Completar (4 puntos)

1. `t.deltaPosition.x` en un Touch de Unity indica __________ del dedo desde el frame anterior en el eje horizontal.

2. `Mathf.Clamp(valor, min, max)` en el pinch sirve para __________ los valores de escala extremos.

3. Para mostrar datos de una API REST en Unity, se usa una __________ (método que puede pausar sin bloquear el hilo principal).

4. Un panel UI que siempre mira a la cámara se llama __________ en diseño 3D.

---

## PARTE III — Análisis y diseño (8 puntos)

**3.1** (3 pt) Analiza este fragmento de código e identifica qué hace cada sección:

```csharp
void Update()
{
    // SECCIÓN A
    if (Input.touchCount == 2)
    {
        Touch t0 = Input.GetTouch(0);
        Touch t1 = Input.GetTouch(1);
        float dist = Vector2.Distance(t0.position, t1.position);
        // ... escalar
    }
    
    // SECCIÓN B
    else if (Input.touchCount == 1)
    {
        Touch t = Input.GetTouch(0);
        if (t.phase == TouchPhase.Moved)
        {
            objeto.transform.Rotate(Vector3.up, -t.deltaPosition.x * 0.3f, Space.World);
        }
    }
}
```

| Sección | ¿Qué hace? | ¿Cuándo se activa? |
|---------|-----------|------------------|
| A | | |
| B | | |
| Else-if | | |

**3.2** (3 pt) Diseña en pseudo-código el sistema de interacción para una app AR de compras donde el usuario puede:
- Colocar un sofá en la sala (tap en plano)
- Escalarlo con pinch
- Rotarlo con dedo
- Ver precio al tocar el sofá

```
// Pseudo-código del sistema de interacción:




```

**3.3** (2 pt) ¿Cuál es la diferencia entre Physics.Raycast y ARRaycastManager.Raycast? ¿Cuándo se usa cada uno?

| | Physics.Raycast | ARRaycastManager.Raycast |
|--|----------------|------------------------|
| Detecta | | |
| Cuándo usar | | |

---

## PARTE IV — Reflexión (4 puntos)

**4.1** (2 pt) El principio de "affordance" en UX significa que el objeto debe parecer interactivo sin necesidad de instrucciones. Propón 2 formas de aplicar affordance visual en tu app AR de proyecto.

> _________________________________________________________________
> _________________________________________________________________

**4.2** (2 pt) Si tu app AR es para usuarios mayores de 60 años (adultos mayores), ¿qué cambios harías en la interacción? Menciona al menos 3 adaptaciones.

> _________________________________________________________________
> _________________________________________________________________
> _________________________________________________________________

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
