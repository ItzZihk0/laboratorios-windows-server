# Lab 01 InfraZihk Gestión centralizada de identidades y alta disponibilidad AD DS + DNS + GPO + Replicación
---
 
## Caso de negocio
 
InfraZihk pasó de 10 personas a 60 en dos años. Sin embargo, su infraestructura nunca escaló con ellos.
 
El resultado: cada usuario tiene una cuenta local en su propia máquina, IT restablece las contraseñas a mano cuando alguien las olvida, no existe ninguna política de seguridad aplicada de forma consistente, y nadie en la organización sabe con certeza quién tiene acceso a qué.
 
Con 60 personas trabajando así, el riesgo no es teórico es una cuestión de tiempo.

| Síntoma | Impacto |
|---|---|
| Cuentas locales en cada PC | Sin control centralizado de accesos |
| Reseteo manual de contraseñas | Tiempo de TI desperdiciado, riesgo de ingeniería social |
| Sin políticas de seguridad | Equipos sin hardening, comportamiento inconsistente |
| Un solo punto de autenticación | Si el servidor falla, nadie trabaja |

---

## ¿Qué se busca solucionar?
 
| Problema actual | Solución implementada |
|---|---|
| Cuentas locales sin gestión centralizada | Active Directory Domain Services (AD DS) |
| Sin resolución de nombres interna | DNS integrado con AD |
| Sin políticas de seguridad | Group Policy Objects (GPO) |
| Punto único de falla en autenticación | DC secundario + replicación AD |
 
El objetivo final es que InfraZihk tenga un dominio funcional (`infrazihk.com`), con usuarios y equipos gestionados centralmente, políticas aplicadas automáticamente y sin depender de un único servidor para autenticar.
 
---

## Topología del entorno

El laboratorio se despliega sobre cinco máquinas virtuales: dos servidores que actúan como Domain Controllers y tres estaciones cliente que simulan los equipos de los usuarios de InfraZihk.

![Topología del entorno](https://i.ibb.co/3591CfWf/Topolog-a-del-entorno.png)

| Componente | Hostname | IP | Rol |
|---|---|---|---|
| Windows Server 2022 | `DC01` | 192.168.1.2 | DC Principal / DNS primario |
| Windows Server 2022 | `DC02` | 192.168.1.3 | DC Secundario / DNS secundario |
| Windows 10 Pro | `WS01` | 192.168.1.x | Cliente de dominio |
| Windows 10 Pro | `WS02` | 192.168.1.x | Cliente de dominio |
| Windows 10 Pro | `WS03` | 192.168.1.x | Cliente de dominio |
 
- **Dominio:** `infrazihk.com`  
- **Red:** `192.168.1.0/24`  
- **Hipervisor:** VMware Workstation  

---

## Fases de implementación
 
Este laboratorio está dividido en cuatro fases. Cada una tiene su propio `README.md` con el paso a paso.

### [`Fase 1 Instalación y promoción de AD DS`](./01-instalacion-ad/README.md)
 
- Configuración de IP estática y nombre de host en DC01
- Instalación del rol AD DS
- Promoción a Domain Controller y creación del dominio `infrazihk.com`
- Creación de estructura de OUs (Departamentos, Usuarios, Equipos, Servidores)
- Creación de usuarios y grupos de seguridad
 
> Sin AD DS no hay dominio. Esta fase es la base de todo lo demás.

### [`Fase 2 Configuración de DNS`](./02-dns/README.md)
 
- Verificación de zonas DNS integradas con AD (`infrazihk.com`)
- Registros A, PTR y SRV críticos para el funcionamiento del dominio
- Configuración de reenviadores para resolución externa
- Unión de clientes WS01, WS02, WS03 al dominio
 
> AD depende completamente de DNS. Una mala configuración aquí rompe la autenticación, las GPOs y la replicación.

### [`Fase 3 Directivas de grupo (GPO)`](./03-gpo/README.md)
 
- GPO de contraseñas y bloqueo de cuenta (política de dominio)
- GPO de escritorio corporativo (fondo, restricciones)
- GPO de seguridad de estaciones (deshabilitar USB, Panel de Control, etc.)
- Aplicación por OU y verificación con `gpresult`
 
> Las GPOs reemplazan la configuración manual equipo por equipo. 60 equipos, una política.

### [`Fase 4 Segundo DC y replicación`](./04-replicacion/README.md) 
 
- Promoción de DC02 como Domain Controller adicional
- Verificación de replicación con `repadmin /replsummary`
- Prueba de failover: apagar DC01 y validar que los clientes siguen autenticándose
- Transferencia y seizing de roles FSMO
 
> Si DC01 falla un lunes a las 9am, nadie puede iniciar sesión. DC02 elimina ese riesgo.

---

## Criterios de validación
 
Al finalizar todas las fases, el laboratorio se considera exitoso si se cumplen **todos** los siguientes criterios:
 
### AD DS
- [⬜] `DC01` y `DC02` son Domain Controllers del dominio `infrazihk.com`
- [⬜] Los usuarios creados pueden iniciar sesión desde WS01, WS02 y WS03
- [⬜] Las OUs reflejan la estructura organizacional de InfraZihk
 
### DNS
- [⬜] `nslookup infrazihk.com` resuelve correctamente desde los clientes
- [⬜] Los registros SRV de Kerberos y LDAP existen en la zona DNS
- [⬜] Los clientes tienen DNS apuntando a `192.168.1.2` (y `192.168.1.3` como secundario)
 
### GPO
- [⬜] La política de contraseñas está activa (`Get-ADDefaultDomainPasswordPolicy`)
- [⬜] El fondo corporativo se aplica a los equipos del dominio
- [⬜] `gpresult /r` en los clientes muestra las GPOs aplicadas correctamente
 
### Replicación
- [⬜] `repadmin /replsummary` no muestra errores entre DC01 y DC02
- [⬜] Con DC01 apagado, los usuarios siguen autenticándose a través de DC02
- [⬜] Los cambios en AD (nuevo usuario) se replican a DC02 en menos de 60 segundos

---
 
## Navegación del laboratorio
 
```
lab-01-infrazihk/
├── README.md             
├── 01-instalacion-ad/
│   ├── README.md
│   └── screenshots/
├── 02-dns/
│   ├── README.md
│   └── screenshots/
├── 03-gpo/
│   ├── README.md
│   └── screenshots/
└── 04-replicacion/
    ├── README.md
    └── screenshots/
```

---

*Parte del repositorio [laboratorios-windows-server](https://github.com/ItzZihk0/laboratorios-windows-server) laboratorios infraestructura windows server.*