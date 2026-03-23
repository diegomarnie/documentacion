# Guía completa de instalación de OpenClaw

## ¿Qué es OpenClaw exactamente?

OpenClaw es un agente de IA personal de código abierto que se ejecuta directamente en tu propia máquina, actuando como puerta de enlace entre modelos de IA y apps de mensajería del día a día.

No es un chatbot que simplemente responde preguntas. Ejecuta acciones, gestiona ficheros, lanza tareas programadas y organiza flujos de trabajo complejos de forma autónoma.

Como persona que viene del mundo web, una analogía útil: es como tener un servidor Node.js corriendo en tu máquina que recibe "peticiones" por WhatsApp en vez de por HTTP, y que tiene acceso a tu sistema de ficheros y terminal.

El proyecto empezó como "Clawdbot" en noviembre de 2025, pasó brevemente a llamarse "Moltbot" el 27 de enero de 2026 por problemas de marca registrada, y relanzó oficialmente como OpenClaw tres días después. Es muy nuevo — pero ya tiene más de 100.000 estrellas en GitHub.

---

## Requisitos del sistema

Antes de instalar nada, comprueba que tienes lo necesario. Como desarrollador web, seguramente ya tienes Node.js instalado, pero puede que en una versión antigua.

OpenClaw requiere Node.js versión 22.16 o superior, con la versión 24 recomendada como runtime preferido.

| Componente | Mínimo | Recomendado |
|---|---|---|
| SO | macOS 12, Ubuntu 20.04, Windows 11 (WSL2) | macOS 14+, Ubuntu 22.04/24.04 |
| Node.js | v22.16+ | v24.x |
| RAM | 4 GB (solo modelos cloud) | 8 GB+ (para IA local con Ollama) |
| Disco | 1 GB | 5 GB+ |

Comprueba tu estado actual con estos comandos en la terminal:

```bash
node --version   # Necesitas v22.x o superior
npm --version    # Necesitas 9.x o superior
git --version    # Cualquier versión reciente sirve
```

> **⚠️ Importante para Windows:** La instalación nativa en Windows no está soportada. OpenClaw depende de herramientas compatibles con POSIX y gestión de procesos basada en Unix. WSL2 proporciona exactamente ese entorno. Si estás en Windows, ve directamente a la sección de WSL2 más abajo.

---

## Instalación en macOS (5 pasos)

### Paso 1 — Instalar Node.js con NVM

NVM (Node Version Manager) es una herramienta que te permite tener varias versiones de Node.js instaladas y cambiar entre ellas. ¿Por qué NVM y no instalar Node.js directamente? NVM evita errores de permisos y permite cambiar entre versiones de Node sin tocar los directorios del sistema.

```bash
# Instalar NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# Recargar la configuración del shell
source ~/.zshrc   # Si usas zsh (macOS moderno)
# o
source ~/.bash_profile   # Si usas bash

# Verificar que NVM funciona
nvm --version

# Instalar Node.js 24 (la versión recomendada)
nvm install 24
nvm use 24
nvm alias default 24

# Verificar
node --version   # Debe mostrar v24.x.x
```

### Paso 2 — Instalar OpenClaw

Tienes dos opciones. La primera (recomendada) lanza un asistente de configuración interactivo:

```bash
curl -fsSL https://openclaw.ai/install.sh | sh
```

La segunda es más directa:

```bash
npm install -g openclaw@latest
```

### Paso 3 — Ejecutar el asistente de configuración

```bash
openclaw onboard --install-daemon
```

Este comando:
- Te pregunta qué modelo de IA quieres usar (Claude, GPT, Gemini, o Ollama local)
- Te pide la API key del proveedor que elijas
- Te pregunta qué app de mensajería conectar
- Instala OpenClaw como un daemon (proceso en segundo plano que arranca automáticamente al encender el ordenador)

### Paso 4 — Verificar que todo funciona

```bash
openclaw --version   # Muestra la versión instalada
openclaw status      # Muestra si el agente está corriendo
```

Salida esperada de `openclaw status`:

```
OpenClaw v1.x.x running
Model: claude-4-sonnet
Channels: telegram (connected)
Uptime: 0d 0h 2m
```

### Paso 5 — Problema con Gatekeeper de macOS (si aparece)

Si macOS te bloquea el binario con un mensaje de "desarrollador no identificado":

```bash
xattr -d com.apple.quarantine $(which openclaw)
```

---

## Instalación en Linux (Ubuntu/Debian)

