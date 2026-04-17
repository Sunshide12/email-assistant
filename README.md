# 📧 Intelligent Email Agent (LangGraph + Python)

Este proyecto es un asistente de correo electrónico autónomo de última generación construido con **LangGraph**. A diferencia de los scripts lineales, este agente utiliza un flujo cíclico con estado, permitiéndole razonar, utilizar herramientas (Gmail/Outlook API) y solicitar supervisión humana antes de realizar acciones críticas.

## 🏗️ Arquitectura del Sistema (Workflow)

El agente opera como un grafo de estados donde cada nodo tiene una responsabilidad específica:



1.  **Ingesta & Monitorización:** Escucha de nuevos correos vía Webhooks o Polling.
2.  **Clasificación (Router):** Determina la intención del email (Spam, Informativo, Acción Requerida).
3.  **Investigación (RAG):** Si requiere acción, recupera contexto de hilos anteriores o bases de datos locales.
4.  **Redacción (Drafting):** Genera una propuesta de respuesta utilizando LLMs.
5.  **Revisión Humana (Breakpoints):** El grafo se pausa. Un humano debe aprobar o editar el borrador.
6.  **Ejecución:** Envío del correo y actualización de la base de datos de memoria.

## 🛠️ Stack Tecnológico (2026 Ready)

* **Orquestador:** [LangGraph](https://www.langchain.com/langgraph) - Para la gestión de ciclos y persistencia de estado.
* **Modelos (Tier Gratuito):**
    * **Google Gemini 1.5 Flash:** Por su ventana de contexto masiva (ideal para hilos largos).
    * **Groq (Llama 3.x):** Para clasificación e inferencia ultrarrápida.
* **Base de Datos de Vectores:** **ChromaDB** (Local) o **Qdrant** para memoria semántica.
* **Herramientas:** **MCP (Model Context Protocol)** para integración segura con calendarios y archivos.
* **Entorno:** Python 3.11+ con soporte para ejecución asíncrona.

## 📂 Estructura del Proyecto

```bash
email-agent/
├── src/
│   ├── agents/
│   │   ├── graph.py          # Definición del flujo, nodos y aristas
│   │   ├── state.py          # Definición del esquema del TypedDict de estado
│   │   └── prompts.py        # System prompts optimizados para emails
│   ├── tools/
│   │   ├── gmail_api.py      # Conexión nativa con Google Cloud
│   │   └── memory_tool.py    # Búsqueda semántica en correos pasados
│   ├── ui/
│   │   └── dashboard.py      # Pequeña interfaz para aprobación (Streamlit/Gradio)
│   └── main.py               # Punto de entrada
├── data/                     # Almacenamiento local de ChromaDB
├── .env.example              # Configuración de API Keys
├── requirements.txt
└── README.md
