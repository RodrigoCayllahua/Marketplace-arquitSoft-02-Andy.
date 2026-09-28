# Enfoque Arquitectónico: Clean Architecture

| Elemento | Descripción Aplicada al Marketplace |
| :--- | :--- |
| **Patrón / Enfoque** | Clean Architecture (Arquitectura Limpia). |
| **Objetivo** | Separar responsabilidades y controlar las dependencias hacia el dominio. |
| **¿Qué problema resuelve?** | Evita el acoplamiento entre la interfaz Angular, las reglas del negocio y las tecnologías externas (API, BD, servicios de pago). |
| **Capas definidas** | Presentación, Aplicación, Dominio e Infraestructura. |
| **Beneficios** | • Facilita el mantenimiento y las pruebas unitarias.<br>• Permite cambiar tecnologías sin alterar la lógica de negocio.<br>• Mejora la organización del código. |

---

## Diagrama de Clean Architecture (Frontend Angular)

```mermaid
flowchart LR
    Usuario([Usuario - Cliente])

    Backend["Sistema Externo<br><b>Marketplace API REST</b><br>Backend Node.js - monolito modular<br>─────────────────────<br>/api/productos<br>/api/pedidos<br>/api/authorization<br>/api/mensajes<br>─────────────────────<br>Se integra con Niubiz y WhatsApp;<br>las credenciales viven solo aqui."]

    subgraph App["Aplicacion: Marketplace Web (Angular 18 - TypeScript) - src/app/"]
        
        subgraph Pres["1. PRESENTACION (src/app/presentacion/)"]
            CatComp["componente<br><b>CatalogoComponent</b><br>lista y filtra productos"]
            EstCar["servicio de estado<br><b>EstadoCarrito</b><br>signals - sin reglas"]
            CarComp["componente<br><b>CarritoComponent</b><br>resumen y confirmar compra"]
            AppComp["componente<br><b>AppComponent</b><br>shell de la aplicacion"]
        end

        subgraph Aplicacion["2. APLICACION - Casos de Uso (src/app/aplicacion/)"]
            CU1["caso de uso<br><b>ConsultarCatalogoCasoUso</b><br>ejecutar()"]
            CU2["caso de uso<br><b>AgregarAlCarritoCasoUso</b><br>ejecutar()"]
            CU3["caso de uso<br><b>RegistrarCompraCasoUso</b><br>ejecutar()"]
        end

        subgraph Dominio["3. DOMINIO - Nucleo (src/app/dominio/)"]
            subgraph Modelos["Modelos (entidades y reglas)"]
                EntProd["entidad<br><b>Producto</b><br>stock, categoria, precio"]
                EntCar["entidad<br><b>Carrito</b><br>inmutable - subtotal, total"]
                EntPed["entidad<br><b>Pedido</b><br>estados - cancelacion"]
                RegPre["reglas<br><b>precios.ts</b><br>comision 10% - IGV 18%"]
            end

            subgraph Contratos["Contratos (puertos)"]
                IntRepoP["interface<br><b>RepositorioProductos</b>"]
                IntRepoPed["interface<br><b>RepositorioPedidos</b>"]
                IntProcP["interface<br><b>ProcesadorPagos</b>"]
                IntNotif["interface<br><b>NotificadorCliente</b>"]
            end
        end

        subgraph Infra["4. INFRAESTRUCTURA (src/app/infraestructura/)"]
            AdRepoP["adaptador<br><b>RepositorioProductosMemoria</b><br><b>RepositorioProductosHttp</b>"]
            AdRepoPed["adaptador<br><b>RepositorioPedidosMemoria</b>"]
            AdProcP["adaptador<br><b>ProcesadorPagosSimulado</b><br><b>ProcesadorPagosNiubiz</b>"]
            AdNotif["adaptador<br><b>NotificadorConsola</b><br><b>NotificadorWhatsApp</b>"]
            
            Tokens["Angular DI<br><b>tokens.ts</b><br>InjectionToken por contrato"]
        end

        Raiz["raiz de composicion<br><b>app.config.ts</b><br>Unico archivo que elige que adaptador cumple cada contrato y lo inyecta"]

    end

    Usuario -->|navegador| Pres
    Pres -->|invoca| Aplicacion
    EstCar -.- EntCar
    Aplicacion -.- Dominio

    AdRepoP -.-|implementa| IntRepoP
    AdRepoPed -.-|implementa| IntRepoPed
    AdProcP -.-|implementa| IntProcP
    AdNotif -.-|implementa| IntNotif

    Tokens -->|registra| Raiz
    Infra -->|HTTP / JSON| Backend
```