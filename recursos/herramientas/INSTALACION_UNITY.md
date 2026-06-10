# Guía de Instalación — Unity 2022.3.62f1

## Paso 1: Instalar Unity Hub

1. Ir a: https://unity.com/download
2. Descargar Unity Hub para tu sistema operativo
3. Instalar con permisos de administrador

## Paso 2: Instalar Unity 2022.3.62f1 (LTS)

En Unity Hub:
1. Sección "Installs" → "Install Editor"
2. Buscar versión **2022.3.62f1** (Long Term Support)
3. Módulos a instalar:
   - ✅ Android Build Support
   - ✅ Android SDK & NDK Tools
   - ✅ OpenJDK
   - ✅ (Opcional) Documentation
4. Clic "Install" — descarga ~8 GB

## Paso 3: Activar Android en el celular para pruebas

1. **Activar Modo Desarrollador:**
   - Ajustes → Acerca del teléfono → "Número de compilación" → tocar 7 veces
2. **Activar Depuración USB:**
   - Ajustes → Opciones de desarrollador → Depuración USB ✅
3. Conectar por USB → seleccionar "Transferir archivos"

## Paso 4: Verificar instalación de ARCore

Tu celular debe ser compatible con ARCore:
- Verificar en: https://developers.google.com/ar/devices
- Instalar app "Google Play Services for AR" desde Play Store si no está instalada

## Paso 5: Instalar Visual Studio Code (editor de código)

1. Descargar VS Code: https://code.visualstudio.com
2. Instalar extensión: "C# Dev Kit" (Microsoft)
3. Extensión: "Unity" (Unity Technologies)

## Comandos de verificación

```bash
# Verificar Git
git --version

# Verificar Java (necesario para builds Android)
java -version

# Verificar ADB (para instalar APKs)
adb version
```

## Solución de problemas frecuentes

| Problema | Solución |
|---------|---------|
| Unity no detecta el celular | Verificar drivers USB, activar Depuración USB |
| Build falla con "SDK not found" | Reinstalar módulo Android SDK en Unity Hub |
| ARCore no funciona en el celular | Verificar compatibilidad en lista de Google |
| IL2CPP error en build | Instalar Android NDK Tools en Unity Hub |
