# 🏢 Corporate Workstation Deployment - TecnoSoluciones S.A.

## 📝 1. Introducción y Escenario Empresarial
Este proyecto documenta el despliegue y la configuración optimizada desde cero de un puesto de trabajo corporativo para la empresa ficticia **TecnoSoluciones S.A.** El objetivo es preparar un entorno de pruebas estandarizado para técnicos de soporte de TI Nivel 1, emulando los flujos de trabajo e infraestructuras reales de producción.

## 🛠️ 2. Arquitectura del Laboratorio (Hardware Virtualizado)
Para no comprometer la estabilidad del sistema anfitrión, la estación de trabajo se ha desplegado en un entorno virtualizado local (Hipervisor). Se han seleccionado los componentes físicos virtuales bajo criterios de eficiencia y rendimiento empresarial:

* **Hipervisor:** Oracle VM VirtualBox 7.1 (Herramienta estándar en portátiles de soporte para pruebas individuales).
* **Sistema Operativo Cliente:** Windows 10 Pro (22H2) x64. Se selecciona la edición **Pro** por ser el estándar corporativo obligatorio que incluye la funcionalidad de unirse a un Dominio centralizado (Active Directory) y directivas de grupo avanzadas.
* **Memoria RAM Asignada:** 4096 MB (4 GB). Configuración idéntica al estándar mínimo de oficina para garantizar la fluidez del sistema operativo y las aplicaciones corporativas.
* **Procesadores (vCPU):** 2 núcleos. Evita cuellos de botella durante las tareas de mantenimiento y ejecución de scripts.
* **Almacenamiento Virtual:** 50 GB en formato VDI (VirtualBox Disk Image) con reserva dinámica. Permite emular el espacio de un usuario estándar optimizando el espacio físico real del disco duro del técnico.

## 🌐 3. Configuración de Red Profesional y Conectividad
A diferencia de las configuraciones domésticas por defecto (NAT), la tarjeta de red de la estación de trabajo se ha modificado bajo estándares corporativos:

* **Modo de Red:** Adaptador Puente (Bridged Adapter).
* **Controlador Virtual:** Realtek PCIe GBE Family Controller (Puenteado con la tarjeta Wi-Fi/Ethernet física).
* **Justificación Técnica:** Este modo permite que el router de la infraestructura asigne una dirección IP independiente y única a la máquina virtual dentro del mismo rango que el equipo real. De este modo, el equipo deja de estar aislado, permitiendo la comunicación bidireccional (pings, carpetas compartidas y soporte remoto), emulando fielmente la inserción de un PC físico en la red local de la oficina.

## 🚀 4. Optimización y Post-Instalación
Tras completar una instalación limpia del sistema operativo creando un usuario de administración puramente local (`Soporte-IT`) para blindar la privacidad corporativa, se procedió a la optimización del entorno:

* **Paquete de Utilidades:** VirtualBox Guest Additions.
* **Arquitectura del Driver:** VBoxWindowsAdditions-amd64 (Específico para sistemas de 64 bits).
* **Equivalencia en Entornos Reales:** Este paso emula exactamente la instalación de los **Drivers o Controladores Oficiales del Fabricante** (Intel, AMD, HP, Lenovo) en un equipo físico. Permite la aceleración gráfica, la integración fluida del puntero del ratón y el escalado automático de la pantalla para adaptarla al monitor de trabajo sin distorsión.

## 🌐 5. Fase 2: Diagnóstico de Red (APIPA frente a NAT)
Durante las pruebas de conectividad iniciales en el entorno virtual por conexión inalámbrica (Wi-Fi), se detectó un fallo clásico de direccionamiento:

* **Incidencia Detectada:** El comando `ipconfig` devolvía una dirección IP **169.254.149.23**.
* **Análisis Técnico:** Se identificó como una dirección **APIPA** (Automatic Private IP Addressing). Esto ocurre cuando la máquina virtual no recibe respuesta del servidor DHCP (el router) debido a bloqueos de seguridad de la tarjeta Wi-Fi física del equipo anfitrión, dejando al sistema operativo aislado y sin internet.
* **Solución Aplicada:** Se cambió el modo de red de Adaptador Puente a **NAT** (Network Address Translation). VirtualBox genera un router virtual intermedio que traduce las peticiones, asignando con éxito una IP corporativa válida (`10.0.2.15`) y garantizando la salida a internet de la estación de trabajo.
* **Validación:** Se ejecutó con éxito el comando `ping google.com` registrando 0% de paquetes perdidos y una latencia óptima de respuesta.

