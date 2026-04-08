# Visor-PDF-HTML

Si se utiliza la version local
Agregar tipos MIME

Debido a que hay archivos  .mjs


En ASP > Web.config
<!-- Error 404.3 (Not Found) -->
<configuration>
  <system.webServer>
    <staticContent>
      <remove fileExtension=".mjs" />
      <remove fileExtension=".map" />
      <mimeMap fileExtension=".mjs" mimeType="text/javascript" />
      <mimeMap fileExtension=".map" mimeType="application/json" />
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

Opción 1 (pdfjsLib.getDocument): Esto carga el PDF en la memoria "invisible" de JavaScript. No le avisa al Visor (la interfaz con botones) que debe dibujarlo. Es como comprar los ingredientes pero no encender la estufa.

Opción 2 (PDFViewerApplicationOptions): Es la correcta, pero a veces el navegador guarda en la "caché local" (LocalStorage) que el último archivo abierto fue el de Mozilla, y le da prioridad a eso. Por eso añadí disablePreferences: true.

Opción 3 (webviewerloaded): En las versiones modernas de PDF.js (como la que usas de mozilla.github.io), el objeto PDFViewerApplication no es global de inmediato porque es un Módulo ES6.



