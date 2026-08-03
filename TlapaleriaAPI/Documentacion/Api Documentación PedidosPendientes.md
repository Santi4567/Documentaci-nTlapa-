
## Agregar 

```http 
curl -X 'POST' \ 'https://localhost:7183/api/PendingOrders' \ -H 'accept: text/plain' \ -H 'Content-Type: application/json' \ -d '{ "productId": 3, "supplierId": 1, "quantityText": "1", "notes": "" }'
```

respuesta 
```
{ "success": true, "message": "Faltante agregado a la libreta correctamente.", "data": { "id": 2, "productId": 3, "product": { "id": 3, "internalCode": "THI-STD", "barcode": "75010000003", "name": "Thinner Estándar", "description": "Solvente para limpieza y pintura", "brand": "Sayer Lack", "location": "Zona de Químicos", "supplierId": 1, "supplier": { "id": 1, "name": "Ferrebastones", "contactName": "Carlos N", "phone": "2222222", "isActive": true }, "supplierPrice": 35, "profitMargin": 40, "lastOrderDate": null, "unitOfMeasure": "LITRO", "currentStock": 200, "allowFractions": false, "presentations": [], "isActive": true, "createdAt": "2026-03-06T15:52:37", "updatedAt": "2026-06-16T13:27:27", "isInventoryTracked": true, "hasExpiration": false, "nextExpirationDate": null }, "supplierId": 1, "supplier": { "id": 1, "name": "Ferrebastones", "contactName": "Carlos N", "phone": "2222222", "isActive": true }, "userId": 1, "user": { "id": 1, "username": "admin@test.com", "name": "Juan Perez", "rolId": 1, "rol": null, "isActive": true }, "quantityText": "1", "notes": "", "status": 0, "createdAt": "2026-08-03T15:53:47.1197379-06:00", "updatedAt": "2026-08-03T15:53:47.1197855-06:00" } }
```


## Actualizar 

```http 
curl -X 'PUT' \ 'https://localhost:7183/api/PendingOrders/1' \ -H 'accept: text/plain' \ -H 'Content-Type: application/json' \ -d '{ "supplierId": 1, "quantityText": "2", "notes": "" }'
```
## Filtro maestro  GET /api/pendingorders/filter

![[Pasted image 20260803155114.png]]

Recordatorio de valores de Enum 

- `0` = Pendiente
    
- `1` = Cancelado
    
- `2` = Completado
    

Aquí tienes exactamente cómo se verían las peticiones HTTP (las URLs que pondrías en tu frontend, Postman o Insomnia) para cada una de tus 5 metas:

### 1. Ver todos los pendientes sin importar sus estados

Como todos los parámetros son opcionales (`null` por defecto), si no envías nada, el API te devuelve todo el historial completo de la tabla.


``` http
GET /api/pendingorders/filter
```

Respuesta
```
{ "success": true, "message": "Filtros aplicados exitosamente", "data": { "data": [ { "id": 2, "productId": 3, "product": { "id": 3, "internalCode": "THI-STD", "barcode": "75010000003", "name": "Thinner Estándar", "description": "Solvente para limpieza y pintura", "brand": "Sayer Lack", "location": "Zona de Químicos", "supplierId": 1, "supplier": null, "supplierPrice": 35, "profitMargin": 40, "lastOrderDate": null, "unitOfMeasure": "LITRO", "currentStock": 200, "allowFractions": false, "presentations": [], "isActive": true, "createdAt": "2026-03-06T15:52:37", "updatedAt": "2026-06-16T13:27:27", "isInventoryTracked": true, "hasExpiration": false, "nextExpirationDate": null }, "supplierId": 1, "supplier": { "id": 1, "name": "Ferrebastones", "contactName": "Carlos N", "phone": "2222222", "isActive": true }, "userId": 1, "user": { "id": 1, "username": "admin@test.com", "name": "Juan Perez", "rolId": 1, "rol": null, "isActive": true }, "quantityText": "1", "notes": "", "status": 0, "createdAt": "2026-08-03T15:53:47", "updatedAt": "2026-08-03T15:53:47" }, { "id": 1, "productId": 2, "product": { "id": 2, "internalCode": "CAB-12-R", "barcode": "75010000002", "name": "Cable THW Calibre 12 Rojo", "description": "Cable de cobre con aislamiento", "brand": "IUSA", "location": "Pasillo 2, Estante C", "supplierId": 1, "supplier": null, "supplierPrice": 900, "profitMargin": 35, "lastOrderDate": null, "unitOfMeasure": "METRO", "currentStock": 609, "allowFractions": true, "presentations": [], "isActive": true, "createdAt": "2026-03-06T15:52:37", "updatedAt": "2026-08-03T11:02:13", "isInventoryTracked": true, "hasExpiration": false, "nextExpirationDate": null }, "supplierId": 2, "supplier": { "id": 2, "name": "Ferre Batones", "contactName": "string", "phone": "2223334445", "isActive": true }, "userId": 1, "user": { "id": 1, "username": "admin@test.com", "name": "Juan Perez", "rolId": 1, "rol": null, "isActive": true }, "quantityText": "1 rollo", "notes": "", "status": 0, "createdAt": "2026-08-03T15:15:26", "updatedAt": "2026-08-03T15:15:26" } ], "totalItems": 2, "totalPages": 1, "currentPage": 1 } }
```
### 2. Ver los pendientes seleccionando el estado

