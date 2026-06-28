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

* **Recurso Creado:** Carpeta de red local denominada `Compartida_Admin`.
* **Configuración de Permisos:** Se accedió al menú de *Uso compartido avanzado* para restringir los accesos del grupo de red global (`Todos`).
* **Seguridad Aplicada:** Se asignaron los permisos exclusivos de **Leer** y **Cambiar**, manteniendo desmarcada la opción de *Control Total*. 
* **Justificación Profesional:** Esta configuración permite a los empleados trabajar, crear, modificar y guardar sus informes diarios con total normalidad, pero evita que usuarios sin privilegios de administración alteren los permisos de red del recurso o bloqueen accidentalmente al departamento de TI.

