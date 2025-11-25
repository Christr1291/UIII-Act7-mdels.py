Sistema de Gestión de Farmacia

-- -----------------------------------------------------
-- PROVEEDOR
-- -----------------------------------------------------
CREATE TABLE proveedor_farmacia (
    id_proveedor INT AUTO_INCREMENT PRIMARY KEY,
    nombre_proveedor VARCHAR(100) NOT NULL,
    contacto_persona VARCHAR(100) NOT NULL,
    telefono VARCHAR(20) NOT NULL,
    email VARCHAR(100) NOT NULL,
    direccion_proveedor VARCHAR(255) NOT NULL,
    ruc VARCHAR(20) NOT NULL,
    fecha_registro DATE NOT NULL
) ENGINE=InnoDB;


-- -----------------------------------------------------
-- MEDICAMENTO
-- -----------------------------------------------------
CREATE TABLE medicamento (
    id_medicamento INT AUTO_INCREMENT PRIMARY KEY,
    nombre_medicamento VARCHAR(255) NOT NULL,
    descripcion TEXT NOT NULL,
    precio_venta DECIMAL(10,2) NOT NULL,
    stock INT NOT NULL,
    proveedor_id INT NOT NULL,
    fecha_vencimiento DATE NOT NULL,
    codigo_barra VARCHAR(50) NOT NULL,
    principio_activo VARCHAR(255) NOT NULL,
    forma_farmaceutica VARCHAR(100) NOT NULL,
    requiere_receta BOOLEAN NOT NULL DEFAULT 0,

    FOREIGN KEY (proveedor_id) REFERENCES proveedor_farmacia(id_proveedor)
        ON DELETE CASCADE
) ENGINE=InnoDB;


-- -----------------------------------------------------
-- CLIENTE FARMACIA
-- -----------------------------------------------------
CREATE TABLE cliente_farmacia (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    telefono VARCHAR(20) NOT NULL,
    email VARCHAR(100) NOT NULL,
    direccion VARCHAR(255) NOT NULL,
    fecha_registro DATE NOT NULL,
    num_seguro_medico VARCHAR(50),
    fecha_nacimiento DATE,
    preferencias TEXT
) ENGINE=InnoDB;


-- -----------------------------------------------------
-- FARMACÉUTICO
-- -----------------------------------------------------
CREATE TABLE farmaceutico (
    id_farmaceutico INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido VARCHAR(100) NOT NULL,
    dni VARCHAR(20) NOT NULL,
    fecha_contratacion DATE NOT NULL,
    salario DECIMAL(10,2) NOT NULL,
    licencia_colegiado VARCHAR(50) NOT NULL,
    turno VARCHAR(50) NOT NULL,
    telefono VARCHAR(20) NOT NULL,
    email VARCHAR(100) NOT NULL
) ENGINE=InnoDB;


-- -----------------------------------------------------
-- RECETA MÉDICA
-- -----------------------------------------------------
CREATE TABLE receta_medica (
    id_receta INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT NOT NULL,
    id_medico_receto INT NOT NULL, -- Ajusta cuando crees el modelo Medico o Veterinario
    fecha_emision DATE NOT NULL,
    fecha_vencimiento DATE NOT NULL,
    diagnostico TEXT NOT NULL,
    medicamento_recetado_texto TEXT NOT NULL,
    dosis_indicada TEXT NOT NULL,
    estado_receta VARCHAR(50) NOT NULL,

    FOREIGN KEY (cliente_id) REFERENCES cliente_farmacia(id_cliente)
        ON DELETE CASCADE
) ENGINE=InnoDB;


-- -----------------------------------------------------
-- VENTA FARMACIA
-- -----------------------------------------------------
CREATE TABLE venta_farmacia (
    id_venta INT AUTO_INCREMENT PRIMARY KEY,
    fecha_venta DATETIME NOT NULL,
    farmaceutico_id INT NOT NULL,
    cliente_id INT NOT NULL,
    total_venta DECIMAL(10,2) NOT NULL,
    metodo_pago VARCHAR(50) NOT NULL,
    descuento_total DECIMAL(5,2) NOT NULL,
    numero_factura VARCHAR(50) NOT NULL,
    estado_venta VARCHAR(50) NOT NULL,

    FOREIGN KEY (farmaceutico_id) REFERENCES farmaceutico(id_farmaceutico)
        ON DELETE CASCADE,

    FOREIGN KEY (cliente_id) REFERENCES cliente_farmacia(id_cliente)
        ON DELETE CASCADE
) ENGINE=InnoDB;


-- -----------------------------------------------------
-- DETALLE VENTA
-- -----------------------------------------------------
CREATE TABLE detalle_venta_farmacia (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    venta_id INT NOT NULL,
    medicamento_id INT NOT NULL,
    cantidad INT NOT NULL,
    precio_unitario_venta DECIMAL(10,2) NOT NULL,
    subtotal DECIMAL(10,2) NOT NULL,
    iva_aplicado DECIMAL(5,2) NOT NULL,
    receta_asociada_id INT,

    FOREIGN KEY (venta_id) REFERENCES venta_farmacia(id_venta)
        ON DELETE CASCADE,

    FOREIGN KEY (medicamento_id) REFERENCES medicamento(id_medicamento)
        ON DELETE CASCADE,

    FOREIGN KEY (receta_asociada_id) REFERENCES receta_medica(id_receta)
        ON DELETE SET NULL
) ENGINE=InnoDB;

<img width="685" height="751" alt="image" src="https://github.com/user-attachments/assets/15596970-1386-47f4-ba65-cd42319291a9" />
