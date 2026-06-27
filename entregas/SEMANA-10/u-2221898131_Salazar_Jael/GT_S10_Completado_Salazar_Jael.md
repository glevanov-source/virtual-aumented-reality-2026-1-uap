# Guía de Trabajo 10 — Interacción Avanzada en AR
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** Salazar Mondragón Jael Santiago  **Código:** 2221898131
**Fecha:** 18/06/2026 | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (4 puntos)

**1.** Para evitar conflicto entre pinch (2 dedos) y rotate (1 dedo), se debe:
- [ ] a) Deshabilitar uno de los dos gestos
- [x] b) Usar `if (touchCount == 2)` para pinch y `else if (touchCount == 1)` para rotate
- [ ] c) Usar callbacks en vez de Update()
- [ ] d) Cada gesto requiere un script separado

**Explicación:**  
Al separar los gestos según la cantidad de dedos, Unity puede distinguir claramente cuándo debe escalar y cuándo debe rotar el objeto.

---

**2.** Un Canvas con Render Mode "World Space" en AR permite:
- [ ] a) Que la UI siempre esté en la misma posición de la pantalla
- [x] b) Que la UI exista como objeto 3D flotante en el espacio AR
- [ ] c) Que la UI sea transparente
- [ ] d) Optimizar automáticamente el número de draw calls

**Explicación:**  
World Space convierte la interfaz en un objeto dentro del mundo 3D, permitiendo colocarla junto a los elementos de realidad aumentada.

---

**3.** `Physics.Raycast` en este contexto (S10) detecta:
- [ ] a) Planos AR detectados por ARCore
- [x] b) Colliders de GameObjects de Unity
- [ ] c) Feature points del SLAM
- [ ] d) La posición de la luz en la escena

**Explicación:**  
Physics.Raycast interactúa con los colliders de los objetos de Unity para detectar toques, clics o selecciones.

---

**4.** `LookAt(Camera.main.transform)` aplicado a un panel UI:
- [ ] a) Hace que el panel se mueva hacia la cámara
- [x] b) Rota el panel para que siempre enfrente a la cámara (billboard)
- [ ] c) Cambia el tamaño del panel según la distancia
- [ ] d) Desactiva el panel cuando la cámara se aleja

**Explicación:**  
La función LookAt orienta el objeto hacia la cámara para que el usuario siempre pueda leer la información mostrada.

---

## PARTE II — Completar (4 puntos)

1. `t.deltaPosition.x` en un Touch de Unity indica **el desplazamiento horizontal** del dedo desde el frame anterior en el eje horizontal.

2. `Mathf.Clamp(valor, min, max)` en el pinch sirve para **limitar** los valores de escala extremos.

3. Para mostrar datos de una API REST en Unity, se usa una **corrutina (Coroutine)** (método que puede pausar sin bloquear el hilo principal).

4. Un panel UI que siempre mira a la cámara se llama **Billboard** en diseño 3D.

---

## PARTE III — Análisis y diseño (8 puntos)

### 3.1 (3 pt) Analiza este fragmento de código e identifica qué hace cada sección:

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
| A | Calcula la distancia entre dos dedos para realizar escalado (pinch) | Cuando hay exactamente 2 dedos tocando la pantalla |
| B | Rota el objeto utilizando el movimiento horizontal del dedo | Cuando hay 1 dedo moviéndose sobre la pantalla |
| Else-if | Evita que rotación y escalado ocurran al mismo tiempo | Cuando no se cumple la condición de dos dedos |

**Explicación:**  
El código separa las interacciones según la cantidad de dedos detectados, evitando conflictos entre los gestos de escalado y rotación.

---

### 3.2 (3 pt) Diseña en pseudo-código el sistema de interacción para una app AR de compras donde el usuario puede:
- Colocar un sofá en la sala (tap en plano)
- Escalarlo con pinch
- Rotarlo con dedo
- Ver precio al tocar el sofá

```
// Pseudo-código del sistema de interacción:

Si usuario toca un plano AR
    Instanciar sofá en la posición detectada

Si Input.touchCount == 2
    Calcular distancia entre dedos
    Escalar sofá según distancia

Si Input.touchCount == 1 y el dedo se mueve
    Rotar sofá según movimiento horizontal

Si usuario toca el collider del sofá
    Mostrar panel con precio y descripción

Si usuario toca fuera del sofá
    Ocultar panel de información
```

**Explicación:**  
El sistema combina interacción AR para colocar el objeto, gestos táctiles para manipularlo y selección mediante Raycast para mostrar información.

---

### 3.3 (2 pt) ¿Cuál es la diferencia entre Physics.Raycast y ARRaycastManager.Raycast? ¿Cuándo se usa cada uno?

| | Physics.Raycast | ARRaycastManager.Raycast |
|--|----------------|------------------------|
| Detecta | Colliders de objetos Unity | Planos y superficies detectadas por ARCore/ARKit |
| Cuándo usar | Seleccionar o interactuar con objetos existentes | Colocar objetos virtuales sobre superficies reales |

**Explicación:**  
ARRaycastManager se utiliza para encontrar superficies del entorno real, mientras que Physics.Raycast se usa para interactuar con objetos virtuales ya creados.

---

## PARTE IV — Reflexión (4 puntos)

### 4.1 (2 pt) El principio de "affordance" en UX significa que el objeto debe parecer interactivo sin necesidad de instrucciones. Propón 2 formas de aplicar affordance visual en tu app AR de proyecto.

> Mostrar un efecto de resaltado o brillo cuando el usuario apunta a un componente de hardware.
>
> Utilizar iconos o botones visibles sobre los componentes para indicar que pueden tocarse y mostrar información adicional.

**Explicación:**  
Los elementos visuales ayudan al usuario a identificar rápidamente qué objetos son interactivos sin necesidad de leer instrucciones.

---

### 4.2 (2 pt) Si tu app AR es para usuarios mayores de 60 años (adultos mayores), ¿qué cambios harías en la interacción? Menciona al menos 3 adaptaciones.

> Utilizar botones más grandes y fáciles de presionar.
>
> Aumentar el tamaño de textos y etiquetas informativas.
>
> Reducir la cantidad de gestos complejos y reemplazarlos por botones simples para rotar o escalar objetos.

**Explicación:**  
Estas adaptaciones mejoran la accesibilidad y reducen la dificultad de uso para personas con menor experiencia tecnológica o limitaciones visuales y motoras.

---

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*