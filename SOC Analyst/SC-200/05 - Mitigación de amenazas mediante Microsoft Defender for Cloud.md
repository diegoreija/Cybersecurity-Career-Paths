# 🛡️ MANUAL DE MITIGACIÓN DE AMENAZAS MEDIANTE MICROSOFT DEFENDER FOR CLOUD

### *Guía de Referencia Operativa, Gestión de Postura (CSPM) y Protección de Cargas de Trabajo (CWP) — Basada Estrictamente en la Fuente Oficial*

---

`Difficulty: Beginner` | `Platform: Microsoft Defender for Cloud` | `Category: Cloud Security (CSPM &amp; CWP)` | `Frameworks: MCSB | Azure Arc` | `Source: Notion / Official Documentation`

---

&gt; **Nota Metodológica de Cobertura:** Este documento constituye la síntesis completa y detallada de la guía oficial sobre mitigación de amenazas mediante Microsoft Defender for Cloud. Toda la información contenida en este manual se deriva estrictamente de los contenidos y conceptos presentados en la fuente de origen, manteniendo la máxima fidelidad conceptual, procesal y terminológica sin incorporar suposiciones ni elementos externos.

---

## 🚀 Matriz de Consulta Rápida: Componentes, Planes y Capacidades Defensivas

| Módulo / Elemento               | Concepto o Mecanismo Clave         | Componentes / Definición Operativa                                                                                           | Resultado u Objetivo Principal                                                                             |
| ------------------------------- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Fundamentos**                 | Gestión de Postura y Protección    | Integración de pilares CSPM (Cloud Security Posture Management) y CWP (Cloud Workload Protection).                           | Reforzar la seguridad en Azure, entornos híbridos y plataformas multicloud (AWS y GCP).                    |
| **Pilar CSPM**                  | Postura de Seguridad               | Visibilidad continua sobre la situación de seguridad actual y guías de protección.                                           | Identificar configuraciones deficientes y proporcionar recomendaciones de mejora eficaz.                   |
| **Pilar CWP**                   | Protección de Cargas de Trabajo    | Alertas de seguridad impulsadas por Microsoft Threat Intelligence específicas por recurso.                                   | Proporcionar defensas avanzadas para servidores, bases de datos, contenedores y App Service.               |
| **Despliegue Nativo**           | Aprovisionamiento Automático       | Protección por defecto en servicios PaaS de Azure (Service Fabric, SQL Database, Cuentas de almacenamiento).                 | Supervisión e inventario continuo sin necesidad de implementaciones o agentes adicionales.                 |
| **Extensión Híbrida**           | Integración con Azure Arc          | Conexión de servidores y máquinas virtuales fuera de Azure (AWS, GCP, servidores locales).                                   | Habilitar Azure Policy, inventario centralizado y recomendaciones de seguridad personalizadas.             |
| **Defender Servidores: Plan 1** | Endpoint Protection Básico         | Despliegue de Microsoft Defender for Endpoint en servidores con facturación por hora.                                        | Visualización de vulnerabilidades y telemetría de servidores directamente en Defender for Cloud.           |
| **Defender Servidores: Plan 2** | Protección Avanzada                | Incluye Plan 1, Agentless scanning, File Integrity Monitoring (FIM), JIT VM Access y 500 MB/día de ingesta en Log Analytics. | Evaluación profunda de vulnerabilidades, inventario sin agente y control de acceso Just-In-Time.           |
| **Agentless Scanning**          | Escaneo Sin Agente                 | Evaluación de discos y volumen de servidores en el Plan 2 sin agentes en el punto de conexión.                               | Detección de inventario de software, análisis de seguridad, vulnerabilidades y credenciales no protegidas. |
| **Líneas Base de Seguridad**    | Microsoft Cloud Security Benchmark | Evaluación mediante la extensión Azure Machine Configuration, auditd (Linux) y servicios nativos (Windows).                  | Garantizar el cumplimiento continuo frente a las directrices de seguridad oficiales de Microsoft.          |
| **Defender para App Service**   | Protección de Web Apps y APIs      | Análisis de registros internos, peticiones/respuestas HTTP, interfaz de administración y VM subyacente.                      | Identificar exámenes generalizados, ataques distribuidos y accesos no autorizados en entornos Windows.     |

---

## 1\. Fundamentos y Arquitectura de Microsoft Defender for Cloud

Microsoft Defender for Cloud se presenta como una solución integral diseñada para administrar la postura de seguridad y proporcionar protección avanzada contra ciberamenazas. Su objetivo fundamental es reforzar la seguridad de los activos de información alojados tanto en la infraestructura nativa de Azure como en entornos híbridos y plataformas multicloud, incluyendo AWS (Amazon Web Services) y GCP (Google Cloud Platform). La arquitectura del sistema se fundamenta en la integración de dos pilares esenciales de la seguridad en la nube: la Gestión de la Postura de Seguridad en la Nube (Cloud Security Posture Management o CSPM) y la Protección de Cargas de Trabajo en la Nube (Cloud Workload Protection o CWP).

