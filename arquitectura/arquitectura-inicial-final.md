# Arquitectura inicial del sistema

## 1. Diagrama de arquitectura en 3 capas (Ejercicio 09)

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

flowchart TD
    %% ACTORES
    subgraph ACTORES ["ACTORES"]
        Cliente ["Cliente"]
        Seller ["Seller"]
        Admin ["Administrador"]
    end

    %% PRESENTACIÓN
    subgraph PRESENTACION ["PRESENTACIÓN"]
        Web ["Aplicación Web / API REST"]
    end

    %% LÓGICA DE NEGOCIO
    subgraph NEGOCIO ["LÓGICA DE NEGOCIO"]
        Usuarios ["Usuarios"]
        Sellers ["Sellers"]
        Catalogo ["Catálogo"]
        Carrito ["Carrito"]
        Pedidos ["Pedidos"]
    end

    %% DATOS
    subgraph DATOS ["DATOS"]
        BD ["Base de datos"]
    end

    %% SISTEMAS EXTERNOS
    subgraph EXTERNOS ["SISTEMAS EXTERNOS"]
        Pago ["Pasarela de pago"]
        ERP ["ERP"]
        Envio ["Servicio de envío"]
    end

    %% RELACIONES
    ACTORES --> PRESENTACION
    PRESENTACION --> NEGOCIO
    NEGOCIO --> DATOS
    NEGOCIO -->|"Integraciones"| EXTERNOS

flowchart TD
    %% ACTORES
    subgraph ACTORES ["ACTORES"]
        Cliente ["Cliente"]
        Seller ["Seller"]
        Admin ["Administrador"]
    end

    %% PRESENTACIÓN
    subgraph PRESENTACION ["PRESENTACIÓN"]
        Web ["Aplicación Web / API REST"]
    end

    %% LÓGICA DE NEGOCIO
    subgraph NEGOCIO ["LÓGICA DE NEGOCIO"]
        Usuarios ["Usuarios"]
        Sellers ["Sellers"]
        Catalogo ["Catálogo"]
        Carrito ["Carrito"]
        Pedidos ["Pedidos"]
    end

    %% DATOS
    subgraph DATOS ["DATOS"]
        BD ["Base de datos"]
    end

    %% SISTEMAS EXTERNOS
    subgraph EXTERNOS ["SISTEMAS EXTERNOS"]
        Pago ["Pasarela de pago"]
        ERP ["ERP"]
        Envio ["Servicio de envío"]
    end

    %% RELACIONES
    ACTORES --> PRESENTACION
    PRESENTACION --> NEGOCIO
    NEGOCIO --> DATOS
    NEGOCIO -->|"Integraciones"| EXTERNOS

