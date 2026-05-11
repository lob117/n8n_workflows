# n8n_workflows
repository contains a specialized n8n workflow designed to automate the extraction of metadata from various file formats. It provides a scalable solution for processing documents, images, or media files and converting their embedded properties into structured JSON data.

# n8n Document AI Suite: De Scripts Locales a API Service

Este repositorio contiene una suite de flujos de trabajo de n8n para la gestión inteligente de documentos PDF utilizando modelos de IA locales (Ollama).

🛠️ Requisitos Previos e Instalación del Entorno
Para asegurar el correcto funcionamiento de este flujo, es imperativo configurar el entorno en el siguiente orden:

1. Instalación de Docker (Linux / Ubuntu)
El sistema se basa íntegramente en contenedores. Si estás utilizando Ubuntu, sigue la documentación oficial para instalar el motor de Docker:

Guía de instalación: Docker Desktop for Linux (Ubuntu)

2. Configuración de NVIDIA CUDA Cores
Para que los modelos de IA (como Llama 3.1 en Ollama) procesen la información de manera eficiente, el contenedor debe tener acceso a la GPU.

Asegúrate de tener instalados los drivers de NVIDIA en tu sistema host.

Instala el NVIDIA Container Toolkit para permitir que Docker utilice los CUDA cores.

3. Despliegue del n8n AI Starter Kit
Este proyecto fue desarrollado sobre la infraestructura del kit de inicio de IA auto-hospedado de n8n. Debes instalar y ejecutar este repositorio primero:

Repositorio: n8n-io/self-hosted-ai-starter-kit

Este kit levanta automáticamente mediante Docker las instancias de n8n, Ollama (para la IA local) y la base de datos necesaria.



## 📂 Flujos de Trabajo Incluidos

### 1. Extractor de Expedientes (Búsqueda Local)
**Archivo:** `work_flow_metadata_busqueda.json`
* **Función:** Escanea directorios mediante comandos `find` y extrae metadatos específicos (números de expediente, fechas).
* **Modelo:** Llama 3.1.

### 2. Sintetizador de Documentos Largos (Recursivo)
**Archivo:** `work_flow_meta.json`
* **Función:** Procesa PDFs extensos dividiéndolos en fragmentos (chunks) con solapamiento para no perder contexto.
* **Modelo:** Mistral (vía Ollama).
* **Especialidad:** Identificación de tablas y gráficas.

### 3. API de Procesamiento bajo Demanda (Webhook)
**Archivo:** `work_hook_api.json`
* **Función:** Transforma n8n en un endpoint de API. Recibe una ruta de archivo y devuelve el análisis.
* **Endpoint:** `POST /webhook/process-pdf`
* **Cuerpo esperado (JSON):**
    ```json
    {
      "pdfPath": "/demo-data/mi_archivo.pdf"
    }
    ```
* **Respuesta:** Retorna un JSON con el estado del procesamiento y la ubicación del resultado final.

---

## 🔧 Configuración de los Workflows

1.  **Importación:** Importe los archivos `.json` en su instancia de n8n.
2.  **Credenciales:** Configure el nodo de **Ollama** con la URL `http://host.docker.internal:11434`.
3.  **Rutas de Archivo:** Asegúrese de que los volúmenes de Docker en su `docker-compose.yaml` coincidan con la ruta `/demo-data/` utilizada en los nodos.

## 🧠 Características Técnicas Destacadas
* **Chunking Semántico:** Fragmentación de 2000 caracteres con 200 de solapamiento para mantener la coherencia.
* **Validación de JSON:** Lógica en JavaScript para asegurar que las respuestas de la IA sean parseables.
* **Consolidación de Contexto:** Uso de memoria interna en bucles para síntesis multietapa.
