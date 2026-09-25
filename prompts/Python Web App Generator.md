Below is a more precise version of your prompt. I’ve made explicit that the Python program should act as a **configurable web-app generator**, preserve the requirements previously established for *ConSultar*, and produce both the generated HTML and a detailed Markdown configuration/usage guide.

I now want you to create a complete **Python application designed to run in Visual Studio Code (VS Code)** that can automatically generate the web application described in my previous requirements, using the information and specifications I have provided throughout this conversation.

The Python program must act as a **highly configurable web-application generator**. Its purpose is to generate the complete `ConSultar` web application as a self-contained HTML file, while allowing the developer to configure virtually every relevant aspect of the generated application without having to modify the generator's core logic.

## 1. Main objective

Create a Python program that can be executed locally from **VS Code** and that generates the complete `ConSultar` web application described in my previous prompt.

The generated application must preserve all the previously specified functionality, including:

- Application name: **ConSultar**
  
- Spanish (Spain) user interface.
  
- Two-step AI consultation workflow.
  
- Prompt improvement in the first query.
  
- Final query using the improved prompt.
  
- Configurable model selection.
  
- Configurable token limit.
  
- User role/system prompt.
  
- User consultation.
  
- Markdown rendering of the final result.
  
- Markdown export.
  
- Word/DOCX export using `docx.js`.
  
- PDF export using `jsPDF`.
  
- Configurable output filename.
  
- Automatic file-extension handling.
  
- Responsive and modern user interface.
  
- Predominantly light-blue visual design.
  
- Error handling and status messages.
  
- Footer:  
  **Rafa puerto (r.puerto@umh.es)**
  

The Python generator must not simply contain a hard-coded HTML template with a few variables. It should be designed as a **reusable and extensible application generator**.

* * *

## 2. Python execution environment

The generated Python program must be suitable for execution from **Visual Studio Code**.

Assume a modern Python 3 environment.

Provide:

- A clear project structure.
  
- A `requirements.txt` file if external Python packages are required.
  
- Clear installation instructions.
  
- Clear execution instructions.
  
- Appropriate error handling for missing dependencies.
  
- Cross-platform compatibility where reasonably possible for Windows, macOS and Linux.
  

The program should preferably use the Python standard library whenever practical.

If external Python libraries are required, explain why they are necessary and include them in `requirements.txt`.

* * *

## 3. Configuration architecture

The most important requirement is that the generator must be **highly configurable**.

Do not scatter configuration values throughout the Python source code.

Create a centralized configuration system.

For example, configuration could be stored in:

```
config.yaml
```

or:

```
config.json
```

or another appropriate human-readable configuration format.

Choose the most appropriate approach and explain your choice.

The configuration should allow the user to customize, where applicable:

### Application

- Application name.
  
- Application description.
  
- HTML filename.
  
- Output directory.
  
- HTML language.
  
- Interface language.
  
- Footer text.
  
- Author information.
  

### Models

- Available AI models.
  
- Default model.
  
- Model display names.
  
- Model identifiers.
  
- Token limits.
  
- Default token count.
  
- Other model-specific parameters.
  

The generator should obtain the available model information from the configuration rather than requiring the Python source code to be modified.

### AI/API configuration

Make configurable:

- API provider.
  
- API endpoint.
  
- Authentication mechanism.
  
- API key configuration.
  
- Request parameters.
  
- Temperature, if supported.
  
- Maximum tokens.
  
- Timeout.
  
- Retry count.
  
- Other relevant API parameters.
  

**Security requirement:**

Never hard-code API keys or other secrets into the generated source code.

The configuration should support environment variables, for example:

```
OPENAI_API_KEY
```

or another configurable environment-variable name.

If the generated browser application requires an API key, clearly explain the associated security implications. Do not expose a real secret in the generated HTML.

* * *

## 4. Prompt configuration

The two-step consultation process must be configurable.

The configuration should allow the user to modify:

### First query

