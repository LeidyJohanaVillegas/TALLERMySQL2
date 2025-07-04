# TALLERMySQL2
Taller de practica Skill MySQL2
# TALLERMySQL

# Descripción del taller.

Este taller está diseñado para profundizar en el manejo y optimización de bases de datos MySQL. A través de ejercicios prácticos, se explorarán temas avanzados para reforzar el conocimiento en normalización, joins, consultas complejas, subconsultas, procedimientos almacenados, funciones definidas por el usuario y triggers. Requisitos previos: Conocimiento básico de SQL y MySQL MySQL instalado y configurado en tu máquina Objetivos: Al finalizar este taller, el participante será capaz de: 1. Diseñar bases de datos optimizadas mediante técnicas de normalización. 2. Realizar consultas avanzadas en múltiples tablas. 3. Utilizar subconsultas para consultas complejas. 4. Crear y ejecutar procedimientos almacenados y funciones definidas por el usuario. 5. Implementar triggers para automatizar operaciones en la base de datos.

```
## Base de datos

CREATE DATABASE mysql2;
USE mysql2;

CREATE TABLE UbicacionClientes (
  id INT PRIMARY KEY AUTO_INCREMENT,
  cliente_id INT,
  direccion VARCHAR(255),
  ciudad VARCHAR(100),
  estado VARCHAR(50),
  codigo_postal VARCHAR(10),
  pais VARCHAR(50)
); 

CREATE TABLE Clientes (
  id INT PRIMARY KEY AUTO_INCREMENT, 
  nombre VARCHAR(100),
  apellido VARCHAR(50),
  email VARCHAR(100) UNIQUE,
  direccion_id INT,
  FOREIGN KEY (direccion_id) REFERENCES UbicacionClientes(id)
);

CREATE TABLE Pedidos (
  id INT PRIMARY KEY AUTO_INCREMENT,
  cliente_id INT, 
  fecha DATE,
  FOREIGN KEY (cliente_id) REFERENCES Clientes(id)
);

CREATE TABLE HistorialPedidos (
  id INT PRIMARY KEY AUTO_INCREMENT,
  pedido_id INT,
  cliente_id INT,
  fecha_anterior DATE,
  total_anterior DECIMAL(10,2),
  estado_anterior VARCHAR(50),
  fecha_modificacion DATE,
  usuario_modificacion VARCHAR(100),
  FOREIGN KEY (pedido_id) REFERENCES Pedidos(id),
  FOREIGN KEY (cliente_id) REFERENCES Clientes(id)
);

CREATE TABLE Puestos (
  id INT PRIMARY KEY AUTO_INCREMENT, 
  nombre_puesto VARCHAR(50) UNIQUE,
  salario DECIMAL(10,2)
);

CREATE TABLE DatosEmpleados (
  id INT PRIMARY KEY,
  nombre VARCHAR(50),
  apellido VARCHAR(50),
  fecha_contratacion DATE,
  puesto_id INT,
  FOREIGN KEY (puesto_id) REFERENCES Puestos(id)
);

CREATE TABLE Proveedores (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(100),
  dirección VARCHAR(255)
);

CREATE TABLE ContactoProveedor (
  id INT PRIMARY KEY AUTO_INCREMENT,
  proveedor_id INT,
  nombreContacto VARCHAR(100),
  telefono VARCHAR(20),
  FOREIGN KEY (proveedor_id) REFERENCES Proveedores(id)
);

CREATE TABLE TipoTelefonos (
  id INT PRIMARY KEY AUTO_INCREMENT,
  tipo_tel VARCHAR(20)
);

CREATE TABLE Telefonos ( 
  id INT PRIMARY KEY AUTO_INCREMENT,
  cliente_id INT,
  num_tel VARCHAR(10),
  tipo_id INT,
  FOREIGN KEY (cliente_id) REFERENCES Clientes(id),
  FOREIGN KEY (tipo_id) REFERENCES TipoTelefonos(id)
);

CREATE TABLE TiposProductos (
  id INT PRIMARY KEY AUTO_INCREMENT,
  tipo_nombre VARCHAR(100) NOT NULL, 
  descripcion TEXT,
  padre_id INT, 
  FOREIGN KEY (padre_id) REFERENCES TiposProductos(id)
); 

CREATE TABLE Productos (
  id INT PRIMARY KEY AUTO_INCREMENT,
  nombre VARCHAR(100),
  precio DECIMAL(10,2),
  proveedor_id INT, 
  tipo_id INT,
  FOREIGN KEY (proveedor_id) REFERENCES Proveedores(id),
  FOREIGN KEY (tipo_id) REFERENCES TiposProductos(id)
);

CREATE TABLE DetallesPedido (
  id INT PRIMARY KEY AUTO_INCREMENT,
  pedido_id INT, 
  producto_id INT,
  cantidad INT, 
  precio DECIMAL(10,2),
  FOREIGN KEY (pedido_id) REFERENCES Pedidos(id),
  FOREIGN KEY (producto_id) REFERENCES Productos(id)
);

CREATE TABLE Empleados (
  id INT PRIMARY KEY AUTO_INCREMENT, 
  nombre VARCHAR(100), 
  puesto VARCHAR(50),
  salario DECIMAL(10,2),
  fecha_contratacion DATE
);

CREATE TABLE EmpleadoProveedor (
  empleado_id INT,
  proveedor_id INT,
  PRIMARY KEY (empleado_id, proveedor_id), 
  FOREIGN KEY (empleado_id) REFERENCES Empleados(id),
  FOREIGN KEY (proveedor_id) REFERENCES Proveedores(id)
);

CREATE TABLE Ubicaciones( 
  id INT PRIMARY KEY AUTO_INCREMENT,
  entidad_tipo VARCHAR(50),
  entidad_id INT,
  dirección VARCHAR(255), 
  ciudad VARCHAR(100), 
  estado VARCHAR(50), 
  código_postal VARCHAR(10),
  país VARCHAR(50)
);
```
## Datos ingresados
```
INSERT INTO UbicacionClientes (direccion, ciudad, estado, codigo_postal, pais) VALUES
-- Colombia
('Calle 123', 'Bogotá', 'Cundinamarca', '110111', 'Colombia'),
('Av. Central 456', 'Medellín', 'Antioquia', '050021', 'Colombia'),
('Carrera 45 #12-34', 'Cali', 'Valle del Cauca', '760001', 'Colombia'),
('Calle 89A #45-12', 'Barranquilla', 'Atlántico', '080001', 'Colombia'),
('Transversal 23 #76-98', 'Cartagena', 'Bolívar', '130001', 'Colombia'),
('Avenida 15 #36-78', 'Bucaramanga', 'Santander', '680001', 'Colombia'),
('Diagonal 67 #45-32', 'Pereira', 'Risaralda', '660001', 'Colombia'),
('Carrera 7 #21-45', 'Manizales', 'Caldas', '170001', 'Colombia'),
('Calle 10 #5-99', 'Neiva', 'Huila', '410001', 'Colombia'),
('Av. El Poblado #34-56', 'Medellín', 'Antioquia', '050022', 'Colombia'),
('Calle 123B #67-89', 'Bogotá', 'Cundinamarca', '110112', 'Colombia'),
('Carrera 9 #45-12', 'Villavicencio', 'Meta', '500001', 'Colombia'),
('Carrera 45 #12-34', 'Cali', 'Valle del Cauca', '760001', 'Colombia'),
('Calle 89A #45-12', 'Barranquilla', 'Atlántico', '080001', 'Colombia'),
('Transversal 23 #76-98', 'Cartagena', 'Bolívar', '130001', 'Colombia'),
('Avenida 15 #36-78', 'Bucaramanga', 'Santander', '680001', 'Colombia'),
-- México
('Av. Reforma 123', 'Ciudad de México', 'CDMX', '01000', 'México'),
('Calle Juárez 45', 'Guadalajara', 'Jalisco', '44100', 'México'),
('Blvd. Díaz Ordaz 567', 'Monterrey', 'Nuevo León', '64000', 'México'),
('Av. Insurgentes Sur 321', 'Toluca', 'Estado de México', '50000', 'México'),
-- Argentina
('Av. 9 de Julio 101', 'Buenos Aires', 'Buenos Aires', 'C1002', 'Argentina'),
('Calle San Martín 456', 'Córdoba', 'Córdoba', 'X5000', 'Argentina'),
('Bv. Oroño 789', 'Rosario', 'Santa Fe', 'S2000', 'Argentina'),
('Calle Rivadavia 234', 'Mendoza', 'Mendoza', 'M5500', 'Argentina'),
-- Perú
('Av. Arequipa 1234', 'Lima', 'Lima', '15001', 'Perú'),
('Jr. Ayacucho 345', 'Cusco', 'Cusco', '08002', 'Perú'),
('Calle Los Álamos 567', 'Trujillo', 'La Libertad', '13001', 'Perú'),
('Av. Grau 890', 'Arequipa', 'Arequipa', '04001', 'Perú'),
-- Chile
('Av. Providencia 100', 'Santiago', 'Región Metropolitana', '8320000', 'Chile'),
('Calle Valparaíso 200', 'Viña del Mar', 'Valparaíso', '2520000', 'Chile'),
('Av. Alemania 789', 'Temuco', 'Araucanía', '4780000', 'Chile'),
('Pasaje Los Andes 321', 'Antofagasta', 'Antofagasta', '1240000', 'Chile');

INSERT INTO Clientes (nombre, apellido, email, direccion_id) VALUES
('Juan', 'Pérez', 'juan@example.com', 1),
('María', 'Gómez', 'maria@example.com', 2),
('Carlos', 'Ramírez', 'carlos.ramirez@example.com', 3),
('Ana', 'Martínez', 'ana.martinez@example.com', 4),
('Luis', 'Torres', 'luis.torres@example.com', 5),
('Camila', 'López', 'camila.lopez@example.com', 6),
('Andrés', 'Moreno', 'andres.moreno@example.com', 7),
('Laura', 'Castro', 'laura.castro@example.com', 8),
('Felipe', 'García', 'felipe.garcia@example.com', 9),
('Valentina', 'Suárez', 'valentina.suarez@example.com', 10),
('Sebastián', 'Rojas', 'sebastian.rojas@example.com', 11),
('Isabella', 'Navarro', 'isabella.navarro@example.com', 12),
('Mateo', 'Vargas', 'mateo.vargas@example.com', 13),
('Daniela', 'Mendoza', 'daniela.mendoza@example.com', 14),
('Tomás', 'Delgado', 'tomas.delgado@example.com', 15),
('Sofía', 'Carrillo', 'sofia.carrillo@example.com', 16),
('Emilio', 'Ortega', 'emilio.ortega@example.com', 17),
('Luciana', 'Sánchez', 'luciana.sanchez@example.com', 18),
('Martín', 'Ruiz', 'martin.ruiz@example.com', 19),
('Gabriela', 'Pineda', 'gabriela.pineda@example.com', 20),
('Julián', 'Acosta', 'julian.acosta@example.com', 21),
('Renata', 'Peña', 'renata.pena@example.com', 22),
('Simón', 'Cortés', 'simon.cortes@example.com', 23),
('Victoria', 'Cárdenas', 'victoria.cardenas@example.com', 24),
('Diego', 'Luna', 'diego.luna@example.com', 25),
('Antonia', 'Salazar', 'antonia.salazar@example.com', 26),
('Esteban', 'Rosales', 'esteban.rosales@example.com', 27),
('Florencia', 'Arias', 'florencia.arias@example.com', 28),
('Maximiliano', 'Figueroa', 'max.figueroa@example.com', 29),
('Regina', 'Cabrera', 'regina.cabrera@example.com', 30),
('Benjamín', 'Núñez', 'benjamin.nunez@example.com', 31),
('Paulina', 'Mejía', 'paulina.mejia@example.com', 32);

INSERT INTO Pedidos (cliente_id, fecha) VALUES
(1, '2025-06-01'),
(2, '2025-06-15'),
(3, '2025-06-03'),
(4, '2025-06-17'),
(5, '2025-06-05'),
(6, '2025-06-20'),
(7, '2025-06-07'),
(8, '2025-06-22'),
(9, '2025-06-09'),
(10, '2025-06-25'),
(11, '2025-06-11'),
(12, '2025-06-27'),
(13, '2025-06-13'),
(14, '2025-06-29'),
(15, '2025-06-30'),
(16, '2025-07-01'),
(17, '2025-07-02'),
(18, '2025-07-03'),
(19, '2025-07-04'),
(20, '2025-07-05'),
(21, '2025-07-06'),
(22, '2025-07-07'),
(23, '2025-07-08'),
(24, '2025-07-09'),
(25, '2025-07-10'),
(26, '2025-07-11'),
(27, '2025-07-12'),
(28, '2025-07-13'),
(29, '2025-07-14'),
(30, '2025-07-15'),
(31, '2025-07-16'),
(32, '2025-07-17');

INSERT INTO HistorialPedidos (pedido_id, cliente_id, fecha_anterior, total_anterior, estado_anterior, fecha_modificacion, usuario_modificacion) VALUES
(1, 1, '2025-05-30', 200000, 'Pendiente', '2025-06-01', 'admin'),
(2, 2, '2025-06-10', 350000, 'Entregado', '2025-06-15', 'admin'),
(3, 3, '2025-06-01', 150000, 'Pendiente', '2025-06-03', 'admin'),
(4, 4, '2025-06-15', 250000, 'Cancelado', '2025-06-17', 'soporte'),
(5, 5, '2025-06-03', 180000, 'Entregado', '2025-06-05', 'admin'),
(6, 6, '2025-06-18', 220000, 'Pendiente', '2025-06-20', 'admin'),
(7, 7, '2025-06-05', 195000, 'Pendiente', '2025-06-07', 'admin'),
(8, 8, '2025-06-20', 210000, 'Entregado', '2025-06-22', 'soporte'),
(9, 9, '2025-06-07', 230000, 'Cancelado', '2025-06-09', 'admin'),
(10, 10, '2025-06-23', 300000, 'Pendiente', '2025-06-25', 'admin'),
(11, 11, '2025-06-09', 170000, 'Entregado', '2025-06-11', 'admin'),
(12, 12, '2025-06-25', 265000, 'Pendiente', '2025-06-27', 'soporte'),
(13, 13, '2025-06-11', 320000, 'Cancelado', '2025-06-13', 'admin'),
(14, 14, '2025-06-27', 205000, 'Pendiente', '2025-06-29', 'admin'),
(15, 15, '2025-06-28', 150000, 'Entregado', '2025-06-30', 'soporte'),
(16, 16, '2025-06-30', 270000, 'Pendiente', '2025-07-01', 'admin'),
(17, 17, '2025-07-01', 310000, 'Entregado', '2025-07-02', 'admin'),
(18, 18, '2025-07-02', 245000, 'Pendiente', '2025-07-03', 'admin'),
(19, 19, '2025-07-03', 230000, 'Pendiente', '2025-07-04', 'admin'),
(20, 20, '2025-07-04', 280000, 'Entregado', '2025-07-05', 'soporte'),
(21, 21, '2025-07-05', 295000, 'Pendiente', '2025-07-06', 'admin'),
(22, 22, '2025-07-06', 260000, 'Cancelado', '2025-07-07', 'admin'),
(23, 23, '2025-07-07', 275000, 'Pendiente', '2025-07-08', 'admin'),
(24, 24, '2025-07-08', 225000, 'Entregado', '2025-07-09', 'soporte'),
(25, 25, '2025-07-09', 240000, 'Pendiente', '2025-07-10', 'admin'),
(26, 26, '2025-07-10', 285000, 'Entregado', '2025-07-11', 'admin'),
(27, 27, '2025-07-11', 300000, 'Pendiente', '2025-07-12', 'soporte'),
(28, 28, '2025-07-12', 215000, 'Pendiente', '2025-07-13', 'admin'),
(29, 29, '2025-07-13', 195000, 'Cancelado', '2025-07-14', 'admin'),
(30, 30, '2025-07-14', 310000, 'Pendiente', '2025-07-15', 'soporte'),
(31, 31, '2025-07-15', 265000, 'Entregado', '2025-07-16', 'admin'),
(32, 32, '2025-07-16', 250000, 'Pendiente', '2025-07-17', 'admin');

INSERT INTO Puestos (nombre_puesto, salario) VALUES
('Vendedor', 1800000),
('Repartidor', 1500000),
('Técnico de Reparación', 2000000),
('Asistente Administrativo', 1600000),
('Gerente de Ventas', 3200000),
('Supervisor de Logística', 2800000),
('Encargado de Almacén', 1700000),
('Soporte Técnico', 1900000),
('Jefe de Operaciones', 3500000),
('Recepcionista', 1400000),
('Auxiliar de Servicios Generales', 1300000),
('Contador', 3000000),
('Analista de Datos', 3100000),
('Desarrollador Web', 3300000),
('Diseñador Gráfico', 2900000),
('Líder de Proyecto', 3600000),
('Mensajero', 1350000),
('Auditor Interno', 3400000),
('Especialista en Inventario', 2200000),
('Community Manager', 2400000),
('Jefe de Mantenimiento', 2500000),
('Encargado de Compras', 2300000),
('Analista de Recursos Humanos', 2700000),
('Capacitador', 2600000),
('Conductor de Camión', 2100000),
('Supervisor de Técnicos', 3000000),
('Ingeniero de Soporte', 3300000),
('Asesor Comercial', 2000000),
('Secretaria Ejecutiva', 1800000),
('Gerente General', 5000000),
('Coordinador de Ruta', 2600000),
('Operario de Producción', 1600000);

INSERT INTO DatosEmpleados (id, nombre, apellido, fecha_contratacion, puesto_id) VALUES
(1, 'Carlos', 'Ramírez', '2023-02-01', 1),
(2, 'Ana', 'López', '2024-05-15', 2),
(3, 'Luis', 'Gómez', '2022-11-10', 3),
(4, 'María', 'Torres', '2023-06-20', 4),
(5, 'Jorge', 'Martínez', '2021-03-05', 5),
(6, 'Lucía', 'Fernández', '2022-09-14', 6),
(7, 'Pedro', 'Suárez', '2023-01-30', 7),
(8, 'Valentina', 'Morales', '2024-01-12', 8),
(9, 'Ricardo', 'Navarro', '2020-07-25', 9),
(10, 'Sofía', 'García', '2022-12-03', 10),
(11, 'Andrés', 'Vargas', '2023-04-18', 11),
(12, 'Natalia', 'Cruz', '2023-08-21', 12),
(13, 'Manuel', 'Castro', '2021-10-30', 13),
(14, 'Camila', 'Ríos', '2023-07-09', 14),
(15, 'Esteban', 'Luna', '2024-03-17', 15),
(16, 'Juliana', 'Salazar', '2022-01-04', 16),
(17, 'Tomás', 'Ortega', '2023-11-22', 17),
(18, 'Diana', 'Peña', '2021-06-08', 18),
(19, 'Sebastián', 'León', '2024-02-28', 19),
(20, 'Daniela', 'Chacón', '2023-09-16', 20),
(21, 'Martín', 'Serrano', '2022-10-11', 21),
(22, 'Isabela', 'Zamora', '2023-05-06', 22),
(23, 'Pablo', 'Mejía', '2021-12-25', 23),
(24, 'Sara', 'Herrera', '2023-03-03', 24),
(25, 'Héctor', 'Guzmán', '2020-08-19', 25),
(26, 'Mónica', 'Delgado', '2023-01-15', 26),
(27, 'Felipe', 'Aguilar', '2022-05-27', 27),
(28, 'Laura', 'Mendoza', '2024-06-10', 28),
(29, 'Diego', 'Valencia', '2021-07-02', 29),
(30, 'Paula', 'Córdoba', '2022-04-07', 30),
(31, 'Iván', 'Parra', '2023-10-13', 31),
(32, 'Elena', 'Bonilla', '2023-12-01', 32);

INSERT INTO Proveedores (nombre, dirección) VALUES
('Proveedora Andina', 'Carrera 10 #12-34'),
('Distribuciones Norte', 'Av. Norte #45-67'),
('Suministros del Sur', 'Calle 20 #15-89'),
('TecnoRepuestos S.A.S', 'Cra 50 #32-21'),
('Logística Express', 'Av. Central #90-10'),
('Repuestos Rápidos', 'Transversal 8 #45-13'),
('Insumos y Más', 'Diagonal 20 #10-77'),
('Distribuciones Universal', 'Calle 100 #50-50'),
('Comercializadora La 33', 'Av. 33 #44-12'),
('Reparaciones Globales', 'Cra 25 #20-20'),
('Multisuministros El Sol', 'Calle 77 #66-30'),
('Repuestos Alfa', 'Carrera 3 #18-45'),
('Soluciones Técnicas', 'Av. Oriental #60-60'),
('MegaDistribuidora', 'Transv. 9 #27-14'),
('ElectroServicios S.A.S', 'Calle 40 #22-90'),
('Industria y Hogar', 'Cra 15 #35-55'),
('Distribuciones Delta', 'Av. Las Palmas #100-40'),
('Proveedores Modernos', 'Calle 22 #11-17'),
('LogiParts Colombia', 'Carrera 17 #76-88'),
('Proveeduría El Centro', 'Calle 5 #8-33'),
('TodoRepuesto', 'Av. Sur #33-21'),
('Suministros Eléctricos S.A.', 'Carrera 40 #20-10'),
('Central de Repuestos', 'Calle 60 #30-45'),
('Equipos y Suministros S.A.S', 'Cra 45 #19-80'),
('Distribuidora GlobalParts', 'Diagonal 30 #22-66'),
('FerreElectro Suministros', 'Cra 8 #15-12'),
('Proveeduría Nacional', 'Av. Bolívar #90-21'),
('Insumos y Equipos Pro', 'Calle 9 #17-90'),
('Servicios Industriales SAS', 'Carrera 21 #40-30'),
('MasterPro Repuestos', 'Cra 2 #50-50'),
('Suministros Técnicos JJ', 'Calle 70 #25-33'),
('Logística y Repuestos Express', 'Transv. 33 #14-44');

INSERT INTO TipoTelefonos (tipo_tel) VALUES
('Móvil'), ('Fijo'), ('Trabajo'), ('Fax'), ('Casa'), ('Oficina'), ('Emergencia'), ('WhatsApp'), ('VoIP'), ('Otro');

INSERT INTO Telefonos (cliente_id, num_tel, tipo_id) VALUES
(1, '3205551234', 1), (2, '6041234567', 2), (3, '3217896541', 1), (4, '6042345678', 2), (5, '3104567890', 3), (6, '3011234567', 1), (7, '6048765432', 4), (8, '3109988776', 5), (9, '3001122334', 1), (10, '6041122334', 2), (11, '3134567890', 6), (12, '3149876543', 1), (13, '6043456789', 2), (14, '3206677889', 7), (15, '3225566778', 1), (16, '6012345678', 2), (17, '3013344556', 8), (18, '3044455667', 1), (19, '3202233445', 9), (20, '6049988776', 2), (21, '3156655443', 10), (22, '3163344556', 1), (23, '6011239876', 2), (24, '3015566778', 3), (25, '3204443322', 1), (26, '6047766554', 4), (27, '3126655443', 5), (28, '3004455667', 1), (29, '3137788990', 6), (30, '3153344556', 1), (31, '6019988776', 2), (32, '3112233445', 7);

INSERT INTO TiposProductos (tipo_nombre, descripcion, padre_id) VALUES
-- Categorías principales
('Electrodomésticos', 'Productos eléctricos para el hogar', NULL),
('Tecnología', 'Dispositivos electrónicos y gadgets', NULL),
('Repuestos', 'Piezas y componentes para reparación', NULL),
('Accesorios', 'Complementos para productos principales', NULL),
('Muebles', 'Muebles funcionales para el hogar', NULL),
-- Subcategorías de Electrodomésticos (padre_id = 1)
('Lavadoras', 'Lavadoras automáticas y semiautomáticas', 1),
('Neveras', 'Refrigeradores y congeladores', 1),
('Microondas', 'Hornos microondas', 1),
('Licuadoras', 'Licuadoras de cocina', 1),
('Aspiradoras', 'Aspiradoras eléctricas', 1),
('Secadoras', 'Secadoras de ropa', 1),
-- Subcategorías de Tecnología (padre_id = 2)
('Celulares', 'Teléfonos móviles', 2),
('Computadores', 'PCs, laptops y tablets', 2),
('Televisores', 'Smart TVs y LED TVs', 2),
('Accesorios electrónicos', 'Cables, cargadores, adaptadores', 2),
('Consolas', 'Consolas de videojuegos', 2),
-- Subcategorías de Repuestos (padre_id = 3)
('Motores', 'Motores para electrodomésticos', 3),
('Filtros', 'Filtros de agua, aire y otros', 3),
('Bandejas', 'Bandejas de lavadoras y refrigeradores', 3),
('Mangueras', 'Mangueras de entrada y salida', 3),
('Placas electrónicas', 'Circuitos y tarjetas electrónicas', 3),
-- Subcategorías de Accesorios (padre_id = 4)
('Soportes', 'Soportes para instalación', 4),
('Ruedas', 'Ruedas de transporte o soporte', 4),
('Tapas', 'Tapas plásticas o metálicas', 4),
('Perillas', 'Botones de control o selección', 4),
('Manijas', 'Manijas para puertas y tapas', 4),
-- Subcategorías de Muebles (padre_id = 5)
('Mesas para TV', 'Mesas y soportes para televisores', 5),
('Muebles de lavandería', 'Organizadores y mesas para lavandería', 5),
('Gabinetes', 'Gabinetes multiuso', 5),
('Estantes', 'Estantes para almacenamiento', 5),
('Bancos', 'Bancos o sillas complementarias', 5);


INSERT INTO productos (nombre, precio, proveedor_id, tipo_id) VALUES
('Lavadora LG 20kg', 1800000, 1, 2),
('Lavadora Samsung 18kg', 1650000, 2, 2),
('Lavadora Whirlpool 22kg', 1900000, 1, 2),
('Lavadora Haceb 16kg', 1500000, 2, 2),
('Panel Digital LG', 120000, 1, 21),
('Panel Samsung Smart', 135000, 2, 21),
('Bomba de Agua Universal', 25000, 1, 16),
('Tubería de desagüe universal', 18000, 1, 19),
('Filtro de agua', 22000, 2, 17),
('Manguera de entrada LG', 15000, 2, 19),
('Motor Samsung 10V', 180000, 1, 16),
('Bandeja Whirlpool 24cm', 32000, 2, 18),
('Placa electrónica Haceb', 210000, 1, 20),
('Soporte metálico para lavadora', 40000, 2, 21),
('Ruedas para lavadora x4', 28000, 2, 22),
('Tapa superior LG blanca', 35000, 1, 23),
('Perilla de selección Haceb', 10000, 1, 24),
('Manija puerta Whirlpool', 17000, 2, 25),
('Celular Samsung A34', 950000, 2, 11),
('Laptop Lenovo i5', 2400000, 1, 12),
('Smart TV LG 50"', 1800000, 2, 13),
('Cargador universal', 35000, 1, 14),
('Consola Xbox Series S', 1600000, 2, 15),
('Mesa para TV madera', 320000, 1, 26),
('Gabinete 3 puertas', 480000, 2, 28),
('Estante plástico multiusos', 98000, 1, 29),
('Banco plegable metálico', 75000, 2, 30),
('Organizador lavandería', 125000, 1, 27),
('Juego de cables lavadora', 45000, 2, 14),
('Nevera Samsung 250L', 2100000, 1, 7),
('Microondas LG 1000W', 420000, 2, 8),
('Aspiradora Electrolux 2L', 350000, 1, 10);

INSERT INTO Empleados (nombre, puesto, salario, fecha_contratacion) VALUES
('Carlos Ramírez', 'Vendedor', 1800000, '2023-02-01'),
('Ana López', 'Repartidor', 1500000, '2024-05-15'),
('Lucía Torres', 'Vendedor', 1800000, '2022-11-20'),
('José Martínez', 'Repartidor', 1500000, '2023-06-05'),
('María Gómez', 'Vendedor', 1800000, '2023-03-10'),
('David Pérez', 'Repartidor', 1500000, '2024-01-18'),
('Laura Rodríguez', 'Vendedor', 1800000, '2022-08-09'),
('Andrés Moreno', 'Repartidor', 1500000, '2023-04-25'),
('Sandra Patiño', 'Vendedor', 1800000, '2023-12-12'),
('Felipe Herrera', 'Repartidor', 1500000, '2022-10-03'),
('Carolina Vargas', 'Vendedor', 1800000, '2024-02-20'),
('Miguel Salazar', 'Repartidor', 1500000, '2023-07-14'),
('Natalia Ríos', 'Vendedor', 1800000, '2024-03-30'),
('Julián Mendoza', 'Repartidor', 1500000, '2022-09-01'),
('Viviana Castro', 'Vendedor', 1800000, '2023-05-08'),
('Esteban Ramírez', 'Repartidor', 1500000, '2023-01-27'),
('Camila Duarte', 'Vendedor', 1800000, '2024-04-11'),
('Juan Pablo Gutiérrez', 'Repartidor', 1500000, '2023-06-16'),
('Sara Cárdenas', 'Vendedor', 1800000, '2022-07-04'),
('Diego Luna', 'Repartidor', 1500000, '2023-08-21'),
('Paula Acosta', 'Vendedor', 1800000, '2024-06-28'),
('Santiago Prieto', 'Repartidor', 1500000, '2022-05-12'),
('Daniela Becerra', 'Vendedor', 1800000, '2023-09-09'),
('Ricardo León', 'Repartidor', 1500000, '2023-10-30'),
('Mónica Fernández', 'Vendedor', 1800000, '2024-01-03'),
('Héctor Niño', 'Repartidor', 1500000, '2022-12-19'),
('Elena Torres', 'Vendedor', 1800000, '2023-11-23'),
('Tomás Aguilar', 'Repartidor', 1500000, '2024-05-01'),
('Isabel Cortés', 'Vendedor', 1800000, '2022-06-16'),
('Mauricio Beltrán', 'Repartidor', 1500000, '2023-02-14'),
('Juliana Guerrero', 'Vendedor', 1800000, '2024-03-19'),
('Emilio Silva', 'Repartidor', 1500000, '2023-07-02');

INSERT INTO EmpleadoProveedor (empleado_id, proveedor_id) VALUES
(1, 1), (2, 2), (3, 1), (4, 3), (5, 2), (6, 4), (7, 1), (8, 2), (9, 3), (10, 4), (11, 5), (12, 1), (13, 6), (14, 2), (15, 3), (16, 4), (17, 5), (18, 6), (19, 1), (20, 2), (21, 3), (22, 4), (23, 5), (24, 6), (25, 1), (26, 2), (27, 3), (28, 4), (29, 5), (30, 6), (31, 1), (32, 2);


INSERT INTO Ubicaciones (entidad_tipo, entidad_id, dirección, ciudad, estado, código_postal, país) VALUES
('Cliente', 1, 'Calle 123', 'Bogotá', 'Cundinamarca', '110111', 'Colombia'),
('Cliente', 2, 'Av. Central 456', 'Medellín', 'Antioquia', '050021', 'Colombia'),
('Proveedor', 1, 'Carrera 10 #12-34', 'Bogotá', 'Cundinamarca', '110111', 'Colombia'),
('Proveedor', 2, 'Av. Norte #45-67', 'Medellín', 'Antioquia', '050021', 'Colombia'),
('Cliente', 3, 'Calle 45 #21-56', 'Cali', 'Valle del Cauca', '760001', 'Colombia'),
('Cliente', 4, 'Cra. 7 #64-23', 'Barranquilla', 'Atlántico', '080001', 'Colombia'),
('Cliente', 5, 'Calle 30 #15-45', 'Cartagena', 'Bolívar', '130001', 'Colombia'),
('Cliente', 6, 'Av. El Poblado 55-21', 'Medellín', 'Antioquia', '050022', 'Colombia'),
('Cliente', 7, 'Calle 18 #10-90', 'Pereira', 'Risaralda', '660001', 'Colombia'),
('Cliente', 8, 'Carrera 9 #25-60', 'Manizales', 'Caldas', '170001', 'Colombia'),
('Proveedor', 3, 'Zona Industrial Km 4', 'Cali', 'Valle del Cauca', '760002', 'Colombia'),
('Proveedor', 4, 'Bodega 2, Vía 40', 'Barranquilla', 'Atlántico', '080002', 'Colombia'),
('Proveedor', 5, 'Av. Circunvalar 120', 'Cartagena', 'Bolívar', '130002', 'Colombia'),
('Proveedor', 6, 'Calle 5 #12-34', 'Bogotá', 'Cundinamarca', '110112', 'Colombia'),
('Empleado', 1, 'Calle 50 #14-20', 'Bogotá', 'Cundinamarca', '110113', 'Colombia'),
('Empleado', 2, 'Av. Oriental 33-14', 'Medellín', 'Antioquia', '050023', 'Colombia'),
('Empleado', 3, 'Cra. 8 #40-19', 'Cali', 'Valle del Cauca', '760003', 'Colombia'),
('Empleado', 4, 'Calle 23 #7-89', 'Pereira', 'Risaralda', '660002', 'Colombia'),
('Empleado', 5, 'Calle 26 #9-15', 'Manizales', 'Caldas', '170002', 'Colombia'),
('Empleado', 6, 'Carrera 11 #60-33', 'Barranquilla', 'Atlántico', '080003', 'Colombia'),
('Empleado', 7, 'Av. Chile #100-45', 'Bogotá', 'Cundinamarca', '110114', 'Colombia'),
('Empleado', 8, 'Calle 44 #22-10', 'Medellín', 'Antioquia', '050024', 'Colombia'),
('Cliente', 9, 'Cra. 30 #45-12', 'Bucaramanga', 'Santander', '680001', 'Colombia'),
('Cliente', 10, 'Calle 6 #14-88', 'Ibagué', 'Tolima', '730001', 'Colombia'),
('Proveedor', 7, 'Av. Pasoancho 75-20', 'Cali', 'Valle del Cauca', '760004', 'Colombia'),
('Proveedor', 8, 'Cra. 50 #80-99', 'Barranquilla', 'Atlántico', '080004', 'Colombia'),
('Proveedor', 9, 'Zona Franca KM 6', 'Cartagena', 'Bolívar', '130003', 'Colombia'),
('Empleado', 9, 'Cra. 4 #16-60', 'Cúcuta', 'Norte de Santander', '540001', 'Colombia'),
('Empleado', 10, 'Calle 8 #21-33', 'Neiva', 'Huila', '410001', 'Colombia'),
('Empleado', 11, 'Av. 3ra Norte #18-10', 'Palmira', 'Valle del Cauca', '763532', 'Colombia'),
('Cliente', 11, 'Calle 2 #4-15', 'Villavicencio', 'Meta', '500001', 'Colombia'),
('Cliente', 12, 'Cra. 16 #9-22', 'Tunja', 'Boyacá', '150001', 'Colombia'),
('Cliente', 13, 'Av. Providencia 245', 'Santiago', 'Región Metropolitana', '8320000', 'Chile'),
('Cliente', 14, 'Calle 100 #20-50', 'Ciudad de México', 'CDMX', '03100', 'México'),
('Proveedor', 10, 'Calle Corrientes 3000', 'Buenos Aires', 'CABA', 'C1043', 'Argentina'),
('Empleado', 12, 'Jirón de la Union 500', 'Lima', 'Lima', '15001', 'Perú');
```

