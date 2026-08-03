```sql 
-- 1. Apagamos temporalmente la revisión de llaves foráneas
SET FOREIGN_KEY_CHECKS = 0;

-- 2. Estructura y Datos de 'roles'
DROP TABLE IF EXISTS `roles`;
CREATE TABLE `roles` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `Nombre` varchar(50) COLLATE utf8mb4_spanish_ci NOT NULL,
  PRIMARY KEY (`Id`),
  UNIQUE KEY `Nombre` (`Nombre`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `roles` VALUES (1,'Admin'),(4,'Administracion'),(2,'Gerente'),(3,'Vendedor');

-- 3. Estructura y Datos de 'permisos'
DROP TABLE IF EXISTS `permisos`;
CREATE TABLE `permisos` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `NombreSistema` varchar(100) COLLATE utf8mb4_spanish_ci NOT NULL,
  `Descripcion` varchar(200) COLLATE utf8mb4_spanish_ci DEFAULT NULL,
  PRIMARY KEY (`Id`),
  UNIQUE KEY `NombreSistema` (`NombreSistema`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `permisos` VALUES (1,'add.users','Crear usuarios'),(2,'edit.users','Editar usuarios'),(3,'delete.users','Eliminar usuarios'),(4,'view.users','Ver lista de usuarios'),(5,'add.products','Agregar productos'),(6,'view.products','Ver productos'),(7,'edit.products','Editar productos'),(8,'delete.products','Eliminar productos'),(9,'users.reset_password','Permite cambiar la contrase├▒a de otros usuarios'),(10,'view.suppliers','Ver lista de proveedores'),(11,'add.suppliers','Agregar nuevos proveedores'),(12,'edit.suppliers','Editar informaci├│n de proveedores'),(13,'delete.suppliers','Eliminar (desactivar) proveedores'),(14,'add.pendingorders','Agregar productos a la lista de pendientes '),(15,'view.pendingorders','Ver productos pendientes'),(17,'add.inventorymovements','Agregar movimientos de inventario'),(18,'view.inventorymovements','Ver movimientos de inventario'),(19,'edit.pendingorders','Editar productos pendientes'),(20,'view.returns','Ver lista de devoluciones'),(21,'add.returns','Agregar nuevas devoluciones'),(22,'add.sales','Registrar nuevas ventas'),(23,'view.sales','Ver historial de ventas');

-- 4. Estructura y Datos de 'rolpermiso'
DROP TABLE IF EXISTS `rolpermiso`;
CREATE TABLE `rolpermiso` (
  `RolId` int NOT NULL,
  `PermisoId` int NOT NULL,
  PRIMARY KEY (`RolId`,`PermisoId`),
  KEY `FK_RolPermiso_Permisos` (`PermisoId`),
  CONSTRAINT `FK_RolPermiso_Permisos` FOREIGN KEY (`PermisoId`) REFERENCES `permisos` (`Id`) ON DELETE CASCADE,
  CONSTRAINT `FK_RolPermiso_Roles` FOREIGN KEY (`RolId`) REFERENCES `roles` (`Id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish2_ci;

INSERT INTO `rolpermiso` VALUES (1,1),(2,1),(1,2),(1,3),(1,4),(2,4),(1,5),(1,6),(2,6),(3,6),(1,7),(2,7),(1,8),(1,9),(1,10),(1,11),(1,12),(1,13),(1,14),(1,15),(1,17),(1,18),(1,19),(1,20),(1,21),(1,22),(1,23);

