# Laboratorio 05 — App AR con Múltiples Marcadores
## ⚠️ VERSIÓN ESTUDIANTE

**Tiempo estimado:** 90 minutos

---

## Objetivo

Construir una app AR donde 3 marcadores impresos activen 3 objetos 3D diferentes con animación.

---

## PASO 1 — Preparar marcadores (15 min)

Descargar o diseñar 3 imágenes con alta textura:
- `marcador01.png` — alta textura (foto de naturaleza, mapa, etc.)
- `marcador02.png` — alta textura diferente
- `marcador03.png` — alta textura diferente

Imprimir o mostrar en pantalla de otro celular. Asegurarse de que son diferentes entre sí.

---

## PASO 2 — Crear la Image Library (15 min)

1. `Assets → Create → XR → Reference Image Library` → nombre: `LibMarcadores`
2. Agregar las 3 imágenes, nombrar: "marcador01", "marcador02", "marcador03"
3. Especificar tamaño físico (ej: 0.1 m si el marcador mide 10 cm)
4. Habilitar "Keep Texture at Runtime"

---

## PASO 3 — Crear 3 prefabs distintos (10 min)

- **Prefab01:** Esfera roja con Rotation Animator (rota en Y)
- **Prefab02:** Cubo azul que sube y baja (seno en Y)
- **Prefab03:** Cápsula verde que pulsa (escala oscila entre 0.8 y 1.2)

Crear 3 scripts simples de animación y asignarlos a cada prefab.

---

## PASO 4 — Implementar GestorMarcadores (30 min)

Usar el script de la sesión. Adaptar para 3 marcadores.

Configurar en el Inspector:
- Asignar LibMarcadores al ARTrackedImageManager
- Llenar el array de MarcadorObjeto con los 3 pares nombre-prefab

---

## PASO 5 — Agregar texto informativo por marcador (15 min)

Agregar al prefab un objeto hijo con TextMeshPro que muestre el nombre del marcador al detectarse.

---

## Evidencias

```
□ 1. Captura de la Image Library con 3 imágenes configuradas
□ 2. Captura de Hierarchy con ARTrackedImageManager configurado
□ 3. Video mostrando los 3 marcadores activando objetos diferentes
□ 4. Captura del script GestorMarcadores en el Inspector
□ 5. Link al commit de GitHub
```

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
