# Laboratorio 11 — Tour VR con Google Cardboard
**Estudiante:** Palacios Vergaray Jhener Erick **Código:** 2231890156

## Desarrollo del laboratorio en Unity

---

## Objetivo

El objetivo del laboratorio fue desarrollar un recorrido virtual compuesto por tres habitaciones utilizando Google Cardboard. Para ello se implementó navegación mediante gaze, objetos interactivos con paneles informativos y un entorno inmersivo mediante Skybox.

---

## PASO 1 — Configuración del proyecto para Cardboard

Se inició configurando el proyecto para que pudiera ejecutarse en dispositivos Android compatibles con Google Cardboard.

Primero se instaló el paquete **Google Cardboard XR Plugin** desde:

`Window → Package Manager`

Posteriormente se habilitó el plugin en:

`Edit → Project Settings → XR Plug-in Management → Android`

marcando la opción **Cardboard**.

También se modificaron las opciones de compilación dentro de **Player Settings**, utilizando la siguiente configuración:

| Configuración | Valor |
|--------------|-------|
| Graphics API | OpenGLES3 |
| Scripting Backend | IL2CPP |
| Arquitectura | ARM64 |
| Min API | 26 |

Con esta configuración el proyecto quedó listo para ejecutarse correctamente en un teléfono Android utilizando Google Cardboard.

---

## PASO 2 — Creación de las tres salas

Se construyeron tres habitaciones utilizando únicamente primitivas de Unity.

Cada sala fue creada con:

- Un cubo aplanado para representar el suelo.
- Cubos para formar las paredes.

Para diferenciarlas visualmente se utilizaron distintos colores:

- Sala 1 → Azul
- Sala 2 → Verde
- Sala 3 → Roja

Entre cada habitación se colocó un plano vertical con transparencia para representar un portal que permitiría el cambio de una sala a otra.

---

## PASO 3 — Implementación del sistema GazeCursor

Se implementó el sistema **GazeCursor**, encargado de detectar el objeto que el usuario observa dentro de la experiencia VR.

A cada portal se le añadió el script **Portal.cs**, el cual permite trasladar al usuario al siguiente punto del recorrido cuando permanece mirando el portal durante el tiempo establecido.

```csharp
using UnityEngine;

public class Portal : MonoBehaviour
{
    public Transform destino;

    public void AlSerMirado()
    {
        Camera.main.transform.parent.position = destino.position;
    }
}
```

El sistema de gaze utiliza el método:

`SendMessage("AlSerMirado")`

para ejecutar automáticamente la acción correspondiente sobre el objeto observado.

De esta forma la navegación puede realizarse sin utilizar controles físicos.

---

## PASO 4 — Objetos con información

En cada una de las habitaciones se añadió un objeto interactivo.

Cada objeto cuenta con un panel informativo que puede mostrarse u ocultarse mediante la mirada del usuario.

Para ello se implementó el script **ObjetoInfo.cs**.

```csharp
using UnityEngine;

public class ObjetoInfo : MonoBehaviour
{
    public GameObject panelInfo;

    public void AlSerMirado()
    {
        panelInfo.SetActive(!panelInfo.activeSelf);
    }
}
```

El panel fue creado mediante un **Canvas en modo World Space** y contiene un componente **TextMeshPro** para mostrar la información del objeto correspondiente.

---

## PASO 5 — Configuración del Skybox

Para proporcionar una mayor sensación de inmersión dentro del recorrido virtual se configuró un Skybox desde:

`Window → Rendering → Lighting → Environment`

Se utilizó un material con el shader:

- Skybox/6 Sided

o

- Skybox/Panoramic

Esta configuración permitió rodear completamente al usuario con el entorno virtual.

---

## Pruebas realizadas

Se realizaron diversas pruebas para comprobar el correcto funcionamiento del recorrido VR.

| Acción | Resultado |
|--------|-----------|
| Mirar un portal | El usuario cambia correctamente de sala |
| Mirar un objeto | Se muestra u oculta el panel de información |
| Girar la cabeza | La cámara responde al movimiento del usuario |
| Activar el gaze | La acción se ejecuta después del tiempo establecido |
| Recorrer las tres salas | La navegación funciona correctamente entre todos los portales |

---

## Evidencias realizadas

### Evidencia 1

Captura de **Project Settings**, mostrando Google Cardboard habilitado dentro de **XR Plug-in Management**.

**Explicación:**  
La imagen permite comprobar que el proyecto fue configurado correctamente para ejecutarse con Google Cardboard en dispositivos Android.

---

### Evidencia 2

Captura de la **Scene View** mostrando las tres habitaciones y los portales que conectan cada una de ellas.

**Explicación:**  
Se aprecia la distribución del escenario VR y la organización de las salas que conforman el recorrido virtual.

---

### Evidencia 3

Captura del componente **GazeCursor** en el Inspector con todos sus campos correctamente asignados.

**Explicación:**  
La configuración confirma que el sistema de mirada dispone de las referencias necesarias para detectar e interactuar con los objetos.

---

### Evidencia 4

Video demostrando la navegación entre las diferentes salas utilizando únicamente la mirada sobre los portales.

**Explicación:**  
La prueba evidencia que el cambio de una habitación a otra se realiza correctamente mediante el sistema de gaze.

---

### Evidencia 5

Video mostrando un objeto interactivo cuyo panel de información aparece al ser observado.

**Explicación:**  
Se verifica el funcionamiento del objeto informativo y la visualización del contenido mediante la interacción por mirada.

---

### Evidencia 6

Commit realizado en GitHub con los scripts, escenas, prefabs y demás archivos correspondientes al laboratorio.

**Explicación:**  
El repositorio almacena el avance del proyecto y registra los cambios implementados durante el desarrollo del laboratorio.

---

## Conclusión

Durante este laboratorio se implementó un recorrido virtual básico utilizando Google Cardboard como plataforma de visualización. Además de configurar el proyecto para Android, se desarrolló un sistema de navegación basado en gaze que permite recorrer diferentes habitaciones sin utilizar controles adicionales.

Asimismo, se incorporaron paneles informativos y un Skybox para mejorar la experiencia inmersiva. Con ello se logró comprender el funcionamiento de la interacción por mirada y su aplicación en experiencias educativas desarrolladas con Unity.