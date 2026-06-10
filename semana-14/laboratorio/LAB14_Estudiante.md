# Laboratorio 14 — Pruebas de Usabilidad y Optimización Final
## ⚠️ VERSIÓN ESTUDIANTE

**Tiempo estimado:** 90 minutos

---

## Objetivo

Aplicar el ciclo completo de evaluación de usabilidad a tu proyecto XR y hacer las optimizaciones identificadas.

---

## PASO 1 — Prueba con 2 usuarios (30 min)

1. Encontrar 2 compañeros que NO hayan visto tu app antes
2. Darles la tarea: "Usa la app durante 5 minutos sin que yo te explique nada"
3. Observar en silencio — anotar cada vez que se atascan o dudan
4. Al final, hacerles completar el SUS (10 preguntas)

**Tabla de observación:**
| Minuto | Observación | Tipo de problema |
|--------|-------------|----------------|
| | | |

---

## PASO 2 — Profiler de rendimiento (20 min)

1. `Window → Analysis → Profiler`
2. Correr la app → hacer las acciones principales
3. Anotar:
   - FPS promedio: ______
   - Draw Calls máximo: ______
   - Spike de GC más grande: ______
   - Script más lento: ______

---

## PASO 3 — Corregir al menos 3 problemas (30 min)

Basándote en Paso 1 y 2, corregir los 3 problemas más críticos:

1. Problema #1 (prioridad más alta): __________ → Solución implementada: __________
2. Problema #2: __________ → Solución: __________
3. Problema #3: __________ → Solución: __________

---

## PASO 4 — Re-verificar métricas (10 min)

Después de las correcciones, volver a medir:
- FPS antes → después: ____ → ____
- Draw Calls: ____ → ____
- SUS estimado (¿mejoraría?): ____

---

## Evidencias

```
□ 1. Tabla de observación de pruebas con usuarios
□ 2. Dos cuestionarios SUS completados
□ 3. Captura del Profiler antes de optimizaciones
□ 4. Capturas de los 3 cambios realizados
□ 5. Comparativa de métricas antes/después
□ 6. Link al commit con las correcciones
```

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
