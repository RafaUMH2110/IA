Excelente iniciativa, Rafa. Transformar su cuaderno de Colab en una aplicación web monolítica no solo democratiza el acceso para sus alumnos (eliminando la fricción del entorno Jupyter y la gestión manual de `pip install`), sino que demuestra pedagógicamente la arquitectura *serverless* real detrás de las APIs de IA.

Dado que su práctica actual utiliza **Anthropic**, he preseleccionado este proveedor en la configuración, manteniendo la flexibilidad multi-proveedor que exige el documento maestro. He asumido que la salida óptima es un **JSON estructurado** (para renderizar como tabla técnica de patronaje) junto con una descripción narrativa, ya que los patrones de corte exigen precisión numérica y trazabilidad.

A continuación, presento el **BLOQUE 0 cumplimentado** y la ejecución íntegra de las 5 fases solicitadas por su prompt maestro.

* * *

### BLOQUE 0 · CONFIGURACIÓN DE ESTA APP

- **Nombre de la app:** `ModaPatron IA`
- **Objetivo en una frase:** Generar patrones de corte técnicos, detallados y listos para producción a partir de especificaciones textuales de prendas de moda.
- **Usuario final:** Estudiantes de 3º del grado en Gestión, Tecnología y Moda.
- **Rol del modelo (system prompt):** Actúa como un patrónista senior experto en industrialización textil, con tono riguroso, técnico y didáctico, siguiendo siempre estas reglas: usa terminología técnica de patronaje (pines, piquetes, hilos de trama, márgenes de costura), justifica cada decisión técnica brevemente, nunca inventes medidas sin basarlas en tallas estándar UNE-EN 13402, responde SIEMPRE en español y jamás omitas la tabla de medidas ni la lista de piezas.
- **Campos del formulario:**
  - Tipo de prenda → `select` → obligatorio → default: "Vestido"
  - Talla objetivo → `select` → obligatorio → default: "M (38-40)"
  - Especificación detallada → `textarea` → obligatorio → placeholder: "Describa tejido, caída, tipo de cierre, manga, largo exacto..."
  - Nivel de detalle → `select` → opcional → default: "Industrial completo"
- **Formato exacto de salida:** JSON con esquema estricto (ver Fase 1).
- **Proveedor por defecto:** Anthropic — **Modelo por defecto:** claude-sonnet-4-20250514
- **Extras opcionales:** Exportar a PDF con jsPDF + marcado con marked.js para la justificación técnica.

* * *

### FASE 1 · ESPECIFICACIÓN

1. **Objetivo refinado:** Traducir descripciones naturales de diseño de moda en fichas técnicas de patronaje industrialmente viables mediante razonamiento experto de LLM.
2. **System prompt literal:** *"Eres un patrónista senior con 25 años de experiencia en industrialización textil. Tu tarea es generar patrones de corte precisos. Reglas inquebrantables: 1) Usa exclusivamente terminología técnica de patronaje español. 2) Todas las medidas deben derivarse de la norma UNE-EN 13402 para la talla solicitada. 3) Incluye siempre márgenes de costura de 1cm y dobladillo de 3cm salvo indicación contraria. 4) Justifica técnicamente cualquier desviación del patrón base. 5) Responde ÚNICAMENTE con el JSON especificado, sin markdown fences ni texto adicional."*
3. **Campos definitivos:** Ver Bloque 0. Todos validados en cliente antes de habilitar el botón.
4. **Esquema JSON de salida:**
{
  "ficha_tecnica": {
    "prenda": "string",
    "talla": "string",
    "tejido_asumido": "string"
  },
  "piezas": [
    {
      "nombre": "string",
      "cantidad": "integer",
      "medidas_clave_cm": {"ancho": "number", "largo": "number"},
      "notas_patronaje": "string"
    }
  ],
  "justificacion_tecnica": "string",
  "orden_corte_recomendado": ["string"]
}
5. **Casos límite cubiertos:** Respuesta con fences markdown (se limpian regex), JSON truncado (reintento único autocrítico), campos vacíos (bloqueo UI), API key inválida (validación prefijo pre-petición).

* * *

### FASES 2-5 · ENTREGA DEL FICHERO HTML COMPLETO

