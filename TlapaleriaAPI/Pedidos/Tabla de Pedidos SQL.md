
Se detallan a continuación la estructura de la tabla de Pedidos 

```SQL
CREATE TABLE PendingOrders (
    Id INT AUTO_INCREMENT PRIMARY KEY,
    
    -- RELACIONES
    ProductId INT NOT NULL,     -- Para saber EXACTAMENTE qué producto es
    SupplierId INT NULL,        -- Opcional al inicio (puedes anotarlo y luego decidir a quién pedirle)
    UserId INT NOT NULL,        -- Quién lo anotó en la libreta (El empleado)

    -- DATOS DE LA LISTA
    QuantityText VARCHAR(100) NOT NULL, -- Ej: "1", "3 bolsas", "Media caja"
    Notes TEXT NULL,                    -- Ej: "Preguntar por el precio si es menor de 100..."
    
    -- EL FLUJO DE TRABAJO (La magia)
    Status VARCHAR(30) NOT NULL DEFAULT 'Pendiente', 
    -- Estados sugeridos: 
    -- 'Pendiente' (Recién anotado)
    -- 'Pedido' (Ya se le encargó al proveedor)
    -- 'No Llego' (El proveedor falló, hay que cambiar SupplierId y regresarlo a Pendiente)
    -- 'Completado' (Ya llegó y se metió al inventario)

    CreatedAt DATETIME DEFAULT CURRENT_TIMESTAMP,
    UpdatedAt DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
```

# Relaciones 
Sus relaciones con las Tablas de Productos(products), Proveedores(suppliers) y Usuarios(users) 

```SQL 
    -- CONSTRAINTS (Para que no haya datos fantasma)
    CONSTRAINT FK_Pending_Products FOREIGN KEY (ProductId) REFERENCES Products(Id),
    CONSTRAINT FK_Pending_Suppliers FOREIGN KEY (SupplierId) REFERENCES Suppliers(Id),
    CONSTRAINT FK_Pending_Users FOREIGN KEY (UserId) REFERENCES Users(Id)
) CHARACTER SET utf8mb4 COLLATE utf8mb4_spanish_ci;
```

# Imagen de relaciones 

![[Tabla_Pendientes_Relaciones.png]]