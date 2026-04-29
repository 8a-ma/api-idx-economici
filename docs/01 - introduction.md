# Problema
Actualmente existe una carencia de un punto control centralizado y determinista para la sincronización de indicadores económicos y legales en Chile. La dispersión de fuentes (CNE, SII, DIRECON) y la varibilidad en los tiempos de publicación introducen riesgos de integridad en los cálculos de seguridad social, tributación y contratación indexados, lo que puede derivar en incumplimientos normativos y pérdidas financieras para los actores del sistema.

# Objetivo general
Diseñar y formalizar un sistema de **Sincronización de Estados Económicos Legales** (SEEL) para el mercado chileno, capaz de garantizar una precisión del 100% en la entrega de variables críticas (UF, UTM, IVP, Topes Imponibles y Combustible) mediante el uso de el uso de especificación formal y una arquitectura de acoplamiento mínimo, logrando una disponibilidad operativa de grado industrial antes del término del ciclo actual.

# Objetivos específicos
## 1. Modelamiento de la Persistencia Multivariante
Implementar un almacén de datos normalizado que gestione la serie de tiempo de indicadores (UF, UTM, IVP, IPC, UTA) con una granularidad diaria, permitiendo la recuperación de estados históricos con una complejidad computacional de $O(1)$ o $O(\log n)$.

## 2. Desarrollo de Motores de Ingesta Resilientes
Diseñar una arquitectura de extracción doble (API oficial + Scrapers de contingencia) con validación cruzada, asegurando que la actualización de los indicadores del día $t$ se realice antes de las 08:00 AM CLT con un nivel de confianza del $99.9\%$.

## 3. Formalización del Motor de Cálculo Contractual
Establecer las funciones aritméticas para el reajuste y cálculo de mora mediante métodos formales, garantizando que el redondeo y la aplicación de tasas de interés compuestas sigan estrictamente la normativa de la Superintendencia de Bancos e Instituciones Financieras (SBIF) y el Banco Central.

## 4. Exposición de Interfaces de Servicio (Endpoints)
Implementar una capa de servicios REST que exponga la lógica de negocio mediante tres módulos críticos:
    - **Módulo de Consulta Cronológica:** Obtención de escalares financieros por fecha.
    - **Módulo de Conversión Dinámica:** Transformación de unidades monetarias indexadas en tiempo real.
    - **Módulo de Proyección y Reajuste:** Cálculo de montos finales considerando delta de tiempo y tasas de interés variables.
