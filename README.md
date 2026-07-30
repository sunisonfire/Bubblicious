# 🐾 Bubblicious — Sistema Automatizado de Pedidos para Cafetería
> **Automatización de pedidos e integración de cocina mediante n8n, Telegram Bot API y Google Sheets.**
sheets: https://docs.google.com/spreadsheets/d/1qJi4nragkXleTPK5rgU2Kd1f-gvQ8lW8lhr7XYEYb7Y/edit?usp=sharing
> bot delivery: t.me/catfeteria_bot
> bot cocina: t.me/BoobieAlert_bot
---
# Update: Examen 1

## Objetivo

Se implementó una mejora en el flujo de automatización del bot de pedidos desarrollado en n8n, incorporando una lógica de control de inventario y notificaciones automáticas para la administración.

## Funcionalidades implementadas

### 1. Descuento automático de inventario

Una vez que el cliente confirma su pedido y este se registra correctamente, el sistema descuenta automáticamente la cantidad comprada del stock disponible en la hoja **MENU** de Google Sheets.

### 2. Detección de stock crítico

Después de actualizar el inventario, el flujo verifica si el nuevo stock del producto es menor o igual a **3 unidades** mediante un nodo **IF**.

### 3. Notificación automática al administrador

Cuando un producto alcanza un stock crítico, el sistema envía automáticamente un mensaje mediante el bot de Telegram de administración con el siguiente formato:

> ⚠️ ALERTA DE STOCK
> El producto **[Nombre del producto]** solo tiene **[Cantidad]** unidades disponibles. Favor reabastecer.

De esta manera, el personal de cocina o el administrador recibe una alerta antes de que el producto se agote completamente.

### 4. Actualización automática de productos agotados

Cuando el stock llega a **0**, el sistema identifica el producto como agotado y actualiza automáticamente la información correspondiente en la hoja **MENU**, evitando que continúe apareciendo como disponible para futuras compras.

## Archivos actualizados

* Workflow actualizado (`.json`) con la lógica de inventario y alertas.
* Documento `README.md` actualizado con la explicación de la implementación.

## Evidencias

Para demostrar el correcto funcionamiento del examen se anexan las siguientes evidencias:

<img width="1197" height="384" alt="image" src="https://github.com/user-attachments/assets/1bb2a9b4-86b8-46bd-906c-d976b30bdd8c" />



2. Captura de Telegram donde se observa la alerta enviada automáticamente al administrador.
<img width="487" height="80" alt="image" src="https://github.com/user-attachments/assets/185b2e6b-ed4e-4a01-89b3-044c0ff21e55" />

3. Captura de Google Sheets evidenciando la actualización del stock después de un pedido exitoso y, cuando corresponde, el estado de producto agotado.
<img width="905" height="277" alt="image" src="https://github.com/user-attachments/assets/b6639f3a-96f9-4da6-bb98-0ac3b4b22823" />


## Resultado

Con esta actualización el sistema no solo administra el inventario automáticamente, sino que también informa de manera proactiva cuando un producto está próximo a agotarse, permitiendo una reposición oportuna y mejorando la gestión de la cafetería.

## 📌 Problemática y Solución

En entornos institucionales como oficinas, universidades o grandes centros de trabajo, la gestión de pedidos de cafetería suele ser ineficiente, provocando filas largas y errores en la toma de pedidos manual. La falta de un sistema digitalizado impide que el personal de cocina organice sus tareas y que los usuarios conozcan el tiempo real de entrega de sus productos.

**Bubblicious** es una solución integral de automatización basada en **n8n** que convierte a Telegram en una terminal de pedidos inteligente. El sistema opera a través de dos bots independientes (uno para clientes con una cálida personalidad inspirada en un café de gatos, y otro exclusivo para el personal de cocina), sincronizados en tiempo real con **Google Sheets** como base de datos centralizada.

---

## 🎯 Objetivos del Proyecto

* **Interfaz Conversacional:** Implementar un sistema de pedidos digital a través de Telegram.
* **Cálculo y Generación Automática:** Calcular totales, gestionar carritos y generar identificadores únicos de orden (`id_pedido`).
* **Gestión del Ciclo de Vida:** Administrar estados dinámicos (*Recibido*, *En preparación*, *En camino*, *Entregado*, *Cancelado*).
* **Centralización en la Nube:** Utilizar Google Sheets para inventario, usuarios, sesiones y registro de pedidos.
* **Notificaciones Push Automáticas:** Informar al cliente instantáneamente sobre cada cambio en el estado de su pedido.
* **Sistema de Fidelización:** Sumar puntos acumulables por cada pedido completado con éxito.

---

## 🛠️ Tecnologías Utilizadas

