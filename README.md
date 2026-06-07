# Fútbol Americano — Sistema de Estadísticas con Java RMI

## ¿De qué trata?

El proyecto simula un servicio de estadísticas deportivas para equipos de fútbol americano. Cada equipo tiene tres métricas:

| Métrica | Descripción |
|---------|-------------|
| **Yardas por tierra** | Yardas ganadas en jugadas de carrera (*rushing yards*) |
| **Yardas por pase** | Yardas ganadas en jugadas de pase (*passing yards*) |
| **Intercambios** | Pérdidas de balón (*turnovers*) |

Al iniciar, el servidor carga datos de ejemplo para tres equipos: **Argentina**, **Colombia** y **Ecuador**. Los clientes se conectan de forma remota para interactuar con esas estadísticas.

## Arquitectura

```
┌─────────────────────┐         RMI          ┌─────────────────────┐
│  EstadisticasCliente │ ◄──────────────────► │  EstadisticasServidor│
│  (consola)           │   consultas / updates │  + EstadisticasImpl  │
│                      │ ◄── notificaciones ── │  (datos en memoria)  │
└─────────────────────┘                        └─────────────────────┘
```

### Componentes

| Archivo | Rol |
|---------|-----|
| `EstadisticasInterfaz.java` | Interfaz remota del servidor: consultas, actualizaciones y registro de clientes |
| `EstadisticasImpl.java` | Implementación del servidor; almacena estadísticas en un `HashMap` y notifica a los clientes registrados |
| `EstadisticasServidor.java` | Punto de entrada del servidor; crea el registro RMI y publica el servicio |
| `EstadisticasClienteInterfaz.java` | Interfaz de callback para notificaciones push al cliente |
| `EstadisticasCliente.java` | Cliente de consola con menú interactivo |

## Funcionalidades

- **Consultar estadísticas** de un equipo por nombre
- **Actualizar estadísticas** (yardas por tierra, por pase e intercambios)
- **Ver tabla completa** de todos los equipos registrados
- **Notificaciones en tiempo real**: cuando un cliente actualiza un equipo, los demás clientes conectados reciben un aviso automático

## Tecnologías utilizadas

- **Java** (JDK 8 o superior recomendado)
- **Java RMI** — comunicación distribuida cliente-servidor
- **`java.rmi.registry`** — registro de objetos remotos
- **`Serializable`** — transferencia de objetos (`EquipoEstadisticas`) entre procesos
- **Consola** — interfaz de usuario por línea de comandos (`Scanner`)

## Requisitos previos

- JDK instalado y disponible en el `PATH`
- Varias terminales si quieres probar notificaciones entre múltiples clientes

## Compilación

Desde la raíz del proyecto:

```bash
javac futbol_americano/*.java
```

## Ejecución

### 1. Iniciar el servidor

```bash
java futbol_americano.EstadisticasServidor
```

El programa pedirá un **puerto** para el registro RMI (por ejemplo, `1099`). Si no existe un registro en ese puerto, se crea uno automáticamente.

### 2. Iniciar uno o más clientes

En otra terminal:

```bash
java futbol_americano.EstadisticasCliente
```

El cliente solicitará:

- **Nombre del nodo**: `localhost` si el servidor corre en la misma máquina
- **Puerto**: el mismo que indicaste al arrancar el servidor

### Menú del cliente

```
1. Consultar estadisticas de un equipo.
2. Actualizar estadisticas de un equipo.
3. Tabla de equipos.
4. Salir.
```

## Datos iniciales

| Equipo    | Yardas por tierra | Yardas por pase | Intercambios |
|-----------|-------------------|-----------------|--------------|
| Argentina | 500               | 400             | 2            |
| Colombia  | 300               | 600             | 3            |
| Ecuador   | 160               | 240             | 25           |

## Estructura del proyecto

```
futbol_americano/
├── README.md
└── futbol_americano/
    ├── EstadisticasInterfaz.java
    ├── EstadisticasImpl.java
    ├── EstadisticasServidor.java
    ├── EstadisticasClienteInterfaz.java
    └── EstadisticasCliente.java
```

## Notas

- Los datos se guardan **en memoria**; al detener el servidor se pierden los cambios no persistidos.
- Puedes registrar equipos nuevos actualizando estadísticas con un nombre que aún no exista.
- Para probar las notificaciones, abre dos clientes, actualiza un equipo en uno y observa el mensaje en el otro.

## Licencia

Proyecto educativo. Uso libre con fines de aprendizaje.
