# Laboratorio: Inter-VLAN Routing con DHCP en Cisco Packet Tracer

## Descripción General

Este laboratorio implementa una red segmentada en **3 VLANs** con enrutamiento entre ellas mediante un switch de capa 3 (Multilayer Switch). Se configuran **35 PCs** distribuidas en las VLANs y un servidor DHCP centralizado en el switch de capa 3 para la asignación dinámica de direcciones IP.

---

## Topología de Red

```
                    [3650-24PS Multilayer Switch0]  ← Switch Capa 3
                           /           \
                          /             \
               [Switch Capa 2]       [Switch Capa 2]
               VLAN 10 + 20           VLAN 20 + 30
                /      \                   \
           VLAN 10    VLAN 20           VLAN 30
          (Amarillo)  (Rosado)          (Celeste)
```

---

## Tabla de VLANs

| VLAN | Nombre | Red             | Gateway (SVI)  | Máscara       | Rango DHCP           |
|------|--------|-----------------|----------------|---------------|----------------------|
| 10   | VLAN10 | 192.168.10.0/24 | 192.168.10.1   | 255.255.255.0 | 192.168.10.10 – .254 |
| 20   | VLAN20 | 192.168.20.0/24 | 192.168.20.1   | 255.255.255.0 | 192.168.20.10 – .254 |
| 30   | VLAN30 | 192.168.30.0/24 | 192.168.30.1   | 255.255.255.0 | 192.168.30.10 – .254 |

---

## Distribución de PCs por VLAN

### VLAN 10 (192.168.10.0/24) — Zona Amarilla
PC0, PC1, PC3, PC4, PC5, PC12, PC13, PC14, PC15, PC16, PC18

### VLAN 20 (192.168.20.0/24) — Zona Rosada
PC2, PC6, PC7, PC9, PC10, PC11, PC19, PC20, PC21, PC22, PC23, PC24

### VLAN 30 (192.168.30.0/24) — Zona Celeste
PC8, PC25, PC26, PC27, PC28, PC29, PC30, PC31, PC32, PC33, PC34, PC35, PC36, PC37

---

## Dispositivos del Laboratorio

| Dispositivo          | Modelo         | Rol                                  |
|----------------------|----------------|--------------------------------------|
| Multilayer Switch0   | 3650-24PS      | Switch Capa 3 / Router / DHCP Server |
| Switch Capa 2 (Izq.) | 2960           | Acceso VLAN 10 y 20                  |
| Switch Capa 2 (Der.) | 2960           | Acceso VLAN 20 y 30                  |

---

## Configuración Paso a Paso

---

### 1. Switch de Capa 3 — Multilayer Switch0 (3650-24PS)

#### 1.1 Configuración Básica

```
enable
configure terminal
hostname MLS0
ip routing
```

#### 1.2 Crear VLANs

```
vlan 10
 name VLAN10
vlan 20
 name VLAN20
vlan 30
 name VLAN30
exit
```

#### 1.3 Configurar Interfaces SVI (Gateways)

```
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown

interface vlan 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown
```

#### 1.4 Configurar Puertos Trunk hacia los Switches de Capa 2

```
! Puerto hacia Switch Capa 2 Izquierdo
interface GigabitEthernet1/0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20

! Puerto hacia Switch Capa 2 Derecho
interface GigabitEthernet1/0/2
 switchport mode trunk
 switchport trunk allowed vlan 20,30
```

> **Nota:** Ajusta los números de interfaz según la conexión real en tu topología.

#### 1.5 Configurar DHCP para las 3 VLANs

```
! Excluir las IPs de gateways y reservadas
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.20.1 192.168.20.9
ip dhcp excluded-address 192.168.30.1 192.168.30.9

! Pool DHCP para VLAN 10
ip dhcp pool VLAN10_POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8

! Pool DHCP para VLAN 20
ip dhcp pool VLAN20_POOL
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8

! Pool DHCP para VLAN 30
ip dhcp pool VLAN30_POOL
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 8.8.8.8
```

#### 1.6 Guardar Configuración

```
end
write memory
```

---

### 2. Switch de Capa 2 — Izquierdo (VLAN 10 y 20)

#### 2.1 Configuración Básica

```
enable
configure terminal
hostname SW-IZQ
```

#### 2.2 Crear VLANs

```
vlan 10
 name VLAN10
vlan 20
 name VLAN20
exit
```

#### 2.3 Puerto Trunk hacia el Switch de Capa 3

```
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown
```

#### 2.4 Puertos de Acceso — VLAN 10