The prompt responsible for improving/optimizing the user's original consultation.

Allow configuration of:

- System prompt.
  
- User prompt template.
  
- Model.
  
- Token limit.
  
- Temperature.
  
- Other supported parameters.
  

### Second query

Allow configuration of:

- System prompt.
  
- User prompt template.
  
- Model.
  
- Token limit.
  
- Temperature.
  
- Other supported parameters.
  

The Python generator should make it possible to modify these prompts without changing the generator's Python source code.

Use clear placeholders where appropriate, for example:

```
{{consulta}}
{{consulta_mejorada}}
{{rol_usuario}}
{{modelo}}
```

Document all supported placeholders.

* * *

## 5. Web interface configuration

The generator must allow the appearance and behavior of the generated HTML application to be customized.

Make configurable, where appropriate:

- Page title.
  
- Header title.
  
- Subtitle.
  
- Section titles.
  
- Labels.
  
- Button text.
  
- Help text.
  
- Placeholder text.
  
- Error messages.
  
- Loading messages.
  
- Success messages.
  
- Footer.
  
- Default values.
  
- Visibility of optional controls.
  

For example:

```
ui:
  title: "ConSultar"
  subtitle: "Mejora y ejecuta tus consultas mediante IA"
  language: "es-ES"
  buttons:
    consult: "Consultar"
    export: "Exportar fichero"
```

The exact configuration format is up to you, but it must be clear and easy to understand.

* * *

## 6. Visual design configuration

The generated web application's visual appearance must also be configurable.

Allow configuration of:

- Primary color.
  
- Secondary color.
  
- Background color.
  
- Text color.
  
- Accent color.
  
- Border color.
  
- Button colors.
  
- Font family.
  
- Font sizes.
  
- Border radius.
  
- Shadows.
  
- Spacing.
  
- Maximum content width.
  

For example:

```
theme:
  primary: "#2563EB"
  secondary: "#DBEAFE"
  background: "#F8FAFC"
  text: "#1E293B"
  accent: "#60A5FA"
```

The default theme should remain consistent with the previous requirement: **very light colors with a predominance of blue**.

* * *

## 7. Export configuration

The generator must allow export functionality to be configured.

Supported formats must include:

- Markdown (`.md`)
  
- Word (`.docx`)
  
- PDF (`.pdf`)
  

Allow configuration of:

- Whether each export format is enabled.
  
- Default export format.
  
- Default filename.
  
- File extension.
  
- Document title.
  
- PDF page size.
  
- PDF margins.
  
- PDF font configuration.
  
- Word document formatting.
  
- Markdown formatting.
  

The generated application must automatically add the correct extension to downloaded files.

* * *

## 8. External JavaScript libraries

The generated HTML application may use external JavaScript libraries loaded from CDNs.

At minimum, support:

- A Markdown renderer.
  
- `docx.js` for Word documents.
  
- `jsPDF` for PDF documents.
  

The CDN URLs must be configurable.

For example:

```
libraries:
  markdown:
    enabled: true
    url: "..."
  docx:
    enabled: true
    url: "..."
  jspdf:
    enabled: true
    url: "..."
```

Use stable and appropriate library versions rather than relying unnecessarily on unversioned URLs.

Document the purpose of each dependency.

* * *

## 9. HTML generation architecture

The Python generator should produce a complete, valid HTML5 document.

The generated HTML must include:

```
<!DOCTYPE html>
<html lang="es">
```

The generated application must contain all necessary:

- HTML
  
- CSS
  
- JavaScript
  

in the generated file whenever practical.

The Python application should use a maintainable templating architecture rather than concatenating large amounts of HTML with uncontrolled string operations.

You may use:

- Python string templates.
  
- Jinja2.
  
- Another appropriate templating mechanism.
  

If you use an external templating library, include it in `requirements.txt` and explain how it is used.

* * *

## 10. Project architecture

Design the Python project using a clean and maintainable architecture.

For example:

