# Guía de Trabajo 02 — Fundamentos de Unity y Configuración XR
## ⚠️ VERSIÓN ESTUDIANTE

**Estudiante:** ________________________________  **Código:** ____________
**Fecha:** _________________ | **Puntuación total:** ____/20

---

## PARTE I — Opción Múltiple (6 puntos)

**1.** En Unity, un GameObject sin componentes:
- [ ] a) Genera un error al compilar
- [ ] b) Existe en la escena pero no hace ni muestra nada (excepto tener un Transform)
- [ ] c) Se elimina automáticamente al iniciar el juego
- [ ] d) Es equivalente a un prefab vacío

**2.** El componente Transform de un GameObject:
- [ ] a) Solo existe en objetos 3D, no en cámaras
- [ ] b) Puede eliminarse si no se necesita posicionamiento
- [ ] c) Siempre está presente y no puede eliminarse
- [ ] d) Se comparte entre objetos padre e hijo

**3.** AR Foundation en Unity es:
- [ ] a) Un plugin específico para iOS ARKit
- [ ] b) Una API de bajo nivel para acceder a la GPU
- [ ] c) Un framework multiplataforma que abstrae ARCore y ARKit
- [ ] d) Un asset para crear modelos 3D en AR

**4.** ¿Cuál es el API level mínimo recomendado para ARCore en Android?
- [ ] a) API 19 (Android 4.4)
- [ ] b) API 24 (Android 7.0)
- [ ] c) API 30 (Android 11)
- [ ] d) API 33 (Android 13)

**5.** La arquitectura de coordenadas de Unity usa:
- [ ] a) Coordenadas esféricas (r, θ, φ)
- [ ] b) Sistema Left-handed con Y hacia arriba
- [ ] c) Sistema Right-handed con Z hacia arriba (como Maya)
- [ ] d) Sistema Left-handed con Z hacia atrás

**6.** Para compilar para Android en Unity, el Scripting Backend debe ser:
- [ ] a) Mono (interpreta código en tiempo de ejecución)
- [ ] b) IL2CPP (compila a código nativo, obligatorio para ARM64)
- [ ] c) .NET Framework (para compatibilidad con .NET 4.8)
- [ ] d) WebAssembly (para apps web)

---

## PARTE II — Completar (6 puntos)

1. En Unity, el componente __________ siempre está presente en todo GameObject y define su posición, rotación y escala.

2. El paquete __________ es necesario para habilitar detección de planos en Android con Unity.

3. En la configuración de Build Settings para AR, la arquitectura de CPU obligatoria para Android moderno es __________.

4. El componente que hace visible un objeto 3D en la escena es __________ (y requiere también el componente __________).

5. En el sistema de coordenadas de Unity, el eje __________ apunta hacia el cielo.

6. El XR Origin (antes llamado AR Session Origin) contiene la __________ principal y define el origen del espacio __________.

---

## PARTE III — Caso de Estudio (5 puntos)

Un estudiante ha creado un proyecto Unity y quiere compilarlo para su celular Android y probar AR. Sin embargo, al intentar Build, obtiene estos errores:

```
Error: "ARCore is not supported on this platform"
Warning: "Target API level 21 is below minimum required (24)"
Error: "ARM64 architecture not selected"
```

**3.1** (1 pt) Identifica los 3 problemas de configuración que causaron estos errores.

| Error | Causa | Dónde se configura |
|-------|-------|-------------------|
| ARCore not supported | | |
| API level 21 | | |
| ARM64 not selected | | |

**3.2** (2 pt) Escribe los pasos exactos para corregir cada error, en orden.

> _________________________________________________________________
> _________________________________________________________________
> _________________________________________________________________

**3.3** (2 pt) ¿Por qué se recomienda IL2CPP sobre Mono para apps AR en Android?

> _________________________________________________________________
> _________________________________________________________________

---

## PARTE IV — Reflexión (3 puntos)

**4.1** (1 pt) Dibuja y explica brevemente la jerarquía de una escena AR básica en Unity (AR Session, XR Origin, Camera).

```
[Dibuja aquí la jerarquía]



```

**4.2** (2 pt) Un compañero dice: "No entiendo para qué sirve el XR Origin si ya tenemos la Main Camera". ¿Cómo le explicarías la diferencia y para qué sirve cada uno?

> _________________________________________________________________
> _________________________________________________________________
> _________________________________________________________________

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
