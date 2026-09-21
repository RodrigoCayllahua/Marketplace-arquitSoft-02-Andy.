# Arquitectura inicial del sistema

## Diagrama de arquitectura en capas

```mermaid
flowchart TD
    subgraph PRESENTACION ["PRESENTACIÓN"]
        P["Web / API / Interfaz"]
    end

    subgraph NEGOCIO ["LÓGICA DE NEGOCIO"]
        N["Catálogo<br/>Carrito<br/>Pedidos<br/>Sellers<br/>Usuarios"]
    end

    subgraph DATOS ["DATOS"]
        D["Base de datos"]
    end

    PRESENTACION --> NEGOCIO
    NEGOCIO --> DATOS