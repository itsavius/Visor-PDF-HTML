# Visor-PDF-HTML

# Version local
Agregar tipos MIME

Debido a que hay archivos  .mjs

En Hosting > Apache
```txt
Fluent Localization > Traducciones de la interfaz del visor
AddType text/plain .ftl
JavaScript Module > Código principal de la librería y el worker
AddType text/javascript .mjs
Source Map > Depuración
AddType application/json .map
WebAssembly > Acelerar decodificación de imágenes y tareas pesadas
AddType application/wasm .wasm
```


En ASP > Web.config
```xml
<!-- Error 404.3 (Not Found) -->
<configuration>
  <system.webServer>
    <staticContent>
		<remove fileExtension=".mjs" />
		<mimeMap fileExtension=".mjs" mimeType="text/javascript" />
		<remove fileExtension=".map" />
		<mimeMap fileExtension=".map" mimeType="application/json" />
		<remove fileExtension=".ftl" />
		<mimeMap fileExtension=".ftl" mimeType="text/plain" />
		<remove fileExtension=".wasm" />
		<mimeMap fileExtension=".wasm" mimeType="application/wasm" />
    </staticContent>
  </system.webServer>
</configuration>
<!--Proteje Source Maps .map -->
<location path="js/secret-folder">
  <system.webServer>
    <security>
      <authorization>
        <add accessType="Deny" users="?" /> </authorization>
    </security>
  </system.webServer>
</location>
```

# Formas de importar
```js
  	<script type="module">
	//Opcion 1 CDN
	import * as pdfjsLib from 'https://mozilla.github.io/pdf.js/build/pdf.mjs';
	//Worker CDN
    pdfjsLib.GlobalWorkerOptions.workerSrc =
    'https://mozilla.github.io/pdf.js/build/pdf.worker.min.mjs';
	
	//Opcion 2 CDN
	var { pdfjsLib } = globalThis;
	//Worker CDN
	pdfjsLib.GlobalWorkerOptions.workerSrc ='https://mozilla.github.io/pdf.js/build/pdf.worker.min.mjs';
	</script>
```	

* v1
No funciona - Requiere archivos locales
* v2
No funciona - Requiere archivos locales
* v3
VisorPDF-v3.html?file=Example.pdf
* v4
VisorPDF-v4.html?file=Example.pdf
* v5
VisorPDF-v5.html?file=Example.pdf
* v6
VisorPDF-v6.html?file=Example.pdf
* v7
VisorPDF-v7.html?file=Example.pdf
* v8
VisorPDF-v8.html?file=Example.pdf
* v9
Remplaza archivo default VisorPDF-v9.html
También acepta VisorPDF-v9.html?file=Example.pdf
* v10
Remplaza archivo default VisorPDF-v10.html
También acepta VisorPDF-v10.html?file=Example.pdf
* v11 - OK
Remplaza archivo default VisorPDF-v11.html
También acepta VisorPDF-v11.html?file=Example.pdf
* v12 - OK
Remplaza archivo default VisorPDF-v12.htm
También acepta VisorPDF-v12.html?file=Example.pdf
* vExtra
Permite Arrastrado archivo VisorPDF-renew.html
VisorPDF-renew.html?file=Example.pdf


# Notas

Opción 1 (pdfjsLib.getDocument): Esto carga el PDF en la memoria "invisible" de JavaScript. No le avisa al Visor (la interfaz con botones) que debe dibujarlo. Es como comprar los ingredientes pero no encender la estufa.
```js
    <script>
	pdfjsLib.getDocument('Example.pdf');
    </script>
```	
    
Opción 2 (PDFViewerApplicationOptions): Es la correcta, pero a veces el navegador guarda en la "caché local" (LocalStorage) que el último archivo abierto fue el de Mozilla, y le da prioridad a eso. Por eso añadí disablePreferences: true.
```js
    <script>
	window.PDFViewerApplicationOptions = {
	defaultUrl: "Example.pdf",
	disablePreferences: true 
	};
    </script>
```	
Opción 3 (webviewerloaded): En las versiones modernas de PDF.js (como la que usas de mozilla.github.io), el objeto PDFViewerApplication no es global de inmediato porque es un Módulo ES6.
```js
    <script>
// Para cambiar el PDF, modifica esta variable o pásala por URL: ?file=URL
    const PDF_DEFAULT_URL = 'Example.pdf';

    // Lee el parámetro ?file= de la URL si existe
    const urlParams   = new URLSearchParams(window.location.search);
    const PDF_URL     = urlParams.get('file') || PDF_DEFAULT_URL;

    // ── Esperar a que PDF.js esté completamente listo ────────────
    // 'webviewerloaded' es el evento oficial que dispara viewer.mjs
    // cuando PDFViewerApplication ya está inicializado
    document.addEventListener('webviewerloaded', () => {
      PDFViewerApplication.initializedPromise.then(() => {
        PDFViewerApplication.open({ url: PDF_URL });
      });
    });
    </script>
```	

# Parametros iFrame
<iframe src="https://mozilla.github.io/pdf.js/web/viewer.html#page=1&zoom=auto&pagemode=thumbs&search=&phrase=true" height="600" width="1152" frameborder="0" />