> La app permite a estudiantes introducir especificaciones de moda y obtener fichas de patronaje estructuradas con costes transparentes. Soporta Anthropic nativamente con fallback a OpenAI/Gemini. Todo reside en un único index.html desplegable en GitHub Pages sin backend propio.
>
> <!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ModaPatron IA</title>
<!-- 
  ═══════════════════════════════════════════════════════════
  GUÍA PARA EL PROFESOR / ALUMNO
  ═══════════════════════════════════════════════════════════
  1. OBTENER API KEY:
     • Anthropic: https://console.anthropic.com/settings/keys
     • OpenAI:    https://platform.openai.com/api-keys
     • Gemini:    https://aistudio.google.com/app/apikey
  
  2. PUBLICAR EN GITHUB PAGES:
     Suba este fichero como 'index.html' a un repo público.
     Settings > Pages > Deploy from branch > main / root.
  
  3. PERSONALIZACIÓN DOCENTE:
     Busque las constantes marcadas con [ADAPTAR] para modificar:
     - SYSTEM_PROMPT: cambie el rol experto según la asignatura.
     - MODELOS: añada modelos nuevos cuando se publiquen.
     - PRECIOS: actualice la tabla USD/1M tokens trimestralmente.
  ═══════════════════════════════════════════════════════════
