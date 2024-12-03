## Backup Policy Implementation Plan & Resume

### Plan de Implementación de Políticas de Respaldo

Este plan describe los pasos para implementar una política de respaldo para máquinas virtuales VMware utilizando Google Cloud Backup and DR.

**Fase 1: Configuración e Instalación**

* **Activar el Servicio Google Cloud Backup and DR:** Esto proporciona acceso a la Consola de administración y a un dispositivo de respaldo y recuperación.
* **Crear un depósito de Google Cloud Storage:** Este depósito servirá como el grupo OnVault para almacenar respaldos.
    * Elija una región que se alinee con la ubicación de sus datos de origen y el posible sitio de recuperación ante desastres.
    * Seleccione una clase de almacenamiento (Nearline para respaldos retenidos durante 30 días o menos, Coldline para respaldos retenidos durante 90 días o más).
    * Asegúrese de que el usuario de OnVault tenga acceso al depósito y elimine las cuentas de servicio innecesarias.
* **Conectar el proyecto de respaldo a la red de Google Cloud VMware Engine:** Utilice la conexión de acceso a servicios privados para vincular el proyecto que ejecuta el dispositivo de respaldo y recuperación a la red de Google Cloud VMware Engine.
* **Configurar un usuario de solución en Google Cloud VMware Engine:** Este usuario se utilizará para fines de autenticación durante las operaciones de respaldo.
    * Vaya al cliente vSphere, navegue a Administración > Usuarios y Grupos, y localice un usuario de solución (normalmente solution-user-01).
    * Establezca una contraseña y una descripción para el usuario de la solución.
* **Validar las reglas del cortafuegos:** Garantice una comunicación adecuada entre Google Cloud VMware Engine y el dispositivo de respaldo y recuperación.
    * Verifique la regla del cortafuegos precreada para el dispositivo de respaldo y recuperación.
    * Añada la subred de administración del sistema de su configuración de VMware Engine a la regla del cortafuegos.
    * Incluya los puertos necesarios para NFS (111, 756, 2449, 4001 y 4045) tanto en TCP como en UDP.

**Fase 2: Configuración del Dispositivo de Respaldo y Recuperación**

* **Agregar el depósito de Cloud Storage como grupo OnVault:** En la Consola de administración, navegue a Administrar > Grupos de almacenamiento y agregue el grupo OnVault.
* **Crear plantillas y perfiles del plan de respaldo:** Defina la frecuencia de los respaldos, los períodos de retención y los grupos de destino (grupo de instantáneas o grupo OnVault).
    * Considere la posibilidad de crear diferentes plantillas para varios escenarios de respaldo (por ejemplo, solo instantáneas, instantáneas a OnVault, directo a OnVault).
    * Utilice la retención forzada para evitar la eliminación accidental o intencionada de los respaldos.
* **Añadir el host de vCenter con el usuario de la solución:** Introduzca los detalles de vCenter (nombre, IP, credenciales de usuario de la solución) en la Consola de administración.
* **Validar la resolución de nombres de host ESXi:** Asegúrese de que el dispositivo de respaldo y recuperación puede resolver los nombres de host ESXi utilizando DNS.

**Fase 3: Incorporación y Ejecución del Respaldo**

* **Incorporar máquinas virtuales VMware:** Seleccione las máquinas virtuales de las que desea hacer una copia de seguridad y aplique los planes de copia de seguridad adecuados.
* **Supervisar las tareas de copia de seguridad:** Siga el progreso y el éxito de las tareas de copia de seguridad en la sección Supervisar tareas.

**Fase 4: Acceso y Recuperación del Respaldo**

* **Acceder a los respaldos:** Utilice el Administrador de aplicaciones o la sección Recuperar para localizar la imagen de respaldo deseada.
* **Montar, clonar o restaurar:** Elija el método de recuperación adecuado en función de las necesidades específicas.
    * **Montar:** Proporciona un acceso rápido a la copia de seguridad sin tener que esperar a que se complete la copia de datos completa.
    * **Clonar:** Crea una copia totalmente independiente de la máquina virtual en el almacén de datos de destino.
    * **Restaurar:** Revierte la máquina virtual de origen a un estado anterior utilizando los datos de la copia de seguridad.

