# Guía de Trabajo 13 — Audio Espacial y Háptica en XR
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** ________________________________  **Código:** ____________
**Fecha:** _________________ | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (4 puntos)

**1.** `Spatial Blend = 1` en un AudioSource significa que el sonido es:
- [ ] a) 2D — igual volumen en ambos oídos
- [ ] b) 3D — posicional, varía según distancia y dirección
- [ ] c) Mono — sin efectos espaciales
- [ ] d) Solo se escucha dentro del Collider del objeto

**2.** El `Rolloff Mode: Logarithmic` en un AudioSource simula:
- [ ] a) Que el sonido se repite en bucle logarítmico
- [ ] b) La atenuación natural del sonido en el mundo real (más realista)
- [ ] c) Que el sonido se procesa con HRTF automáticamente
- [ ] d) La velocidad de reproducción logarítmica del clip

**3.** `Handheld.Vibrate()` en Unity:
- [ ] a) Vibra el controller VR con intensidad configurable
- [ ] b) Vibra el celular Android/iOS con una vibración estándar
- [ ] c) Activa el feedback háptico de las gafas Meta Quest
- [ ] d) Crea un efecto de temblor en la cámara de la escena

**4.** HRTF en audio XR sirve para:
- [ ] a) Comprimir archivos de audio para menor tamaño
- [ ] b) Simular cómo los oídos humanos perciben la dirección del sonido
- [ ] c) Sincronizar el audio con la animación de modelos 3D
- [ ] d) Mezclar múltiples clips de audio en tiempo real

---

## PARTE II — Completar (4 puntos)

1. El componente __________ en la cámara principal capta todos los sonidos 3D de la escena (equivale al oído del usuario).

2. `audioSource.PlayOneShot(clip)` es diferente de `audioSource.Play()` porque __________.

3. Un __________ en Unity agrupa sonidos por categoría (música, efectos, voz) y permite aplicar efectos globales.

4. Para simular el eco de una caverna en una escena VR, se usaría una __________ con preset "Cave".

---

## PARTE III — Análisis y diseño (8 puntos)

**3.1** (3 pt) Diseña el sistema de audio para la escena de tu proyecto. Completa:

| Sonido | Tipo (2D/3D) | Trigger | Efecto adicional |
|-------|-------------|---------|-----------------|
| | | | |
| | | | |
| | | | |
| | | | |

**3.2** (3 pt) Explica en qué momento y por qué usarías vibración háptica en tu app XR. ¿Qué eventos la activarían?

> _________________________________________________________________
> _________________________________________________________________

**3.3** (2 pt) ¿Por qué un "Min Distance" muy grande en el AudioSource podría ser un error de diseño?

> _________________________________________________________________

---

## PARTE IV — Reflexión (4 puntos)

**4.1** (2 pt) Menciona 2 formas en que el audio espacial incorrecto puede romper la inmersión en VR.

> _________________________________________________________________
> _________________________________________________________________

**4.2** (2 pt) ¿Cómo usarías el audio como capa de accesibilidad para usuarios con discapacidad visual en tu app XR?

> _________________________________________________________________
> _________________________________________________________________

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