Solo agregas el parámetro `status` con el número correspondiente al Enum.

- **Solo Pendientes:**
    
    
    
    ```http
    GET /api/pendingorders/filter?status=0
    ```

- **Solo Completados:**
    
    HTTP
    
    ```
    GET /api/pendingorders/filter?status=2
    ```
    
 Respuesta 
 
  ```
  { "success": true, "message": "Filtros aplicados exitosamente", "data": { "data": [ { "id": 2, "productId": 3, "product": { "id": 3, "internalCode": "THI-STD", "barcode": "75010000003", "name": "Thinner Estándar", "description": "Solvente para limpieza y pintura", "brand": "Sayer Lack", "location": "Zona de Químicos", "supplierId": 1, "supplier": null, "supplierPrice": 35, "profitMargin": 40, "lastOrderDate": null, "unitOfMeasure": "LITRO", "currentStock": 200, "allowFractions": false, "presentations": [], "isActive": true, "createdAt": "2026-03-06T15:52:37", "updatedAt": "2026-06-16T13:27:27", "isInventoryTracked": true, "hasExpiration": false, "nextExpirationDate": null }, "supplierId": 1, "supplier": { "id": 1, "name": "Ferrebastones", "contactName": "Carlos N", "phone": "2222222", "isActive": true }, "userId": 1, "user": { "id": 1, "username": "admin@test.com", "name": "Juan Perez", "rolId": 1, "rol": null, "isActive": true }, "quantityText": "1", "notes": "", "status": 0, "createdAt": "2026-08-03T15:53:47", "updatedAt": "2026-08-03T15:53:47" }, { "id": 1, "productId": 2, "product": { "id": 2, "internalCode": "CAB-12-R", "barcode": "75010000002", "name": "Cable THW Calibre 12 Rojo", "description": "Cable de cobre con aislamiento", "brand": "IUSA", "location": "Pasillo 2, Estante C", "supplierId": 1, "supplier": null, "supplierPrice": 900, "profitMargin": 35, "lastOrderDate": null, "unitOfMeasure": "METRO", "currentStock": 609, "allowFractions": true, "presentations": [], "isActive": true, "createdAt": "2026-03-06T15:52:37", "updatedAt": "2026-08-03T11:02:13", "isInventoryTracked": true, "hasExpiration": false, "nextExpirationDate": null }, "supplierId": 2, "supplier": { "id": 2, "name": "Ferre Batones", "contactName": "string", "phone": "2223334445", "isActive": true }, "userId": 1, "user": { "id": 1, "username": "admin@test.com", "name": "Juan Perez", "rolId": 1, "rol": null, "isActive": true }, "quantityText": "1 rollo", "notes": "", "status": 0, "createdAt": "2026-08-03T15:15:26", "updatedAt": "2026-08-03T15:15:26" } ], "totalItems": 2, "totalPages": 1, "currentPage": 1 } }
  ```
### 3. Ver todos los movimientos de un producto por su ID (Historial y Fechas)

Aquí combinas el `productId` con las fechas. El formato de fecha estándar para enviar en la URL es `YYYY-MM-DD`.

- **Todo el historial del producto (sin importar fecha):**
    
    HTTP
    
    ```
    GET /api/pendingorders/filter?productId=15
    ```
    
- **Movimientos del producto en un rango de fechas (ej. Todo agosto):**
    
    HTTP
    
    ```
    GET /api/pendingorders/filter?productId=15&startDate=2026-08-01&endDate=2026-08-31
    ```
    
- **Movimientos del producto en un día en específico (Mismo día en inicio y fin):**
    
    HTTP
    
    ```
    GET /api/pendingorders/filter?productId=15&startDate=2026-08-03&endDate=2026-08-03
    ```
    

### 4. Ver los productos que se están pidiendo a un proveedor en específico

Puedes combinar el `supplierId` con estados y fechas según lo que necesite el usuario en ese momento.

- **Todo lo que se le ha pedido al proveedor 3 (Cualquier estado):**
    
    HTTP
    
    ```
    GET /api/pendingorders/filter?supplierId=3
    ```
    
- **Solo lo que está "Pendiente" de pedirle al proveedor 3:**
    
    HTTP
    
    ```
    GET /api/pendingorders/filter?supplierId=3&status=0
    ```
    
- **Lo que se le pidió al proveedor 3 en una fecha específica:**
    
    HTTP
    
    ```
    GET /api/pendingorders/filter?supplierId=3&startDate=2026-08-01&endDate=2026-08-05
    ```
    

### 5. Buscar un producto dentro de la tabla (Buscador de texto)