-- 5. Estructura y Datos de 'users'
DROP TABLE IF EXISTS `users`;
CREATE TABLE `users` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `Username` varchar(50) COLLATE utf8mb4_spanish_ci NOT NULL,
  `Passwd` varchar(255) COLLATE utf8mb4_spanish_ci NOT NULL,
  `Name` varchar(100) COLLATE utf8mb4_spanish_ci NOT NULL,
  `CreatedAt` datetime DEFAULT CURRENT_TIMESTAMP,
  `IsActive` tinyint(1) DEFAULT '1',
  `RolId` int DEFAULT NULL,
  PRIMARY KEY (`Id`),
  UNIQUE KEY `Username` (`Username`),
  KEY `FK_Users_Roles` (`RolId`),
  CONSTRAINT `FK_Users_Roles` FOREIGN KEY (`RolId`) REFERENCES `roles` (`Id`) ON UPDATE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `users` VALUES (1,'admin@test.com','$2a$11$EuQr3Wu8/i1I5x/Bl7XlDOrJr4Oyh.p7FwnK7qe8QHJ2/0B/m7ff2','Juan Perez','2025-12-17 19:20:42',1,1),(3,'juan@test.com','$2a$11$yCN5MAPJK.2s5JEllgZKq.EvYb80yc9TFgz3oKTBeSU.U/n.PgIJS','Juan','2025-12-18 18:02:49',1,3);

-- 6. Estructura y Datos de 'suppliers' (AQUÍ ESTÁ LA MAGIA CORREGIDA)
DROP TABLE IF EXISTS `suppliers`;
CREATE TABLE `suppliers` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `Name` varchar(100) COLLATE utf8mb4_spanish_ci NOT NULL,
  `ContactName` varchar(100) COLLATE utf8mb4_spanish_ci NOT NULL,
  `Phone` varchar(20) COLLATE utf8mb4_spanish_ci NOT NULL,
  `IsActive` tinyint(1) NOT NULL DEFAULT '1', -- Cambiado de bit a tinyint
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

-- Insert usando 1 en vez de _binary
INSERT INTO `suppliers` VALUES (1,'Ferrebastones','Carlos N','2222222',1),(2,'Ferre Batones','string','2223334445',1);

