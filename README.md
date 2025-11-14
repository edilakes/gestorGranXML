# **📑 Gestor de XML y Herramientas Avanzadas (v2.1)**

Esta aplicación web en un solo archivo HTML está diseñada para el análisis y la manipulación avanzada de documentos XML, ofreciendo funcionalidades robustas para manejar la carga de archivos grandes y consultas complejas mediante XPath y capacidades de Inteligencia Artificial.

## **🚀 Funcionalidades Clave**

### **1\. Carga Flexible de Archivos**

La aplicación soporta múltiples métodos de carga, con manejo optimizado para archivos grandes y contenedores comprimidos:

* **Carga Local:** Permite subir archivos **.xml** o **.zip** directamente desde el disco local mediante *drag and drop* o selección de archivo.  
* **Carga por URL:** Permite cargar archivos **.xml** o **.zip** desde una URL externa (sujeto a las políticas de CORS del servidor remoto).  
* **Soporte ZIP:** Si se carga un archivo .zip, la aplicación lo descomprime automáticamente y extrae el **primer archivo .xml** que encuentra en su interior.

### **2\. Consulta XPath (Doble Modo)**

El corazón de la aplicación permite consultar el documento XML cargado mediante dos modos interactivos:

| Modo | Descripción | Herramienta |
| :---- | :---- | :---- |
| **XPath Directa** | Ejecución inmediata de cualquier expresión **XPath 1.0** válida. | Estándar |
| **Lenguaje Natural (IA)** | Utiliza el modelo **Gemini 2.5 Flash** para analizar la estructura simplificada del XML y convertir una pregunta en lenguaje natural (ej. "dame todos los items cuyo precio sea mayor a 10") en una expresión XPath ejecutable. | Gemini API |

### **3\. Gestión Optimización de Resultados**

Para proteger la memoria del navegador y garantizar la estabilidad, la aplicación impone límites estrictos en el procesamiento y visualización de resultados:

* **Límite de Nodos:** La consulta XPath puede devolver millones de nodos, pero la aplicación solo almacena un máximo de **500 nodos** en memoria (lastXPathNodes) para las herramientas de procesamiento posteriores. El conteo total de nodos encontrados es visible en el resultado.

### **4\. Herramientas de Procesamiento Avanzado**

Una vez que se ha ejecutado una consulta XPath, las siguientes herramientas se aplican sobre el conjunto de resultados (máx. 500 nodos):

| Herramienta | Descripción | Protección de Recursos |
| :---- | :---- | :---- |
| **Ver XML Formateado** | Serializa y aplica un *pretty-print* básico al XML de los nodos resultantes. | **Límite de 5 MB:** La salida de texto inyectada en el DOM está estrictamente limitada a 5 megabytes. Si se excede, se trunca y se notifica al usuario, evitando la saturación de memoria. |
| **Convertir a JSON** | Transforma la estructura XML de los nodos a un formato JSON equivalente (usando una conversión estándar XML-to-JSON). | **Límite de 10 MB:** La salida JSON también está limitada a 10 megabytes para su visualización. |
| **Análisis y Resumen con IA** | Envía una muestra de los datos (JSON, limitado a 10,000 caracteres) al modelo **Gemini 2.5 Flash** para generar un resumen perspicaz de la estructura, los patrones y las anomalías de los datos. | **Límite de Contexto:** El contexto enviado a la IA está limitado para garantizar respuestas rápidas y concisas. |

## **🛠️ Forma de Uso**

1. **Cargar Archivo:** Utiliza el área de carga (disco o URL) para seleccionar tu archivo .xml o .zip.  
2. **Verificación:** La aplicación mostrará la raíz del documento y el total de elementos.  
3. **Ejecutar Consulta:**  
   * **Directa:** Introduce //item/price en el campo y haz clic en **"Ejecutar Consulta"**.  
   * **IA:** Cambia a **"Lenguaje Natural (IA)"**, introduce tu pregunta y haz clic en **"Generar y Ejecutar XPath (IA)"**. El XPath generado se mostrará antes de la ejecución.  
4. **Procesar Resultados:** Una vez que se muestren los resultados de XPath (máx. 500 guardados), utiliza cualquiera de los tres botones de las **Herramientas de Procesamiento** para obtener el XML formateado, el JSON o el análisis de la IA.

## **⚠️ Limitaciones y Requisitos**

1. **API Key de Gemini:** Las funciones de "Lenguaje Natural (IA)" y "Análisis y Resumen con IA" requieren la clave API de Gemini. La variable apiKey en el código está inicialmente vacía (const apiKey \= "";).  
2. **CORS:** La carga desde URL puede fallar si el servidor de origen no permite solicitudes de *Cross-Origin Resource Sharing* (CORS).  
3. **XPath 1.0:** Se utiliza el motor XPath nativo del navegador, que generalmente solo soporta la versión 1.0.  
4. **Rendimiento en XML Formateado:** Aunque la aplicación tiene un límite de 5MB, intentar visualizar archivos cercanos a ese tamaño puede causar una desaceleración temporal del navegador debido al *DOM rendering*.  
5. **Solo el Primer XML en ZIP:** Solo se procesa el primer archivo XML encontrado dentro de un contenedor ZIP.

## **💻 Prompt de Creación (Desde Cero)**

El siguiente *prompt* describe detalladamente la aplicación en su estado actual, incluyendo las medidas de seguridad para el manejo de XML grandes:

Crea una aplicación web en un solo archivo HTML, usando Tailwind CSS, para la gestión avanzada de archivos XML. Debe permitir la carga de XML o archivos ZIP que contengan XML, tanto desde disco local (incluyendo Drag and Drop) como desde una URL externa. 

Implementa una herramienta de consulta XPath con dos modos:   
1\) Entrada directa de XPath.  
2\) Conversión de lenguaje natural a XPath utilizando la API de Gemini (debe mostrar el XPath generado antes de ejecutarlo). 

La aplicación debe mostrar información básica del XML cargado. Los resultados de XPath ejecutados deben limitarse a almacenar un máximo de 500 nodos para su procesamiento posterior, aunque debe mostrar el conteo total de resultados.

Además, incluye herramientas post-consulta (que operan sobre los resultados limitados):  
1\) Convertir los resultados de XPath a JSON (con límite de salida de 10MB).  
2\) Un botón para enviar el JSON de los resultados (limitado a 10000 caracteres) a la API de Gemini para un 'Análisis y Resumen'.  
3\) \*\*CRÍTICO:\*\* Una función llamada 'Ver XML de los Resultados Formateado' que serialice los nodos. Esta función debe limitar estrictamente la salida de texto XML formateado inyectada al DOM a 5 megabytes (5 \* 1024 \* 1024 bytes) para prevenir el agotamiento de recursos del navegador con archivos muy grandes. Si el límite se excede, debe truncar la salida, añadir un mensaje de advertencia visible y mostrar un modal de notificación al usuario.

Utiliza un sistema de modal (\`showModal\`) en lugar de \`alert()\` para todas las notificaciones de error y advertencia.  
