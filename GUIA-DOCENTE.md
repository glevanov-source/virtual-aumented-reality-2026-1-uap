
# 🎓 Guía para el Docente — Gestión del Repositorio del Curso

## Realidad Virtual y Aumentada | PSISP08075 | 2026-1

---

## Configuración inicial del repositorio

### 1. Crear el repositorio en GitHub

```bash
# Opción A: Desde GitHub CLI
gh repo create rva-2026-1-uap \
  --public \
  --description "Realidad Virtual y Aumentada PSISP08075 - UAP 2026-1" \
  --clone

# Opción B: Desde la web
# → github.com → New repository → rva-2026-1-uap → Public → Create
```

### 2. Clonar el repositorio localmente

```bash
git clone https://github.com/RubenCarty/rva-2026-1-uap.git
cd rva-2026-1-uap
```

### 3. Subir el contenido inicial

```bash
git status
git add .
git commit -m "feat: estructura inicial del curso RVA 2026-1"
git push origin main
```

### 4. Configurar protección de rama main

En GitHub → Settings → Branches → Add rule:
- Branch name pattern: `main`
- ✅ Require a pull request before merging
- ✅ Require approvals: 1
- ✅ Dismiss stale pull request approvals

---

## Flujo semanal del docente

### Antes de cada clase (24h antes):

```bash
# 1. Ir a la carpeta del repo local
cd ~/rva-2026-1-uap

# 2. Verificar que estás en main y actualizado
git checkout main
git status

# 3. Agregar los materiales de la semana siguiente
# (sesion, guia-trabajo, laboratorio)
git add semana-XX/
git commit -m "feat(s{N}): agregar materiales semana {N} - {tema}"

# 4. Subir al repositorio remoto
git push origin main

# Ahora los estudiantes pueden descargar con:
# git pull upstream main
```

### Después de cada clase (revisar PRs):

```bash
# Ver PRs pendientes en GitHub
# → github.com/RubenCarty/rva-2026-1-uap/pulls

# Para cada PR de estudiante:
# 1. Revisar archivos en la pestaña "Files changed"
# 2. Agregar comentarios en líneas específicas si hay correcciones
# 3. Aprobar o solicitar cambios
# 4. Opcionalmente hacer merge (o dejarlo como evidencia)
```

---

## Comandos Git esenciales para el docente

```bash
# ── Ver el estado completo del repo ──
git status
git log --oneline --all --graph

# ── Actualizar local con remoto ──
git pull origin main
# Explicación: trae los últimos cambios del servidor a tu máquina

# ── Ver ramas de estudiantes ──
git branch -r
# Muestra todas las ramas remotas (incluyendo las de los estudiantes)

# ── Traer una rama de estudiante para revisar ──
git fetch origin u-20221234-semana-01
git checkout u-20221234-semana-01
# Ahora puedes ver su trabajo en tu editor local

# ── Volver a main ──
git checkout main

# ── Agregar materiales nuevos ──
git add semana-10/
git commit -m "feat(s10): assets 3D y multimedia - guías y laboratorio"
git push origin main

# ── Crear una etiqueta de entrega (milestone) ──
git tag -a "semana-09-cierre" -m "Cierre de entregas Semana 9"
git push origin semana-09-cierre

# ── Ver diferencias entre semanas ──
git diff HEAD~1 HEAD --name-only

# ── Revertir un archivo si cometiste un error ──
git checkout HEAD -- semana-10/sesion/S10_Sesion.md
```

---

## Revisar y aprobar PRs de estudiantes

### Checklist de revisión por PR:

```markdown
Al revisar el PR de un estudiante verificar:

□ 1. La rama tiene el formato correcto: u-CODIGO-semana-XX
□ 2. Solo modifica archivos en entregas/SEMANA-XX/u-CODIGO/
□ 3. GT completado con todas las preguntas respondidas
□ 4. LAB con capturas de pantalla de Unity adjuntas
□ 5. Código C# funcional (si aplica al laboratorio)
□ 6. Commit messages descriptivos con nombre y código
□ 7. No modifica archivos del docente (sesion/, guia-trabajo/ original)
```

### Comandos para revisar un PR:

```bash
# Traer la rama del estudiante
git fetch origin
git checkout -b review/u-20221234-semana-01 origin/u-20221234-semana-01

# Revisar cambios
git diff main...review/u-20221234-semana-01

# Ver archivos específicos
cat entregas/SEMANA-01/u-20221234/GT01_completado.md

# Volver a main
git checkout main
git branch -d review/u-20221234-semana-01
```

---

## Estructura de ramas esperada

```
main                          ← Materiales del docente (solo docente modifica)
│
├── u-20221001-semana-01     ← Rama del estudiante 1, semana 1
├── u-20221001-semana-02     ← Rama del estudiante 1, semana 2
├── u-20221002-semana-01     ← Rama del estudiante 2, semana 1
├── u-20221003-semana-01     ← Rama del estudiante 3, semana 1
...
└── u-20224600-semana-16     ← Rama del estudiante 46, semana 16
```

---

## Actualización semanal — Comandos completos

### Semana nueva disponible (script de actualización):

```bash
#!/bin/bash
# uso: ./actualizar_semana.sh 10

SEMANA=$1
echo "Subiendo materiales de la Semana $SEMANA..."

cd ~/rva-2026-1-uap

git checkout main
git pull origin main

git add semana-$(printf "%02d" $SEMANA)/
git status

read -p "¿Confirmar commit? (s/n): " CONFIRM
if [ "$CONFIRM" = "s" ]; then
    git commit -m "feat(s$SEMANA): materiales semana $SEMANA disponibles"
    git push origin main
    echo "✓ Semana $SEMANA publicada para los estudiantes"
fi
```

---

## Hitos del proyecto (Milestones en GitHub)

Configurar en GitHub → Issues → Milestones:

| Hito | Fecha | Semanas |
|------|-------|---------|
| Hito 1 - Kickoff | S4 | S1-S3 |
| Hito 2 - EP (30% rúbrica) | S8 | S5-S8 |
| Hito 3 - EF (100% rúbrica) | S16 | S9-S16 |

---

## Monitoreo de participación

```bash
# Contar PRs abiertos
gh pr list --state open --json number,title,author | jq length

# Ver PRs sin revisar
gh pr list --state open --label "sin-revisar"

# Estudiantes que no han hecho PR esta semana
# Comparar la lista de forks con los PRs abiertos
gh pr list --state all --json author --jq '.[].author.login' | sort | uniq
```

---

*MSc. Ruben Quispe Llacctarimay | Universidad Autónoma del Perú | 2026-1*