### Paso 1 — Prerequisitos del sistema

```bash
sudo apt update && sudo apt install -y curl git build-essential
```

> `build-essential` incluye compiladores de C que algunos paquetes de npm necesitan para compilar código nativo (como `node-gyp`).

### Paso 2 — NVM y Node.js 24

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc

nvm install 24
nvm use 24
nvm alias default 24

node --version   # v24.x.x
npm --version    # 10.x o superior
```

### Paso 3 — Instalar OpenClaw

```bash
# Método recomendado (con asistente)
curl -fsSL https://openclaw.ai/install.sh | sh

# O directamente con npm
npm install -g openclaw@latest
```

### Paso 4 — Onboarding y arrancar el daemon

```bash
openclaw onboard --install-daemon
```

### Paso 5 (Opcional) — Configurar como servicio systemd

Para servidores sin interfaz gráfica, que OpenClaw sobreviva a reinicios:

```bash
sudo nano /etc/systemd/system/openclaw.service
```

Contenido del fichero (cambia `your_username` y la ruta de Node por las tuyas):

```ini
[Unit]
Description=OpenClaw AI Agent Daemon
After=network.target

[Service]
Type=simple
User=your_username
ExecStart=/home/your_username/.nvm/versions/node/v24.x.x/bin/openclaw start
Restart=always
RestartSec=5
Environment="NODE_ENV=production"

[Install]
WantedBy=multi-user.target
```

Activar y arrancar:

```bash
sudo systemctl daemon-reload
sudo systemctl enable openclaw    # Que arranque en cada reinicio
sudo systemctl start openclaw
sudo systemctl status openclaw    # Verificar que está corriendo
```

---

## Instalación en Windows con WSL2

### Habilitar WSL2

**Paso 1:** Abre PowerShell como Administrador

**Paso 2:** Ejecuta:

```powershell
wsl --install
```

Esto instala WSL2 y descarga Ubuntu 24.04 automáticamente. Tarda 3-5 minutos.

**Paso 3:** Reinicia el ordenador cuando te lo pida.

**Paso 4:** Tras reiniciar, se abre una terminal de Ubuntu. Crea tu usuario y contraseña de Linux (independientes del usuario de Windows).

**Paso 5:** Verifica en PowerShell:

```powershell
wsl --version
wsl -l -v   # Debe mostrar Ubuntu corriendo en WSL Version 2
```

### Instalar Node.js y OpenClaw dentro de WSL2

A partir de aquí, todo se hace dentro de la terminal de Ubuntu:

```bash
# Actualizar Ubuntu
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl git build-essential

# Instalar NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc

# Node 24 y OpenClaw
nvm install 24
nvm use 24
nvm alias default 24
npm install -g openclaw@latest

# Configurar
openclaw onboard --install-daemon
```

### Problemas habituales en WSL2

**DNS que no resuelve** (error `Could not resolve host` al hacer npm install):

```bash
sudo rm /etc/resolv.conf
sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
sudo bash -c 'echo "nameserver 1.1.1.1" >> /etc/resolv.conf'
sudo bash -c 'echo "[network]" >> /etc/wsl.conf'
sudo bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
```

**Rendimiento lento del sistema de ficheros:** Guarda tus proyectos dentro del sistema de ficheros de WSL (`~/openclaw-data/`), no en la unidad de Windows montada (`/mnt/c/`).

---

## Instalaciones alternativas

### Docker

```bash
docker pull openclaw/openclaw:latest

docker run -d \
  --name openclaw \
  --restart unless-stopped \
  -e ANTHROPIC_API_KEY=tu-api-key \
  -e TELEGRAM_BOT_TOKEN=tu-token \
  -e TELEGRAM_CHAT_ID=tu-chat-id \
  -v ~/.openclaw:/root/.openclaw \
  openclaw/openclaw:latest
```

Verificar estado:

```bash
docker ps
docker logs openclaw --tail 50
```

### fnm (alternativa más rápida a NVM)

`fnm` (Fast Node Manager) es un gestor de versiones de Node basado en Rust, 35 veces más rápido que NVM:

```bash
# macOS/Linux
curl -fsSL https://fnm.vercel.app/install | bash
source ~/.bashrc

fnm install 24
fnm use 24
fnm default 24