# datos adicionales

```
mysql> INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES
    -> ('Lavadora Haier 16kg', 780.00, 5, 2),
    -> ('Lavadora LG 13kg', 890.00, 5, 2),
    -> ('Lavadora Samsung 18kg', 920.00, 5, 2),
    -> ('Secadora Mabe 14kg', 770.00, 5, 2),
    -> ('Lavadora Whirlpool 20kg', 960.00, 5, 2),
    -> ('Lavadora Electrolux 17kg', 870.00, 5, 2),
    -> ('Lavadora Daewoo 12kg', 720.00, 5, 2),
    -> ('Secadora LG 15kg', 880.00, 5, 2);
Query OK, 8 rows affected (0.00 sec)
Records: 8  Duplicates: 0  Warnings: 0

mysql> INSERT INTO Proveedores (nombre, dirección) VALUES
    -> ('Proveedora Andina', 'Av. Los Andes 123'),
    -> ('Distribuciones Norte', 'Calle Norte 456'),
    -> ('Suministros del Sur', 'Calle Sur 45'),
    -> ('TecnoRepuestos', 'Avenida Central 100'),
    -> ('Importadora Universal', 'Calle 10 #22-33');
Query OK, 5 rows affected (0.01 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> -- Productos para TecnoRepuestos (proveedor_id = 4)
mysql> INSERT INTO Productos (nombre, precio, proveedor_id, tipo_id) VALUES
    -> ('Motor Universal 1/2 HP', 350.00, 4, 1),
    -> ('Correa para lavadora Samsung', 45.00, 4, 1),
    -> ('Sensor de nivel de agua', 28.50, 4, 1),
    -> ('Placa electrónica Whirlpool', 110.00, 4, 1),
    -> ('Valvula de entrada LG', 25.00, 4, 1),
    -> ('Tubo de drenaje flexible', 18.00, 4, 1),
    -> ('Amortiguador Bosch', 33.00, 4, 1),
    -> ('Switch de puerta Electrolux', 19.00, 4, 1);
Query OK, 8 rows affected (0.01 sec)
Records: 8  Duplicates: 0  Warnings: 0

mysql> INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad, precio) VALUES
    -> (11, 2, 1, 450.00),
    -> (12, 2, 1, 450.00),
    -> (13, 2, 1, 450.00),
    -> (14, 2, 1, 450.00);
Query OK, 4 rows affected (0.01 sec)
Records: 4  Duplicates: 0  Warnings: 0

mysql>
mysql> -- Añadir producto_id 3 con 7 pedidos (entrará en la consulta)
mysql> INSERT INTO DetallesPedido (pedido_id, producto_id, cantidad, precio) VALUES
    -> (15, 3, 1, 390.00),
    -> (16, 3, 1, 390.00),
    -> (17, 3, 1, 390.00),
    -> (18, 3, 1, 390.00),
    -> (19, 3, 1, 390.00),
    -> (20, 3, 1, 390.00),
    -> (21, 3, 1, 390.00);
Query OK, 7 rows affected (0.00 sec)
Records: 7  Duplicates: 0  Warnings: 0

mysql> INSERT INTO Empleados (nombre, puesto, salario, fecha_contratacion) VALUES
    -> ('Ana Pérez', 'Técnico', 1800.00, '2023-01-10'),
    -> ('Luis Martínez', 'Técnico', 2200.00, '2022-03-15'),
    -> ('Carla López', 'Técnico', 2500.00, '2021-06-01'),
    -> ('José Gómez', 'Supervisor', 3000.00, '2020-05-20'),
    -> ('Laura Jiménez', 'Supervisor', 3500.00, '2019-09-12'),
    -> ('Pedro Ruiz', 'Supervisor', 2700.00, '2023-11-25'),
    -> ('Lucía Castro', 'Gerente', 4000.00, '2018-08-08'),
    -> ('Miguel Ramírez', 'Gerente', 4200.00, '2017-02-02'),
    -> ('Sofía Sánchez', 'Gerente', 3800.00, '2024-04-30');
Query OK, 9 rows affected (0.01 sec)
Records: 9  Duplicates: 0  Warnings: 0

mysql> -- PEDIDOS para Ana (5 pedidos)
mysql> INSERT INTO Pedidos (cliente_id, fecha) VALUES
    -> (1, '2025-07-01'),
    -> (1, '2025-07-02'),
    -> (1, '2025-07-03'),
    -> (1, '2025-07-04'),
    -> (1, '2025-07-05');
Query OK, 5 rows affected (0.01 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql>
mysql> -- PEDIDOS para Carlos (3 pedidos)
mysql> INSERT INTO Pedidos (cliente_id, fecha) VALUES
    -> (2, '2025-07-01'),
    -> (2, '2025-07-03'),
    -> (2, '2025-07-05');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0

mysql>
mysql> -- PEDIDOS para Marta (1 pedido)
mysql> INSERT INTO Pedidos (cliente_id, fecha) VALUES
    -> (3, '2025-07-02');
Query OK, 1 row affected (0.00 sec)

mysql>
mysql> -- PEDIDOS para Luis (2 pedidos)
mysql> INSERT INTO Pedidos (cliente_id, fecha) VALUES
    -> (4, '2025-07-03'),
    -> (4, '2025-07-04');
Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

mysql> -- Pedidos con distintos totales
mysql>
mysql> -- Pedido 1: total 20
mysql> INSERT INTO Detallespedido (pedido_id, producto_id, cantidad, precio) VALUES
    -> (1, 1, 2, 10.00);  -- 2 * 10 = 20
Query OK, 1 row affected (0.01 sec)

mysql>
mysql> -- Pedido 2: total 45
mysql> INSERT INTO Detallespedido (pedido_id, producto_id, cantidad, precio) VALUES
    -> (2, 2, 3, 15.00);  -- 3 * 15 = 45
Query OK, 1 row affected (0.00 sec)

mysql>
mysql> -- Pedido 3: total 30
mysql> INSERT INTO Detallespedido (pedido_id, producto_id, cantidad, precio) VALUES
    -> (3, 3, 2, 15.00);  -- 2 * 15 = 30
Query OK, 1 row affected (0.00 sec)

mysql>
mysql> -- Pedido 4: total 60
mysql> INSERT INTO Detallespedido (pedido_id, producto_id, cantidad, precio) VALUES
    -> (4, 4, 4, 15.00);  -- 4 * 15 = 60
Query OK, 1 row affected (0.00 sec)

mysql>
mysql> -- Pedido 5: total 10
mysql> INSERT INTO Detallespedido (pedido_id, producto_id, cantidad, precio) VALUES
    -> (5, 5, 1, 10.00);  -- 1 * 10 = 10
Query OK, 1 row affected (0.00 sec)
```

