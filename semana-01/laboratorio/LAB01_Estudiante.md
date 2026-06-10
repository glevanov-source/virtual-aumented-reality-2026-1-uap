# Laboratorio 01 — Instalación y Exploración de Unity XR
## ⚠️ VERSIÓN ESTUDIANTE — Para desarrollar en casa

**Estudiante:** ________________________________  **Código:** ____________
**Tiempo estimado:** 90 minutos | **Herramientas:** Unity 2022.3.62f1, Git

---

## Objetivo del Laboratorio

Al finalizar este laboratorio, habrás:
- ✅ Instalado Unity 2022.3.62f1 con soporte Android
- ✅ Creado tu primer proyecto Unity XR
- ✅ Explorado la interfaz de Unity
- ✅ Creado una escena básica con primitivas 3D
- ✅ Subido el proyecto a GitHub

---

## PASO 1 — Instalar Unity Hub y Unity 2022.3 (20 min)

### 1.1 Descargar Unity Hub

1. Ir a: https://unity.com/download
2. Descargar **Unity Hub** para tu sistema operativo
3. Instalar y abrir Unity Hub

### 1.2 Instalar Unity 2022.3.62f1

En Unity Hub:
1. Clic en **"Install Editor"**
2. Buscar versión **2022.3.62f1** (LTS)
3. Agregar módulos:
   - ✅ **Android Build Support** (obligatorio para AR móvil)
   - ✅ **Android SDK & NDK Tools**
   - ✅ **OpenJDK**
4. Instalar (~8 GB, tomar tiempo)

---

## PASO 2 — Instalar Git (5 min)

Si no tienes Git instalado:
- **Mac:** `brew install git` o descargar de https://git-scm.com
- **Windows:** Descargar Git for Windows de https://git-scm.com

Verificar instalación:
```bash
git --version
# Debe mostrar: git version 2.x.x
```

---

## PASO 3 — Crear tu primer proyecto Unity (15 min)

1. En Unity Hub → **"New project"**
2. Seleccionar template: **3D (Core)**
3. Nombre del proyecto: `RVA-S01-TuNombre` (sin espacios ni acentos)
4. Ubicación: carpeta de fácil acceso (ej. Documentos/Unity)
5. Clic en **"Create project"**

### Explorar la interfaz (5 min):

Identificar y anotar para qué sirve cada panel:

| Panel | ¿Para qué sirve? |
|-------|-----------------|
| Scene View | |
| Game View | |
| Hierarchy | |
| Inspector | |
| Project | |
| Console | |

---

## PASO 4 — Crear tu primera escena 3D (20 min)

### 4.1 Crear primitivas básicas

En Hierarchy → clic derecho → 3D Object:
1. **Cube** → renombrar a "Suelo"
   - Transform Scale: X=10, Y=0.1, Z=10
2. **Sphere** → renombrar a "PuntoDeInteres"
   - Transform Position: X=0, Y=1, Z=0
3. **Capsule** → renombrar a "PersonajeBase"
   - Transform Position: X=2, Y=1, Z=0

### 4.2 Agregar materiales de color

1. En Project → clic derecho → Create → Material
2. Crear 3 materiales: MatRojo, MatAzul, MatVerde
3. Asignar colores en el Inspector (Albedo)
4. Arrastrar materiales a los objetos

### 4.3 Configurar la cámara

1. Seleccionar **Main Camera** en Hierarchy
2. Transform Position: X=0, Y=5, Z=-10
3. Transform Rotation: X=30, Y=0, Z=0

### 4.4 Agregar iluminación

1. En Hierarchy → clic derecho → Light → Directional Light
2. Rotation: X=50, Y=-30, Z=0
3. Intensity: 1.2

---

## PASO 5 — Primer script C# (15 min)

### 5.1 Crear el script

1. Project → Create → C# Script → nombre: `RotarObjeto`
2. Doble clic para abrir en Visual Studio / VS Code

### 5.2 Escribir el código

```csharp
using UnityEngine;

// Este script hace rotar un objeto en el eje Y
// Demuestra el ciclo Update() de Unity
public class RotarObjeto : MonoBehaviour
{
    // Velocidad de rotación en grados por segundo
    public float velocidad = 45f;

    void Update()
    {
        // Cada frame, rotar el objeto
        transform.Rotate(0f, velocidad * Time.deltaTime, 0f);
    }
}
```

### 5.3 Aplicar el script

1. Arrastrar el script `RotarObjeto` a la Sphere en Hierarchy
2. En el Inspector, configurar Velocidad: 90
3. Clic en ▶️ Play para ver la esfera girar

---

## PASO 6 — Subir a GitHub (15 min)

### 6.1 Inicializar Git en el proyecto

```bash
# Ir a la carpeta del proyecto Unity
cd Documentos/Unity/RVA-S01-TuNombre

# Inicializar repositorio Git
git init

# Agregar .gitignore (copiar del repositorio del curso)
curl -o .gitignore https://raw.githubusercontent.com/RubenCarty/rva-2026-1-uap/main/.gitignore

# Agregar archivos y hacer primer commit
git add .
git commit -m "LAB01: primer proyecto Unity con escena básica - TuNombre"
```

### 6.2 Crear repo en GitHub y subir

1. Ir a github.com → "New repository"
2. Nombre: `rva-s01-TuNombre`
3. Privado o público (según preferencia)
4. NO inicializar con README (ya tienes uno)
5. Copiar los comandos que GitHub te proporciona:

```bash
git remote add origin https://github.com/TU-USUARIO/rva-s01-TuNombre.git
git branch -M main
git push -u origin main
```

---

## Evidencias que debes entregar

Guardar en `entregas/SEMANA-01/u-TU-CODIGO/capturas/`:

```
□ 1. Captura de Unity Hub con Unity 2022.3.62f1 instalado
□ 2. Captura de la Scene View con las 3 primitivas y materiales
□ 3. Captura de la Game View con la escena corriendo (Play)
□ 4. Captura del Inspector mostrando el script RotarObjeto
□ 5. Link al repositorio GitHub de tu proyecto
```

---

## Preguntas de reflexión

Responder en el PR al entregar:

1. ¿Qué diferencia hay entre Scene View y Game View?

2. ¿Qué hace `Time.deltaTime` en el script de rotación?

3. Si quisieras que la esfera solo rote cuando se presione la tecla "R", ¿cómo modificarías el script?

---

## ¿Tuviste problemas?

- Abrir un Issue en el repositorio del curso con el error exacto
- Buscar en: https://docs.unity3d.com
- Foro: https://forum.unity.com

*PSISP08075 | Universidad Autónoma del Perú | 2026-1*