-- 7. Estructura y Datos de 'products'
DROP TABLE IF EXISTS `products`;
CREATE TABLE `products` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `InternalCode` varchar(50) COLLATE utf8mb4_spanish_ci NOT NULL,
  `Barcode` varchar(100) COLLATE utf8mb4_spanish_ci DEFAULT NULL,
  `Name` varchar(200) COLLATE utf8mb4_spanish_ci NOT NULL,
  `Description` text COLLATE utf8mb4_spanish_ci,
  `Brand` varchar(100) COLLATE utf8mb4_spanish_ci DEFAULT NULL,
  `Location` varchar(100) COLLATE utf8mb4_spanish_ci DEFAULT NULL,
  `SupplierId` int NOT NULL,
  `SupplierPrice` decimal(10,2) NOT NULL,
  `ProfitMargin` decimal(5,2) DEFAULT NULL,
  `LastOrderDate` datetime DEFAULT NULL,
  `UnitOfMeasure` varchar(20) COLLATE utf8mb4_spanish_ci NOT NULL,
  `CurrentStock` decimal(10,3) NOT NULL DEFAULT '0.000',
  `IsActive` tinyint(1) NOT NULL DEFAULT '1',
  `CreatedAt` datetime DEFAULT CURRENT_TIMESTAMP,
  `UpdatedAt` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  `IsInventoryTracked` tinyint(1) NOT NULL DEFAULT '1',
  `HasExpiration` tinyint(1) NOT NULL DEFAULT '0',
  `NextExpirationDate` datetime DEFAULT NULL,
  `AllowFractions` tinyint(1) NOT NULL DEFAULT '0',
  PRIMARY KEY (`Id`),
  UNIQUE KEY `InternalCode` (`InternalCode`),
  KEY `FK_Products_Suppliers` (`SupplierId`),
  CONSTRAINT `FK_Products_Suppliers` FOREIGN KEY (`SupplierId`) REFERENCES `suppliers` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `products` VALUES (1,'CEM-001','75010000001','Cemento Gris Tipo CPO','Cemento de uso general','Cruz Azul','Pasillo 10, Tarima A',1,150.00,20.00,NULL,'KG',848.000,1,'2026-03-06 15:52:37','2026-06-16 13:19:52',1,0,NULL,0),(2,'CAB-12-R','75010000002','Cable THW Calibre 12 Rojo','Cable de cobre con aislamiento','IUSA','Pasillo 2, Estante C',1,900.00,35.00,NULL,'METRO',609.000,1,'2026-03-06 15:52:37','2026-08-03 11:02:13',1,0,NULL,1),(3,'THI-STD','75010000003','Thinner Est├índar','Solvente para limpieza y pintura','Sayer Lack','Zona de Qu├¡micos',1,35.00,40.00,NULL,'LITRO',200.000,1,'2026-03-06 15:52:37','2026-06-16 13:27:27',1,0,NULL,0),(4,'CIN-AIS','75010000004','Cinta de Aislar Negra 18m','Cinta vin├¡lica uso el├®ctrico','Truper','Mostrador, Caja 1',1,12.00,50.00,NULL,'PZA',150.000,1,'2026-03-06 15:52:37','2026-03-06 15:52:37',1,0,NULL,0),(5,'ALA-REC','75010000005','Alambre Recocido Calibre 16','Alambre para amarre de varilla','DeAcero','Pasillo 9, Suelo',1,22.00,25.00,NULL,'KG',300.000,1,'2026-03-06 15:52:37','2026-03-06 15:52:37',1,0,NULL,0),(6,'PVC-1/2','75010000006','Tubo PVC Hidr├íulico 1/2\"','Tramo de 3 metros','Amanco','Patio Trasero, Rack B',1,38.00,30.00,NULL,'PZA',120.000,1,'2026-03-06 15:52:37','2026-03-06 15:52:37',1,0,NULL,0),(7,'LIJ-A-120','75010000007','Lija de Agua Grano 120','Hoja completa','Fandeli','Pasillo 4, Gaveta 2',1,6.00,60.00,NULL,'PZA',400.000,1,'2026-03-06 15:52:37','2026-03-06 15:52:37',1,0,NULL,0),(8,'CUE-POL-1/4','75010000008','Cuerda de Polipropileno 1/4\"','Amarilla trenzada','Fiero','Pasillo 3, Colgantes',1,2.50,45.00,NULL,'METRO',800.000,1,'2026-03-06 15:52:37','2026-03-06 15:52:37',1,0,NULL,0),(9,'DES-CRU-1/4','75010000009','Desarmador Cruz 1/4 x 4\"','Mango de acetato','Pretul','Pasillo 1, Tablero A',1,28.00,40.00,NULL,'PZA',45.000,1,'2026-03-06 15:52:37','2026-03-06 15:52:37',1,0,NULL,0),(10,'MAL-HEX-1','75010000010','Malla Hexagonal Pajarera 1m','Abertura 25mm','DeAcero','Bodega Superior',1,20.00,35.00,NULL,'METRO',150.000,1,'2026-03-06 15:52:37','2026-03-06 15:52:37',1,0,NULL,0),(11,'SIL-TRA-280','75099903','Silic├│n Transparente Multiusos 280ml',NULL,'Sista','Mostrador, Exhibidor 2',2,65.00,30.00,NULL,'PZA',24.000,0,'2026-03-06 16:05:28','2026-03-06 16:43:33',1,0,NULL,0);