## EJERCICIOS 

# 5. SUBCONSULTAS:
```
5.1. Consultar el producto más caro en cada categoría.
RTA:
SELECT p.nombre AS producto, p.precio, tp_padre.tipo_nombre AS categoria FROM Productos p
JOIN TiposProductos tp ON p.tipo_id = tp.id LEFT JOIN TiposProductos tp_padre ON tp.padre_id = tp_padre.id
WHERE (p.tipo_id, p.precio) IN ( SELECT tipo_id, MAX(precio) FROM Productos WHERE tipo_id IN ( SELECT id FROM TiposProductos WHERE padre_id IS NOT NULL) GROUP BY tipo_id);
+--------------------------------+------------+--------------------+
| producto                       | precio     | categoria          |
+--------------------------------+------------+--------------------+
| Panel Samsung Smart            |  135000.00 | Repuestos          |
| Tubería de desagüe universal   |   18000.00 | Repuestos          |
| Filtro de agua                 |   22000.00 | Repuestos          |
| Motor Samsung 10V              |  180000.00 | Tecnología         |
| Bandeja Whirlpool 24cm         |   32000.00 | Repuestos          |
| Placa electrónica Haceb        |  210000.00 | Repuestos          |
| Ruedas para lavadora x4        |   28000.00 | Accesorios         |
| Tapa superior LG blanca        |   35000.00 | Accesorios         |
| Perilla de selección Haceb     |   10000.00 | Accesorios         |
| Manija puerta Whirlpool        |   17000.00 | Accesorios         |
| Celular Samsung A34            |  950000.00 | Electrodomésticos  |
| Laptop Lenovo i5               | 2400000.00 | Tecnología         |
| Smart TV LG 50"                | 1800000.00 | Tecnología         |
| Consola Xbox Series S          | 1600000.00 | Tecnología         |
| Mesa para TV madera            |  320000.00 | Accesorios         |
| Gabinete 3 puertas             |  480000.00 | Muebles            |
| Estante plástico multiusos     |   98000.00 | Muebles            |
| Banco plegable metálico        |   75000.00 | Muebles            |
| Organizador lavandería         |  125000.00 | Muebles            |
| Juego de cables lavadora       |   45000.00 | Tecnología         |
| Nevera Samsung 250L            | 2100000.00 | Electrodomésticos  |
| Microondas LG 1000W            |  420000.00 | Electrodomésticos  |
| Aspiradora Electrolux 2L       |  350000.00 | Electrodomésticos  |
+--------------------------------+------------+--------------------+

5.2. Encontrar el cliente con mayor total de pedidos.
RTA:
SELECT id, nombre FROM clientes WHERE id = (SELECT cliente_id FROM pedidos GROUP BY cliente_id LIMIT 1);
+----+--------+
| id | nombre |
+----+--------+
|  1 | Juan   |
+----+--------+

5.3. Listar empleados que ganan más que el salario promedio.
RTA:
SELECT id, nombre, puesto, salario FROM Empleados WHERE salario > ( SELECT AVG(salario) FROM Empleados);
+----+--------------------+----------+------------+
| id | nombre             | puesto   | salario    |
+----+--------------------+----------+------------+
|  1 | Carlos Ramírez     | Vendedor | 1800000.00 |
|  3 | Lucía Torres       | Vendedor | 1800000.00 |
|  5 | María Gómez        | Vendedor | 1800000.00 |
|  7 | Laura Rodríguez    | Vendedor | 1800000.00 |
|  9 | Sandra Patiño      | Vendedor | 1800000.00 |
| 11 | Carolina Vargas    | Vendedor | 1800000.00 |
| 13 | Natalia Ríos       | Vendedor | 1800000.00 |
| 15 | Viviana Castro     | Vendedor | 1800000.00 |
| 17 | Camila Duarte      | Vendedor | 1800000.00 |
| 19 | Sara Cárdenas      | Vendedor | 1800000.00 |
| 21 | Paula Acosta       | Vendedor | 1800000.00 |
| 23 | Daniela Becerra    | Vendedor | 1800000.00 |
| 25 | Mónica Fernández   | Vendedor | 1800000.00 |
| 27 | Elena Torres       | Vendedor | 1800000.00 |
| 29 | Isabel Cortés      | Vendedor | 1800000.00 |
| 31 | Juliana Guerrero   | Vendedor | 1800000.00 |
+----+--------------------+----------+------------+

5.4. Consultar productos que han sido pedidos más de 5 veces.
RTA:
SELECT dp.producto_id, p.nombre, COUNT(dp.id) AS veces_pedido FROM Detallespedido dp JOIN productos p ON dp.producto_id = p.id GROUP BY dp.producto_id, p.nombre HAVING COUNT(dp.id) > 5 ORDER BY veces_pedido DESC;
+-------------+-------------------------+--------------+
| producto_id | nombre                  | veces_pedido |
+-------------+-------------------------+--------------+
|           3 | Lavadora Whirlpool 22kg |           12 |
|           1 | Lavadora LG 20kg        |           11 |
|           2 | Lavadora Samsung 18kg   |           11 |
|           5 | Panel Digital LG        |            7 |
+-------------+-------------------------+--------------+

5.5. Listar pedidos cuyo total es mayor al promedio de todos los pedidos.
RTA: 
SELECT pedido_id, SUM(cantidad * precio) AS total FROM DetallesPedido GROUP BY pedido_id HAVING total > ( SELECT AVG(sub.total) FROM (SELECT pedido_id, SUM(cantidad * precio) AS total FROM DetallesPedido GROUP BY pedido_id) AS sub);
+-----------+-------+
| pedido_id | total |
+-----------+-------+
|         1 | 40.00 |
|         2 | 55.00 |
|         3 | 60.00 |
|         4 | 80.00 |
|        13 | 30.00 |
|        15 | 30.00 |
+-----------+-------+

5.6. Seleccionar los 3 proveedores con más productos.
RTA:
SELECT p.proveedor_id, pr.nombre, COUNT(*) AS total_productos FROM Productos p
JOIN Proveedores pr ON p.proveedor_id = pr.id GROUP BY p.proveedor_id ORDER BY total_productos DESC LIMIT 3;
+--------------+----------------------+-----------------+
| proveedor_id | nombre               | total_productos |
+--------------+----------------------+-----------------+
|            1 | Proveedora Andina    |              16 |
|            2 | Distribuciones Norte |              16 |
|            4 | TecnoRepuestos S.A.S |               8 |
+--------------+----------------------+-----------------+

5.7. Consultar productos con precio superior al promedio en su tipo.
RTA:
SELECT p.id, p.nombre, p.precio FROM Productos p JOIN ( SELECT tipo_id, AVG(precio) AS promedio FROM Productos GROUP BY tipo_id) AS sub ON p.tipo_id = sub.tipo_id WHERE p.precio > sub.promedio;
+----+--------------------------------+------------+
| id | nombre                         | precio     |
+----+--------------------------------+------------+
|  1 | Lavadora LG 20kg               | 1800000.00 |
|  3 | Lavadora Whirlpool 22kg        | 1900000.00 |
| 29 | Juego de cables lavadora       |   45000.00 |
| 11 | Motor Samsung 10V              |  180000.00 |
|  8 | Tubería de desagüe universal   |   18000.00 |
|  5 | Panel Digital LG               |  120000.00 |
|  6 | Panel Samsung Smart            |  135000.00 |
+----+--------------------------------+------------+

5.8. Mostrar clientes que han realizado más pedidos que la media.
RTA:
SELECT c.id, c.nombre, c.apellido, COUNT(p.id) AS total_pedidos FROM Clientes c JOIN Pedidos p ON c.id = p.cliente_id GROUP BY c.id HAVING total_pedidos > (SELECT AVG(sub.total) FROM (SELECT COUNT(p2.id) AS total FROM Pedidos p2 GROUP BY p2.cliente_id) AS sub);
+----+--------+-----------+---------------+
| id | nombre | apellido  | total_pedidos |
+----+--------+-----------+---------------+
|  1 | Juan   | Pérez     |             6 |
|  2 | María  | Gómez     |             4 |
|  3 | Carlos | Ramírez   |             2 |
|  4 | Ana    | Martínez  |             3 |
+----+--------+-----------+---------------+

5.9. Encontrar productos cuyo precio es mayor que el promedio de todos los productos.
RTA:
SELECT id, nombre, precio FROM Productos WHERE precio > ( SELECT AVG(precio) FROM Productos);
+----+-------------------------+------------+
| id | nombre                  | precio     |
+----+-------------------------+------------+
|  1 | Lavadora LG 20kg        | 1800000.00 |
|  2 | Lavadora Samsung 18kg   | 1650000.00 |
|  3 | Lavadora Whirlpool 22kg | 1900000.00 |
|  4 | Lavadora Haceb 16kg     | 1500000.00 |
| 19 | Celular Samsung A34     |  950000.00 |
| 20 | Laptop Lenovo i5        | 2400000.00 |
| 21 | Smart TV LG 50"         | 1800000.00 |
| 23 | Consola Xbox Series S   | 1600000.00 |
| 30 | Nevera Samsung 250L     | 2100000.00 |
+----+-------------------------+------------+

5.10. Mostrar empleados cuyo salario es menor al promedio del departamento.
RTA:
SELECT e.id, e.nombre FROM Empleados e JOIN (SELECT puesto, AVG(salario) AS promedio FROM Empleados GROUP BY puesto) AS sub ON e.puesto = sub.puesto WHERE e.salario < sub.promedio;
+----+-----------------+
| id | nombre          |
+----+-----------------+
| 33 | Ana Pérez       |
| 36 | José Gómez      |
| 38 | Pedro Ruiz      |
| 41 | Sofía Sánchez   |
+----+-----------------+
```
6. Procedimientos Almacenados 
```
6.1. Crear un procedimiento para actualizar el precio de todos los productos de un proveedor.
RTA:
DELIMITER //
CREATE PROCEDURE ActualizarPrecioProveedor(
  IN proveedor INT,
  IN nuevo_precio DECIMAL(10,2)
)
BEGIN
  UPDATE Productos
  SET precio = nuevo_precio
  WHERE proveedor_id = proveedor;
END //
DELIMITER ;

6.2. Un procedimiento que devuelva la dirección de un cliente por ID.
RTA:
DELIMITER //
CREATE PROCEDURE ObtenerDireccionCliente(
  IN cliente_id INT
)
BEGIN
  SELECT dirección, ciudad, estado, código_postal, país
  FROM Ubicaciones
  WHERE entidad_tipo = 'cliente' AND entidad_id = cliente_id;
END //
DELIMITER ;

6.3. Crear un procedimiento que registre un pedido nuevo y sus detalles.
RTA:
DELIMITER //
CREATE PROCEDURE RegistrarPedidoConDetalles(
  IN cliente INT,
  IN fecha DATE,
  IN detalles JSON
)
BEGIN
  DECLARE nuevo_pedido_id INT;
  INSERT INTO Pedidos (cliente_id, fecha_pedido)
  VALUES (cliente, fecha);
  SET nuevo_pedido_id = LAST_INSERT_ID();
END //
DELIMITER ;

6.4. Un procedimiento para calcular el total de ventas de un cliente.
RTA:
DELIMITER //
CREATE PROCEDURE TotalVentasCliente(
  IN cliente_id INT
)
BEGIN
  SELECT SUM(dp.cantidad * dp.precio) AS total_ventas
  FROM Pedidos p
  JOIN DetallesPedido dp ON p.id = dp.pedido_id
  WHERE p.cliente_id = cliente_id;
END //
DELIMITER ;

6.5. Crear un procedimiento para obtener los empleados por puesto.
RTA:
DELIMITER //
CREATE PROCEDURE ObtenerEmpleadosPorPuesto(
  IN puesto_nombre VARCHAR(50)
)
BEGIN
  SELECT id, nombre, puesto, salario, fecha_contratacion
  FROM Empleados
  WHERE puesto = puesto_nombre;
END //
DELIMITER ;

6.6. Un procedimiento que actualice el salario de empleados por puesto.
RTA:
DELIMITER //
CREATE PROCEDURE ActualizarSalarioPorPuesto(
  IN puesto_nombre VARCHAR(50),
  IN nuevo_salario DECIMAL(10,2)
)
BEGIN
  UPDATE Empleados
  SET salario = nuevo_salario
  WHERE puesto = puesto_nombre;
END //
DELIMITER ;

6.7. Crear un procedimiento que liste los pedidos entre dos fechas.
RTA:
DELIMITER //
CREATE PROCEDURE ListarPedidosEntreFechas(
  IN fecha_inicio DATE,
  IN fecha_fin DATE
)
BEGIN
  SELECT id, cliente_id, fecha
  FROM Pedidos
  WHERE fecha_pedido BETWEEN fecha_inicio AND fecha_fin;
END //
DELIMITER ;

6.8. Un procedimiento para aplicar un descuento a productos de una categoría.
RTA:
DELIMITER //
CREATE PROCEDURE AplicarDescuentoCategoria(
  IN tipo_producto_id INT,
  IN porcentaje_descuento DECIMAL(5,2)
)
BEGIN
  UPDATE Productos
  SET precio = precio - (precio * (porcentaje_descuento / 100))
  WHERE tipo_id = tipo_producto_id;
END //
DELIMITER ;

6.9. Crear un procedimiento que liste todos los proveedores de un tipo de producto.
RTA:
DELIMITER //
CREATE PROCEDURE ListarProveedoresPorTipoProducto(
  IN tipo_producto_id INT
)
BEGIN
  SELECT DISTINCT pr.*
  FROM Proveedores pr
  JOIN Productos p ON pr.id = p.proveedor_id
  WHERE p.tipo_id = tipo_producto_id;
END //
DELIMITER ;

6.10. Un procedimiento que devuelva el pedido de mayor valor.
RTA:
DELIMITER //
CREATE PROCEDURE PedidoMayorValor()
BEGIN
  SELECT p.id, p.cliente_id, p.fecha_pedido, 
         SUM(dp.cantidad * dp.precio) AS total
  FROM Pedidos p
  JOIN DetallesPedido dp ON p.id = dp.pedido_id
  GROUP BY p.id
  ORDER BY total DESC
  LIMIT 1;
END //
DELIMITER ;
```
REALIZADO POR: Leidy Johana Niño Villegas