npm install -g openclaw@latest
```

---

## Conectar OpenClaw a un modelo de IA y a una app de mensajería

La configuración vive en dos ficheros:
- `~/.openclaw/.env` → secretos (API keys, tokens)
- `~/.openclaw/config.yaml` → comportamiento del agente

### Opción A: Telegram (la más fácil para empezar)

**1. Crear un bot de Telegram:**
- Busca `@BotFather` en Telegram
- Envía `/newbot`, sigue las instrucciones
- BotFather te da un token como: `7312654890:AAF3kLl...`

**2. Conseguir tu Chat ID:**
- Busca `@userinfobot` en Telegram y mándale cualquier mensaje

**3. Editar el fichero `.env`:**

```bash
cd ~/.openclaw
nano .env
```

```env
# Modelo de IA
OPENCLAW_MODEL=claude-4-sonnet
ANTHROPIC_API_KEY=sk-ant-tu-clave-aqui

# Telegram
TELEGRAM_BOT_TOKEN=7312654890:AAF3kLl...
TELEGRAM_CHAT_ID=123456789
```

**4. Reiniciar:**

```bash
openclaw restart
```

### Opción B: Ollama (IA completamente local, sin coste de API)

```bash
# Instalar Ollama
curl -fsSL https://ollama.ai/install.sh | sh

# Descargar un modelo (~6 GB de RAM necesarios)
ollama pull llama4:8b-scout

# Verificar que Ollama corre
ollama serve &
ollama list
```

Configurar OpenClaw para usar Ollama en `.env`:

```env
OPENCLAW_MODEL=ollama/llama4:8b-scout
OPENCLAW_BASE_URL=http://localhost:11434
```

```bash
openclaw restart
```

### Opción C: WhatsApp

```bash
openclaw configure --channel whatsapp
```

Aparece un QR en la terminal. Ábrelo con WhatsApp → **Ajustes → Dispositivos vinculados → Vincular un dispositivo**.

```env
WHATSAPP_DM_POLICY=allowlist
WHATSAPP_ALLOW_FROM=+34612345678
```

### Opción D: Discord

1. Ve a [discord.com/developers/applications](https://discord.com/developers/applications) → New Application
2. Bot → Add Bot → copia el Token
3. Activa **Message Content Intent** en Privileged Gateway Intents
4. Invita el bot al servidor con permisos: Send Messages, Read Message History, View Channels
5. Copia el ID del canal (Discord en modo Developer → clic derecho en canal → Copiar ID)

```env
DISCORD_BOT_TOKEN=MTMwNjkx...tu-token
DISCORD_CHANNEL_ID=1234567890123456789
```

### Opción E: iMessage (solo macOS)

1. Ajustes del Sistema → Privacidad y Seguridad → Accesibilidad → añade Terminal

```env
IMESSAGE_ENABLED=true
IMESSAGE_ALLOWED_SENDERS=+34612345678
```

---

## ClawHub — el "npm" de las skills de OpenClaw

ClawHub es el repositorio de plugins de OpenClaw, comparable a npm para Node.js.

```bash
# Instalar el CLI de ClawHub
npm install -g clawhub@latest

# Buscar skills
clawhub search "calendar"
clawhub search "postgres"

# Instalar una skill
clawhub install username/skill-name

# Listar las instaladas
clawhub list

# Actualizar todas
clawhub update --all
```

> **⚠️ Seguridad:** Las skills ejecutan código con los permisos completos del sistema. Siempre revisa con `clawhub show username/skill-name` antes de instalar skills de autores desconocidos.

### Crear una skill personalizada

```bash
nano ~/.openclaw/workspace/skills/mi-skill.md
```

```yaml
---
name: daily-standup
description: Manda un resumen de standup a Telegram a las 9am
triggers:
  - schedule: "0 9 * * 1-5"   # Lunes a viernes a las 9
  - phrase: "standup"
---

Recopila el estado de estas fuentes y compón un standup:
- Revisa ~/proyectos/ por ficheros modificados en las últimas 24h
- Resume los commits de git de ayer en todos los repos de ~/code/

Formato: bullet points breves — tres secciones: Hecho / Hoy / Bloqueantes
```

---

## Configuración avanzada (config.yaml y Dashboard)

```bash
nano ~/.openclaw/config.yaml
```

```yaml
# Modelo principal
defaultModel: claude-4-sonnet

# Modelo ligero para tareas simples (ahorra costes de API)
lightModel: ollama/llama4:8b-scout

# Servidor web local de OpenClaw
gateway:
  port: 18789         # Cambia si hay conflicto de puertos
  host: 127.0.0.1     # Usa 0.0.0.0 para exponer en la red local