* **[n8n](https://n8n.io/):** Motor principal de orquestación de flujos de trabajo y lógica de negocio.
* **Telegram Bot API:** Interfaz gráfica y conversacional para Clientes y Personal de Cocina.
* **Google Sheets API:** Base de datos relacional/persistente para el catálogo, usuarios y órdenes.
* **JavaScript (Code Nodes in n8n):** Mapeo de datos, transformaciones de texto y gestión de lógica condicional.

---

## 📊 Arquitectura del Modelo de Datos (`Bubblicious_DB`)

La base de datos estructurada en **Google Sheets** cuenta con las siguientes pestañas:

| Hoja | Descripción | Columnas Principales |
| :--- | :--- | :--- |
| **`MENU`** | Catálogo general de productos (Bebidas, Comidas, Snacks) | `id_producto`, `nombre`, `descripcion`, `precio`, `categoria`, `stock` |
| **`PEDIDOS`** | Registro histórico y activo de órdenes confirmadas | `id_pedido`, `telegram_id`, `detalle`, `total`, `estado`, `fecha`, `hora`, `destino`, `destinatario` |
| **`USUARIOS`** | Clientes registrados y programa de fidelidad | `telegram_id`, `nombre`, `documento`, `ciudad`, `puntos` |
| **`SESSIONS`** | Mantiene el carrito de compras temporal del cliente | `telegram_id`, `pantalla_actual`, `carrito_temporal`, `ultimo_cambio` |
| **`KITCHEN_SESSION`** | Máquina de estados para el contexto del cocinero | `telegram_id`, `paso`, `id_pedido` |

---

## 🤖 Módulos del Sistema

### 1. Bot de Clientes
* **Registro de Usuarios:** Alta automática de nuevos clientes en la hoja `USUARIOS`.
* **Navegación Interactiva:** Exploración del menú categorizado (Bebidas, Almuerzos, Snacks).
* **Gestión de Carrito:** Agregar productos, modificar cantidades y vaciado automático tras confirmar.
* **Confirmación de Orden:** Generación automática de `id_pedido` (Ej. `PED-20260724085309`), registro en `PEDIDOS` y alerta inmediata a la cocina.
* **Personalidad del Bot:** Tono cálido, amigable e inspirado en un café temático de gatos 🐱.

---

### 2. Bot de Cocina (Gestión Operativa)
Bot exclusivo para el personal de cocina con comandos estructurados:

* `/start`: Despliega el panel de control principal.
* `/pedidos`: Consulta todos los pedidos en estado **Recibido**.
  ```text
  Telegram Trigger ➔ Switch ➔ /pedidos ➔ Get Rows (PEDIDOS where estado='Recibido') ➔ Code ➔ Telegram
  ```
* `/estado`: Inicia el flujo conversacional paso a paso para actualizar el estado de una orden.

#### 🔄 Flujo Conversacional de Cambio de Estado (`/estado`):
1. **Paso 1 (Solicitud de ID):** El cocinero ingresa `/estado`. El bot verifica/crea la sesión en `KITCHEN_SESSION` asignando `paso = esperando_id` y responde:
   > ✏️ *Escriba el ID del pedido.*
2. **Paso 2 (Validación de ID):** El cocinero envía el código (ej. `PED-20260724085309`). 
   * Si **no existe**, reintenta.
   * Si **existe**, actualiza `paso = esperando_estado` y presenta las opciones:
     > 1️⃣ En preparación  
     > 2️⃣ En camino  
     > 3️⃣ Entregado  
     > 4️⃣ Cancelado  
3. **Paso 3 (Procesamiento y Notificación):** El cocinero responde con el número (`1`, `2`, `3` o `4`).
   * Un nodo **Code** convierte la opción elegida al texto del estado correspondiente.
   * Se actualiza la fila en Google Sheets mediante **Update Row** usando `Match On: id_pedido`.
   * **Notificación Automática:** n8n toma el `telegram_id` del cliente almacenado en la orden y le envía un mensaje push instantáneo:
     ```text
     🐾 ¡Miau! Tenemos novedades sobre tu pedido.
     📦 Pedido: PED-20260724085309
     Ahora está en: 🍳 En preparación
     Te avisaremos cuando vuelva a cambiar de estado.
     ```
   * La sesión en `KITCHEN_SESSION` se reinicia para gestionar una nueva orden.

---

## 🔄 Ciclo de Vida del Pedido

```text
[ Cliente realiza pedido ]
           │
           ▼
     ( Recibido ) ──► [ Cocina consulta /pedidos ]
           │
           ▼
  ( En preparación ) ──► [ Notificación Push a Cliente ]
           │
           ▼
     ( En camino )    ──► [ Notificación Push a Cliente ]
           │
           ▼
     ( Entregado )   ──► [ Notificación + Suma de Puntos de Lealtad ]
           │
           └──────────► ( Cancelado en cualquier momento )
```

---

## 🛠️ Desafíos Técnicos Resueltos Durante el Desarrollo

1. **Persistencia y Actualización Exacta:** Configuración precisa del nodo **Update Row** mediante la clave unívoca `Match On: id_pedido`.
2. **Manejo de Contexto Multi-usuario:** Control de flujo mediante la tabla `KITCHEN_SESSION` para evitar que múltiples cocineros solapen comandos.
3. **Manejo de Respuestas por Defecto:** Configuración correcta de la rama **Default** del nodo `Switch` para capturar cualquier mensaje que no sea un comando explícito.
4. **Transformación de Datos Dinámica:** Ajustes en nodos `Code` para extraer correctamente las variables y objetos desde el payload original de `Telegram Trigger`.
5. **Notificaciones Cruzadas Sin Ramas Adicionales:** Reutilización de un único subflujo dinámico para enviar notificaciones al cliente independiente del estado seleccionado.

---

## 📈 Estado Actual del Proyecto

El sistema se encuentra **completamente operativo de extremo a extremo (E2E)**, cubriendo el ciclo completo de negocio: desde la selección de productos y generación del ticket por parte del usuario, hasta la gestión operativa en cocina y el seguimiento en tiempo real.

---
*Desarrollado con ❤️ para Cafetería Bubblicious.*
