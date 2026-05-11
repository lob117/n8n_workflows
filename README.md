# n8n_workflows
repository contains a specialized n8n workflow designed to automate the extraction of metadata from various file formats. It provides a scalable solution for processing documents, images, or media files and converting their embedded properties into structured JSON data.

Automated PDF Metadata Extraction with n8n and Local AI
Este repositorio contiene un flujo de trabajo avanzado de n8n diseñado para escanear directorios, extraer texto de documentos PDF y generar metadatos estructurados mediante modelos de lenguaje (LLM) ejecutados localmente.

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

🚀 Desarrollo del Workflow
Una vez que el entorno de Docker y el Starter Kit estén operativos, se procede a la implementación del flujo de extracción de metadatos contenido en este repositorio.

Lógica del Sistema
Exploración de Archivos: El flujo utiliza comandos de sistema (find) dentro del contenedor Linux para localizar archivos .pdf de forma recursiva.

Extracción de Contenido: Se procesa el binario del PDF para convertirlo en texto legible.

Procesamiento de IA (Ollama): Se envía el texto al modelo llama3.1:8b para identificar campos clave como números de expediente, resúmenes de proyectos y ubicaciones geográficas.

Validación y Limpieza: Mediante nodos de JavaScript, se asegura que la respuesta de la IA sea un JSON válido y se corrigen posibles errores de formato antes de guardar el resultado.

📥 Importación del Flujo
Descarga el archivo work_flow_metadata_busqueda.json de este repositorio.

En tu instancia de n8n, selecciona Import from File.

Configura tus credenciales de Ollama (usualmente apuntando a http://host.docker.internal:11434 o la IP de tu contenedor).
