# Guía de Trabajo 11 — VR con Google Cardboard
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** Jhener Erick Palacios Vergaray **Código:** 2231890156  
**Fecha:** 25/06/2026 | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (4 puntos)

**1.** Google Cardboard proporciona tracking de:
- [ ] a) 6DOF (posición + orientación)
- [x] b) 3DOF (solo orientación de la cabeza)
- [ ] c) Manos y controladores
- [ ] d) SLAM completo del entorno

**Explicación:**  
Cardboard únicamente detecta hacia dónde gira la cabeza del usuario, sin registrar desplazamientos físicos en el espacio.

---

**2.** El "gaze" como método de interacción en VR Cardboard:
- [ ] a) Usa el micrófono para comandos de voz
- [x] b) Detecta hacia dónde mira el usuario con el head tracking
- [ ] c) Usa la pantalla táctil del celular
- [ ] d) Requiere un joystick Bluetooth

**Explicación:**  
La interacción ocurre cuando el usuario mantiene la mirada sobre un elemento durante un tiempo determinado.

---

**3.** En el GazeCursor, `progressRing.fillAmount = tiempoMirando / tiempoActivacion` hace que:
- [ ] a) El anillo desaparezca al mirar un objeto
- [x] b) El anillo muestre el progreso circular de cuánto tiempo lleva mirando
- [ ] c) El objeto se mueva hacia la cámara
- [ ] d) Se calcule la distancia al objeto mirado

**Explicación:**  
El indicador visual informa cuánto falta para completar la activación mediante la mirada.

---

**4.** La diferencia principal entre Cardboard y Meta Quest en términos de tracking es:
- [ ] a) Cardboard tiene mejor resolución
- [x] b) Meta Quest ofrece 6DOF (movimiento libre) vs Cardboard 3DOF (solo cabeza)
- [ ] c) Cardboard usa ARCore y Quest no
- [ ] d) No hay diferencia técnica

**Explicación:**  
Meta Quest permite desplazarse físicamente, mientras que Cardboard solo reconoce la orientación de la cabeza.

---

## PARTE II — Completar (4 puntos)

1. En VR con Cardboard, la navegación principal sin controllers se hace por **gaze** (mirar un objeto por N segundos).

2. El `Tracked Pose Driver` en la cámara hace que esta siga el **movimiento de la cabeza** del usuario en tiempo real.

3. Para crear el efecto de "mirar un portal y teletransportarse", se puede usar `Camera.main.transform.**position**` para reposicionar al usuario.

4. Un tour VR de 360° puede usar un **Skybox** para envolver visualmente al usuario en una imagen esférica.

---

## PARTE III — Diseño (8 puntos)

### 3.1 (3 pt) Lista al menos 5 puntos de interés y qué información mostraría al ser mirados:

| Punto de interés | Información mostrada al mirar | Tiempo de gaze |
|-----------------|------------------------------|---------------|
| Biblioteca | Horarios, servicios y ubicación | 2 segundos |
| Laboratorios de Ingeniería | Equipos disponibles y cursos relacionados | 2 segundos |
| Auditorio | Eventos académicos y conferencias | 2 segundos |
| Cafetería | Horarios de atención y servicios | 2 segundos |
| Oficina de Bienestar Universitario | Información sobre apoyo estudiantil | 2 segundos |

**Explicación:**  
Cada punto ofrece información útil para orientar a los estudiantes durante su recorrido virtual por el campus.

---

### 3.2 (3 pt) Describe cómo implementarías la navegación entre los 5 puntos (portales, menú de gaze, waypoints).

> Se colocarían portales interactivos en cada punto del recorrido. Al mantener la mirada sobre un portal durante dos segundos, el usuario sería teletransportado al siguiente lugar. Además, cada punto mostraría un pequeño menú mediante gaze para regresar al inicio o continuar el recorrido.

**Explicación:**  
La navegación basada en portales evita desplazamientos complejos y facilita el recorrido utilizando únicamente la mirada.

---

### 3.3 (2 pt) ¿Qué limitaciones de Cardboard tendrías que comunicar a los estudiantes antes de usar el tour? Menciona 2.

> - Solo permite seguimiento de orientación (3DOF), no movimiento físico.
>
> - La interacción depende de la mirada y puede resultar menos precisa que usar controles dedicados.

**Explicación:**  
Conocer estas limitaciones ayuda a que los usuarios comprendan cómo interactuar correctamente con la experiencia VR.

---

## PARTE IV — Reflexión (4 puntos)

### 4.1 (2 pt) ¿Por qué el "gaze time" (tiempo de mirada para activar) no debe ser ni muy corto (0.2s) ni muy largo (5s)? ¿Cuál sería el tiempo óptimo y por qué?

> Un tiempo muy corto puede provocar activaciones accidentales, mientras que uno muy largo puede hacer la interacción lenta y cansada. Un tiempo aproximado de **2 segundos** ofrece un equilibrio entre rapidez y precisión.

**Explicación:**  
Un tiempo intermedio mejora la experiencia del usuario al reducir errores sin generar esperas innecesarias.

---

### 4.2 (2 pt) Propón cómo podría tu grupo usar Google Cardboard en su proyecto final. ¿Tiene sentido técnico o sería forzado?

> Podríamos desarrollar una versión inmersiva del tutor de hardware donde el usuario recorra virtualmente una computadora y observe cada componente mediante Google Cardboard. Esto tendría sentido técnico porque aprovecharía la visualización en 360° y la interacción mediante gaze.

**Explicación:**  
El uso de Cardboard complementaría el proyecto al ofrecer una experiencia inmersiva sin modificar el objetivo educativo principal.

---

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*