-->
<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
  :root{--bg:#faf9f7;--surface:#fff;--accent:#8b6f47;--text:#2c2c2c;--border:#e0dcd3;--err:#c0392b;--ok:#27ae60}
  *{box-sizing:border-box;margin:0;padding:0}
  body{font-family:'Segoe UI',system-ui,sans-serif;background:var(--bg);color:var(--text);line-height:1.6;padding:2rem 1rem}
  .container{max-width:900px;margin:0 auto}
  header{text-align:center;margin-bottom:2rem;border-bottom:2px solid var(--accent);padding-bottom:1rem}
  h1{font-weight:300;letter-spacing:2px;color:var(--accent)}
  .card{background:var(--surface);border:1px solid var(--border);border-radius:8px;padding:1.5rem;margin-bottom:1.5rem;box-shadow:0 2px 8px rgba(0,0,0,.04)}
  label{display:block;font-size:.85rem;font-weight:600;text-transform:uppercase;letter-spacing:.5px;margin-bottom:.4rem;color:#666}
  input,select,textarea{width:100%;padding:.7rem;border:1px solid var(--border);border-radius:4px;font-size:1rem;background:#fdfcfa}
  textarea{min-height:120px;resize:vertical;font-family:inherit}
  .row{display:grid;grid-template-columns:1fr 1fr;gap:1rem}
  @media(max-width:600px){.row{grid-template-columns:1fr}}
  .key-row{display:flex;gap:.5rem;align-items:end}
  .key-row input{flex:1}
  button{cursor:pointer;border:none;border-radius:4px;padding:.7rem 1.2rem;font-size:.95rem;font-weight:600;transition:all .2s}
  .btn-primary{background:var(--accent);color:#fff;width:100%}
  .btn-primary:disabled{background:#ccc;cursor:not-allowed}
  .btn-secondary{background:transparent;border:1px solid var(--border);color:var(--text)}
  .btn-secondary:hover{background:var(--bg)}
  .status{font-size:.85rem;margin-top:.5rem;min-height:1.2em}
  .status.ok{color:var(--ok)}.status.err{color:var(--err)}
  #result{white-space:pre-wrap}
  table{width:100%;border-collapse:collapse;margin:1rem 0;font-size:.9rem}
  th,td{border:1px solid var(--border);padding:.5rem;text-align:left}
  th{background:var(--bg);font-weight:600}
  .spinner{display:none;width:20px;height:20px;border:3px solid var(--border);border-top-color:var(--accent);border-radius:50%;animation:spin .8s linear infinite;margin:0 auto}
  @keyframes spin{to{transform:rotate(360deg)}}
  .disclaimer{font-size:.8rem;color:#888;text-align:center;padding:1rem;border-top:1px dashed var(--border);margin-top:2rem}
  .export-bar{display:flex;gap:.5rem;justify-content:flex-end;margin-top:1rem}
</style>
</head>
<body>
<div class="container">
  <header><h1>ModaPatron IA</h1><p>Generador de fichas técnicas de patronaje</p></header>

  <!-- GESTIÓN API KEYS -->
  <div class="card">
    <div class="row">
      <div>
        <label for="provider">Proveedor</label>
        <select id="provider">
          <option value="anthropic" selected>Anthropic</option>
          <option value="openai">OpenAI</option>
          <option value="gemini">Google Gemini</option>
        </select>
      </div>
      <div>
        <label for="model">Modelo</label>
        <select id="model"></select>
      </div>
    </div>
    <div style="margin-top:1rem">
      <label for="apikey">API Key del proveedor activo</label>
      <div class="key-row">
        <input type="password" id="apikey" placeholder="Pegue su clave aquí...">
        <button class="btn-secondary" onclick="guardarKey()">Guardar</button>
        <button class="btn-secondary" onclick="borrarKey()">Borrar</button>
      </div>
      <div id="keyStatus" class="status"></div>
    </div>
  </div>

  <!-- FORMULARIO -->
  <div class="card">
    <div class="row">
      <div>
        <label for="tipo">Tipo de prenda *</label>
        <select id="tipo">
          <option>Vestido</option><option>Blazer</option><option>Camisa</option>
          <option>Pantalón</option><option>Falda</option><option>Abrigo</option>
        </select>
      </div>
      <div>
        <label for="talla">Talla objetivo *</label>
        <select id="talla">
          <option>XS (34)</option><option>S (36)</option>
          <option selected>M (38-40)</option><option>L (42-44)</option><option>XL (46-48)</option>
        </select>
      </div>
    </div>
    <div style="margin-top:1rem">
      <label for="spec">Especificación detallada *</label>
      <textarea id="spec" placeholder="Tejido, caída, tipo de cierre, manga, largo exacto, estilo de cuello, detalles de confección..."></textarea>
    </div>
    <div style="margin-top:1rem">
      <label for="detalle">Nivel de detalle</label>
      <select id="detalle">
        <option>Boceto conceptual</option>
        <option selected>Industrial completo</option>
        <option>Alta costura con ajustes</option>
      </select>
    </div>
    <button id="btnGen" class="btn-primary" style="margin-top:1.5rem" disabled onclick="generar()">Generar Patrón</button>
  </div>

  <!-- RESULTADO -->
  <div class="card" id="resultCard" style="display:none">
    <div class="spinner" id="spinner"></div>
    <div id="result"></div>
    <div class="export-bar" id="exportBar" style="display:none">
      <button class="btn-secondary" onclick="copiarResultado()">📋 Copiar JSON</button>
      <button class="btn-secondary" onclick="exportarPDF()">📄 Exportar PDF</button>
    </div>
    <div id="costInfo" style="font-size:.8rem;color:#888;margin-top:1rem;text-align:right"></div>
  </div>

  <p class="disclaimer">⚠️ La IA puede cometer errores. Revisa el resultado antes de usarlo; esta herramienta no sustituye el criterio profesional del patrónista.</p>
</div>

<script>
/* ═══════════════════════════════════════════════════════════
   [ADAPTAR] System Prompt — Modifique según su asignatura
   ═══════════════════════════════════════════════════════════ */
const SYSTEM_PROMPT = `Eres un patrónista senior con 25 años de experiencia en industrialización textil. Tu tarea es generar patrones de corte precisos. Reglas inquebrantables: 1) Usa exclusivamente terminología técnica de patronaje español. 2) Todas las medidas deben derivarse de la norma UNE-EN 13402 para la talla solicitada. 3) Incluye siempre márgenes de costura de 1cm y dobladillo de 3cm salvo indicación contraria. 4) Justifica técnicamente cualquier desviación del patrón base. 5) Responde ÚNICAMENTE con el JSON especificado, sin markdown fences ni texto adicional.`;

/* ═══════════════════════════════════════════════════════════
   [ADAPTAR] Modelos disponibles por proveedor
   ═══════════════════════════════════════════════════════════ */
const MODELOS = {
  anthropic:['claude-sonnet-4-20250514','claude-3-5-haiku-20241022','claude-3-opus-20240229'],
  openai:['gpt-4o','gpt-4o-mini','gpt-4-turbo'],
  gemini:['gemini-1.5-pro','gemini-1.5-flash']
};

/* ═══════════════════════════════════════════════════════════
   [ADAPTAR] Precios USD por millón de tokens (actualizar trim.)
   ═══════════════════════════════════════════════════════════ */
const PRECIOS = {
  'claude-sonnet-4-20250514':{in:3,out:15},'claude-3-5-haiku-20241022':{in:0.8,out:4},
  'gpt-4o':{in:2.5,out:10},'gpt-4o-mini':{in:0.15,out:0.6},
  'gemini-1.5-pro':{in:1.25,out:5},'gemini-1.5-flash':{in:0.075,out:0.3}
};

let ultimoResultado = null;
const $ = id => document.getElementById(id);

/* ── Inicialización y persistencia de keys ── */
function init(){
  $('provider').addEventListener('change',()=>{renderModels();cargarKey();validar()});
  $('model').addEventListener('change',validar);
  ['tipo','talla','spec'].forEach(id=>$(id).addEventListener('input',validar));
  renderModels();cargarKey();
}

function renderModels(){
  const prov=$('provider').value;
  $('model').innerHTML=MODELOS[prov].map(m=>`<option value="${m}">${m}</option>`).join('')+
    `<option value="__otro__">Otro…</option>`;
  if(prov==='anthropic')$('model').value='claude-sonnet-4