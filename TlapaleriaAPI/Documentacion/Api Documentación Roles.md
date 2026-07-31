![[RolesSwaggerSanp.png]]
![[RolesSwiggerSnap.png]]

### 1. `GET /api/Roles` Ver Roles

- **¿Para qué sirve?** Obtiene la lista completa de todos los roles registrados en la base de datos (Admin, Vendedor, Gerente, etc.), incluyendo la lista de permisos que tiene asignado cada uno.
    
- **¿Cómo lo usas en React?** Es el endpoint principal que llamas al abrir tu pantalla de **"Administración de Roles"**. Lo usas para llenar la tabla principal donde muestras todos tus puestos de trabajo.
    

### 2. `POST /api/Roles` Crear Roles

- **¿Para qué sirve?** Crea un **nuevo rol** en el sistema y le asigna mágicamente los permisos que hayas elegido en una sola transacción.
    
- **¿Cómo lo usas en React?** Cuando el usuario hace clic en el botón _"Nuevo Rol"_ y llena el formulario. Le envías un JSON con el nombre del puesto (ej. `"Cajero Supervisor"`) y un arreglo con los IDs de los checkboxes de permisos que el administrador seleccionó (`"permisosIds": [1, 2, 5]`).
    

### 3. `GET /api/Roles/permissions` Ver lista de permisos

- **¿Para qué sirve?** Devuelve el **catálogo completo de todos los permisos que existen en el sistema** (ej. `add.users`, `edit.products`, `view.sales`, etc.).
    
- **¿Cómo lo usas en React?** Es fundamental para tus formularios. Cuando abres la ventana modal de _"Crear Rol"_ o _"Editar Rol"_, llamas a este endpoint para **dibujar la lista de checkboxes** en la pantalla y que el administrador pueda palomear qué herramientas darle a ese puesto.
    

### 4. `GET /api/Roles/search/{termino}` Buscar Roles

- **¿Para qué sirve?** Es el **buscador dinámico de roles**. Busca y filtra los roles cuyo nombre coincida con el texto que le mandes.
    
- **¿Cómo lo usas en React?** Lo conectas al _input_ de búsqueda de tu barra superior en la tabla de roles. Si tienes decenas de puestos y el administrador escribe `"caj"`, tu React llama a `/api/Roles/search/caj` y la tabla se actualiza al instante mostrando solo los roles de caja.
    

### 5. `GET /api/Roles/{id}` Ver Permisos de un Rol

- **¿Para qué sirve?** Trae el detalle de **un solo rol en específico**, indicándote exactamente qué permisos tiene encendidos.
    
- **¿Cómo lo usas en React?** Cuando en tu tabla principal hacen clic en el botón de **"Editar"** del rol _Vendedor_ (que supongamos tiene el ID 3). Llamas a `/api/Roles/3` para precargar el nombre en el _input_ de tu formulario y **dejar palomeados (checked)** automáticamente los checkboxes de los permisos que ese rol ya posee.
    

### 6. `PUT /api/Roles/{id}` Actualizar un Rol

- **¿Para qué sirve?** **Actualiza un rol existente**. Te permite cambiarle el nombre (excepto al rol `Admin`, el cual protegimos)
    
- **¿Cómo lo usas en React?** Es la acción del botón _"Guardar Cambios"_ dentro de tu modal de edición de rol.
    

### 7. `DELETE /api/Roles/{id}` Eliminar Roles 

- **¿Para qué sirve?** **Elimina un rol del sistema**. Por seguridad, este endpoint tiene nuestro blindaje especial: rechaza eliminar el rol `Admin` y **lanza un error de protección si intentas borrar un rol que todavía tiene usuarios asignados**.
    
- **¿Cómo lo usas en React?** Lo conectas al botón con icono de basurero en tu tabla. Si el backend te responde con éxito, quitas la fila de la tabla; si te responde con nuestro error blindado, le muestras una alerta clara al usuario: _"No se puede eliminar este rol porque hay empleados que lo tienen asignado. Reasígnalos primero"_.
    

### 8. `GET /api/Roles/{id}/users`

- **¿Para qué sirve?** Devuelve la lista **paginada de todos los usuarios (empleados) que tienen asignado ese rol** en específico. Además, está blindado para que solo los Administradores o personas con el permiso especial `user.privilege_view` puedan consultarlo.
    
- **¿Cómo lo usas en React?** Es perfecto para una vista de auditoría. Puedes agregar un botón en tu tabla que diga _"Ver Personal"_; al presionarlo, abres una ventana donde llamas a `/api/Roles/2/users?pageNumber=1&pageSize=10` para ver de forma paginada y limpia qué cajeros, gerentes o vendedores pertenecen a ese puesto.
    

## 9. `POST /api/Roles/{rolId}/permissions/{permisoId}` (Asignar Permiso)

¿Para qué sirve?Es el endpoint encargado de **vincular una herramienta o capacidad específica a un puesto de trabajo**. Le dice a tu base de datos: _"A partir de este momento, el rol X tiene autorización para ejecutar el permiso Y"_.


## 10. `DELETE /api/Roles/{rolId}/permissions/{permisoId}` (Remover Permiso)

 ¿Para qué sirve? Es la contraparte quirúrgica: **le revoca un acceso específico a un rol sin afectar todo lo demás que ese puesto sabe o puede hacer**.

### 💡 El Resumen Visual

Tienes un motor modular:

- **1, 4 y 5** son para **Leer y Buscar** (llenar tus tablas y formularios).
    
- **3** te da la **materia prima** (los checkboxes de permisos).
    
- **2, 6 y 7** son las acciones de **Guardar, Modificar y Borrar** (el CRUD puro).
    
- **8** es tu herramienta de **Auditoría de Personal**.