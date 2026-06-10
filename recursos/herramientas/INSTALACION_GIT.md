# Guía de Instalación — Git y GitHub

## 1. Instalar Git

### Windows
1. Descargar: https://git-scm.com/download/win
2. Instalar con opciones por defecto
3. Verificar: abrir PowerShell → `git --version`

### macOS
```bash
# Con Homebrew:
brew install git

# Verificar:
git --version
```

### Linux (Ubuntu/Debian)
```bash
sudo apt install git
git --version
```

## 2. Configurar Git

```bash
# Configurar identidad (una sola vez)
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# Verificar configuración
git config --list
```

## 3. Crear cuenta en GitHub

1. Ir a: https://github.com
2. Clic "Sign up"
3. Usar tu correo universitario si es posible
4. Completar verificación
5. **Guardar** tu usuario y contraseña

## 4. Comandos Git esenciales para el curso

```bash
# Clonar el repositorio del curso (después de hacer fork)
git clone https://github.com/TU-USUARIO/rva-2026-1-uap.git

# Ver estado de los archivos
git status

# Ver cambios
git diff

# Agregar archivos al staging
git add archivo.md
git add .  # todos los archivos (cuidado)

# Hacer commit
git commit -m "S05: guia de trabajo lab completada"

# Subir al repositorio remoto
git push origin TU-RAMA

# Actualizar desde el repositorio del docente
git pull upstream main
```

## 5. Flujo de trabajo del curso

```
1. Fork del repo del curso → en tu cuenta de GitHub
2. Clone → en tu computadora
3. Crear rama → git checkout -b u-TU-CODIGO
4. Hacer trabajo → editar archivos
5. Commit → git commit -m "mensaje descriptivo"
6. Push → git push origin u-TU-CODIGO
7. Pull Request → en GitHub
8. El docente revisa y aprueba
```

## 6. Configurar token de acceso (GitHub)

GitHub ya no acepta contraseña para push. Usar token:
1. GitHub → Settings → Developer Settings → Personal Access Tokens
2. "Generate new token (classic)"
3. Seleccionar `repo` scope
4. Copiar el token → usarlo como contraseña al hacer push
