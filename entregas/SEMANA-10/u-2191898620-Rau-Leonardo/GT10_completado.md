# Guía de Trabajo 10 — Interacción Avanzada en AR
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** Leonardo Rau Bravo **Código:** 2191898620
**Fecha:** 16/06/2026 | **Puntuación total:** 20/20

---

## PARTE I — Opción Múltiple (4 puntos)

**1.** Para evitar conflicto entre pinch (2 dedos) y rotate (1 dedo), se debe:
- [ ] a) Deshabilitar uno de los dos gestos
- [X] b) Usar `if (touchCount == 2)` para pinch y `else if (touchCount == 1)` para rotate
- [ ] c) Usar callbacks en vez de Update()
- [ ] d) Cada gesto requiere un script separado

**Justificación**
Primero se debe identificar cuántos dedos están tocando la pantalla para ejecutar únicamente el gesto correspondiente y así evitar que ambos se activen al mismo tiempo y generen comportamientos inesperados.

**2.** Un Canvas con Render Mode "World Space" en AR permite:
- [ ] a) Que la UI siempre esté en la misma posición de la pantalla
- [X] b) Que la UI exista como objeto 3D flotante en el espacio AR
- [ ] c) Que la UI sea transparente
- [ ] d) Optimizar automáticamente el número de draw calls

**Justificación**
El modo World Space hace que la interfaz forme parte del entorno de realidad aumentada, permitiendo ubicarla y manipularla como un objeto dentro del espacio 3D.

**3.** `Physics.Raycast` en este contexto (S10) detecta:
- [ ] a) Planos AR detectados por ARCore
- [X] b) Colliders de GameObjects de Unity
- [ ] c) Feature points del SLAM
- [ ] d) La posición de la luz en la escena

**Justificación**
`Physics.Raycast` se utiliza para lanzar un rayo y comprobar si impacta con los Colliders de los objetos de Unity, permitiendo seleccionarlos o interactuar con ellos dentro de la escena.

**4.** `LookAt(Camera.main.transform)` aplicado a un panel UI:
- [ ] a) Hace que el panel se mueva hacia la cámara
- [X] b) Rota el panel para que siempre enfrente a la cámara (billboard)
- [ ] c) Cambia el tamaño del panel según la distancia
- [ ] d) Desactiva el panel cuando la cámara se aleja

**Justificación**
`LookAt()` orienta el panel hacia la posición de la cámara, logrando el efecto billboard, de modo que el usuario pueda ver la interfaz de frente sin importar desde dónde la observe.


---

## PARTE II — Completar (4 puntos)

1. `t.deltaPosition.x` en un Touch de Unity indica __el desplazamiento horizontal__ del dedo desde el frame anterior en el eje horizontal.

2. `Mathf.Clamp(valor, min, max)` en el pinch sirve para __limitar__ los valores de escala extremos.

3. Para mostrar datos de una API REST en Unity, se usa una __corutina (Coroutine)__ (método que puede pausar sin bloquear el hilo principal).

4. Un panel UI que siempre mira a la cámara se llama __billboard__ en diseño 3D.

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
| A |Detecta dos toques en la pantalla, obtiene ambos dedos y calcula la distancia entre ellos para realizar el gesto de pinch|Cuando el usuario toca la pantalla con dos dedos (`Input.touchCount == 2`).|
| B |Detecta el movimiento de un dedo y rota el objeto sobre el eje Y utilizando `deltaPosition.x.`|Cuando hay un solo dedo y este se está moviendo (`TouchPhase.Moved`).|
| Else-if |Evita que el código de rotación se ejecute al mismo tiempo que el pinch, permitiendo que solo se procese un gesto por vez.|Se activa únicamente cuando no se cumple la condición anterior de dos dedos y existe un solo toque.|

**3.2** (3 pt) Diseña en pseudo-código el sistema de interacción para una app AR de compras donde el usuario puede:
- Colocar un sofá en la sala (tap en plano)
- Escalarlo con pinch
- Rotarlo con dedo
- Ver precio al tocar el sofá

```
// Pseudo-código del sistema de interacción:

Inicio

Si el usuario hace tap sobre un plano AR entonces
    Instanciar el sofá en la posición detectada
Fin Si

Si el sofá ya fue colocado entonces

    Si hay 2 dedos tocando la pantalla entonces
        Calcular la distancia entre ambos dedos
        Ajustar la escala del sofá (pinch)
    Sino si hay 1 dedo moviéndose entonces
        Rotar el sofá según el movimiento horizontal del dedo
    Fin Si

    Si el usuario hace tap sobre el sofá entonces
        Mostrar un panel UI con el precio del producto
    Fin Si

Fin Si


```

**3.3** (2 pt) ¿Cuál es la diferencia entre Physics.Raycast y ARRaycastManager.Raycast? ¿Cuándo se usa cada uno?

| | Physics.Raycast | ARRaycastManager.Raycast |
|--|----------------|------------------------|
| Detecta |Los Colliders de los GameObjects de Unity.|Los planos y puntos detectados por ARCore/ARKit en el entorno real.|
|Cuando usar |Cuando se necesita interactuar con objetos virtuales, por ejemplo seleccionar, tocar o mostrar información de un objeto 3D.|Cuando se desea colocar objetos en superficies reales detectadas por la aplicación AR, como el piso o una mesa.|

---

## PARTE IV — Reflexión (4 puntos)

**4.1** (2 pt) El principio de "affordance" en UX significa que el objeto debe parecer interactivo sin necesidad de instrucciones. Propón 2 formas de aplicar affordance visual en tu app AR de proyecto.

> __Mostrar íconos o indicadores visuales sobre el objeto, como un símbolo de rotación o una lupa con flechas, para que el usuario intuya que puede girarlo o cambiar su tamaño mediante gestos.__
>
_________________________________________________________________
> __Resaltar el objeto cuando sea seleccionado, por ejemplo, agregando un contorno brillante o cambiando ligeramente su color para indicar que se puede tocar e interactuar con él.__

**4.2** (2 pt) Si tu app AR es para usuarios mayores de 60 años (adultos mayores), ¿qué cambios harías en la interacción? Menciona al menos 3 adaptaciones.

> __Utilizar botones e íconos más grandes, para que sean más fáciles de identificar y presionar con precisión.__
>________________________________________________________________
> __Simplificar los gestos de interacción, reduciendo la cantidad de acciones complejas y ofreciendo alternativas más sencillas, como botones para rotar o escalar en lugar de depender únicamente de gestos multitáctiles.__
> _________________________________________________________________
> __Emplear textos más grandes y con alto contraste, facilitando la lectura de instrucciones, precios e información importante dentro de la aplicación.__


*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
