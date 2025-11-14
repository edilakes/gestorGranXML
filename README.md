# **📂 Gestor de Archivos XML y ZIP (Local/URL)**

## **🌟 Descripción General**

Esta aplicación web de una sola página (.html) está diseñada para facilitar la carga y el análisis de archivos XML, incluyendo aquellos que están comprimidos en formato ZIP, tanto desde el disco local del usuario como desde una URL externa. Utiliza JavaScript nativo para el procesamiento de XML y la librería JSZip para la descompresión.

Su principal utilidad es permitir la ejecución de consultas **XPath** en documentos XML, proporcionando una herramienta ligera y accesible para inspeccionar el contenido de archivos grandes sin necesidad de herramientas de escritorio o configuración de servidor.

## **✨ Funcionalidades Principales**

1. **Carga Múltiple de Orígenes:**  
   * **Local:** Soporte para subir archivos desde el disco (.xml o .zip).  
   * **URL Externa:** Posibilidad de descargar y procesar archivos (.xml o .zip) desde cualquier URL accesible.  
2. **Descompresión Integrada:**  
   * Soporte para archivos **ZIP (.zip)** que contengan un único archivo XML. La aplicación lo descomprime automáticamente en memoria para su procesamiento.  
3. **Análisis XML:**  
   * Una vez cargado y parseado, muestra información clave del documento (nombre del elemento raíz, número total de nodos).  
4. **Consulta XPath:**  
   * Permite al usuario introducir una expresión XPath y ejecutarla en tiempo real sobre el documento XML cargado.  
   * Muestra los resultados de la consulta, incluyendo el tipo de nodo (elemento, atributo, texto) y una vista previa de su contenido.  
5. **Diseño Responsivo:**  
   * Interfaz moderna y adaptable gracias al uso de Tailwind CSS, garantizando una buena experiencia en dispositivos móviles y de escritorio.

## **🛠️ Tecnologías Utilizadas**

* **HTML5 y JavaScript (ES6+):** Estructura y lógica de la aplicación.  
* **Tailwind CSS:** Framework de utilidad para el diseño y la estética.  
* **JSZip:** Librería JavaScript para manejar la lectura y descompresión de archivos ZIP en el cliente.  
* **API DOMParser:** Utilizada para transformar la cadena de texto XML en un objeto Document navegable.  
* **API XPath:** Utilizada mediante el método document.evaluate() para ejecutar las consultas.

## **🚀 Forma de Uso**

La aplicación es completamente autocontenida y se ejecuta simplemente abriendo el archivo xml\_manager.html en cualquier navegador moderno.

### **A. Carga desde Disco Local**

1. Haga clic en el área de carga o arrastre un archivo (.xml o .zip).  
2. El archivo será leído. Si es un ZIP, se descomprimirá el primer archivo XML encontrado.  
3. Una vez procesado, el **Panel de Información del XML** se hará visible.

### **B. Carga desde URL Externa**

1. Introduzca la URL completa del archivo (ej. https://ejemplo.com/data.zip) en el campo de texto.  
2. Haga clic en el botón **"Cargar URL"**.  
3. La aplicación intentará descargar el contenido.  
   * Si la descarga es exitosa, se procesará el contenido como XML o ArrayBuffer (para ZIP).  
   * Si falla (especialmente por CORS), se mostrará un mensaje de error detallado.

### **C. Ejecución de Consultas XPath**

1. Una vez que el panel XML está visible, escriba una expresión XPath en el campo **"Consulta XPath"** (ej. //libro\[precio \> 20\]/titulo).  
2. Haga clic en **"Ejecutar Consulta"**.  
3. Los resultados se mostrarán en la sección inferior, limitados a 500 elementos para evitar bloqueos del navegador en documentos muy grandes.

## **🛑 Limitaciones Importantes**

Esta aplicación está limitada por las capacidades del entorno de navegador, lo que impone las siguientes restricciones:

1. **Límite de Memoria (Archivos Grandes):** Los navegadores tienen límites de memoria estrictos. Intentar cargar y parsear archivos XML que superen los **100-200 MB** (dependiendo del dispositivo) puede provocar que la aplicación se congele o se cierre por falta de memoria (Out of Memory).  
2. **Restricciones de CORS (Carga por URL):** Para que la carga desde URL funcione, el servidor que aloja el archivo debe enviar los encabezados HTTP que permitan el acceso desde otro dominio (CORS). Si el servidor no lo permite, la descarga fallará con un error de seguridad del navegador.  
3. **Soporte de Compresión:** Solo se soporta el formato **ZIP (.zip)**. Otros formatos como .7z, .rar o .tar son demasiado complejos de implementar en un código JavaScript autocontenido y no son compatibles.  
4. **Procesamiento DOM:** La aplicación usa DOMParser, que carga el documento completo en la memoria del navegador. No es un *parser* de *streaming* (SAX), lo que reitera la limitación de archivos grandes.

## **💡 Prompt de Creación para un Agente**

Para recrear esta aplicación usando un agente de desarrollo, se debe usar un prompt muy específico que detalle las tecnologías y los requisitos de un solo archivo.

**Prompt Recomendado:**

"Quiero desarrollar una aplicación web de una sola página en HTML, JavaScript y Tailwind CSS que funcione como un gestor de archivos XML. La salida debe ser un único archivo xml\_manager.html.

**Requisitos:**

1. Permitir la carga de archivos desde **dos fuentes**: disco local (mediante input de archivo o drag-and-drop) y una URL externa (mediante un campo de texto).  
2. Los formatos de archivo soportados son: **.xml** y **.zip**.  
3. Si el archivo es un **.zip**, debe utilizar la librería JSZip (cargada desde CDN) para descomprimirlo y extraer el primer archivo **.xml** encontrado.  
4. Una vez cargado el contenido XML, debe parsearse en un objeto DOM.  
5. Debe tener un campo de entrada para consultas **XPath** y un botón para ejecutar la consulta sobre el documento cargado.  
6. Los resultados de la consulta deben mostrarse en una lista, indicando el tipo de nodo (elemento, texto, atributo) y una previsualización de su valor, limitando la visualización a los primeros 500 resultados.  
7. Incluir indicadores de carga y mensajes de error específicos (incluyendo errores de parseo XML y de red/CORS) usando un modal en lugar de alert().  
8. Utilizar Tailwind CSS para un diseño limpio, moderno y completamente responsivo."

*Fin del Documento*
