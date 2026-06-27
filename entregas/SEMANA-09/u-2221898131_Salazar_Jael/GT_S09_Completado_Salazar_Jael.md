# Guía de Trabajo 09 — Diseño de Escenarios 3D
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** Salazar Mondragón Jael Santiago  **Código: 2221898131** 
**Fecha:** 12/06/2026 | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (6 puntos)

**1.** ProBuilder en Unity permite:
- [ ] a) Importar modelos de Blender automáticamente
- [x] b) Crear geometría 3D directamente dentro del editor de Unity
- [ ] c) Generar texturas procedurales para cualquier objeto
- [ ] d) Compilar shaders personalizados para AR

**Explicación:**  
ProBuilder es una herramienta de modelado integrada en Unity. Permite crear y editar objetos 3D sin necesidad de usar software externo como Blender.

---

**2.** En iluminación "Baked", la iluminación:
- [ ] a) Se calcula en tiempo real en cada frame
- [x] b) Se precalcula y se guarda en texturas (lightmaps) — cero costo en runtime
- [ ] c) Se descarga desde un servidor en la nube
- [ ] d) Solo funciona con luz ambiental, no con luces direccionales

**Explicación:**  
La iluminación se calcula antes de ejecutar la aplicación y se almacena en lightmaps. Esto mejora significativamente el rendimiento durante la ejecución.

---

**3.** "Static Batching" en Unity:
- [ ] a) Combina todos los objetos en una sola malla en tiempo real
- [x] b) Agrupa objetos estáticos con el mismo material en un solo draw call
- [ ] c) Reduce el número de polígonos de los objetos automáticamente
- [ ] d) Solo funciona con objetos instanciados desde Prefabs

**Explicación:**  
Static Batching reduce la cantidad de draw calls al agrupar objetos estáticos que comparten material. Esto disminuye la carga de renderizado.

---

**4.** LOD Group significa:
- [ ] a) Layer Of Darkness — control de sombras por capa
- [x] b) Level Of Detail — cambia la complejidad del modelo según distancia
- [ ] c) List Of Draw calls — optimización de renderizado
- [ ] d) Load On Demand — carga objetos cuando son necesarios

**Explicación:**  
LOD utiliza versiones simplificadas de un modelo cuando este se encuentra lejos de la cámara. Así se reducen polígonos y se mejora el rendimiento.

---

**5.** Para AR en celular Android de gama media, ¿cuántos draw calls máximo se recomienda?
- [ ] a) ≤500 draw calls
- [ ] b) ≤250 draw calls
- [x] c) ≤100 draw calls
- [ ] d) No hay límite, el driver los gestiona automáticamente

**Explicación:**  
Las aplicaciones XR móviles requieren optimización estricta. Mantener los draw calls por debajo de 100 ayuda a conservar una tasa de FPS estable.

---

**6.** Occlusion Culling en Unity:
- [ ] a) Reduce la resolución de texturas en tiempo real
- [x] b) Evita renderizar objetos que están ocultos detrás de otros
- [ ] c) Elimina triángulos duplicados de la malla
- [ ] d) Combina materiales similares en uno solo

**Explicación:**  
Occlusion Culling evita dibujar objetos que el usuario no puede ver. Esto reduce trabajo innecesario para la GPU.

---

## PARTE II — Completar (6 puntos)

1. El número de **Draw Calls** indica cuántas llamadas de renderizado hace el GPU por frame — debe mantenerse bajo en XR móvil.

2. Para que Static Batching funcione, los objetos deben estar marcados como **Static** en el Inspector de Unity.

3. El proceso de calcular la iluminación y guardarla en texturas se llama hacer **Bake** (o "baking de iluminación").

4. GPU Instancing es útil cuando hay muchos objetos **idénticos** (mismo prefab, mismo material) en la escena.

5. Los **Lightmaps** son texturas que almacenan la **iluminación** precalculada de la escena estática.

6. En ProBuilder, para crear un pasillo o sala a partir de un cubo, se usa la operación de **Extrusión** de caras.

---

## PARTE III — Análisis de escena (5 puntos)

Un estudiante reporta estos valores en la ventana **Stats** de Unity:

```
Draw Calls: 347
Tris: 2.1M
Verts: 1.8M
FPS (Game View): 12
```

### 3.1 (2 pt) Identifica los 3 problemas principales de rendimiento basándote en los valores.

| Valor | Problema | Impacto |
|-------|---------|---------|
| Draw Calls: 347 | Exceso de draw calls | Mayor carga para CPU y renderizado lento |
| Tris: 2.1M | Demasiados polígonos en la escena | Sobrecarga de GPU y menor rendimiento |
| FPS: 12 | Tasa de cuadros muy baja | Experiencia poco fluida y posible mareo en XR |

**Explicación:**  
Los draw calls son muy altos para XR móvil, la cantidad de triángulos es excesiva y el FPS está muy por debajo de los 30 FPS mínimos recomendados.

---

### 3.2 (3 pt) Propón 3 técnicas de optimización específicas para mejorar esta escena. Para cada una, indica cómo la aplicarías.

| Técnica | Cómo aplicarla | Mejora esperada |
|---------|---------------|----------------|
| Static Batching | Marcar objetos estáticos con el mismo material como Static | Reducir draw calls |
| LOD Group | Crear versiones simplificadas de objetos lejanos | Reducir cantidad de triángulos renderizados |
| Occlusion Culling | Habilitar Occlusion Culling en paredes y estructuras | Evitar renderizar objetos ocultos |

**Explicación:**  
Estas técnicas atacan directamente los problemas observados. Reducen draw calls, disminuyen polígonos visibles y evitan renderizados innecesarios.

---

## PARTE IV — Diseño de escenario (3 puntos)

### 4.1 (3 pt) Para el escenario de tu proyecto grupal, completa:

| Elemento | ¿Estático o dinámico? | Técnica de optimización | Material (descripción) |
|---------|----------------------|------------------------|----------------------|
| Placa madre | Estático | Static Batching | Material PBR con textura de circuito electrónico |
| CPU | Estático | Lightmap Baked | Material metálico con acabado brillante |
| Memoria RAM | Estático | GPU Instancing | Material plástico y metálico |
| Etiquetas informativas UI | Dinámico | Canvas optimizado | Material UI transparente con texto educativo |

**Explicación:**  
En el proyecto AR PC Hardware Tutor la mayoría de componentes físicos permanecen estáticos. Solo las etiquetas e interfaces cambian según la interacción del usuario.