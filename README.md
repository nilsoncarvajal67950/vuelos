# AEROLÍNEA ✈

# CÓDIGO

CREATE DATABASE aerolinea;
USE aerolinea;

-- Tabla de Países
CREATE TABLE Pais (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);

-- Tabla de Ciudades
CREATE TABLE Ciudad (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30),
  IdPais_fk INT,
  CONSTRAINT FK_PaisCiudad FOREIGN KEY (IdPais_fk) REFERENCES Pais(Id)
);

-- Tabla de Aeropuertos
CREATE TABLE Aeropuerto (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30),
  IdCiudad_fk INT,
  CONSTRAINT FK_CiudadAeropuerto FOREIGN KEY (IdCiudad_fk) REFERENCES Ciudad(Id)
);

-- Tabla de Puertas (Gates)
CREATE TABLE Gate (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  NroPuerta INT,
  IdAeropuerto_fk INT,
  CONSTRAINT FK_AeropuertoGate FOREIGN KEY (IdAeropuerto_fk) REFERENCES Aeropuerto(Id)
);

-- Tabla de Aerolíneas
CREATE TABLE Aerolinea (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);

-- Tabla de Roles de Tripulación
CREATE TABLE RolTripulacion (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);

-- Tabla de Empleados
CREATE TABLE Empleado (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30),
  IdRol_fk INT,
  IdAero_fk INT,
  IdAeropuerto_fk INT,
  CONSTRAINT FK_RolEmpleado FOREIGN KEY (IdRol_fk) REFERENCES RolTripulacion(Id),
  CONSTRAINT FK_AeroEmpleado FOREIGN KEY (IdAero_fk) REFERENCES Aerolinea(Id),
  CONSTRAINT FK_AeropuertoEmpleado FOREIGN KEY (IdAeropuerto_fk) REFERENCES Aeropuerto(Id)
);

-- Tabla de Trayectos
CREATE TABLE Trayecto (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  FechaTrayecto DATE,
  Valor DOUBLE,
  CiudadOrigen VARCHAR(30),
  CiudadDestino VARCHAR(30)
);

-- Tabla de Fabricantes
CREATE TABLE Fabricante (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(40)
);

-- Tabla de Modelos de Aviones
CREATE TABLE Modelo (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30),
  IdFabricante_fk INT,
  CONSTRAINT FK_FabricanteModelo FOREIGN KEY (IdFabricante_fk) REFERENCES Fabricante(Id)
);

-- Tabla de Estados
CREATE TABLE Estado (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);

-- Tabla de Aviones
CREATE TABLE Avion (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  NroMatricula VARCHAR(30),
  Capacidad INT,
  FechaFabricacion DATE,
  IdEstado_fk INT,
  IdModelo_fk INT,
  IdAero_fk INT,
  CONSTRAINT FK_EstadoAvion FOREIGN KEY (IdEstado_fk) REFERENCES Estado(Id),
  CONSTRAINT FK_ModeloAvion FOREIGN KEY (IdModelo_fk) REFERENCES Modelo(Id),
  CONSTRAINT FK_AeroAvion FOREIGN KEY (IdAero_fk) REFERENCES Aerolinea(Id)
);

-- Tabla de Revisiones de Aviones
CREATE TABLE Revision (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Fecha DATE,
  IdAvion_fk INT,
  CONSTRAINT FK_AvionRevision FOREIGN KEY (IdAvion_fk) REFERENCES Avion(Id)
);

-- Tabla de Empleados en Revisiones
CREATE TABLE RevEmpleado (
  IdEmpleado_fk INT,
  IdRevision_fk INT,
  CONSTRAINT FK_EmpleadoRevEmpleado FOREIGN KEY (IdEmpleado_fk) REFERENCES Empleado(Id),
  CONSTRAINT FK_RevisionEmpleado FOREIGN KEY (IdRevision_fk) REFERENCES Revision(Id)
);

-- Tabla de Reservas de Viaje
CREATE TABLE ReservaViaje (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Fecha DATE,
  IdTrayecto_fk INT,
  CONSTRAINT FK_TrayectoReserva FOREIGN KEY (IdTrayecto_fk) REFERENCES Trayecto(Id)
);

-- Tabla de Tipos de Documento
CREATE TABLE TipoDocumento (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(30)
);

-- Tabla de Clientes
CREATE TABLE Cliente (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Edad INT,
  IdTipo_fk INT,
  CONSTRAINT FK_TipoCliente FOREIGN KEY (IdTipo_fk) REFERENCES TipoDocumento(Id)
);

-- Tabla de Tarifas de Vuelo
CREATE TABLE TarifaVuelo (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  Descripcion VARCHAR(100),
  Valor DECIMAL(10, 2)
);

-- Tabla de Detalles de Reserva
CREATE TABLE DetalleReserva (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  IdCliente_fk INT,
  IdTarifa_fk INT,
  IdReserva_fk INT,
  ValorTarifa DOUBLE,
  CONSTRAINT FK_DetalleCliente FOREIGN KEY (IdCliente_fk) REFERENCES Cliente(Id),
  CONSTRAINT FK_DetalleTarifa FOREIGN KEY (IdTarifa_fk) REFERENCES TarifaVuelo(Id),
  CONSTRAINT FK_DetalleReserva FOREIGN KEY (IdReserva_fk) REFERENCES ReservaViaje(Id)
);

-- Tabla de Escalas
CREATE TABLE Escala (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  IdTrayecto_fk INT,
  IdAvion_fk INT,
  IdGate_fk INT,
  IdAeropuerto_fk INT,
  NroVuelo VARCHAR(20),
  CONSTRAINT FK_TrayectoEscala FOREIGN KEY (IdTrayecto_fk) REFERENCES Trayecto(Id),
  CONSTRAINT FK_EscalaAvion FOREIGN KEY (IdAvion_fk) REFERENCES Avion(Id),
  CONSTRAINT FK_GateEscala FOREIGN KEY (IdGate_fk) REFERENCES Gate(Id),
  CONSTRAINT FK_AeropuertoEscala FOREIGN KEY (IdAeropuerto_fk) REFERENCES Aeropuerto(Id)
);

-- Tabla de Tripulación en Trayectos
CREATE TABLE TrayectoTripulacion (
  IdEmpleado_fk INT,
  IdEscala_fk INT,
  CONSTRAINT FK_EmpleadoTrayecto FOREIGN KEY (IdEmpleado_fk) REFERENCES Empleado(Id),
  CONSTRAINT FK_EscalaTrayecto FOREIGN KEY (IdEscala_fk) REFERENCES Escala(Id)
);

-- Tabla de Detalles de Revisión
CREATE TABLE DetalleRevision (
  Id INT AUTO_INCREMENT PRIMARY KEY,
  IdRevision_fk INT,
  Descripcion VARCHAR(50),
  FechaRev DATE,
  IdEmpleado_fk INT,
  CONSTRAINT FK_RevisionDetalle FOREIGN KEY (IdRevision_fk) REFERENCES Revision(Id),
  CONSTRAINT FK_EmpleadoDetalle FOREIGN KEY (IdEmpleado_fk) REFERENCES Empleado(Id)
);