-- 8. Estructura y Datos de 'productpresentations' (OTRO CORREGIDO)
DROP TABLE IF EXISTS `productpresentations`;
CREATE TABLE `productpresentations` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `ProductId` int NOT NULL,
  `Name` varchar(50) COLLATE utf8mb4_spanish2_ci NOT NULL,
  `Code` varchar(50) COLLATE utf8mb4_spanish2_ci DEFAULT NULL,
  `Barcode` varchar(100) COLLATE utf8mb4_spanish2_ci DEFAULT NULL,
  `Price` decimal(10,2) NOT NULL,
  `StockFactor` decimal(10,4) NOT NULL,
  `IsActive` tinyint(1) NOT NULL DEFAULT '1', -- Cambiado de bit a tinyint
  PRIMARY KEY (`Id`),
  KEY `FK_Presentations_Product` (`ProductId`),
  CONSTRAINT `FK_Presentations_Product` FOREIGN KEY (`ProductId`) REFERENCES `products` (`Id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish2_ci;

-- Insert usando 1 (True) y 0 (False) en vez de _binary
INSERT INTO `productpresentations` VALUES (2,1,'Bulto 50kg','CEM-BUL','75010000001-B',190.00,50.0000,1),(3,1,'Kilo Suelto','CEM-KG',NULL,6.00,1.0000,1),(4,2,'Metro','CAB-MET',NULL,15.00,1.0000,1),(5,2,'Rollo 100m','CAB-ROL','75010000002-R',1350.00,100.0000,1),(6,3,'Litro','THI-L',NULL,55.00,1.0000,1),(7,3,'Medio Litro','THI-ML',NULL,30.00,0.5000,1),(8,4,'Pieza','CIN-PZA','75010000004-P',22.00,1.0000,1),(9,5,'Kilo','ALA-KG',NULL,32.00,1.0000,1),(10,5,'Medio Kilo','ALA-MKG',NULL,18.00,0.5000,1),(11,6,'Tramo (3m)','PVC-TRA','75010000006-T',55.00,1.0000,1),(12,7,'Hoja','LIJ-HOJ','75010000007-H',12.00,1.0000,1),(13,8,'Metro','CUE-MET',NULL,5.00,1.0000,1),(14,9,'Pieza','DES-PZA','75010000009-P',45.00,1.0000,1),(15,10,'Metro','MAL-MET',NULL,35.00,1.0000,1),(16,10,'Rollo 45m','MAL-ROL','75010000010-R',1200.00,45.0000,1),(19,11,'Cartucho',NULL,'75099903',95.00,1.0000,0);

-- 9. Estructura y Datos de 'sales'
DROP TABLE IF EXISTS `sales`;
CREATE TABLE `sales` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `Folio` varchar(50) COLLATE utf8mb4_spanish_ci NOT NULL,
  `TotalAmount` decimal(10,2) NOT NULL,
  `PaymentMethod` varchar(50) COLLATE utf8mb4_spanish_ci NOT NULL,
  `PaymentReference` varchar(100) COLLATE utf8mb4_spanish_ci DEFAULT NULL,
  `UserId` int NOT NULL,
  `IsActive` tinyint(1) NOT NULL DEFAULT '1',
  `CreatedAt` datetime DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`Id`),
  UNIQUE KEY `Folio` (`Folio`),
  KEY `FK_Sales_Users` (`UserId`),
  CONSTRAINT `FK_Sales_Users` FOREIGN KEY (`UserId`) REFERENCES `users` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `sales` VALUES (1,'TKT-260411154905',380.00,'Transferencia',NULL,1,1,'2026-04-11 15:49:05'),(2,'TKT-260411155632',12.00,'Transferencia',NULL,1,1,'2026-04-11 15:56:32'),(3,'TKT-260411155752',380.00,'Transferencia',NULL,1,1,'2026-04-11 15:57:52'),(4,'TKT-260616125818745-',1405.00,'Efectivo',NULL,1,0,'2026-06-16 12:58:18'),(5,'TKT-260723151018408-9787',22.50,'Efectivo','50',1,1,'2026-07-23 15:10:18');

-- 10. Estructura y Datos de 'saledetails'
DROP TABLE IF EXISTS `saledetails`;
CREATE TABLE `saledetails` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `SaleId` int NOT NULL,
  `ProductId` int NOT NULL,
  `PresentationId` int NOT NULL,
  `ProductName` varchar(150) COLLATE utf8mb4_spanish_ci NOT NULL,
  `Brand` varchar(100) COLLATE utf8mb4_spanish_ci DEFAULT NULL,
  `Quantity` decimal(10,3) NOT NULL,
  `StockFactorApplied` decimal(10,3) NOT NULL,
  `UnitPrice` decimal(10,2) NOT NULL,
  `Subtotal` decimal(10,2) NOT NULL,
  `IsActive` tinyint(1) NOT NULL DEFAULT '1',
  `UpdatedAt` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`Id`),
  KEY `FK_SaleDetails_Sales` (`SaleId`),
  KEY `FK_SaleDetails_Products` (`ProductId`),
  KEY `FK_SaleDetails_Presentations` (`PresentationId`),
  CONSTRAINT `FK_SaleDetails_Presentations` FOREIGN KEY (`PresentationId`) REFERENCES `productpresentations` (`Id`),
  CONSTRAINT `FK_SaleDetails_Products` FOREIGN KEY (`ProductId`) REFERENCES `products` (`Id`),
  CONSTRAINT `FK_SaleDetails_Sales` FOREIGN KEY (`SaleId`) REFERENCES `sales` (`Id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `saledetails` VALUES (1,1,1,2,'Cemento Gris Tipo CPO - Bulto 50kg','Cruz Azul',2.000,50.000,190.00,380.00,1,'2026-05-29 13:53:24'),(2,2,1,3,'Cemento Gris Tipo CPO - Kilo Suelto','Cruz Azul',2.000,1.000,6.00,12.00,1,'2026-05-29 13:53:24'),(3,3,1,2,'Cemento Gris Tipo CPO - Bulto 50kg','Cruz Azul',2.000,50.000,190.00,380.00,1,'2026-05-29 13:53:24'),(4,4,3,6,'Thinner Est├índar - Litro','Sayer Lack',1.000,1.000,55.00,55.00,1,'2026-06-16 12:58:18'),(5,4,2,5,'Cable THW Calibre 12 Rojo - Rollo 100m','IUSA',1.000,100.000,1350.00,1350.00,1,'2026-06-16 12:58:18'),(6,5,2,4,'Cable THW Calibre 12 Rojo - Metro','IUSA',1.500,1.000,15.00,22.50,1,'2026-07-23 15:10:18');

-- 11. Estructura y Datos de 'returns'
DROP TABLE IF EXISTS `returns`;
CREATE TABLE `returns` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `ReturnFolio` varchar(50) COLLATE utf8mb4_spanish_ci NOT NULL,
  `SaleId` int NOT NULL,
  `UserId` int NOT NULL,
  `TotalRefunded` decimal(10,2) NOT NULL,
  `Reason` varchar(255) COLLATE utf8mb4_spanish_ci DEFAULT NULL,
  `CreatedAt` datetime DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`Id`),
  UNIQUE KEY `ReturnFolio` (`ReturnFolio`),
  KEY `FK_Returns_Sales` (`SaleId`),
  KEY `FK_Returns_Users` (`UserId`),
  CONSTRAINT `FK_Returns_Sales` FOREIGN KEY (`SaleId`) REFERENCES `sales` (`Id`),
  CONSTRAINT `FK_Returns_Users` FOREIGN KEY (`UserId`) REFERENCES `users` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `returns` VALUES (1,'DEV-260616131702305-1701',4,1,1350.00,'Devolucion de un produto','2026-06-16 13:17:02'),(2,'DEV-260616131952670-6304',3,1,190.00,'Devolucion de un bulto por merma','2026-06-16 13:19:52'),(3,'DEV-260616132727064-4809',4,1,55.00,'Devolucion de un bulto por merma','2026-06-16 13:27:27');

