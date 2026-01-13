# Analisis-de-gastos-pipeline
Proceso que saca datos claves sobre gastos, desde correos electrónicos, los analiza con IA y sube a una hoja de cálculo a google sheets.

📧 Email Invoice Processor → Google Sheets.  
Este proyecto automatiza la lectura de correos electrónicos con facturas, extrae información relevante usando LLMs vía Ollama, procesa los datos y los carga automáticamente en Google Sheets para llevar control de gastos.
Está pensado principalmente para correos de facturación (por ejemplo, notificaciones de compras) recibidos en Gmail, pero puede adaptarse a otros proveedores IMAP.
🚀 Funcionalidades principales
📥 Lectura de correos no leídos desde una carpeta específica (ej. Facturas)
🧠 Extracción inteligente de datos usando un modelo LLM local (Ollama):
Comercio
Fecha
Hora
Monto
Moneda
Anulación de factura
🧹 Limpieza y normalización de datos
Manejo de tildes
Conversión de fechas
Montos negativos para anulaciones
Unificación de monedas (CRC / USD)
📊 Carga automática en Google Sheets
Inserta los datos en la siguiente fila disponible
No sobrescribe información existente
🔁 Flujo completamente automatizado con un solo método (ejecutar())
🧱 Arquitectura del proyecto
El proyecto se basa en la clase:
EmailProcessor
Que implementa 4 etapas principales:
Lectura de correos (get_unread_emails)
Procesamiento con IA (procesar_correos)
Carga a Google Sheets (subir_datos_a_gsheets)
Ejecución completa del flujo (ejecutar)
📦 Dependencias
Asegúrate de tener instaladas las siguientes librerías:
pip install polars pandas numpy beautifulsoup4 gspread ollama
También necesitas:
Python 3.9+
Una cuenta de Google con acceso a Google Sheets
Credenciales de Google Service Account
Ollama instalado y corriendo localmente
🧠 Modelo de IA utilizado
El script utiliza el modelo:
gemma3:4b
A través de Ollama para interpretar el texto de las facturas.
⚠️ Asegúrate de tener el modelo descargado:
ollama pull gemma3:4b
Puedes cambiar el modelo modificando la variable Modelo dentro del método procesar_correos.
🔐 Configuración previa
1️⃣ Gmail
Habilita contraseñas de aplicación
Usa IMAP
Crea una carpeta específica (ej. Facturas)
2️⃣ Google Sheets
Crea una hoja con una pestaña llamada Base
Debe tener al menos las columnas:
Gasto
Fecha
3️⃣ Credenciales Google
Crea un Service Account
Descarga el archivo JSON
Comparte la hoja de Google Sheets con el email del Service Account
▶️ Uso
Ejemplo de ejecución:
if __name__ == "__main__":
    processor = EmailProcessor(
        imap_server="imap.gmail.com",
        email_address="correo@gmail.com",
        app_password="APP_PASSWORD",
        gsheets_key="ID_DE_GOOGLE_SHEET",
        credenciales_path="credenciales.json",
        folder="Facturas"
    )

    processor.ejecutar()
Al ejecutarse:
Lee correos no leídos
Extrae y procesa los datos
Inserta los registros en Google Sheets
Deja los correos como leídos
📤 Output final
El resultado final que se sube a Google Sheets contiene las columnas:
Comercio
Monto_CRC
Monto_USD
Fecha
⚠️ Consideraciones
El procesamiento con LLM puede ser lento si hay muchos correos
El modelo puede fallar en facturas con formatos poco comunes
Se recomienda revisar periódicamente los datos generados
🛠 Posibles mejoras futuras
Manejo de adjuntos PDF
Conversión automática USD → CRC
Etiquetado automático de correos procesados
Soporte multi-hoja
Logs estructurados
📄 Licencia
Uso libre para fines personales o educativos.
Si lo usas en producción, hazlo bajo tu propia responsabilidad.
Si quieres, puedo:
Ajustarlo a inglés.
