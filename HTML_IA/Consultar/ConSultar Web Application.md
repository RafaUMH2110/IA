Starting from the Python code I provide, create a complete web application named **"ConSultar"** as a **single self-contained HTML file**.

The application must reproduce the functionality of the provided Python code as closely as possible, adapting it to run entirely in the browser. The final deliverable must consist of **one `.html` file** containing the HTML, CSS, and JavaScript required by the application.

## 1. Application purpose

The application is designed to improve a user's prompt through a two-step process:

1. The user's original **Consulta** is first processed by the prompt-improvement logic contained in the provided Python code.
  
2. The improved prompt is then used as the user prompt in the second/final query.
  
3. The result of this final query must be displayed on the web page as rendered Markdown.
  

The models, API configuration, prompt structure, and other relevant logic must be derived from the Python code provided with this request.

Do not invent model names or configuration values when equivalent information is available in the Python code.

## 2. User interface

Create a clean, modern, elegant and responsive interface in **Spanish (Spain)**.

The page should use a very light visual design, with **light blue as the predominant color**, complemented by white, light grey and subtle blue accents.

Include an appropriate application header with:

- Application name: **ConSultar**
  
- A short explanation of what the application does.
  
- Clear visual separation between configuration, query input, and results.
  

### Configuration fields

Include the following controls:

#### "Elegir Modelo"

A dropdown (`select`) containing the models available in the Python code.

The list of models must be generated from the models defined or referenced in the provided Python code.

#### "Número de tokens"

A numeric input labeled:

**Número de tokens**

Its default value must be:

**4096**

The control should include sensible validation according to the requirements of the underlying API/code.

#### "Exportar resultado"

A checkbox labeled:

**Exportar resultado**

When this checkbox is enabled, display an additional section containing:

- A dropdown labeled **"Formato de exportación"**
  
- An input text field labeled **"Nombre del archivo"**
  

The available export formats must be:

- Markdown
  
- Word
  
- PDF
  

The corresponding extensions must be:

- Markdown → `.md`
  
- Word → `.docx`
  
- PDF → `.pdf`
  

The file extension must be added **automatically** based on the selected export format.

If the user enters a filename that already contains an extension, avoid creating duplicated extensions such as `resultado.md.md`.

The filename must be sanitized as necessary to avoid invalid filesystem characters.

## 3. Export formats

Implement the following export mechanisms directly in the HTML application:

### Markdown

Export the result as a `.md` file containing the Markdown representation of the generated response.

### Word

Use **docx.js** to generate a `.docx` Microsoft Word document.

The generated document must contain the final response and must be properly structured so that Markdown elements such as headings, paragraphs, lists and emphasis are converted into appropriate Word formatting whenever reasonably possible.

### PDF

Use **jsPDF** to generate a `.pdf` file.

The PDF must contain the final response in a readable and appropriately formatted layout.

Pay particular attention to:

- Page margins
  
- Line wrapping
  
- Multiple pages
  
- Headings
  
- Paragraph spacing
  
- Lists
  
- UTF-8/Spanish characters, including accents and characters such as `ñ`
  

If external JavaScript libraries are required, load them from reliable CDN sources and clearly document the dependencies in comments.

## 4. Filename handling

When the user enables **"Exportar resultado"**, allow them to enter the desired filename.

For example:

`mi_consulta`

If the selected format is Markdown, the downloaded file must be:

`mi_consulta.md`

If Word is selected:

`mi_consulta.docx`

If PDF is selected:

`mi_consulta.pdf`

The extension should always correspond to the selected export format.

## 5. Query inputs

Include the following text inputs:

### "Rol del usuario"

This field corresponds to the **`system_prompt` of the second/final query**.

Provide a sufficiently large multiline `<textarea>` because the user may enter a long system prompt.

### "Consulta"

This field corresponds to the **user prompt of the second/final query**.

