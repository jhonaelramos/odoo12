# Reportes QWeb

Los informes se escriben en HTML / QWeb, como todas las vistas regulares en Odoo. 
Puede usar las herramientas habituales de control de flujo QWeb.
La representación de PDF en sí es realizada por wkhtmltopdf.

Si desea crear un informe sobre un determinado modelo, 
deberá definir este Informe y la plantilla de Informe que utilizará. 
Si lo desea, también puede especificar un formato de papel específico para este informe. 
Finalmente, si necesita acceder a más de su modelo, puede definir una clase de Informes 
personalizados que le da acceso a más modelos y registros en la plantilla.

## Reporte


Cada informe debe ser declarado por una acción de informe.

Para simplificar, un elemento de acceso directo <report> 
está disponible para definir un informe, 
en lugar de tener que configurar la acción y sus alrededores manualmente. 
Ese <report> puede tomar los siguientes atributos:
