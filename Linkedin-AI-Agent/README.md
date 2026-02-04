# 🤖 Agente de IA para LinkedIn con n8n & Telegram

Este proyecto es una automatización avanzada construida en **n8n** que actúa como un "Departamento Editorial" autónomo. El sistema investiga temas, redacta posts técnicos, genera imágenes y permite a un humano revisar, editar y publicar el contenido directamente desde **Telegram**.

![n8n](https://img.shields.io/badge/n8n-Workflow-orange?style=flat-square&logo=n8n)
![Status](https://img.shields.io/badge/Status-Active-success?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)

## 📋 Características Principales

* **🧠 Generación Autónoma:** Investiga tendencias técnicas o desarrolla temas educativos usando **GPT-4o** y **Perplexity AI**.
* **🎨 Estudio de Diseño IA:** Crea imágenes personalizadas para cada post utilizando **Google Gemini** (creación) y **DALL·E** (edición).
* **📱 Control Humano (Human-in-the-loop):** Interfaz completa vía **Telegram** para solicitar cambios ("hazlo más divertido", "cambia la foto"), aprobar o rechazar borradores.
* **💾 Memoria y Estado:** Utiliza **Google Sheets** como base de datos para gestionar estados (`PENDING`, `PUBLISH`) y evitar repetir temas pasados.
* **🚀 Publicación Automática:** Publica directamente en tu perfil personal de **LinkedIn** (texto + imagen) tras tu confirmación.

---

## 🏗️ Arquitectura del Sistema

El sistema se divide en dos flujos de trabajo (Workflows) interconectados:

### 1. ⚙️ El Generador (`Linkedin V1.0`)
Es el "Back-end" creativo. Se encarga de la creación desde cero.
* **Lógica de Decisión:** Decide si buscar noticias de actualidad (vía **Perplexity**) o explicar un concepto técnico (vía **OpenAI**).
* **Anti-Repetición:** Consulta el historial en Google Sheets para no repetir temas.
* **Generación de Assets:** Redacta el copy técnico y genera un prompt de diseño para crear la imagen con Google Gemini.
* **Almacenamiento:** Guarda el borrador en Google Sheets y sube la imagen a Google Drive.

### 2. 🎮 El Controlador (`Controlador LinkedIn`)
Es el "Front-end" interactivo. Gestiona la conversación contigo.
* **Trigger:** Mensajes de Telegram.
* **Agente Inteligente:** Un Agente LangChain interpreta tu intención:
    * *"Quiero un post sobre React"* → Llama al Generador.
    * *"Cambia el texto, hazlo más corto"* → Edita el borrador actual (GPT-4o).
    * *"Cambia la imagen por algo más oscuro"* → Edita la imagen actual (DALL·E).
    * *"Publicar"* → Envía el contenido a la API de LinkedIn.

---

## 🛠️ Stack Tecnológico

* **Orquestador:** [n8n](https://n8n.io/)
* **LLMs (Cerebro):** OpenAI (GPT-4o) y Google Gemini.
* **Búsqueda (Web Browsing):** Perplexity AI API.
* **Generación de Imagen:** Google Gemini (Generación base) / OpenAI DALL·E (Edición).
* **Base de Datos:** Google Sheets.
* **Almacenamiento de Archivos:** Google Drive.
* **Interfaz de Usuario:** Telegram Bot API.
* **Red Social:** LinkedIn API.

---

## 🚀 Instalación y Configuración

### 1. Prerrequisitos
Necesitarás configurar las siguientes credenciales en tu instancia de n8n:
* OpenAI API Key.
* Google Cloud Console (OAuth2 para Drive y Sheets, API Key para Palm/Gemini).
* Telegram Bot Token (vía BotFather).
* Perplexity API Key.
* LinkedIn OAuth2 Client.

### 2. Configuración de Google Sheets
Crea una hoja de cálculo con una pestaña llamada `Publicaciones`. La primera fila debe contener EXACTAMENTE estas columnas:

| Columna | Descripción |
| :--- | :--- |
| `id` | Identificador único del post (UUID) |
| `created_at` | Fecha de creación |
| `status` | Estados: `PENDING`, `PUBLISH`, `ARCHIVED`, `DONE` |
| `topic` | El título o tema del post |
| `draft_text` | El cuerpo del mensaje generado |
| `current_image_prompt` | El prompt usado para la imagen |
| `media_url` | Enlace directo a la imagen en Google Drive |
| `media_type` | Generalmente "IMAGEN" |

### 3. Importación de Workflows
1.  Importa el archivo JSON del Generador (`Linkedin_V1.0`).
2.  Importa el archivo JSON del Controlador (`Controlador_LinkedIn`).
3.  **Nota:** En el nodo "Call Linkedin V1.2" dentro del Controlador, asegúrate de seleccionar el ID correcto del workflow del Generador que acabas de importar.

---

## 🤖 Guía de Uso (Comandos Telegram)

Una vez el bot esté activo en Telegram, puedes hablarle naturalmente:

**Para crear contenido:**
> "Sorpréndeme" (Busca una noticia random)
> "Quiero un post sobre Kubernetes" (Genera contenido educativo)
> "Noticias de Ciberseguridad" (Busca actualidad específica)

**Para editar (cuando ya tienes una propuesta):**
> "Reescribe el texto, hazlo más corto y con más emojis"
> "Cambia la foto, quiero que salga un ordenador futurista"

**Para publicar:**
> "Publicar"
> "Lánzalo con imagen"

---

## 📊 Diagrama de Flujo

```mermaid
graph TD
    User[Usuario Telegram] -->|Mensaje| Controller[Workflow Controlador]
    Controller -->|Agente LangChain| Switch{Decisión IA}
    
    Switch -->|Nuevo Tema| Generator[Workflow Generador]
    Switch -->|Editar Texto| EditorTXT[GPT-4o Rewrite]
    Switch -->|Editar Imagen| EditorIMG[DALL·E Edit]
    Switch -->|Publicar| LinkedIn[LinkedIn API]
    Switch -->|Conversar| Chat[Chat Simple]
    
    subgraph Lógica del Generador
    Generator -->|Check DB| GSheets[Google Sheets]
    Generator -->|Investigar| Perplexity[Perplexity AI]
    Generator -->|Redactar| GPT[GPT-4o]
    Generator -->|Crear Imagen| Gemini[Gemini Vision]
    end
    
    Generator -->|Guardar| GDrive[Google Drive]
    Generator -->|Notificar| User
    LinkedIn -->|Actualizar Estado| GSheets
