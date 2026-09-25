Perfecto, Rafa. He verificado la integridad estructural del código (balance de llaves y paréntesis) para garantizar que no haya errores de sintaxis en la nueva funcionalidad.

Aquí tiene la versión actualizada de `app.py` y `templates/index.html` con las funciones de **exportación a PDF y JSON** integradas directamente en la interfaz.

### Cambios Principales

1. **Interfaz Web (`index.html`):** Se han añadido dos botones debajo del resultado: **"Exportar PDF"** y **"Exportar JSON"**.
2. **Lógica de Exportación:**
  - **PDF:** Utiliza la librería `jspdf` (incluida en el CDN) para generar un documento con el resultado formateado.
  - **JSON:** Genera un archivo descargable con el resultado en formato JSON, ideal para análisis posterior o integración con otros sistemas.
3. **Persistencia de Resultados:** El script guarda el último resultado en una variable global `ultimoResultado` para permitir su exportación inmediata.

* * *

### 1. `app.py` (Sin cambios, ya que la exportación es cliente)

El servidor `app.py` no necesita modificaciones, ya que la generación de archivos se realiza en el navegador del cliente (lado cliente), lo que reduce la carga del servidor y mantiene la privacidad.

* * *

### 2. `templates/index.html` (Actualizado con Exportación)

index1.html