## 📂 6. Recursos Compartidos en Red y Seguridad
Para resolver un ticket simulado del departamento de administración, se desplegó un espacio de almacenamiento compartido bajo estrictas políticas de acceso:
<div align="center">
  <img width="358" height="319" alt="Captura de pantalla 2026-06-28 171615"     src="https://github.com/user-attachments/assets/6b7aeefd-5a56-4e6a-a976-f65860ac5637" />
</div

* **Recurso Creado:** Carpeta de red local denominada `Compartida_Admin`.
* **Configuración de Permisos:** Se accedió al menú de *Uso compartido avanzado* para restringir los accesos del grupo de red global (`Todos`).

* **Seguridad Aplicada:** Se asignaron los permisos exclusivos de **Leer** y **Cambiar**, manteniendo desmarcada la opción de *Control Total*. 
* **Justificación Profesional:** Esta configuración permite a los empleados trabajar, crear, modificar y guardar sus informes diarios con total normalidad, pero evita que usuarios sin privilegios de administración alteren los permisos de red del recurso o bloqueen accidentalmente al departamento de TI.

## 🔐 7. Fase 3: Gestión de Identidades y Auditoría de Seguridad (ID 4724)
Se simuló un escenario real de soporte técnico de Nivel 1 ante la pérdida de credenciales de un empleado de la oficina, aplicando herramientas de administración avanzada y auditoría del sistema:

* **Gestión de Usuarios Locales:** Mediante la consola avanzada de Microsoft `lusrmgr.msc`, se dio de alta una nueva cuenta local corporativa estándar denominada `empleado1`, configurando directivas de expiración seguras.
* **Resolución del Incidente:** Ante la solicitud de restablecimiento de contraseña del usuario, se aplicó una sobreescritura forzada de credenciales mediante privilegios de administración local (`Soporte-IT`), solucionando el bloqueo de acceso de forma inmediata.
* **Auditoría del Sistema (Visor de Eventos):** Para verificar la seguridad y mantener el control de accesos, se analizó el registro secreto de Windows a través de la consola `eventvwr.msc`.
* **Identificación del Evento:** Se localizó con éxito el registro en la sección de *Seguridad* bajo el **ID de evento 4724** ("Se intentó restablecer la contraseña de una cuenta"). El análisis del evento permitió auditar con precisión quirúrgica el segundo exacto del cambio, la cuenta de origen (`Soporte-IT`) y la cuenta de destino afectada (`empleado1`).

## 🔒 8. Gestión de Bajas y Suspensión de Cuentas de Usuario
Se resolvió un ticket urgente del departamento de Recursos Humanos ante la baja laboral de un trabajador, aplicando políticas de retención de datos y seguridad perimetral:

* **Incidencia Reales de Oficina:** Necesidad de revocar el acceso inmediato al sistema a un empleado (`empleado1`) garantizando la confidencialidad de la empresa, pero sin destruir sus perfiles ni informes locales para futuras auditorías.
* **Acción Técnica Aplicada:** Se denegó la opción de "Eliminar cuenta" (lo que causaría pérdida irreversible de datos) y se procedió a marcar la directiva **"La cuenta está deshabilitada"** desde la consola `lusrmgr.msc`.
* **Resultado e Impacto Profesional:** El sistema operativo Windows oculta y congela el inicio de sesión del usuario de forma inmediata, bloqueando cualquier intento de acceso físico o remoto. Sin embargo, toda la estructura de carpetas, informes y documentos locales del empleado permanecen intactas y legibles en el almacenamiento virtual para el departamento de TI y los auditores.

## 💾 9. Fase 4: Ampliación de Almacenamiento e Infraestructura de Datos
Se resolvió un ticket del departamento de administración para ampliar el almacenamiento local de la estación de trabajo y blindar la seguridad de la información corporativa:

* **Arquitectura de Discos:** Se procedió al montaje virtualizado de una segunda unidad de almacenamiento dinámico de **20 GB** en formato VDI conectada al puerto SATA, aplicando el principio empresarial de **separación del Sistema Operativo (`C:`) y los datos críticos (`E:`)**.
* **Inicialización Avanzada:** Desde la consola `diskmgmt.msc` (Administración de discos), se inicializó el nuevo hardware bajo el estilo de partición moderno **GPT (Tabla de particiones GUID)** para garantizar la resiliencia del sistema.
* **Formateo y Estándar de Empresa:** Se generó un *Nuevo volumen simple* asignando la letra de unidad `E:` y aplicando el sistema de archivos **NTFS** bajo la etiqueta `DATOS_EMPRESA`.
* **Justificación Teórica:** La selección de NTFS es obligatoria en entornos de producción para permitir la gestión de permisos de seguridad de archivos y encriptación. La separación de unidades garantiza que, ante una infección por malware o corrupción crítica del sistema en el disco principal, el volumen de datos permanezca completamente aislado, intacto y disponible para su recuperación inmediata.