```
interface range FastEthernet0/1 - FastEthernet0/11
 switchport mode access
 switchport access vlan 10
 no shutdown
```

#### 2.5 Puertos de Acceso — VLAN 20

```
interface range FastEthernet0/12 - FastEthernet0/23
 switchport mode access
 switchport access vlan 20
 no shutdown
```

#### 2.6 Guardar Configuración

```
end
write memory
```

---

### 3. Switch de Capa 2 — Derecho (VLAN 20 y 30)

#### 3.1 Configuración Básica

```
enable
configure terminal
hostname SW-DER
```

#### 3.2 Crear VLANs

```
vlan 20
 name VLAN20
vlan 30
 name VLAN30
exit
```

#### 3.3 Puerto Trunk hacia el Switch de Capa 3

```
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 20,30
 no shutdown
```

#### 3.4 Puertos de Acceso — VLAN 20

```
interface range FastEthernet0/1 - FastEthernet0/5
 switchport mode access
 switchport access vlan 20
 no shutdown
```

#### 3.5 Puertos de Acceso — VLAN 30

```
interface range FastEthernet0/6 - FastEthernet0/23
 switchport mode access
 switchport access vlan 30
 no shutdown
```

#### 3.6 Guardar Configuración

```
end
write memory
```

---

### 4. Configuración de las PCs

En cada PC, ir a **Desktop → IP Configuration** y seleccionar **DHCP**.

| Campo           | VLAN 10        | VLAN 20        | VLAN 30        |
|-----------------|----------------|----------------|----------------|
| IP Address      | 192.168.10.x   | 192.168.20.x   | 192.168.30.x   |
| Subnet Mask     | 255.255.255.0  | 255.255.255.0  | 255.255.255.0  |
| Default Gateway | 192.168.10.1   | 192.168.20.1   | 192.168.30.1   |
| DNS Server      | 8.8.8.8        | 8.8.8.8        | 8.8.8.8        |

---

## Verificación

### Switch de Capa 3

```
show vlan brief
show ip interface brief
show ip route
show ip dhcp binding
show ip dhcp pool
```

### Switches de Capa 2

```
show vlan brief
show interfaces trunk
```

### Prueba de Conectividad (desde cualquier PC)

```
ping 192.168.10.1     ! gateway VLAN 10
ping 192.168.20.1     ! gateway VLAN 20
ping 192.168.30.1     ! gateway VLAN 30
ping 192.168.20.10    ! PC en otra VLAN
```

---

## Diagrama de Direccionamiento

```
┌─────────────────────────────────────────────────────────────┐
│              MULTILAYER SWITCH 0 (3650-24PS)                │
│                                                             │
│  SVI VLAN 10: 192.168.10.1/24  (Gateway VLAN 10)           │
│  SVI VLAN 20: 192.168.20.1/24  (Gateway VLAN 20)           │
│  SVI VLAN 30: 192.168.30.1/24  (Gateway VLAN 30)           │
│                                                             │
│  DHCP Server activo para las 3 VLANs                       │
│  ip routing habilitado                                      │
└──────────────┬──────────────────────────┬───────────────────┘
               │ Trunk (VLAN 10,20)       │ Trunk (VLAN 20,30)
               │                          │
  ┌────────────▼──────────┐   ┌───────────▼───────────┐
  │   SW-IZQ (Capa 2)     │   │   SW-DER (Capa 2)     │
  │                       │   │                       │
  │ Access VLAN 10:       │   │ Access VLAN 30:       │
  │ PC0,1,3,4,5,12-16,18 │   │ PC8,25-37             │
  │                       │   │                       │
  │ Access VLAN 20:       │   │ Access VLAN 20:       │
  │ PC2,6,7,9-11,19-24   │   │ (PCs adicionales)     │
  └───────────────────────┘   └───────────────────────┘
```

---

## Troubleshooting

| Problema                    | Causa Probable                  | Solución                                        |
|-----------------------------|---------------------------------|-------------------------------------------------|
| PC no obtiene IP por DHCP   | Puerto no en VLAN correcta      | Verificar `show vlan brief` y reasignar         |
| No hay ping entre VLANs     | `ip routing` no habilitado      | Ejecutar `ip routing` en el MLS                 |
| Trunk no funciona           | VLAN no permitida en trunk      | `switchport trunk allowed vlan add X`           |
| DHCP no asigna IPs          | Pool mal configurado            | Verificar `show ip dhcp pool` y `binding`       |
| SVI sin IP                  | Interfaz VLAN apagada           | Ejecutar `no shutdown` en la SVI                |

---


