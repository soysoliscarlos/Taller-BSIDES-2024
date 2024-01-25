
# Taller Dominando Microsoft Entra ID : Gestión de Identidades y Accesos en la Era Cloud

##### Taller intensivo de 3 horas para profesionales de TI

**DESCRIPCIÓN**: Este taller intensivo de 3 horas está diseñado para profesionales de TI, administradores de sistemas y cualquier persona interesada en la seguridad de la identidad en la nube. Enfocándonos en Microsoft Entra ID, exploraremos cómo esta poderosa herramienta puede ayudarte a gestionar y proteger las identidades y accesos en tu organización.

**OBJETIVOS**:

- Comprender los fundamentos y la importancia de la gestión de identidades en la nube con Microsoft Entra ID.
- Aprender a configurar y administrar Microsoft Entra ID en un entorno de Microsoft.
- Profundizar en características avanzadas, incluyendo la gestión de accesos privilegiados, las revisiones de acceso y la gestión de riesgos de identidad.
- Realizar ejercicios prácticos para aplicar los conceptos aprendidos en escenarios del mundo real.

# Tabla de Contenidos

1. [NOTAS PREVIAS](#notas-previas)
2. [ACTIVAR LICENCIA DE ENTRA ID P2](#activar-licencia-de-entra-id-p2)
3. [CONFIGURAR SERVIDOR DE AD DS](#configurar-servidor-de-ad-ds)
4. [CONFIGURAR MICROSOFT ENTRA ID CONNECT (EN LA MÁQUINA VIRTUAL)](#configurar-microsoft-entra-id-connect-en-la-máquina-virtual)
5. [CONFIGURAR ACCESO CONDICIONAL](#configurar-acceso-condicional)
6. [ASIGNAR LICENCIAS A USUARIOS](#asignar-licencias-a-usuarios)
7. [CONFIGURAR PRIVILEGED IDENTITY MANAGEMENT](#configurar-privileged-identity-management)
8. [CONFIGURAR ACCESO CONDICIONAL DESDE PLANTILLA (OPCIONAL)](#configurar-acceso-condicional-desde-plantilla-opcional)

## NOTAS PREVIAS

Se usarán dos navegadores (o perfiles de navegadores):

- Navegador A: Para el usuario Global Administrator del Tenant
- Navegador B: Para el usuario creado en AD DS

[Ir a Tabla de Contenidos](#tabla-de-contenidos)

## ACTIVAR LICENCIA DE ENTRA ID P2

**En Navegador A**

1. Ingresar en <https://entra.microsoft.com>
2. En el menú de la izquierda ir a **Billing > Licenses**
3. En Licenses, ir al menú “**All Products**”
4. En “**All Products**” hacer clic en **Try/Buy**
5. Activar Microsoft Entra ID P2
6. Ingresar Correo eletrónico
7. Completar formulario.
8. Si le pide crear un nuevo Tenant, continue el proceso.

[Ir a Tabla de Contenidos](#tabla-de-contenidos)

## CONFIGURAR SERVIDOR DE AD DS

1. En el Server Manager, ir a “**Add Roles and features**”
2. Instalar el rol “**Active Directory Domain Services**”, luego seguir las instrucciones
3. Luego de realizar la instalación del Rol, se activará una notificación en el Server Manager, ingresar en la alerta para promover el servidor como controlador de dominio, luego seguir las instrucciones:
    1. Add a New Forest: Indicar nombre del Bosque de dominio.
    2. Escribir Contraseña DSRM
    3. Clic en continuar, hasta  al botón de instalar
    4. Luego, reiniciar la máquina virtual.
4. En en menu del Sistema operativo, en **Active directory Users and Computer**, crear un usuario (recordar la contraseña), Deshabilitar la opción “User must change password at next logon”

[Ir a Tabla de Contenidos](#tabla-de-contenidos)

## CONFIGURAR MICROSOFT ENTRA ID CONNECT (EN LA MÁQUINA VIRTUAL)

1. Desde la máquina virtual ingresar en <https://entra.microsoft.com>
2. Ir a **Identity > Hybrid management > Microsoft Entra Connect > Cloud sync**.
3. Seleccione Agente a la izquierda.
4. Seleccione Descargar el agente local y, después, Aceptar términos y descargar.
5. Una vez descargado el paquete del agente de aprovisionamiento de Microsoft Entra, ejecute el archivo de instalación **AADConnectProvisioningAgentSetup.exe** desde la carpeta de descargas.
6. En la pantalla de presentación, elija Acepto los términos y las condiciones de la licencia y seleccione Instalar.
7. Cuando finalice la instalación, se iniciará el asistente para configuración. Seleccione Siguiente para iniciar la configuración.
8. En la pantalla Seleccionar extensión, seleccione Aprovisionamiento basado en **HR (Workday y SuccessFactors) / Microsoft Entra Connect Cloud Sync** y haga clic en Siguiente.
9. Inicie sesión con su cuenta de administrador global de Microsoft Entra.
10. Inicie sesión con su cuenta de administrador de dominio de Active Directory. Seleccione Aceptar y Siguiente para continuar.
11. En la pantalla Conectar Active Directory, si el nombre de su dominio aparece en Dominios configurados, continúe en el paso siguiente. De lo contrario, escriba el nombre del dominio de Active Directory y seleccione Agregar directorio.
12. Seleccione Next (Siguiente) para continuar.
13. En la pantalla Configuración completa, seleccione Confirmar.
14. Una vez completada esta operación, se le debería notificar que la configuración del agente se ha comprobado correctamente. Puede seleccionar Salir.
15. Si la pantalla de presentación inicial no desaparece, seleccione Cerrar.
16. En el portal de <https://entra.microsoft.com> en **Identity > Hybrid management > Microsoft Entra Connect > Cloud sync**. En **Agents**, validar que aparece el agente.
17. Luego, en **Identity > Hybrid management > Microsoft Entra Connect > Cloud sync** ir a **New Configuration** y seleccionar “**AD to Microsoft Entra ID Sync**”
18. Escoger al dominio del AD DS, habilitar el **Password Hash Syncronization**
19. Y hacer clic en crear.
20. En **overview**, en **Enable your configuration (required)**, hacer clic en **Review and enable**.

[Ir a Tabla de Contenidos](#tabla-de-contenidos)

## CONFIGURAR ACCESO CONDICIONAL

**En Navegador A**

1. Ir a **Protection > Conditional Access > Named Locations**, hacer clic en **Countries Location**
    1. Indicar nombre
    2. Escoger pais: Panamá
2. En **Protection > Conditional Access**, hacer clic en “**Create new Policy**”
    1. Indicar nombre de la política.
    2. En Users:
        1. Include > “**All Users**”
        2. Exclude,
            1. Marcar “**Directory Roles**”, escoger “**Global Administrator**”
            2. Marcar “**Users and groups**”, escoger usuario “**On-Premises Directory Synchronization Service Account**”
        3. En Target resource, marcar “**All cloud apps”**
        4. En “**Conditions**” ir a “**Locations**”,
            1. Marcar Yes en “**Configure**”
            2. Include, marcar “**Any location**”
            3. Exclude, marcar **selected locations**. Escoger la locación creada en el paso 1
        5. En **Enable Policy**, Marcar “**On**”
        6. Puede aparecer un mensaje donde pide deshabilitar el “**Security Defaults**”, proceder a desactivar el mismo.
        7. Hacer clic en “**Save**”
3. Ir a **Identity > Users > All Users** y verificar que está el usuario previamente creado en el AD DS.
**En Navegador B**
4. Hacer inicio de sesión en <https://office.com> con el usuario que se creó en el AD DS (Usar el UPN indicado en Entra ID y la contraseña asignada en el AD DS)
**En Navegador A**
5. Notifique que pudieron hacer inicio de sesión y cerrar la sesión del usuario.
6. Repitiendo, las instrucciones del paso 1, cambiar el país por uno diferente a Panamá.
**En Navegador B**
7. Espere unos minutos, luego, repetir las instrucciones del paso 4, comparta con el grupo los resultados.
8. Cierre la sesión
**En Navegador A**
9. Repita las instrucciones del paso 1.

[Ir a Tabla de Contenidos](#tabla-de-contenidos)

## ASIGNAR LICENCIAS A USUARIOS

**En Navegador A**

1. Ir a **Identity > Users > All Users** e ingresar en cada perfil de usuario.
2. Ingresar en la pestaña **Properties**
3. En el apartado **Setting** hacer clic en el botón de edición (es una imagen de un lápiz)
4. En **Usage location** escoger el país: **Panamá**
5. Hacer clic en *Save* y cerrar la pestaña de *Properties*
6. De nuevo, en la pestaña del usuario, ir a **Manage > Licenses** y hacer clic en **Assignments**
7. En **Select licenses**, seleccionar **Microsoft Entra ID P**2
8. Hacer clic en **Save** y cerrar la pestaña de **Update license assignments**
9. Repetir para cada usuario creado

[Ir a Tabla de Contenidos](#tabla-de-contenidos)

## CONFIGURAR PRIVILEGED IDENTITY MANAGEMENT

**En Navegador A**

1. En <https://entra.microsoft.com/> ir a **Identity governance > Privileged Identity Management**
2. En **Privileged Identity Management** ir a **Manage > Microsoft Entra roles**
3. En **Microsoft Entra roles** ir a **Manage > Roles** y escoger el rol **User Administrator**
4. En **User Administrator**, ir a **Manage > Assignments**, y hacer clic en **Add assignments**
5. En **Membership**, hacer clic en **Select member(s)** y escoger el usuario sincronizado con el AD DS
6. Hacer clic en **Next**, dejar **Assignment type: Eligible** y el check en **Permanently eligible**
7. Hacer clic **Assign**
8. En la pantalla del rol **User Administrator**, ir a **Manage > Roles settings** y hacer clic en **Edit** y hacer las siguientes configuraciones:
    1. On activation, require: **None**
    2. Require justification on activation: **check**
    3. Require approval to actívate: **check**
    4. Select approver(s): Escoger el usuario **Global administrador**
    5. Hacer clic en **Update**

**En Navegador B**

9. En <https://entra.microsoft.com/> Ir a **Identity > Users > All Users** y tartar de editar las propiedades del usuario **On-Premises Directory Synchronization Service Account**
10. Validar que no puede editar las propiedades del usuario.
11. Luego, ir a **Identity governance > Privileged Identity Management**, luego ir a **Tasks > My roles**
12. En el rol **User Administrator**, hacer clic en la acción “**Activate**”
13. Completar el formulario de “**Reason**”
14. Hacer clic en **Activate**

**En Navegador A**
15. Ir a **Identity governance > Privileged Identity Management**, luego, ir a **Tasks > Approve requests**
16. En **Requests for role activations**, seleccionar el rol pendiente y hacer clic en **Approve**
17. Completar el formulario de “**Justification**”
18. Hacer Clic en **Confirm**.

**En Navegador B**

19. Ir a **Identity > Users > All Users** y tartar de editar las propiedades del usuario **On-Premises Directory Synchronization Service Account**
20. Validar que puede editar las propiedades del usuario.

[Ir a Tabla de Contenidos](#tabla-de-contenidos)

## CONFIGURAR ACCESO CONDICIONAL DESDE PLANTILLA (OPCIONAL)

**En Navegador A**

2. En **Protection > Conditional Access**, hacer clic en “**Create new Policy from template**”
3. Navegue y revise todas las plantillas disponibles.
4. Escoja la plantilla “**Securing security info registration**”
5. Haga clic en **Review + create**
6. Dejar como está y hacer clic en **Create**
7. Ir a **Policies**, ingresar en la política recién creada con el nombre “**Securing security info registration**”
8. Ir a **Users > Exclude** y en **Users and Groups**, incluir al usuario **On-Premises Directory Synchronization Service Account** en la lista de usuarios excluidos.
9. Explorar las demás opciones de configuración.
10. En **Enable Policy**, Marcar “**On**”
11. Hacer clic en “**Save**”

**En Navegador B**

12. Desde otro navegador, hacer inicio de sesión en <https://office.com> con el usuario que se creó en el AD DS (Usar el UPN indicado en Entra ID y la contraseña asignada en el AD DS)
13. Comparta sus resultados.

**En Navegador A**

14. Luego, Ir a **Protection > Conditional Access > Named Locations**, hacer clic en **IP ranges location**:
    1. Definir un nombre
    2. Hacer check en **Mark as trusted location**
    3. En el simbolo de **+** agregar el rango de red (el rango de red es su IP pública con /32 al final, Ejemplo 8.8.8.8/32)
    4. Hacer clic en **Save**

**En Navegador B**

15. Nuevamente, Desde otro navegador, hacer inicio de sesión en <https://office.com> con el usuario que se creó en el AD DS (Usar el UPN indicado en Entra ID y la contraseña asignada en el AD DS)
16. Completar la configuración del MFA

[Ir a Tabla de Contenidos](#tabla-de-contenidos)
