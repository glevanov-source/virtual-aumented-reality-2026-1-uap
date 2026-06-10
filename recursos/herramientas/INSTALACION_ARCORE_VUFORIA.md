# Guía de Instalación — ARCore y Vuforia en Unity

## ARCore (AR Foundation)

### En Unity, vía Package Manager:

1. `Window → Package Manager`
2. Dropdown: "Unity Registry"
3. Instalar en orden:
   - **AR Foundation** 5.1.x
   - **Google ARCore XR Plugin** 5.1.x
   - **XR Plugin Management** (se instala automáticamente)

### Configuración obligatoria:

**Build Settings:**
```
File → Build Settings → Android → Switch Platform
```

**Player Settings (Edit → Project Settings → Player):**
```
Other Settings:
- Scripting Backend: IL2CPP
- Target Architectures: ✅ ARM64
- Minimum API Level: Android 7.0 (API 24)
- Camera Usage Description: "Usa la cámara para Realidad Aumentada"
```

**XR Plugin Management:**
```
Edit → Project Settings → XR Plug-in Management → Android:
- ✅ ARCore
```

---

## Vuforia Engine

### Registrar cuenta:

1. Ir a: https://developer.vuforia.com
2. "Register" → crear cuenta gratuita
3. "License Manager" → "Get Development Key" → copiar License Key

### Instalar Vuforia en Unity:

**Opción 1 — Package Manager:**
```
Window → Package Manager → "+" → Add by URL:
https://developer.vuforia.com/packagemanager (verificar URL actual en portal)
```

**Opción 2 — Asset Store:**
Buscar "Vuforia Engine" en Package Manager pestaña Asset Store.

### Configurar License Key:

```
Edit → Project Settings → Vuforia Engine → App License Key
(pegar la key del portal)
```

### Crear Image Database:

1. Portal Vuforia → "Target Manager" → "Add Database"
2. Agregar imágenes (Image Targets)
3. "Download Database" → Unity Package
4. `Assets → Import Package → Custom Package` → seleccionar el .unitypackage

---

## Solución de problemas

| Error | Causa | Solución |
|-------|-------|---------|
| "ARCore not supported" | Plugin no activado | XR Plug-in Management → Android → ✅ ARCore |
| "Vuforia License Invalid" | Key incorrecta o espacios | Pegar la key sin espacios al inicio/final |
| "Build failed: ARM64 not found" | SDK desactualizado | Unity Hub → Reinstall → agregar Android SDK |
| "Min API 21 not supported" | Configuración incorrecta | Player Settings → Minimum API Level → 24 |
