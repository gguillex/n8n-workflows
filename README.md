# ⚡ n8n Automation Lab

Bienvenido a mi colección personal de automatizaciones. Aquí iré subiendo diferentes agentes, bots y flujos de trabajo avanzados ("workflows") listos para importar en **n8n**.

El objetivo es compartir herramientas útiles para potenciar la productividad y la creación de contenido.

## 📂 Agentes Disponibles

### 1. 🤖 [LinkedIn AI Agent](./Linkedin-AI-Agent)
> *Automatización completa para crear marca personal.*
* **Qué hace:** Busca temas (o recibe manuales), redacta posts con IA, genera imágenes con DALL-E/Stable Diffusion y gestiona la publicación.
* **Integraciones:** OpenAI, Google Gemini, Google Drive, Telegram...
* **[📂 Ver archivos y documentación](./Linkedin-AI-Agent)**

### 2. 🍽️ [Horeca LeadGen System](./Horeca-LeadGen-System)
> *Sistema autónomo de auditoría y ventas B2B para restaurantes.*
* **Qué hace:** Escanea Google Maps buscando negocios locales, audita sus webs con IA para detectar "dolores" (menús PDF, motores de reservas con altas comisiones) y envía emails fríos hiper-personalizados ofreciendo soluciones específicas.
* **Integraciones:** Google Places API, OpenAI (GPT-4o), Google Sheets, Telegram.
* **[📂 Ver archivos y documentación](./Horeca-LeadGen-System)**

### 3. 🔜 Próximamente...
* *Aquí iré subiendo nuevos bots (Telegram, Email, Scrapers, etc.).*

---

## 🛠️ Cómo usar estos flujos
1.  **Descarga:** Clona este repositorio o descarga el archivo `.json` de la carpeta que te interese.
2.  **Importa:** En tu n8n, ve a `Workflows` > `Add Workflow` > `Import from File`.
3.  **Configura:**
    * Cada flujo necesita sus propias credenciales (API Keys).
    * Revisa los nodos marcados en rojo al importar y conecta tus cuentas.

## ⚠️ Aviso Legal
Estos flujos se comparten con fines educativos. Asegúrate de revisar las políticas de uso de las APIs conectadas (LinkedIn, OpenAI, Google, etc.).

---
⭐ **Si te son útiles, ¡dale una estrella al repo!**
