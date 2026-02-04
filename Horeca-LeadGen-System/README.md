# 🤖 Horeca LeadGen & Outreach Automation

Este repositorio contiene un sistema automatizado de **prospección y contacto en frío** diseñado para el sector de la restauración (Horeca). 

El sistema utiliza **n8n**, **Google Places API** y **OpenAI (GPT-4o)** para localizar restaurantes, analizar su madurez tecnológica y enviar correos electrónicos hiper-personalizados basados en sus debilidades operativas.

## 🚀 Flujo de Trabajo

El sistema se divide en dos workflows principales:

### 1. 🕵️ Buscador y Auditor (Hunter)
Este workflow se ejecuta automáticamente y realiza las siguientes tareas:
1.  **Búsqueda Local:** Selecciona aleatoriamente un nicho (Pizzería, Sushi, Tapas, etc.) y busca en Google Maps mediante API.
2.  **Filtrado:** Descarta negocios que ya existen en la base de datos.
3.  **Scraping & Análisis:**
    * Visita la web del restaurante.
    * Extrae el texto y lo pasa a **GPT-4o**.
    * **Detecta Stack Tecnológico:** ¿Usan TheFork/CoverManager? ¿Tienen menú en PDF o QR? ¿Tienen pixel de Facebook?
4.  **Enriquecimiento:** Busca emails corporativos y redes sociales.
5.  **Almacenamiento:** Guarda los leads cualificados en Google Sheets y notifica vía Telegram.

### 2. 📧 Redactor y Contacto (Sender)
Este workflow lee los leads cualificados y ejecuta la estrategia de ventas:
1.  **Lectura:** Toma los leads en estado "Pendiente".
2.  **Lógica de "Dolor" (Pain Points):** GPT-4o selecciona la estrategia de venta según la auditoría previa:
    * *Si usan TheFork/UberEats:* El email ataca el problema de las **comisiones**.
    * *Si usan PDF:* El email ataca la dificultad de **actualizar precios**.
    * *Si solo tienen teléfono:* El email ataca las **llamadas perdidas/no-shows**.
3.  **Generación de Copy:** Redacta un email único, simulando ser escrito desde un móvil (sin jerga corporativa).
4.  **Envío:** Envía el correo mediante SMTP (Gmail/Outlook).
5.  **Actualización:** Marca el lead como "Contactado" en el Sheet.

## 🛠️ Stack Tecnológico

* **Orquestador:** [n8n](https://n8n.io/)
* **Fuentes de Datos:** Google Places API (New)
* **IA / NLP:** OpenAI (GPT-4o Mini)
* **Base de Datos:** Google Sheets
* **Notificaciones:** Telegram Bot
* **Email:** SMTP Node

## 📋 Requisitos Previos

Para implementar estos workflows necesitas:

1.  Una instancia de n8n (Self-hosted o Cloud).
2.  **Google Cloud Console:** API Key habilitada para *Places API (New)*.
3.  **OpenAI API Key:** Con acceso a modelos GPT-4.
4.  **Telegram Bot:** Token y Chat ID para reportes.
5.  **Google Sheets:** Una hoja con las columnas: `Id_place`, `Nombre`, `Web`, `Email`, `Tecnologia`, `Marketing`, `Estado`, `Nicho`.

## ⚙️ Instalación

1.  Clona este repositorio.
2.  Abre tu instancia de n8n.
3.  Ve a **Workflows** > **Import from File**.
4.  Selecciona los archivos `.json` de la carpeta `/workflows`.
5.  Configura tus credenciales en los nodos marcados (Google, OpenAI, SMTP).
6.  Ajusta las variables del nodo "Ruleta de Negocios" con tu ciudad objetivo.

## ⚠️ Nota de Privacidad

Los archivos JSON han sido limpiados de credenciales reales. Asegúrate de configurar tus propias credenciales en n8n antes de activar los workflows.