```
conSultar-generator/
│
├── main.py
├── config.yaml
├── requirements.txt
├── README.md
│
├── templates/
│   ├── index.html
│   ├── styles.css
│   └── app.js
│
├── generator/
│   ├── __init__.py
│   ├── config.py
│   ├── generator.py
│   ├── templates.py
│   └── validators.py
│
└── output/
    └── consultar.html
```

You may choose a different architecture if you consider it technically superior.

However, the architecture must be:

- Modular.
  
- Easy to understand.
  
- Easy to extend.
  
- Easy to test.
  
- Easy to configure.
  

Avoid putting the entire implementation into one huge Python function.

* * *

## 11. Command-line interface

The Python generator should provide a useful command-line interface.

For example:

```
python main.py
```

should generate the application using the default configuration.

Also support useful options such as:

```
python main.py --config config.yaml
python main.py --output ./dist
python main.py --validate
```

Add other arguments if they provide meaningful functionality.

The CLI should provide clear feedback about:

- Configuration loaded.
  
- Validation status.
  
- Output location.
  
- Generated filename.
  
- Errors and warnings.
  

* * *

## 12. Configuration validation

Before generating the HTML application, validate the configuration.

Detect problems such as:

- Missing required configuration.
  
- Invalid model definitions.
  
- Invalid colors.
  
- Invalid URLs.
  
- Invalid numeric values.
  
- Invalid export formats.
  
- Missing prompt templates.
  
- Invalid filenames.
  
- Unsupported parameters.
  

Display clear and useful error messages.

Do not generate a partially invalid application when a critical configuration error is detected.

* * *

## 13. Separation between generator and generated application

Maintain a strict conceptual separation between:

### Python generator

Responsible for:

- Reading configuration.
  
- Validating configuration.
  
- Loading templates.
  
- Injecting configuration into templates.
  
- Generating the final HTML.
  
- Creating the output directory.
  
- Reporting errors.
  

### Generated web application

Responsible for:

- User interaction.
  
- API communication.
  
- Prompt improvement.
  
- Final query.
  
- Markdown rendering.
  
- Exporting documents.
  
- Displaying errors and status messages.
  

The Python generator itself should not need to execute the AI queries.

Its primary purpose is to **generate the web application**.

* * *

## 14. Security

Pay particular attention to security.

The generated application must not:

- Hard-code API secrets.
  
- Expose private credentials unnecessarily.
  
- Inject unescaped configuration values into HTML.
  
- Introduce avoidable XSS vulnerabilities.
  
- Trust arbitrary Markdown/HTML without appropriate sanitization.
  

If Markdown is rendered as HTML, sanitize the generated HTML when appropriate.

Clearly document the limitations of calling AI APIs directly from a browser, particularly regarding API keys and CORS.

If the architecture requires a backend/proxy for secure API access, explain this limitation and provide an extensible architecture that could support it.

* * *

## 15. Code quality

The Python source code must be:

- Elegant.
  
- Professional.
  
- Modular.
  
- Readable.
  
- Maintainable.
  
- Well structured.
  
- Robust.
  
- Thoroughly commented.
  

**All source-code comments, documentation strings, error messages and configuration explanations must be written in Spanish (Spain).**

Use:

- Type hints where useful.
  
- Descriptive names.
  
- Small, focused functions.
  
- Constants for fixed values.
  
- Proper exception handling.
  
- Logging where appropriate.
  

Avoid:

- Dead code.
  
- Unnecessary duplication.
  
- Extremely long functions.
  
- Magic numbers.
  
- Hard-coded configuration values.
  
- Unexplained implementation decisions.
  

* * *

## 16. Documentation

In addition to the Python application, generate a comprehensive Markdown document named:

```
README.md
```

The README must be written entirely in **Spanish (Spain)**.

It must explain, in detail:

### Installation

- Required Python version.
  
- How to create a virtual environment.
  
- How to activate it.
  
- How to install dependencies.
  
- How to configure the application.
  

### Execution

