
![[InventarioSwagger.png]]

### Petición Manual de Inventario (`POST /api/inventorymovements`) 


Este es el único endpoint donde el usuario decide explícitamente qué tipo de alteración está haciendo (desde la pantalla de Kardex o Inventario). Aquí es donde el frontend ahora **debe mandar el número entero** correspondiente a tu `Enum` en lugar de enviar texto.

Recordando los valores de tu Enum:

- **1** = Entrada
    
- **2** = Merma
    
- **3** = Ajuste Positivo
    
- **4** = Ajuste Negativo
    

**Ejemplo enviando una Entrada:**



```JSON
{
  "productId": 8,
  "movementType": 1, 
  "quantity": 5.5,
  "notes": "Llegó mercancía del proveedor Truper"
}
```

**Ejemplo enviando una Merma:**


```JSON
{
  "productId": 12,
  "movementType": 2, 
  "quantity": 1,
  "notes": "La lata de pintura llegó derramada, se tira a la basura."
}
```

**Ejemplo enviando un Ajustes Positivos:**


```JSON
{
  "productId": 12,
  "movementType": 2, 
  "quantity": 1,
  "notes": "Se encontraron material en almacen"
}
```

**Ejemplo enviando un Ajustes Negativo:**


```JSON
{
  "productId": 12,
  "movementType": 2, 
  "quantity": 1,
  "notes": "Producto no encontrado. Ajuste aprovado por gerente"
}
```

## Mostrar Kardex /api/InventoryMovements/product/ID

```
curl -X 'GET' \ 'https://localhost:7183/api/InventoryMovements/product/2?page=1&pageSize=50' \ -H 'accept: text/plain'`
```


## Endpoint Maestro 4 en 1


![[InventarioswaggerSnap2.png]]

### 1. Traer los últimos movimientos de inventario (Paginado, tope 100)

Esta URL trae el historial general de toda la tlapalería sin ningún filtro adicional.

```HTTP
GET /api/inventorymovements?page=1&pageSize=100
```

### 2. Traer movimientos por rango de fechas o fecha específica (Paginado, tope 100)

Para buscar todo lo que ocurrió dentro de un periodo de tiempo.

**Por rango de fechas (Ej. del 1 de Julio al 31 de Julio):**

```HTTP
GET /api/inventorymovements?startDate=2026-07-01&endDate=2026-07-31&page=1&pageSize=100
```

**Por un día en específico (Ej. solo el 31 de Julio):** _(Nota: Al omitir `endDate`, tu controlador asume automáticamente que quieres ver solo ese día)._

```HTTP
GET /api/inventorymovements?startDate=2026-07-31&page=1&pageSize=100
```

### 3. Traer movimientos de un producto por rango de fechas o fecha específica (Paginado, tope 100)

Para hacer la auditoría del kardex de un solo artículo (ejemplo: Producto ID = `15`).

**De un producto en un rango de fechas:**

```HTTP
GET /api/inventorymovements?productId=15&startDate=2026-07-01&endDate=2026-07-31&page=1&pageSize=100
```

**De un producto en un día en específico:**

```HTTP
GET /api/inventorymovements?productId=15&startDate=2026-07-31&page=1&pageSize=100
```

### 4. Traer movimientos filtrados por su tipo (Paginado, tope 100)

Para ver solamente una categoría de movimientos usando el catálogo numérico del Enum.

**Solo Entradas (1):**

```url
GET /api/inventorymovements?movementType=1&page=1&pageSize=100
```

**Solo Mermas (2):**

```url
GET /api/inventorymovements?movementType=2&page=1&pageSize=100
```

**Solo Ajustes Positivos (3):**

```url
GET /api/inventorymovements?movementType=3&page=1&pageSize=100
```

**Solo Ajustes Negativos (4):**

```HTTP
GET /api/inventorymovements?movementType=4&page=1&pageSize=100
```

**Solo Ventas de mostrador (5):**

```HTTP
GET /api/inventorymovements?movementType=5&page=1&pageSize=100
```

**Solo Devoluciones (6):**

```HTTP
GET /api/inventorymovements?movementType=6&page=1&pageSize=100
```