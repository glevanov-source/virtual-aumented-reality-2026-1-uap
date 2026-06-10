# Guía de Trabajo 11 — VR con Google Cardboard
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** ________________________________  **Código:** ____________
**Fecha:** _________________ | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (4 puntos)

**1.** Google Cardboard proporciona tracking de:
- [ ] a) 6DOF (posición + orientación)
- [ ] b) 3DOF (solo orientación de la cabeza)
- [ ] c) Manos y controladores
- [ ] d) SLAM completo del entorno

**2.** El "gaze" como método de interacción en VR Cardboard:
- [ ] a) Usa el micrófono para comandos de voz
- [ ] b) Detecta hacia dónde mira el usuario con el head tracking
- [ ] c) Usa la pantalla táctil del celular
- [ ] d) Requiere un joystick Bluetooth

**3.** En el GazeCursor, `progressRing.fillAmount = tiempoMirando / tiempoActivacion` hace que:
- [ ] a) El anillo desaparezca al mirar un objeto
- [ ] b) El anillo muestre el progreso circular de cuánto tiempo lleva mirando
- [ ] c) El objeto se mueva hacia la cámara
- [ ] d) Se calcule la distancia al objeto mirado

**4.** La diferencia principal entre Cardboard y Meta Quest en términos de tracking es:
- [ ] a) Cardboard tiene mejor resolución
- [ ] b) Meta Quest ofrece 6DOF (movimiento libre) vs Cardboard 3DOF (solo cabeza)
- [ ] c) Cardboard usa ARCore y Quest no
- [ ] d) No hay diferencia técnica

---

## PARTE II — Completar (4 puntos)

1. En VR con Cardboard, la navegación principal sin controllers se hace por __________ (mirar un objeto por N segundos).

2. El `Tracked Pose Driver` en la cámara hace que esta siga el __________ del usuario en tiempo real.

3. Para crear el efecto de "mirar un portal y teletransportarse", se puede usar `Camera.main.transform.__________` para reposicionar al usuario.

4. Un tour VR de 360° puede usar un __________ para envolver visualmente al usuario en una imagen esférica.

---

## PARTE III — Diseño (8 puntos)

Diseña un tour VR educativo del campus de la Universidad Autónoma del Perú para estudiantes de primer año.

**3.1** (3 pt) Lista al menos 5 puntos de interés y qué información mostraría al ser mirados:

| Punto de interés | Información mostrada al mirar | Tiempo de gaze |
|-----------------|------------------------------|---------------|
| | | |
| | | |
| | | |
| | | |
| | | |

**3.2** (3 pt) Describe cómo implementarías la navegación entre los 5 puntos (portales, menú de gaze, waypoints).

> _________________________________________________________________
> _________________________________________________________________

**3.3** (2 pt) ¿Qué limitaciones de Cardboard tendrías que comunicar a los estudiantes antes de usar el tour? Menciona 2.

> _________________________________________________________________

---

## PARTE IV — Reflexión (4 puntos)

**4.1** (2 pt) ¿Por qué el "gaze time" (tiempo de mirada para activar) no debe ser ni muy corto (0.2s) ni muy largo (5s)? ¿Cuál sería el tiempo óptimo y por qué?

> _________________________________________________________________
> _________________________________________________________________

**4.2** (2 pt) Propón cómo podría tu grupo usar Google Cardboard en su proyecto final. ¿Tiene sentido técnico o sería forzado?

> _________________________________________________________________
> _________________________________________________________________

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