-- 12. Estructura y Datos de 'returndetails'
DROP TABLE IF EXISTS `returndetails`;
CREATE TABLE `returndetails` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `ReturnId` int NOT NULL,
  `SaleDetailId` int NOT NULL,
  `QuantityReturned` decimal(10,3) NOT NULL,
  `RefundAmount` decimal(10,2) NOT NULL,
  PRIMARY KEY (`Id`),
  KEY `FK_ReturnDetails_Returns` (`ReturnId`),
  KEY `FK_ReturnDetails_SaleDetails` (`SaleDetailId`),
  CONSTRAINT `FK_ReturnDetails_Returns` FOREIGN KEY (`ReturnId`) REFERENCES `returns` (`Id`) ON DELETE CASCADE,
  CONSTRAINT `FK_ReturnDetails_SaleDetails` FOREIGN KEY (`SaleDetailId`) REFERENCES `saledetails` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `returndetails` VALUES (1,1,5,1.000,1350.00),(2,2,3,1.000,190.00),(3,3,4,1.000,55.00);

-- 13. Estructura y Datos de 'inventorymovements'
DROP TABLE IF EXISTS `inventorymovements`;
CREATE TABLE `inventorymovements` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `ProductId` int NOT NULL,
  `UserId` int NOT NULL,
  `MovementType` int NOT NULL,
  `Quantity` decimal(10,3) NOT NULL,
  `PreviousStock` decimal(10,3) NOT NULL,
  `NewStock` decimal(10,3) NOT NULL,
  `Notes` text COLLATE utf8mb4_spanish_ci,
  `CreatedAt` datetime DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`Id`),
  KEY `FK_Movements_Products` (`ProductId`),
  KEY `FK_Movements_Users` (`UserId`),
  CONSTRAINT `FK_Movements_Products` FOREIGN KEY (`ProductId`) REFERENCES `products` (`Id`),
  CONSTRAINT `FK_Movements_Users` FOREIGN KEY (`UserId`) REFERENCES `users` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `inventorymovements` VALUES (1,2,1,1,10.000,509.000,519.000,'string','2026-07-31 14:05:21'),(2,2,1,1,100.000,519.000,619.000,'Llego mercancia','2026-08-03 11:01:45'),(3,2,1,4,10.000,619.000,609.000,'Ajuste negativo','2026-08-03 11:02:13');