However, the original consultation must first be processed by the **prompt-improvement process represented by the first query in the provided Python code**.

The intended workflow is:

```text
Usuario escribe "Consulta"
        ↓
Primera consulta:
mejora/optimiza la consulta
        ↓
Consulta mejorada
        ↓
Segunda consulta:
system_prompt = "Rol del usuario"
user_prompt = consulta mejorada
        ↓
Respuesta final
        ↓
Mostrar resultado como Markdown
```

The implementation must preserve this two-step logic from the Python code rather than simply sending the original query directly to the final model.

## 6. Main buttons

Add the following buttons:

### "Consultar"

This button must execute the complete two-step consultation process.

It should:

1. Validate the required fields.
  
2. Execute the first query that improves the user's original consultation.
  
3. Obtain the improved prompt.
  
4. Execute the second/final query using:
  
  - The value of **"Rol del usuario"** as the `system_prompt`.
    
  - The improved consultation as the final `user_prompt`.
    
5. Display the final response in the results area.
  

While the operation is running:

- Disable the button to prevent accidental duplicate requests.
  
- Display an appropriate loading indicator.
  
- Clearly inform the user that the consultation is being processed.
  

Errors must be handled gracefully and displayed in Spanish in a dedicated error/status area.

### "Exportar fichero"

This button must export the currently displayed final result using the selected export format and filename.

It should remain disabled until a valid result is available.

If the user has not generated a result yet, display an appropriate message instead of attempting the export.

If **"Exportar resultado"** is not selected, the export controls and/or export action should be disabled appropriately.

## 7. Result display

After the final consultation has completed, display the response in a dedicated **"Resultado"** section.

The response must be rendered as **Markdown**, rather than displayed as raw Markdown text.

Support, at minimum:

- Headings
  
- Paragraphs
  
- Bold
  
- Italic
  
- Ordered lists
  
- Unordered lists
  
- Links
  
- Code blocks
  
- Inline code
  
- Blockquotes
  
- Tables where supported by the Markdown renderer
  

Use a well-established Markdown library if necessary.

The rendered result should be visually comfortable to read, with appropriate typography, spacing and contrast.

## 8. API and Python-code adaptation

The Python code provided with this prompt is the authoritative source for:

- Available models
  
- API endpoints
  
- Authentication/configuration approach
  
- Prompt-improvement logic
  
- Model parameters
  
- The structure of the first query
  
- The structure of the second query
  

Translate the Python functionality into browser-compatible JavaScript while preserving its intended behavior.

Where browser limitations prevent a direct conversion, implement the closest practical browser-based equivalent and clearly comment the relevant code.

Do not silently omit functionality that exists in the Python code.

If the Python code uses an API key or other secret credential, **do not hard-code a real secret into the generated application**. Structure the application so that credentials can be supplied safely according to the API's requirements, and clearly indicate in the code where the configuration belongs.

## 9. Single-file requirement

The final application must be contained in **one HTML file**.

The file should include:

```html
<!DOCTYPE html>
<html lang="es">
...
</html>
```

Include:

- HTML
  
- CSS
  
- JavaScript
  

inside the same file whenever practical.

External libraries may be loaded from CDNs when necessary, but the application itself must not require multiple local files.

## 10. Design requirements

The interface should be:

- Modern
  
- Elegant
  
- Professional
  
- Responsive
  
- Accessible
  
- Easy to understand
  
- Suitable for desktop and tablet use
  
- Usable on mobile devices where practical
  

Use a predominantly **very light blue and white color palette**.

Use cards, panels, subtle borders, rounded corners and restrained shadows to visually separate the different sections.

Suggested page structure:

