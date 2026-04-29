# Funcional
## 1. Módulo de Adquisición y Sincronización (MAS)
RF-1.1: Ingesta Multi-fuente: El sistema deberá extraer datos mediante APIs oficiales y, en su defecto, **scrapers** automatizados para los indicadores UF, IVP, UTM, IPC, UTA, Sueldo Mínimo, Topes Imponibles y Combustibles por comuna.

RF-1.2: Validación de Integridad: El sistema debe contrastar el valor obtenido contra una regla de desviación máxima. Si un valor varía más de un $\sigma$ (desviación estándar) respecto al promedio histórico reciente, el sistema debe marcar el dato como "Pendiente de Validación Humana".

RF-1.3: Sincronización Cronometrada: La actualización de indicadores diarios debe completarse antes de las 08:00 AM CLT.

## 2. Módulo de Servicios de Consulta (MSC)
RF-2.1: Consulta Puntual: El sistema debe retornar el valor escalar de cualquier indicador para una fecha específica $t$ (pasada o presente).

RF-2.2: Consulta de Series Temporales: El sistema debe permitir la extracción de un rango de valores $[t_1, t_2]$ para análisis de tendencias o auditorías.

RF-2.3: Metadatos de Fuente: Cada respuesta de la API debe incluir el origen del dato y la marca de tiempo de la última sincronización para asegurar la trazabilidad legal.

## 3. Módulo de Lógica Contractual y Cálculo (MLC)
RF-3.1: Conversión Transversal: El sistema debe realizar la conversión entre dos unidades cualesquiera (ej. UTM a UF) utilizando la fecha de referencia proporcionada por el cliente.

RF-3.2: Reajuste de Montos: El sistema debe calcular el valor presente de una deuda histórica aplicando la variación de la unidad seleccionada entre dos fechas $t_{inicio}$ y $t_{final}$.

RF-3.3: Aplicación de Interés Compuesto: El sistema debe permitir integrar una tasa de interés mensual (o anual) al cálculo de reajuste, siguiendo la convención de cálculo de 30 días o días reales según se parametrice.

## 4. Módulo de Administración y Supervisión (MAS)
RF-4.1: Override Manual: El administrador podrá corregir o ingresar manualmente un indicador en caso de caída masiva de fuentes externas o errores en la publicación oficial.

RF-4.2: Auditoría de Cambios: El sistema debe registrar un log inalterable de quién, cuándo y por qué se modificó un valor manualmente.

# Especificación de Interacción Humano-Sistema (HCI)
El diseño de la interfaz se rige por el principio de **Defensa en Profundidad** para minimizar el riesgo de que un cliente de la API procese un dato erróneo.

## 1. Ergonomía del Dato y Semántica REST
HCI-1.1: Codificación de Estados de Veracidad: La API no solo entregará el valor, sino un campo `status_verification` que puede ser `VERIFIED` (fuente oficial), `EXTRAPOLATED` (calculado por el sistema) o `MANUAL_OVERRIDE`. Esto permite al sistema cliente decidir si confía en el dato para operaciones críticas.

HCI-1.2: Invarianza de Formato (ISO 8601): Todas las fechas de entrada y salida deben seguir el estándar `YYYY-MM-DD`. No se aceptarán formatos regionales para evitar errores de interpretación mes/día.

HCI-1.3: Precisión Numérica Explícita: Los valores monetarios se entregarán como strings o decimals de alta precisión para evitar errores de redondeo de punto flotante en el lado del cliente.

## 2. Gestión de Fallos e Interlocks
HCI-2.1: Mensajería de Error: En lugar de errores genéricos (e.g., "Error 500"), la API proporcionará códigos de error diagnósticos (e.g., `ERR_SOURCE_DEGRADED`, `ERR_DATE_OUT_OF_BOUNDS`) detallando la causa raíz.

HCI-2.2: Mecanismo de Safe-State: Si el sistema detecta que un indicador crítico (como la UF) no ha sido actualizado, el HCI del administrador debe disparar una alerta proactiva (Push/Email) antes de que el primer cliente consulte el dato fallido.

## 3. Roles y Responsabilidades
Consumidor (Sistemas Externos): El HCI debe garantizar que el desarrollador externo integre la API en menos de 10 minutos mediante una documentación autogenerada (OpenAPI/Swagger) que cumpla con el rigor de un manual técnico industrial.

