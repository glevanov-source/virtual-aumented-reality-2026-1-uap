# Guía de Trabajo 07 — Interacción en VR con XR Interaction Toolkit
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** ________________________________  **Código:** ____________
**Fecha:** _________________ | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (6 puntos)

**1.** El XRGrabInteractable requiere que el objeto tenga:
- [ ] a) Solo un Collider
- [ ] b) Rigidbody + Collider + XRGrabInteractable
- [ ] c) Solo XRGrabInteractable
- [ ] d) Rigidbody + XRController

**2.** ¿Por qué se prefiere la teleportación sobre el joystick para locomoción en VR?
- [ ] a) Es más rápida en términos de velocidad de movimiento
- [ ] b) El movimiento continuo (joystick) puede causar cinetosis (mareo)
- [ ] c) Los controllers VR no tienen joystick
- [ ] d) La teleportación es más fácil de programar

**3.** El XR Device Simulator sirve para:
- [ ] a) Simular el comportamiento de ARCore en el editor
- [ ] b) Probar interacciones VR en el editor sin gafas físicas
- [ ] c) Crear assets 3D para entornos VR
- [ ] d) Medir el rendimiento de la app VR en dispositivos

**4.** `selectEntered` en XRGrabInteractable se dispara cuando:
- [ ] a) El rayo del controller apunta al objeto
- [ ] b) El controller agarra el objeto
- [ ] c) El objeto colisiona con otro objeto
- [ ] d) El usuario mira el objeto (gaze)

**5.** El `Attach Transform` de un XRGrabInteractable define:
- [ ] a) El punto de spawn del objeto en la escena
- [ ] b) El punto de agarre — donde el controller "sujeta" el objeto
- [ ] c) La rotación máxima al lanzar el objeto
- [ ] d) La velocidad de movimiento del objeto al arrastrarse

**6.** ¿Qué componente en el XR Rig gestiona el movimiento y rotación del usuario?
- [ ] a) XR Grab Interactable
- [ ] b) Locomotion System
- [ ] c) XR Ray Interactor
- [ ] d) Tracked Pose Driver

---

## PARTE II — Completar (6 puntos)

1. El componente __________ permite que el controller emita un "rayo" para seleccionar objetos a distancia.

2. `Movement Type: __________ Tracking` es la opción más suave para objetos agarrables físicamente realistas.

3. Para que un objeto vuele al ser lanzado, la opción __________ debe estar activada en XRGrabInteractable.

4. El __________ es el control de teclado en XR Device Simulator para agarrar un objeto.

5. La "cinetosis" en VR es causada por __________ entre el movimiento percibido visualmente y el que siente el cuerpo.

6. Un `Teleportation Area` en el suelo permite al usuario __________ a esa zona usando el controller.

---

## PARTE III — Diseño (5 puntos)

Diseña la arquitectura de una escena VR para un laboratorio de química virtual donde los estudiantes mezclan reactivos.

**3.1** (2 pt) Lista los componentes Unity necesarios para cada elemento:

| Elemento | Componentes Unity necesarios |
|----------|------------------------------|
| Probeta (objeto agarrable) | |
| Mesa del laboratorio (superficie) | |
| XR Rig del usuario | |
| Botella de reactivo (lanzable) | |

**3.2** (3 pt) ¿Cómo evitarías que el estudiante "atraviese" las mesas con la mano virtual? ¿Qué consideraciones de diseño tendrías?

> _________________________________________________________________
> _________________________________________________________________
> _________________________________________________________________

---

## PARTE IV — Reflexión (3 puntos)

**4.1** (3 pt) Menciona 3 principios de diseño de interacción VR y justifica por qué son importantes con un ejemplo concreto de tu proyecto.

| Principio | Justificación con ejemplo |
|----------|--------------------------|
| | |
| | |
| | |

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
