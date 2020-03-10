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

**id**
la identificación externa del registro generado

**name** (obligatorio)
solo es útil como mnemónico / descripción del informe cuando se busca uno en una lista de algún tipo.

**model** (obligatorio)
el modelo sobre el que tratará su informe

**report_type** (obligatorio)
qweb-pdf para informes en PDF o qweb-html para HTML

**report_name**
el nombre de su informe (que será el nombre de la salida PDF)

**groups**
Many2many campo para los grupos autorizados para ver / usar el informe actual

**attachment_use**
si se establece en True, el informe se almacenará como un archivo adjunto del registro utilizando el nombre generado por la expresión del archivo adjunto; puede usar esto si necesita que su informe se genere solo una vez (por razones legales, por ejemplo)

**attachment**
expresión de python que define el nombre del informe; el registro es accesible como el objeto variable

**paperformat**
Identificación externa del formato de papel que desea utilizar (por defecto es el formato de papel de la empresa si no se especifica)

**Ejemplo**
```
<report
    id="account_invoices"
    model="account.invoice"
    string="Invoices"
    report_type="qweb-pdf"
    name="account.report_invoice"
    file="account.report_invoice"
    attachment_use="True"
    attachment="(object.state in ('open','paid')) and
        ('INV'+(object.number or '').replace('/','')+'.pdf')"
/>
```

## Plantilla de Reporte

Plantilla mínima viable

Una plantilla mínima se vería así:

```
<template id="report_invoice">
    <t t-call="web.html_container">
        <t t-foreach="docs" t-as="o">
            <t t-call="web.external_layout">
                <div class="page">
                    <h2>Report title</h2>
                    <p>This object's name is <span t-field="o.name"/></p>
                </div>
            </t>
        </t>
    </t>
</template>
```
llamando **external_layout** agregará el encabezado y pie de página predeterminados en su informe. El cuerpo del PDF será el contenido dentro del **<div class="page">**. La plantilla es **id** debe ser el nombre especificado en la declaración del informe; por ejemplo **account.report_invoice** para el informe anterior. Como se trata de una plantilla QWeb, puede acceder a todos los campos de **docs** objetos recibidos por la plantilla.
    
Hay algunas variables específicas accesibles en los informes, principalmente:
**docs**
registros para el informe actual
**doc_ids**
lista de identificadores para los registros de docs
**doc_model**
modelo para los registros de docs
**time**
una referencia al tiempo de la biblioteca estándar de Python
**user**
**res.user** Registro para la usuario que imprime el informe
**res_company**
registro de la empresa del usuario actual

Si desea acceder a otros registros / modelos en la plantilla, necesitará un informe personalizado.