-- 14. Estructura y Datos de 'pendingorders'
DROP TABLE IF EXISTS `pendingorders`;
CREATE TABLE `pendingorders` (
  `Id` int NOT NULL AUTO_INCREMENT,
  `ProductId` int NOT NULL,
  `SupplierId` int DEFAULT NULL,
  `UserId` int NOT NULL,
  `QuantityText` varchar(100) COLLATE utf8mb4_spanish_ci NOT NULL,
  `Notes` text COLLATE utf8mb4_spanish_ci,
  `Status` varchar(30) COLLATE utf8mb4_spanish_ci NOT NULL DEFAULT 'Pendiente',
  `CreatedAt` datetime DEFAULT CURRENT_TIMESTAMP,
  `UpdatedAt` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`Id`),
  KEY `FK_Pending_Products` (`ProductId`),
  KEY `FK_Pending_Suppliers` (`SupplierId`),
  KEY `FK_Pending_Users` (`UserId`),
  CONSTRAINT `FK_Pending_Products` FOREIGN KEY (`ProductId`) REFERENCES `products` (`Id`),
  CONSTRAINT `FK_Pending_Suppliers` FOREIGN KEY (`SupplierId`) REFERENCES `suppliers` (`Id`),
  CONSTRAINT `FK_Pending_Users` FOREIGN KEY (`UserId`) REFERENCES `users` (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_spanish_ci;

INSERT INTO `pendingorders` VALUES (4,1,1,1,'2 cosas',NULL,'Pendiente','2026-03-09 13:31:59','2026-03-23 13:26:32');

-- 15. Volvemos a encender la revisión de llaves foráneas
SET FOREIGN_KEY_CHECKS = 1;
```


![[BDSnap2026_08_03.png]]