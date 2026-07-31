
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