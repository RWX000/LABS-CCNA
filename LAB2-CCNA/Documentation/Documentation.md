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



## Diagrama de Direccionamiento

```
┌─────────────────────────────────────────────────────────────┐
│              MULTILAYER SWITCH 0 (3650-24PS)                │
│                                                             │
│  SVI VLAN 10: 192.168.10.1/24  (Gateway VLAN 10)            │
│  SVI VLAN 20: 192.168.20.1/24  (Gateway VLAN 20)            │
│  SVI VLAN 30: 192.168.30.1/24  (Gateway VLAN 30)            │
│                                                             │
│  DHCP Server activo para las 3 VLANs                        │
│  ip routing habilitado                                      │
 └───────────────────────────────────────────────────────────┘
             │ Trunk (VLAN 10,20)                 │ Trunk (VLAN 20,30)
             │                                    │
 ────────────▼──────────               ───────────▼───────────
│   SW-IZQ (Capa 2)    │              │   SW-DER (Capa 2)     │
│                      │              │                       │
│ Access VLAN 10:      │              │ Access VLAN 30:       │
│ PC0,1,3,4,5,12-16,18 │  ----------- │ PC8,25-37             │
│                      │              │                       │
│ Access VLAN 20:      │              │ Access VLAN 20:       │
│ PC2,6,7,9-11,19-24   │              │ (PCs adicionales)     │
 ───────────────────────               ─────────────────────── 

