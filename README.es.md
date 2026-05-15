# DeepSeek Obsidian Skills

> Skills de agente para Obsidian, empaquetados y documentados para [DeepSeek TUI](https://github.com/deepseek-tui/deepseek-tui).

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![DeepSeek TUI](https://img.shields.io/badge/DeepSeek%20TUI-%3E%3D0.8.x-blue)](https://github.com/deepseek-tui/deepseek-tui)
[![Obsidian](https://img.shields.io/badge/Obsidian-%3E%3D1.5-purple)](https://obsidian.md)

[English](README.md) | [Español](README.es.md) | [中文](README.zh.md)

Estos skills permiten que DeepSeek TUI interactúe con tu bóveda de Obsidian — leer, crear, buscar, editar notas, gestionar tareas, inspeccionar propiedades e incluso desarrollar plugins — todo desde la terminal.

---

## Tabla de contenidos

- [¿Qué son los Skills de DeepSeek?](#qué-son-los-skills-de-deepseek)
- [Requisitos previos](#requisitos-previos)
- [Instalación](#instalación)
- [Skills disponibles](#skills-disponibles)
- [Verificación](#verificación)
- [Ejemplos de uso](#ejemplos-de-uso)
- [Limitaciones](#limitaciones)
- [Recomendaciones y buenas prácticas](#recomendaciones-y-buenas-prácticas)
- [Solución de problemas](#solución-de-problemas)
- [FAQ](#faq)
- [Contribuir](#contribuir)
- [Licencia](#licencia)
- [Créditos](#créditos)

---

## ¿Qué son los Skills de DeepSeek?

Los skills son archivos markdown que extienden las capacidades de DeepSeek TUI. Al instalarse, se inyectan en el prompt del sistema, proporcionando al agente de IA conocimiento preciso sobre herramientas, convenciones y flujos de trabajo específicos.

Los skills se almacenan en:

```
~/.deepseek/skills/<nombre-del-skill>/SKILL.md
```

DeepSeek TUI los descubre automáticamente al iniciar. Puedes invocar un skill mencionando su nombre en la conversación, o se activa automáticamente cuando tu solicitud coincide con su descripción.

> Más información: [Documentación de Skills de DeepSeek TUI](https://github.com/deepseek-tui/deepseek-tui)

---

## Requisitos previos

### Obligatorios

| Requisito | Versión mínima | Notas |
|-----------|----------------|-------|
| **DeepSeek TUI** | 0.8.x o superior | El soporte de skills se añadió en 0.8.x |
| **Obsidian Desktop** | 1.5.0 o superior | Debe estar **abierto y en ejecución** al usar el skill |
| **Obsidian CLI** | Incluido con Obsidian | El binario `obsidian` debe estar en tu PATH |

### Opcionales

| Paquete | Propósito |
|---------|-----------|
| Node.js 18+ | Necesario para algunos comandos `obsidian dev:*` |
| Git | Útil para versionar la bóveda junto con el uso de CLI |

### Soporte de sistemas operativos

| SO | Soportado | Notas |
|----|-----------|-------|
| **macOS** | ✅ Soporte completo | Ruta CLI: `/Applications/Obsidian.app/Contents/Resources/` |
| **Windows** | ✅ Soporte completo | CLI incluido con el instalador |
| **Linux** | ✅ Soporte completo | AppImage puede requerir extracción para acceder al CLI |
| **Headless / Servidor** | ❌ No soportado | Obsidian requiere un entorno de escritorio gráfico |

---

## Instalación

### Método 1: Via skill-installer (recomendado)

Si tienes el skill `skill-installer` habilitado en DeepSeek TUI:

```
/skill install github:DiegoMartinez-Git/deepseek-obsidian-cli
```

O desde una conversación:

> "Instala los skills de deepseek-obsidian-cli desde GitHub"

Esto clona el repositorio y copia los skills en `~/.deepseek/skills/`.

### Método 2: Manual — clonar el repositorio

```bash
# Clonar en una ubicación temporal
git clone https://github.com/DiegoMartinez-Git/deepseek-obsidian-cli.git /tmp/deepseek-obsidian-cli

# Copiar el/los skill(s) que quieras
cp -r /tmp/deepseek-obsidian-cli/skills/obsidian-cli ~/.deepseek/skills/obsidian-cli

# Limpiar
rm -rf /tmp/deepseek-obsidian-cli
```

### Método 3: Manual — archivo único

Si solo necesitas `obsidian-cli`:

```bash
mkdir -p ~/.deepseek/skills/obsidian-cli/references
curl -o ~/.deepseek/skills/obsidian-cli/SKILL.md \
  https://raw.githubusercontent.com/DiegoMartinez-Git/deepseek-obsidian-cli/main/skills/obsidian-cli/SKILL.md
curl -o ~/.deepseek/skills/obsidian-cli/references/COMMANDS.md \
  https://raw.githubusercontent.com/DiegoMartinez-Git/deepseek-obsidian-cli/main/skills/obsidian-cli/references/COMMANDS.md
```

### Configurar el CLI de Obsidian

Después de instalar el skill, asegúrate de que el comando `obsidian` sea accesible:

**macOS:**
```bash
echo 'export PATH="/Applications/Obsidian.app/Contents/Resources:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Linux (AppImage):**
```bash
# Extraer la AppImage (una sola vez)
/path/to/Obsidian-*.AppImage --appimage-extract
# El CLI estará en squashfs-root/usr/bin/obsidian
sudo cp squashfs-root/usr/bin/obsidian /usr/local/bin/
obsidian --version
```

**Windows (PowerShell):**
```powershell
[Environment]::SetEnvironmentVariable(
  "Path",
  "$env:Path;C:\Users\$env:USERNAME\AppData\Local\Programs\Obsidian",
  "User"
)
```

---

## Skills disponibles

| Skill | Descripción | Requiere |
|-------|-------------|----------|
| [obsidian-cli](skills/obsidian-cli) | Leer, crear, buscar y gestionar notas; notas diarias, tareas, propiedades, etiquetas, backlinks; desarrollo de plugins y temas con recarga en caliente, inspección del DOM, capturas de pantalla y ejecución de JS | Obsidian Desktop en ejecución |

Próximamente más skills — contribuciones bienvenidas.

---

## Verificación

Después de la instalación, verifica que todo funcione:

### 1. Comprobar que el archivo del skill existe

```bash
ls -la ~/.deepseek/skills/obsidian-cli/SKILL.md
# Debería mostrar el archivo con tamaño > 1KB
```

### 2. Comprobar que el CLI de Obsidian es accesible

```bash
obsidian --version
# Debería imprimir un número de versión
```

### 3. Prueba rápida

Con Obsidian abierto y una bóveda activa:

```bash
obsidian search query="test" limit=1
# Debería devolver resultados o "No results found" (ambos son válidos)
```

### 4. Verificar en DeepSeek TUI

Inicia una nueva sesión de DeepSeek TUI. El skill debería aparecer en `## Skills` en el prompt del sistema. Pregunta:

> "¿Qué skills tienes disponibles?"

`obsidian-cli` debería aparecer en la lista.

---

## Ejemplos de uso

Una vez instalado, simplemente habla de forma natural con DeepSeek TUI. El skill se activa automáticamente:

### Gestión de notas

> "Lee mi nota llamada Proyecto Alpha"
> → Ejecuta: `obsidian read file="Proyecto Alpha"`

> "Crea una nueva nota titulada Notas de reunión con la fecha de hoy en el contenido"
> → Ejecuta: `obsidian create name="Notas de reunión" content="..."`

### Notas diarias y tareas

> "¿Qué tareas tengo pendientes hoy?"
> → Ejecuta: `obsidian tasks daily todo`

> "Añade una tarea a mi nota diaria: revisar el PR"
> → Ejecuta: `obsidian daily:append content="- [ ] revisar el PR"`

### Búsqueda y navegación

> "Encuentra todas las notas que mencionen la palabra presupuesto en mi bóveda Trabajo"
> → Ejecuta: `obsidian vault="Trabajo" search query="presupuesto"`

> "¿Cuáles son mis etiquetas más usadas?"
> → Ejecuta: `obsidian tags sort=count counts`

### Desarrollo de plugins

> "Recarga mi plugin llamado my-theme y revisa si hay errores"
> → Ejecuta: `obsidian plugin:reload id=my-theme` luego `obsidian dev:errors`

> "Toma una captura de pantalla de la UI de mi plugin"
> → Ejecuta: `obsidian dev:screenshot path=captura.png`

---

## Limitaciones

### 🔴 Crítico — Sin soporte headless

Obsidian es una aplicación de escritorio (basada en Electron). El CLI se comunica con un **proceso de Obsidian en ejecución**. Esto significa:

- ❌ NO funciona en servidores, contenedores o VMs headless
- ❌ NO funciona sobre SSH sin reenvío X
- ❌ NO funciona en pipelines CI/CD
- ❌ NO funciona en Raspberry Pi sin entorno de escritorio

Si necesitas gestionar una bóveda sin interfaz gráfica, considera:
- Editar directamente los archivos `.md` en el directorio de la bóveda
- Usar Git para versionar la bóveda
- API de Obsidian Sync (requiere suscripción)

### 🟡 Peculiaridades por plataforma

- **Linux**: AppImage requiere `--appimage-extract` para acceder al CLI; Wayland puede causar problemas de renderizado con algunos comandos `dev:*`
- **macOS**: Gatekeeper puede bloquear el CLI en la primera ejecución; permitirlo en Preferencias del Sistema > Seguridad
- **Windows**: La ruta del CLI difiere entre la versión de MS Store y el instalador directo

### 🟡 A veces requiere que Obsidian tenga el foco

Algunos comandos (especialmente `dev:screenshot`) necesitan que Obsidian sea la ventana activa. Si otra aplicación tiene el foco, los resultados pueden ser inconsistentes.

### 🟡 Los comandos de desarrollo de plugins requieren el plugin

`obsidian dev:errors`, `dev:console` y `dev:dom` solo funcionan para plugins **que estés desarrollando**. No puedes inspeccionar el interior de plugins de terceros.

### 🟢 Sin límites de tasa de API

A diferencia de las APIs en la nube, el CLI de Obsidian no tiene límites de tasa — es local. Sin embargo, llamadas excesivas a `search` o `dev:dom` pueden afectar la capacidad de respuesta de Obsidian.

---

## Recomendaciones y buenas prácticas

### Para la gestión de notas diarias

- Usa el flag `silent` para evitar cambios de ventana de Obsidian al crear notas en lote
- Prefiere `daily:append` sobre `daily:read` + edición manual para registrar tareas
- Agrupa múltiples anexos en un solo comando con separador `\n`

### Para el desarrollo de plugins

- Sigue siempre el ciclo: **recargar → errores → captura → consola**
- Guarda las capturas en una carpeta `screenshots/` dentro de tu bóveda para referencia fácil
- Usa `dev:console level=error` antes de publicar para detectar problemas ocultos
- El comando `eval` es potente — envuelve JS complejo en una sola línea, o usa un archivo `.js` y cárgalo

### Seguridad

- El comando `obsidian eval code="..."` ejecuta JavaScript arbitrario en el contexto de la app de Obsidian — solo úsalo con código de confianza
- DeepSeek TUI puede sugerir comandos `eval`; siempre revísalos antes de ejecutarlos en bóvedas sensibles
- Mantén tu instalación de Obsidian actualizada para los parches de seguridad

### Rendimiento

- Limita `search` con `limit=` para evitar miles de resultados en la transcripción
- Usa el flag `total` cuando solo necesites conteos
- Cierra bóvedas no utilizadas para reducir el uso de memoria

### Organización de la bóveda

- Estandariza las propiedades del frontmatter (`status`, `type`, `tags`) para que `property:set` y `property:get` sean más útiles
- Usa nombres de notas consistentes para mejor resolución con `file=`
- Enlaza notas con wikilinks — `backlinks` y `outgoing-links` dependen de ellos

---

## Solución de problemas

### `obsidian: command not found`

El CLI no está en tu PATH. Comprueba tu configuración de PATH:

```bash
echo $PATH | tr ':' '\n' | grep -i obsidian
```

Si no aparece nada, vuelve a añadirlo (ver [Configurar el CLI de Obsidian](#configurar-el-cli-de-obsidian)).

### Los comandos se cuelgan o agotan el tiempo de espera

Esto suele significar que Obsidian no está en ejecución o no responde:

1. Verifica que Obsidian esté abierto (revisa tu barra de tareas/dock)
2. Intenta hacer clic en la ventana de Obsidian para darle el foco
3. Si el problema persiste, reinicia Obsidian

### `No vault found` o `No active file`

Puede deberse a:
- No hay ninguna bóveda abierta en Obsidian (abre una primero)
- Necesitas especificar la bóveda explícitamente: `vault="Mi Bóveda"`
- El archivo activo se cerró; usa `file=` o `path=` explícitamente

### Errores de permisos en Linux (AppImage)

```bash
# Si la extracción de AppImage falla:
chmod +x Obsidian-*.AppImage
./Obsidian-*.AppImage --appimage-extract

# Si /usr/local/bin/obsidian da permiso denegado:
sudo chmod +x /usr/local/bin/obsidian
```

### El skill no aparece en DeepSeek TUI

1. Verifica que el archivo existe en `~/.deepseek/skills/obsidian-cli/SKILL.md`
2. Comprueba que el archivo tenga frontmatter YAML válido con `name` y `description`
3. Reinicia DeepSeek TUI completamente (no solo `/compact`)
4. Revisa que `~/.deepseek/config.toml` no tenga `obsidian-cli` en una lista de deshabilitados

### Gatekeeper de macOS bloquea el CLI

```bash
# Si aparece una advertencia de seguridad al ejecutar obsidian:
xattr -d com.apple.quarantine /Applications/Obsidian.app
```

---

## FAQ

**P: ¿Puedo usar esto en múltiples bóvedas?**
R: Sí. Usa `vault="Nombre de la Bóveda"` como primer parámetro en cualquier comando.

**P: ¿Funciona con Obsidian Sync?**
R: El CLI opera sobre archivos locales. Las bóvedas sincronizadas funcionan, pero el CLI no interactúa directamente con la API de Obsidian Sync.

**P: ¿Puedo programar la creación automatizada de notas?**
R: Sí, pero Obsidian debe estar en ejecución a la hora programada. Usa cron/systemd timers con precaución — fallarán silenciosamente si Obsidian no está abierto.

**P: ¿Puedo contribuir con skills adicionales de Obsidian?**
R: Por supuesto. Consulta [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Contribuir

Las contribuciones son bienvenidas. Consulta [CONTRIBUTING.md](CONTRIBUTING.md) para las pautas.

## Licencia

MIT — consulta [LICENSE](LICENSE) para más detalles.

## Créditos

- [Especificación de Agent Skills](https://agentskills.io) por Anthropic
- [Obsidian](https://obsidian.md) por el equipo de Obsidian
- [DeepSeek TUI](https://github.com/deepseek-tui/deepseek-tui)
