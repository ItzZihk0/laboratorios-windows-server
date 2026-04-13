# Laboratorios Infraestructura Windows Server

Repositorio de laboratorios prácticos enfocados en la administración y configuración de entornos Windows Server en escenarios reales de infraestructura.

Cada laboratorio aborda servicios críticos del ecosistema Microsoft como Active Directory, directivas de grupo, DNS, DHCP y más, incluyendo diseño, implementación y validación de cada configuración.

![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D6?style=flat&logo=windows&logoColor=white)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1-5391FE?style=flat&logo=powershell&logoColor=white)
![Active Directory](https://img.shields.io/badge/Active_Directory-AD_DS-0078D6?style=flat&logo=microsoft&logoColor=white)
![Estado](https://img.shields.io/badge/Estado-En%20progreso-blue?style=flat-square)
![Licencia](https://img.shields.io/badge/Licencia-MIT-lightgrey?style=flat-square)

---

## Enfoque
 
Los laboratorios están diseñados para demostrar competencias en:
 
- Diseño e implementación de Active Directory en escenarios empresariales
- Gestión centralizada de identidades, accesos y políticas de seguridad (GPO)
- Alta disponibilidad con múltiples controladores de dominio y replicación
- Troubleshooting y validación de configuraciones en dominio

---

## Prerrequisitos

Para reproducir los laboratorios se recomienda el siguiente entorno:

| Componente | Mínimo recomendado |
|---|---|
| RAM | 16 GB |
| Almacenamiento | 100 GB libres |
| CPU | 6 núcleos con soporte de virtualización (VT-x / AMD-V) |
| Hipervisor | VMware Workstation 16+ o equivalente |
| Sistema operativo host | Windows 10/11 Pro o Linux |

> Las ISOs de Windows Server 2022 y Windows 10 pueden obtenerse desde el  
> [Centro de evaluación de Microsoft](https://www.microsoft.com/es-es/evalcenter/download-windows-server-2022).

---

## Índice de laboratorios

| # | Empresa | Problema | Temas | Estado |
|---|---|---|---|---|
| 01 | [InfraZihk]() | Sin gestión centralizada de identidades ni políticas | AD DS + DNS + GPO + Replicación | En progreso |
| 02 | Por definir | Sin asignación de IPs centralizada | DHCP | Próximamente |
| 03 | Por definir | Sin control de acceso a archivos compartidos | File Server + NTFS + GPO | Próximamente |
| 04 | Por definir | Entorno AD sin hardening | Hardening Active Directory | Próximamente |

---

## Entorno del laboratorio

| Componente | Detalle |
|------------|---------|
| Hipervisor | VMware Workstation |
| Sistema operativo | Windows Server 2022 |
| Dominio simulado | `infrazihk.com` |
| Red | 192.168.1.0/24 |
| DC Principal | `DC01` — 192.168.1.2 |
| DC Secundario | `DC02` — 192.168.1.3 |
| Cliente 1 | `WS01` — Windows 10 Pro |
| Cliente 2 | `WS02` — Windows 10 Pro |
| Cliente 3 | `WS03` — Windows 10 Pro |

Las VMs están en red interna con acceso a internet a través del adaptador del host.

---

## Estructura del repositorio

```
infrazihk-windows-server/
│
├── README.md
│
├── lab-01-infrazihk/
│   ├── README.md                   
│   │
│   ├── lab-01-ad-ds-dns-gpo-replicacion/
│   │   ├── README.md
│   │   │
│   │   ├── 01-instalacion-ad/
│   │   │   ├── README.md
│   │   │   └── screenshots/
│   │   │
│   │   ├── 02-dns/
│   │   │   ├── README.md
│   │   │   └── screenshots/
│   │   │
│   │   ├── 03-gpo/
│   │   │   ├── README.md
│   │   │   └── screenshots/
│   │   │
│   │   ├── 04-replicacion/
│   │   │   ├── README.md
│   │   │   └── screenshots/
│
└── _recursos/
    └── base-vms.md
```
---

## ¿Cómo usar los labs?

1. Clona el repositorio
```bash
   git clone https://github.com/ItzZihk0/laboratorios-windows-server.git
```
2. Lee el `README.md` general para entender el entorno base
3. Entra al laboratorio que te interese y sigue su `README.md` interno
4. Cada lab es independiente, pero se recomienda seguir el orden numérico si eres nuevo en el tema

---

*Parte del portafolio técnico de infraestructura windows server ItzZihk0*