El pilar de CSPM se orienta a proporcionar visibilidad centralizada sobre el estado de seguridad actual del patrimonio digital, generando guías de protección y recomendaciones procesables para mitigar riesgos y elevar el nivel defensivo de la organización. Por su parte, el pilar de CWP proporciona capacidades de detección activa y defensas inteligentes adaptadas a cada tipo de carga de trabajo específica, como servidores físicos y virtuales, bases de datos corporativas y entornos de contenedores. Estas capacidades de detección de CWP están impulsadas directamente por la inteligencia sobre amenazas de Microsoft Threat Intelligence.

Como herramienta integrada de forma nativa en Azure, Microsoft Defender for Cloud permite un despliegue simplificado mediante mecanismos de aprovisionamiento automático, asegurando que los recursos recién creados estén protegidos por defecto. En el caso de los servicios de Plataforma como Servicio (PaaS), tales como Service Fabric, Azure SQL Database y las Cuentas de almacenamiento de Azure, el sistema realiza la supervisión y aplica controles de seguridad sin requerir la instalación de agentes adicionales ni configuraciones complejas. Para extender estas capacidades defensivas a servidores y máquinas virtuales ubicados fuera de Azure (en entornos locales o en nubes de otros proveedores como AWS y GCP), se requiere la integración con Azure Arc. Al registrar los equipos externos mediante Azure Arc, estos se proyectan como recursos nativos dentro del portal de Azure, lo que permite auditar su estado mediante Azure Policy y aplicar recomendaciones de seguridad personalizadas.

---

## 2\. Cloud Workload Protection (CWP) por Servicio: Defender para Servidores y App Service

El componente Cloud Workload Protection (CWP) adapta sus algoritmos y mecanismos de supervisión a las particularidades operativas de cada servicio gestionado dentro de la infraestructura tecnológica, destacando las protecciones diseñadas para servidores y para aplicaciones web.

### 2.1\. Microsoft Defender para Servidores: Planes y Funcionalidades

Microsoft Defender para servidores ofrece capacidades avanzadas de detección de amenazas y mitigación de riesgos para máquinas que ejecutan sistemas operativos Windows y Linux en cualquier ubicación, abarcando Azure, AWS, GCP y centros de datos locales. La oferta del servicio se estructura en dos niveles operativos denominados Plan 1 y Plan 2.

El Plan 1 se enfoca en la protección del endpoint mediante la integración automática de Microsoft Defender for Endpoint en los servidores objetivo. Su modelo de licenciamiento aplica una facturación basada en el uso por hora en lugar de suscripciones fijas por dispositivo, consolidando la telemetría de vulnerabilidades y los eventos de seguridad directamente dentro del panel de Microsoft Defender for Cloud.

El Plan 2 extiende de forma considerable las capacidades defensivas del Plan 1 al incorporar funcionalidades avanzadas de auditoría e investigación forense. Entre estas adiciones destaca el análisis sin agente (agentless scanning), la supervisión de la integridad de los archivos (File Integrity Monitoring o FIM), el control de acceso Just-In-Time (JIT) a máquinas virtuales para reducir la exposición de puertos de administración, y la inclusión de una cuota de ingesta gratuita de 500 MB de datos diarios por servidor en las áreas de trabajo de Log Analytics para tipos de datos de seguridad específicos.

La capacidad de agentless scanning presente en el Plan 2 representa un avance operativo relevante, ya que permite realizar inventarios exhaustivos de software, evaluar vulnerabilidades del sistema operativo y detectar presencia de software malicioso o credenciales expuestas directamente desde los volúmenes de almacenamiento subyacentes, sin generar impacto de rendimiento ni requerir la instalación de agentes en el punto de conexión.

Para la evaluación de las configuraciones del sistema operativo, Defender para servidores utiliza la extensión Azure Machine Configuration, la cual audita la postura del equipo frente a las líneas base de seguridad establecidas en la norma Microsoft Cloud Security Benchmark (MCSB). En sistemas Linux, la recolección de eventos del sistema se apoya en el marco de auditoría nativo auditd, mientras que en sistemas Windows se integra directamente con las herramientas y servicios de auditoría del sistema operativo para consolidar alertas y recomendaciones de corrección bajo un formato unificado.

### 2.2\. Microsoft Defender para App Service

Azure App Service es una plataforma administrada diseñada para hospedar aplicaciones web y APIs. Microsoft Defender para App Service aprovecha la escala de la infraestructura en la nube para identificar patrones de ataque dirigidos específicamente contra los recursos de aplicaciones web.

A diferencia de los controles perimetrales tradicionales que analizan únicamente solicitudes individuales, el sistema examina los registros internos de la plataforma App Service para detectar metodologías de ataque avanzadas que resultarían invisibles desde un único host aisladamente, tales como reconocimientos masivos, escaneos distribuidos y peticiones no autorizadas procedentes de subconjuntos coordinados de direcciones IP.

El proceso de evaluación de Defender for Cloud abarca la totalidad del entorno de la aplicación, analizando la instancia de la máquina virtual donde se ejecuta el servicio, la interfaz de administración de App Service y el flujo de solicitudes y respuestas procesadas por las aplicaciones. En el caso de los planes de ejecución basados en Windows, la herramienta posee acceso directo a los espacios aislados (sandboxes) y a las máquinas virtuales subyacentes. Esta visibilidad profunda le permite identificar indicadores de compromiso e infecciones existentes incluso si Microsoft Defender for Cloud fue activado con posterioridad al momento en que la aplicación sufriera el acceso no autorizado inicial.
