# PostgreSQL con Docker

# Creacion del Contenedor
```bash
docker run -d --name postgres_container -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin -e POSTGRES_DB=tienda -p 5433:5432 -v pgdata:/var/lib/postgresql/data --restart=unless-stopped postgres:15
```

## Conectar al contenedor de docker
```bash
docker exec -it postgres_container bash
```

## Conectar con PostgreSQL bajo Consola
```bash
psql --host=localhost --username=admin -d tienda --password

psql -h localhost -U admin -d tienda -W
```

# Tablas

## 1. clientes

```SQL
CREATE TABLE clientes(
    id VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(40) NOT NULL,
    apellidos VARCHAR(100) NOT NULL,
    celular DECIMAL(10,0) NOT NULL,
    direccion VARCHAR(80) NOT NULL,
    correo_electronico VARCHAR(70) NOT NULL
);
```

## 2. Categorias

```SQL
CREATE TABLE categorias(
    id INT PRIMARY KEY,
    descripcion VARCHAR(45) NOT NULL,
    estado SMALLINT
);
```

## 3. Compras

```SQL
CREATE TABLE compras(
    id_compra INT PRIMARY KEY,
    id_cliente VARCHAR(20) NOT NULL,
    fecha DATE NOT NULL,
    medio_pago CHAR(1) NOT NULL,
    comentario VARCHAR(300) NOT NULL,
    estado CHAR(1) NOT NULL,
        FOREIGN KEY (id_cliente) REFERENCES clientes(id) ON DELETE CASCADE
);
```

## 4. Productos

```SQL
CREATE TABLE productos(
    id_producto INT PRIMARY KEY,
    id_categoria INT NOT NULL,
    nombre VARCHAR(45) NOT NULL,
    codigo_barras VARCHAR(150) NOT NULL,
    precio_venta DECIMAL(16,2) NOT NULL,
    cantidad_stock INT NOT NULL,
    estado SMALLINT NOT NULL,
        FOREIGN KEY (id_categoria) REFERENCES categorias(id) ON DELETE CASCADE
);
```


## 5. Compras Productos

```SQL
CREATE TABLE compras_productos(
    id_compra INT NOT NULL,
    id_producto INT NOT NULL,
    cantidad INT NOT NULL,
    total DECIMAL(16,2) NOT NULL,
    estado SMALLINT NOT NULL,
    PRIMARY KEY (id_compra, id_producto),
    FOREIGN KEY (id_compra) REFERENCES compras(id_compra) ON DELETE CASCADE,
    FOREIGN KEY (id_producto) REFERENCES productos(id_producto) ON DELETE CASCADE
);

```

# Insert datos 

## 1. Clientes

```SQL
INSERT INTO clientes (id, nombre, apellidos, celular, direccion, correo_electronico) VALUES
('CC1001', 'Juan', 'Pérez Gómez', 3001234567, 'Cra 10 #12-34', 'juan.perez@mail.com'),
('CC1002', 'María', 'López Ríos', 3012345678, 'Cl 20 #5-67', 'maria.lopez@mail.com'),
('CC1003', 'Carlos', 'Martínez Ruiz', 3023456789, 'Av 15 #45-21', 'carlos.martinez@mail.com'),
('CC1004', 'Ana', 'García Torres', 3034567890, 'Cra 7 #89-12', 'ana.garcia@mail.com'),
('CC1005', 'Pedro', 'Ramírez Díaz', 3045678901, 'Cl 50 #12-56', 'pedro.ramirez@mail.com'),
('CC1006', 'Laura', 'Suárez Peña', 3056789012, 'Cra 15 #33-44', 'laura.suarez@mail.com'),
('CC1007', 'Andrés', 'Castro Mejía', 3067890123, 'Cl 8 #21-11', 'andres.castro@mail.com'),
('CC1008', 'Valentina', 'Morales Cárdenas', 3078901234, 'Av 30 #10-20', 'valentina.morales@mail.com'),
('CC1009', 'Santiago', 'Ortega Parra', 3089012345, 'Cra 22 #5-99', 'santiago.ortega@mail.com'),
('CC1010', 'Daniela', 'Rincón Vargas', 3090123456, 'Cl 12 #8-66', 'daniela.rincon@mail.com');
```

## 2. Categorias

```SQL
INSERT INTO categorias (id, descripcion, estado) VALUES
(1, 'Bebidas', 1),
(2, 'Snacks', 1),
(3, 'Aseo', 1),
(4, 'Lácteos', 1),
(5, 'Panadería', 1),
(6, 'Cereales', 1),
(7, 'Enlatados', 1),
(8, 'Confitería', 1),
(9, 'Granos', 1),
(10, 'Frutas', 1);
```

## 3. Compras

```SQL
INSERT INTO compras (id_compra, id_cliente, fecha, medio_pago, comentario, estado) VALUES
(1, 'CC1001', '2025-08-01', 'E', 'Compra en efectivo', 'A'),
(2, 'CC1002', '2025-08-02', 'T', 'Compra con tarjeta', 'A'),
(3, 'CC1003', '2025-08-03', 'E', 'Compra de rutina', 'A'),
(4, 'CC1004', '2025-08-04', 'T', 'Compra mensual', 'A'),
(5, 'CC1005', '2025-08-05', 'E', 'Compra grande', 'A'),
(6, 'CC1006', '2025-08-06', 'T', 'Compra semanal', 'A'),
(7, 'CC1007', '2025-08-07', 'E', 'Compra ocasional', 'A'),
(8, 'CC1008', '2025-08-08', 'T', 'Compra variada', 'A'),
(9, 'CC1009', '2025-08-09', 'E', 'Compra de prueba', 'A'),
(10, 'CC1010', '2025-08-10', 'T', 'Compra de emergencia', 'A');
```

## 4. Productos

```SQL
INSERT INTO productos (id_producto, id_categoria, nombre, codigo_barras, precio_venta, cantidad_stock, estado) VALUES
(101, 1, 'Coca Cola 1.5L', '7701234567890', 5500, 50, 1),
(102, 2, 'Papas Margarita 200g', '7700987654321', 3500, 100, 1),
(103, 3, 'Jabón Rey 300g', '7701122334455', 2500, 80, 1),
(104, 4, 'Leche Alpina 1L', '7705566778899', 4200, 60, 1),
(105, 5, 'Pan Bimbo Grande', '7706677889900', 4800, 40, 1),
(106, 6, 'Corn Flakes 500g', '7707788990011', 5200, 35, 1),
(107, 7, 'Atún Van Camps 160g', '7708899001122', 6300, 70, 1),
(108, 8, 'Chocoramo Individual', '7709900112233', 2500, 120, 1),
(109, 9, 'Arroz Diana 1kg', '7701111222333', 3800, 200, 1),
(110, 10, 'Manzana Roja Unidad', '7702222333444', 1200, 150, 1);
```

## 5. Compras Productos

```SQL
INSERT INTO compras_productos (id_compra, id_producto, cantidad, total, estado) VALUES
(1, 101, 2, 11000, 1),
(1, 102, 3, 10500, 1),
(2, 103, 4, 10000, 1),
(3, 104, 1, 4200, 1),
(4, 105, 5, 24000, 1),
(5, 106, 2, 10400, 1),
(6, 107, 3, 18900, 1),
(7, 108, 6, 15000, 1),
(8, 109, 1, 3800, 1),
(9, 110, 4, 4800, 1);
```