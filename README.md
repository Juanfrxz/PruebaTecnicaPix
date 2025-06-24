# RpaPix

## Descripción
RpaPix es un proceso de Automatización Robótica de Procesos (RPA) basado en la plantilla universal de PIX RPA. Su objetivo general es automatizar el análisis diario de productos disponibles en una tienda online simulada.

**Objetivo General:** Desarrollar un proceso RPA que integre consumo de APIs, almacenamiento en base de datos, generación de reportes en Excel, automatización web y subida de archivos a OneDrive.

**Contexto:** Una empresa ficticia necesita analizar diariamente los productos de su tienda online para generar estadísticas y compartir resultados de forma automatizada en la nube y mediante un formulario web. El flujo consta de:
1. Obtener productos desde Fake Store API.
2. Respaldar la respuesta en JSON y subirla a OneDrive.
3. Almacenar los productos en base de datos.
4. Generar un reporte Excel con estadísticas.
5. Subir el reporte a OneDrive.
6. Enviar el reporte a través de un formulario web.
7. Capturar evidencias del proceso.

## Estructura del proyecto

```
RpaPix/
└── Proyecto/
    ├── Data/
    │   ├── Config.xlsx                  # Archivo de configuración con parámetros iniciales
    │   └── pix-robotics-<clave>.json    # Credenciales de servicio (Google Cloud)
    ├── Exceptions_Screenshots/          # Capturas de pantalla de errores ocurridos en tiempo de ejecución
    ├── Framework/                       # Módulos base de automatización (reutilizables)
    │   ├── InitApplications.pix         # Inicia las aplicaciones necesarias
    │   ├── CloseApplications.pix        # Cierra las aplicaciones al finalizar
    │   ├── DB.pix                       # Conexión y operaciones de base de datos
    │   ├── RequestGet.pix               # Envío de solicitudes HTTP GET genéricas
    │   ├── RequestGetData.pix           # Obtención y parseo de datos JSON
    │   ├── SetTransactionStatus.pix     # Actualización de estado de transacción
    │   ├── TakeScreenshot.pix           # Captura de pantalla para debug y auditoría
    │   └── ...                          # Otros scripts de utilidad
    ├── json/                            # Archivos de salida JSON generados por el proceso
    │   └── Productos_YYYYMMDD_HHMMSS.json
    ├── Main.pix                         # Script principal que orquesta todo el flujo de trabajo
    ├── ProcessTransactionItem.pix       # Subproceso que maneja cada transacción Pix individual
    ├── Proyecto.pixproj                 # Archivo de proyecto de la herramienta RPA utilizada
    └── Reposte/                         # Reportes y resultados finales en formato Excel
        └── Reporte_YYYY-MM-DD.xlsx
```

## Tecnologías utilizadas

- **Herramienta RPA (.pix)**: Plataforma de automatización de procesos que ejecuta los scripts `.pix` y gestiona el proyecto `.pixproj`.
- **Google Cloud JSON**: Archivo de credenciales para acceder a servicios de Google Cloud (por ejemplo, para integrar APIs externas).
- **Microsoft Excel (.xlsx)**: Formato de archivo para configuraciones iniciales (`Config.xlsx`) y generación de reportes finales.
- **JSON**: Formato de intercambio de datos estructurados usado para entradas y salidas de productos.

## Requerimientos Técnicos

1. **Consumo de API Pública**
   - Fuente: Fake Store API (`https://fakestoreapi.com/products`)
   - Guardar respuesta completa en `json/Productos_YYYY-MM-DD.json`
   - Subir el JSON a OneDrive en `/RPA/Logs/Productos_YYYY-MM-DD.json`
   - Extraer campos: id, title, price, category, description

2. **Almacenamiento en Base de Datos**
   - Base de datos SQLite (u otra a elección) con tabla `Productos`
   - Columnas: id (PK), title, price, category, description, fecha_insercion
   - Validar duplicados (no insertar si el id existe)

3. **Generación de Reporte Excel**
   - Archivo `Reporte_YYYY-MM-DD.xlsx` con:
     - Hoja **Productos**: lista completa de productos registrados
     - Hoja **Resumen**:
       - Total de productos
       - Precio promedio general
       - Precio promedio por categoría
       - Cantidad de productos por categoría
   - Guardar localmente en `Reportes/` y subir a OneDrive en `/RPA/Reportes/`

4. **Automatización Web**
   - Completar y enviar un formulario (Google Forms, Jotform o Typeform) con:
     - Nombre del colaborador, fecha de generación, comentarios y subida de archivo
   - Capturar evidencia de confirmación en `Exceptions_Screenshots/formulario_confirmacion.png`

5. **Integración con OneDrive**
   - Usar Microsoft Graph API con autenticación no interactiva
   - Crear rutas si no existen y manejar versiones o sobrescritura
   - Registrar el resultado de la carga en logs

## Cómo ejecutar

1. Abre la herramienta RPA y carga el proyecto `Proyecto.pixproj`.
2. Asegúrate de tener el archivo `Config.xlsx` correctamente configurado en `Data/`.
3. Verifica que la credencial `pix-robotics-<clave>.json` esté disponible y apuntando al servicio deseado.
4. Ejecuta el script `Main.pix` para iniciar el flujo de trabajo completo.
5. Revisa la carpeta `Reposte/` para obtener el reporte generado en formato Excel.
6. Ante errores, consulta las capturas en `Exceptions_Screenshots/`.