Explain how to execute the generator from VS Code and from a terminal.

For example:

```
python main.py
```

### Configuration

Explain every configuration parameter.

Include a complete example configuration.

Explain:

- Application settings.
  
- Models.
  
- API settings.
  
- Prompt configuration.
  
- UI configuration.
  
- Theme.
  
- Export formats.
  
- CDN libraries.
  
- Output configuration.
  

### Prompt templates

Explain all supported placeholders, such as:

```
{{consulta}}
{{consulta_mejorada}}
{{rol_usuario}}
```

and provide examples.

### Generated application

Explain how the generated HTML application works.

Describe:

1. User input.
  
2. First AI query.
  
3. Prompt improvement.
  
4. Second AI query.
  
5. Markdown rendering.
  
6. Export process.
  

### Security

Explain:

- API key management.
  
- Browser-based API calls.
  
- CORS.
  
- XSS considerations.
  
- Why secrets should not be embedded in the HTML.
  
- Recommended deployment architectures.
  

### Troubleshooting

Include common problems and their solutions.

* * *

## 17. Example configuration

The README must contain a complete example configuration file.

For example:

```
application:
  name: "ConSultar"
  language: "es-ES"
  output_file: "consultar.html"

api:
  provider: "..."
  endpoint: "..."
  api_key_env: "..."

models:
  default: "..."
  available:
    - id: "..."
      name: "..."

prompts:
  improvement:
    system: "..."
    user: "{{consulta}}"

  final:
    system: "{{rol_usuario}}"
    user: "{{consulta_mejorada}}"

ui:
  title: "ConSultar"
  subtitle: "Mejora y ejecuta tus consultas"

theme:
  primary: "#2563EB"
  background: "#F8FAFC"

exports:
  markdown: true
  word: true
  pdf: true
```

This is only an example. Adapt the actual configuration structure to the implementation you develop.

* * *

## 18. Generated files

The Python generator must produce, at minimum:

```
output/
└── consultar.html
```

and the project must include:

```
main.py
config.yaml
requirements.txt
README.md
```

If additional files are necessary for a clean architecture, include them.

Do not unnecessarily duplicate generated code.

* * *

## 19. Testing

Include a basic testing strategy.

Where practical, provide tests for:

- Configuration loading.
  
- Configuration validation.
  
- Filename validation.
  
- Template generation.
  
- Placeholder substitution.
  
- HTML generation.
  
- Error handling.
  

If you create tests, use Python's standard `unittest` framework or another clearly documented testing framework.

Explain how to run the tests.

* * *

## 20. Final output requirements

Your response must provide the **complete implementation**, not pseudocode.

Provide all files required for the project, clearly separated by filename.

At minimum, provide:

1. `main.py`
  
2. `config.yaml`
  
3. `requirements.txt`
  
4. `README.md`
  

If additional files are required by your architecture, provide them as well.

The generated application must be capable of producing the previously specified `ConSultar` HTML application.

Before presenting the final answer, verify that:

- The Python code is executable.
  
- The project structure is coherent.
  
- Configuration is centralized.
  
- Configuration is validated.
  
- The generator can produce the HTML application.
  
- The generated HTML implements the previously specified functionality.
  
- The two-step AI query workflow is preserved.
  
- Models are configurable.
  
- Prompts are configurable.
  
- API settings are configurable.
  
- The UI is configurable.
  
- The theme is configurable.
  
- Export formats are configurable.
  
- API secrets are not hard-coded.
  
- The application is localized to Spanish (Spain).
  
- The source code comments and documentation are written in Spanish (Spain).
  
- The generated README explains installation, configuration, operation, architecture and troubleshooting.
  

The objective is not merely to produce a Python script that generates HTML, but to create a **professional, reusable, extensible and highly configurable web-application generator** that can be maintained and adapted easily in the future.

This version also makes an important architectural distinction: **Python generates the web application; the generated web application performs the actual AI interaction**. That separation makes the project substantially easier to maintain and extend.