# Memoria persistente entre sesiones
memory:
  enabled: true
  maxEntries: 1000

# Logs
log:
  level: info
  rotation: daily
  maxFiles: 7
```

### Dashboard web

```bash
openclaw dashboard
```

Accesible en `http://127.0.0.1:18789/`. Muestra estado del agente, tareas recientes, skills instaladas, memoria y logs en tiempo real.

### Hot model switching

Cambia de modelo para un mensaje concreto sin reconfigurar nada:

```
[use ollama/deepseek-coder] Revisa esta función de JavaScript y sugiere mejoras
```

---

## Diagnóstico con `openclaw doctor`

```bash
openclaw doctor
```

Salida de ejemplo:

```
✅ Node.js v24.2.0 — OK
✅ openclaw v1.8.3 — OK
✅ Daemon — running (PID 3847)
✅ Gateway — reachable at http://127.0.0.1:18789
✅ AI Model — claude-4-sonnet responding (latency: 412ms)
⚠️  Telegram — TELEGRAM_CHAT_ID not set in .env
❌ Ollama — connection refused at localhost:11434
```

Para verificar compatibilidad tras actualizar:

```bash
openclaw doctor --config
```

### Logs

```bash
openclaw logs --follow      # En tiempo real
openclaw logs --tail 100    # Últimas 100 líneas
openclaw logs --level error # Solo errores
```

---

## Los 10 errores más comunes y cómo arreglarlos

### Error 1: `node: version not supported`

Node.js está por debajo de v22.16.

```bash
nvm install 24 && nvm use 24
```

### Error 2: `EACCES: permission denied, mkdir`

Node.js fue instalado con `apt` o el instalador del sistema, no con NVM. **Nunca uses `sudo npm install -g`**. Reinstala Node con NVM.

### Error 3: `openclaw: command not found`

El directorio bin de npm global no está en el PATH:

```bash
export PATH="$PATH:$(npm root -g)/../.bin"
echo 'export PATH="$PATH:$(npm root -g)/../.bin"' >> ~/.zshrc
source ~/.zshrc
```

### Error 4: La instalación se queda colgada en `npm install`

```bash
npm cache clean --force
npm install -g openclaw@latest
```

### Error 5: Gatekeeper de macOS bloquea la ejecución

```bash
xattr -d com.apple.quarantine $(which openclaw)
```

### Error 6: DNS no resuelve en WSL2

Ver el fix de resolv.conf de la sección WSL2.

### Error 7: El bot de Telegram no responde

Comprueba el token en `.env` (sin espacios extra), verifica el chat ID con `@userinfobot`, y ejecuta:

```bash
openclaw status
openclaw start
openclaw logs --tail 50
```

### Error 8: `Ollama - connection refused`

```bash
ollama serve &
# En Linux, para arranque automático:
sudo systemctl enable ollama && sudo systemctl start ollama
```

### Error 9: Error de build de `sharp` en macOS

```bash
xcode-select --install
npm cache clean --force
npm install -g openclaw@latest --ignore-scripts
```

### Error 10: El gateway aparece como DOWN

Otro proceso usa el puerto 18789:

```bash
lsof -i :18789
```

Cambia el puerto en `config.yaml` → `gateway.port: 18800` y reinicia.

---

## Actualizar OpenClaw

```bash
npm install -g openclaw@latest
openclaw restart
openclaw doctor --config
```

## Desinstalar OpenClaw

```bash
openclaw stop
npm uninstall -g openclaw
rm -rf ~/.openclaw   # Opcional: borra toda la configuración y memoria
```

---

## ¿Dónde hospedar OpenClaw?

| Caso de uso | Host recomendado | Coste mensual |
|---|---|---|
| Solo para probar | PC/portátil personal | 0 € |
| Privacidad total, IA local | Mac mini M-series en casa | ~1-3 € (electricidad) |
| Gratis en la nube | Oracle Cloud Free Tier | 0 € |
| Mejor relación calidad/precio | Hetzner CX22 | ~4,90 € |
| Cloud + Ollama local | Hetzner CX32 | ~9,80 € |
| iMessage obligatorio | Mac (casa o Mac mini) | ~1-3 € |

> Para aprender y trastear, tu propio PC es más que suficiente. No necesitas el portátil encendido 24/7 — arranca OpenClaw cuando lo uses y ciérralo al terminar, igual que el servidor de desarrollo de React/Vite.
