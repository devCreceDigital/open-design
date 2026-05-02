# Agent Rule: Open Design Local + Vercel Deployment

## Objetivo

1. Abrir el proyecto open-source `open-design` de nexu-io en máquina local.
2. Dejar corriendo el servidor local y proporcionar la URL.
3. Preparar y ejecutar un despliegue del web layer en Vercel usando la CLI.

## Suposiciones de Entorno

- Entorno tipo Linux/macOS con bash/zsh (o Windows con PowerShell/cmd).
- Node, git, y CLI de Vercel instalados.
- NO usar `sudo` ni modificar archivos fuera de `~/Projects/open-design`.

## Reglas Generales

- **Ejecutar UNA operación a la vez.** Después de cada comando importante:
  - Resumir qué se hizo.
  - Indicar el comando exacto ejecutado.
  - Copiar el output relevante o el error si algo falló.
- **Si un comando falla, DETENER.** No inventar pasos. Explicar:
  - Qué comando falló.
  - El mensaje de error.
  - 1–2 opciones concretas para solucionarlo, esperando confirmación antes de ejecutarlas.
- NO borrar ni modificar archivos fuera de `~/Projects/open-design`.
- Si se asume algo (ej. carpeta `~/Projects`), ejecutarlo yavisar claramente qué se asumió.
- Si se requiere iniciar sesión (ej. Vercel CLI) y no está logueado, DETENER y especificar el comando que el usuario debe ejecutar.

---

## FASE 1: Verificación de Entorno

### 1. Verificar Node
```bash
node --version
```
- **Si NO es 24.x:** NO intentar cambiar la versión. Decir exactamente el comando para instalar Node 24 con nvm o mise.

### 2. Verificar Git
```bash
git --version
```
- Si no está instalado, detener y dar comando de instalación según el sistema.

### 3. Verificar Corepack
```bash
corepack --version
```
- Si falla: explicar qué es y el comando para activarlo (`corepack enable`), ejecutar solo si el usuario está de acuerdo.

### 4. Verificar pnpm vía Corepack
```bash
corepack pnpm --version
```
- La versión debe ser 10.33.x.
- Si falla, proponer cómo ajustar sin ejecutar.

---

## FASE 2: Revisar Repositorio

### 5. Asegurar carpeta de trabajo
```bash
cd ~/Projects
```

### 6. Confirmar ubicación
```bash
pwd
ls
```
Verificar que contiene `package.json`, estructura del proyecto, etc.

---

## FASE 3: Instalación de Dependencias

### 7. Activar corepack (si aplica)
```bash
corepack enable
corepack pnpm --version
```

### 8. Instalar dependencias
```bash
pnpm install
```
- Puede tardar varios minutos.
- Si hay errores, DETENER, copiar error completo, proponer soluciones sin ejecutar.

---

## FASE 4: Levantar Servidor Local

### 9. Arrancar daemon + web
```bash
pnpm tools-dev run web
```
- Aclarar si corre en foreground o background.
- Cuando imprima la URL (ej. `web → http://localhost:3000`), copiarla exactamente.
- Indicar que esa es la URL para abrir en navegador.

### 10. (Opcional) Abrir URL en navegador
- macOS: `open <url>`
- Linux: `xdg-open <url>`
- Solo ejecutar si se está seguro del sistema. Si hay dudas, preguntar antes.

### 11. Confirmar en logs detección de CLIs de agentes
- Si hay línea indicando detección de agentes (Claude Code, Cursor, etc.), reportar.
- Si no detecta nada, explicar cómo configurarlo más adelante (sin cambiar nada ahora).

### Resumen de fase 4
Dar resumen corto:
- Ruta del proyecto
- Comando para volver a levantar
- URL local

---

## FASE 5: Deployment en Vercel (Web Layer)

### 12. Verificar CLI de Vercel
```bash
vercel --version
```
- Si falla: indicar cómo instalarla (`npm install -g vercel`), marcar con [VERIFICAR], no instalar sin permiso.
- Indicar que necesita ejecutar `vercel login` si no está autenticado.

### 13. Ubicación del proyecto
```bash
cd ~/Projects/open-design
```

### 14. Primera configuración de proyecto en Vercel
```bash
vercel
```
- **ACEPTAR valores por defecto** a menos que el README del proyecto indique explícitamente algo distinto.
- Si pregunta por:
  - Nombre del proyecto: usar `open-design`.
  - Directorio de salida: usar valor por defecto detectado. Si no está seguro, detener y preguntar.
  - Comando de build: usar valor por defecto. NO inventar nuevo comando sin confirmación.
  - NO cambiar variables de entorno sin consultar.

### 15. Despliegue de producción
```bash
vercel --prod
```
- Cuando termine, dar:
  - URL de preview
  - URL de producción (si son distintas, aclarar)

---

## FASE 6: Resumen Final

Dar resumen estructurado de no más de 10 líneas:

- **[LOCAL]**
  - Ruta del proyecto en máquina
  - Comando para arrancar servidor local
  - URL local

- **[VERCEL]**
  - Comando para redeployar (ej. `vercel --prod` desde `~/Projects/open-design`)
  - URL de producción
  - Decisiones tomadas durante `vercel init` (framework detectado, build command, output dir), marcar claramente las asumidas.

---

## Regla de Prioridad

Si en cualquier momento algo no cuadra con la documentación del repo (README indica instrucción diferente para build o directorio de salida), **PRIORIZAR sobre valores por defecto de Vercel** y explicar antes de aplicar el cambio.