Envías lo que el usuario escriba en la barra de búsqueda al parámetro `search`. Esto buscará por nombre, código interno o código de barras.

HTTP

```
GET /api/pendingorders/filter?search=clavos
```

### 💡 Nota sobre la Paginación

Recuerda que en cualquiera de estas 5 peticiones, siempre puedes agregar `page` y `pageSize` al final de la URL si necesitas controlar la paginación desde tu frontend (si no los mandas, por defecto te traerá la página 1 con 50 registros).

**Ejemplo combinando todo (Búsqueda + Paginación):**

HTTP

```
GET /api/pendingorders/filter?search=cemento&status=0&page=2&pageSize=20
```


## Actualizar el estado 

```http
curl -X 'GET' \ 'https://localhost:7183/api/PendingOrders/filter?status=0&page=1&pageSize=50' \ -H 'accept: text/plain'
```

 ```respuesta
 { "success": true, "message": "Filtros aplicados exitosamente", "data": { "data": [ { "id": 2, "productId": 3, "product": { "id": 3, "internalCode": "THI-STD", "barcode": "75010000003", "name": "Thinner Estándar", "description": "Solvente para limpieza y pintura", "brand": "Sayer Lack", "location": "Zona de Químicos", "supplierId": 1, "supplier": null, "supplierPrice": 35, "profitMargin": 40, "lastOrderDate": null, "unitOfMeasure": "LITRO", "currentStock": 200, "allowFractions": false, "presentations": [], "isActive": true, "createdAt": "2026-03-06T15:52:37", "updatedAt": "2026-06-16T13:27:27", "isInventoryTracked": true, "hasExpiration": false, "nextExpirationDate": null }, "supplierId": 1, "supplier": { "id": 1, "name": "Ferrebastones", "contactName": "Carlos N", "phone": "2222222", "isActive": true }, "userId": 1, "user": { "id": 1, "username": "admin@test.com", "name": "Juan Perez", "rolId": 1, "rol": null, "isActive": true }, "quantityText": "1", "notes": "", "status": 0, "createdAt": "2026-08-03T15:53:47", "updatedAt": "2026-08-03T15:53:47" }, { "id": 1, "productId": 2, "product": { "id": 2, "internalCode": "CAB-12-R", "barcode": "75010000002", "name": "Cable THW Calibre 12 Rojo", "description": "Cable de cobre con aislamiento", "brand": "IUSA", "location": "Pasillo 2, Estante C", "supplierId": 1, "supplier": null, "supplierPrice": 900, "profitMargin": 35, "lastOrderDate": null, "unitOfMeasure": "METRO", "currentStock": 609, "allowFractions": true, "presentations": [], "isActive": true, "createdAt": "2026-03-06T15:52:37", "updatedAt": "2026-08-03T11:02:13", "isInventoryTracked": true, "hasExpiration": false, "nextExpirationDate": null }, "supplierId": 1, "supplier": { "id": 1, "name": "Ferrebastones", "contactName": "Carlos N", "phone": "2222222", "isActive": true }, "userId": 1, "user": { "id": 1, "username": "admin@test.com", "name": "Juan Perez", "rolId": 1, "rol": null, "isActive": true }, "quantityText": "2", "notes": "", "status": 0, "createdAt": "2026-08-03T15:15:26", "updatedAt": "2026-08-03T16:02:15" } ], "totalItems": 2, "totalPages": 1, "currentPage": 1 } }
 ```


## traer datos de la tabla con el ID 

```http
curl -X 'GET' \ 'https://localhost:7183/api/PendingOrders/3' \ -H 'accept: text/plain'
```


respuesta 
```
{ "success": true, "message": "Operación exitosa", "data": { "id": 3, "productId": 2, "product": { "id": 2, "internalCode": "CAB-12-R", "barcode": "75010000002", "name": "Cable THW Calibre 12 Rojo", "description": "Cable de cobre con aislamiento", "brand": "IUSA", "location": "Pasillo 2, Estante C", "supplierId": 1, "supplier": { "id": 1, "name": "Ferrebastones", "contactName": "Carlos N", "phone": "2222222", "isActive": true }, "supplierPrice": 900, "profitMargin": 35, "lastOrderDate": null, "unitOfMeasure": "METRO", "currentStock": 609, "allowFractions": true, "presentations": [], "isActive": true, "createdAt": "2026-03-06T15:52:37", "updatedAt": "2026-08-03T11:02:13", "isInventoryTracked": true, "hasExpiration": false, "nextExpirationDate": null }, "supplierId": 1, "supplier": { "id": 1, "name": "Ferrebastones", "contactName": "Carlos N", "phone": "2222222", "isActive": true }, "userId": 1, "user": { "id": 1, "username": "admin@test.com", "name": "Juan Perez", "rolId": 1, "rol": null, "isActive": true }, "quantityText": "1", "notes": "", "status": 0, "createdAt": "2026-08-03T16:04:57", "updatedAt": "2026-08-03T16:04:57" } }
```