```text
┌─────────────────────────────────────────────┐
│                  ConSultar                   │
│       Mejora y ejecuta tus consultas        │
└─────────────────────────────────────────────┘

┌── Configuración ────────────────────────────┐
│ Elegir Modelo       [▼]                     │
│ Número de tokens    [4096]                  │
│ ☑ Exportar resultado                         │
│                                             │
│ Formato            [Markdown ▼]             │
│ Nombre del archivo [resultado]              │
└─────────────────────────────────────────────┘

┌── Consulta ─────────────────────────────────┐
│ Rol del usuario                             │
│ [                                           │
│                                             │
│ ]                                           │
│                                             │
│ Consulta                                    │
│ [                                           │
│                                             │
│ ]                                           │
│                                             │
│              [ Consultar ]                  │
└─────────────────────────────────────────────┘

┌── Resultado ─────────────────────────────────┐
│                                             │
│          Rendered Markdown                  │
│                                             │
│                                             │
│             [ Exportar fichero ]             │
└─────────────────────────────────────────────┘

                 Rafa puerto
              (r.puerto@umh.es)
```

The exact design does not have to reproduce this ASCII layout; use your own judgment to create a polished interface.

## 11. Language and localization

All visible text in the web application must be in **Spanish from Spain (es-ES)**.

This includes:

- Labels
  
- Buttons
  
- Explanations
  
- Validation messages
  
- Error messages
  
- Loading messages
  
- Export messages
  
- Help text
  
- Tooltips
  
- Status messages
  

All generated documents must also use **Spanish (Spain)** as their language where the document format supports language metadata.

The application itself must use:

```html
<html lang="es">
```

## 12. Accessibility

Follow good accessibility practices, including:

- Proper `<label>` elements for form controls.
  
- Sufficient color contrast.
  
- Keyboard accessibility.
  
- Visible focus states.
  
- Appropriate ARIA attributes where useful.
  
- Meaningful button and control labels.
  
- Status messages that can be understood by screen readers.
  

Do not rely exclusively on color to communicate errors or states.

## 13. Code quality

The code must be **elegant, structured, maintainable and very thoroughly commented**.

Organize the JavaScript into logical functions/modules, for example:

```javascript
initializeApp()
validateForm()
improvePrompt()
executeFinalQuery()
renderResult()
exportMarkdown()
exportWord()
exportPDF()
updateExportControls()
showStatus()
showError()
```

Use clear variable and function names.

Avoid unnecessary global variables.

Avoid duplicated code.

Separate, as far as practical:

- Configuration
  
- UI handling
  
- API communication
  
- Prompt processing
  
- Markdown rendering
  
- Export functionality
  
- Error handling
  

Add comments explaining important implementation decisions and any adaptations made from the Python code.

## 14. Error handling

Implement robust error handling for cases such as:

- Missing required fields
  
- Invalid token values
  
- Missing model
  
- API authentication errors
  
- Network errors
  
- API timeouts
  
- Invalid API responses
  
- Markdown rendering errors
  
- Export errors
  
- Empty results
  

Errors must be presented clearly in Spanish and should help the user understand what needs to be corrected.

Do not expose sensitive credentials or unnecessary technical information to the user.

## 15. Final footer

At the bottom of the page, display the following text in a small, discreet font:

**Rafa puerto (r.puerto@umh.es)**

## 16. Final deliverable

Return the **complete HTML source code** for the application.

Do not provide pseudocode or a partial implementation.

The result must be directly saveable as, for example:

`consultar.html`

and opened in a modern browser.

Before providing the final code, verify that:

- The application is contained in a single HTML file.
  
- The two-step query workflow is implemented.
  
- The models come from the provided Python code.
  
- The default token count is 4096.
  
- Markdown rendering works.
  
- Markdown, DOCX and PDF export are implemented.
  
- File extensions are added automatically.
  
- The UI is in Spanish (Spain).
  
- Generated documents use Spanish (Spain) where technically possible.
  
- The interface is responsive and visually polished.
  
- Errors are handled gracefully.
  
- The footer contains exactly:  
  **Rafa puerto (r.puerto@umh.es)**
  

Use the provided Python code as the source of truth for the application's underlying functionality.