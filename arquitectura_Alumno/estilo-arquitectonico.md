# Estilo Arquitectónico Seleccionado

**Estilo:** Monolito Modular con Organización en Capas.

## Justificación
Permite mantener todo el sistema en una única unidad de despliegue para simplificar la infraestructura inicial, mientras mantiene una separación lógica estricta por módulos (Usuarios, Sellers, Catálogo, Carrito, Pedidos) para facilitar la escalabilidad futura y la mantenibilidad.

## Diagrama de Arquitectura (Monolito Backend)

```mermaid
flowchart TD
    Cliente([Cliente])
    Seller([Seller])
    Admin([Administrador])

    ClienteWeb["Cliente Web<br><i>[Navegador · HTML / CSS / JavaScript]</i>"]

    Cliente --> ClienteWeb
    Seller --> ClienteWeb
    Admin --> ClienteWeb

    Pasarela["«sistema externo»<br><b>Pasarela de pagos</b><br><i>(p. ej. Culqi / Niubiz)</i>"]
    Envios["«sistema externo»<br><b>Servicio de envíos</b><br><i>(API del courier)</i>"]
    BD[(PostgreSQL<br>marketplace_db)]

    subgraph Monolito["«monolito» Marketplace Backend [Node.js 20 LTS - Express]<br>Una sola aplicación · un solo proceso · un solo despliegue"]
        
        MW["<b>Middlewares Express (transversales)</b><br>cors · express.json() · auth (JWT) · validación de entrada · manejo de errores · logger"]

        subgraph Capa1["1. CAPA DE PRESENTACIÓN"]
            direction LR
            subgraph ModUsrP["módulo usuarios"]
                UR["usuarios.routes.js"] --> UC["usuarios.controller.js"]
            end
            subgraph ModSelP["módulo sellers"]
                SR["sellers.routes.js"] --> SC["sellers.controller.js"]
            end
            subgraph ModCatP["módulo catálogo"]
                CR["catalogo.routes.js"] --> CC["catalogo.controller.js"]
            end
            subgraph ModCarP["módulo carrito"]
                CarR["carrito.routes.js"] --> CarC["carrito.controller.js"]
            end
            subgraph ModPedP["módulo pedidos"]
                PR["pedidos.routes.js"] --> PC["pedidos.controller.js"]
            end
        end

        subgraph Capa2["2. CAPA DE LÓGICA DE NEGOCIO"]
            US["<b>usuarios.service.js</b>"]
            SS["<b>sellers.service.js</b>"]
            CatS["<b>catalogo.service.js</b>"]
            CarS["<b>carrito.service.js</b>"]
            PS["<b>pedidos.service.js</b>"]
        end

        subgraph Capa3["3. CAPA DE DATOS"]
            UD["usuarios.repository.js"]
            SD["sellers.repository.js"]
            CatD["catalogo.repository.js"]
            CarD["carrito.repository.js"]
            PD["pedidos.repository.js"]
            
            ORM["<b>Acceso a datos compartido</b><br>Sequelize (ORM) · modelos · pool de conexiones"]
        end
    end

    ClienteWeb -->|HTTPS / JSON /api/v1/*| MW
    MW --> Capa1

    UC --> US
    SC --> SS
    CC --> CatS
    CarC --> CarS
    PC --> PS

    CarS -.- CatS
    PS -.- CatS
    PS -.- SS
    SS -.- US

    US --> UD
    SS --> SD
    CatS --> CatD
    CarS --> CarD
    PS --> PD

    UD --> ORM
    SD --> ORM
    CatD --> ORM
    CarD --> ORM
    PD --> ORM

    ORM -->|SQL / TCP 5432| BD
    PS <-->|HTTPS / REST| Pasarela
    PS <-->|HTTPS / REST| Envios
```