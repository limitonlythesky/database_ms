# SQL queries in PostgreSQL
#### Creating tables for entities in database
```
CREATE TABLE Driver(
    	driver_id INT NOT NULL,
    	driver_f_name VARCHAR(255) NOT NULL,
 	    driver_I_name VARCHAR(255) NOT NULL,
    	PRIMARY KEY (driver_id)
);

CREATE TABLE Ambulance_service(
 	    ambulance_id INT NOT NULL,
 	    ambalunce_service_name VARCHAR(255) NOT NULL,
    	driver_id INT NOT NULL,
    	hospital_id INT NOT NULL, 
    	PRIMARY KEY (ambulance_id),
    	FOREIGN KEY (driver_id) REFERENCES Driver (driver_id),
    	FOREIGN KEY (hospital_id) REFERENCES Hospitals(hospital_id)
);

CREATE TABLE Hospitals (
 	    hospital_id INT NOT NULL,
    	hospital_name VARCHAR(255) NOT NULL, 
    	hospital_address VARCHAR(255) NOT NULL,
  	    town VARCHAR(255) NOT NULL, 
    	hospital_status VARCHAR(255) NOT NULL,
    	doctor_id INT NOT NULL,
    	PRIMARY KEY (hospital_id),
    	FOREIGN KEY (doctor_id) REFERENCES Doctor(doctor_id)
);

CREATE TABLE Medical_treatments ( 
    	medicament_id INT NOT NULL,     
    	medicament_name VARCHAR(255) NOT NULL,
    	hospital_id INT NOT NULL,
    	PRIMARY KEY (medicament_id),
    	FOREIGN KEY (hospital_id) REFERENCES Hospitals(hospital_id)
);

CREATE TABLE Specialization(
 	    specialization_id INT NOT NULL, 
    	specialization_name INT(255) NOT NULL,
    	PRIMARY KEY (specialization_id)
);

CREATE TABLE Doctor ( 
   	    doctor_id INT NOT NULL,
    	doctor_f_name VARCHAR(255) NOT NULL,
    	doctor_l_name VARCHAR(255) NOT NULL,
    	doctor_gender VARCHAR(255) NOT NULL, 
   	    doctor_birth_of_date DATE NOT NULL, 
    	specialization_id INT NOT NULL,
    	PRIMARY KEY (doctor_id),
    	FOREIGN KEY (specialization_id) REFERENCES Specialization(specialization_id)
);

CREATE TABLE Appointment(
    	appointment_id INT NOT NULL, 
    	appointment_date DATE NOT NULL,
    	doctor_id INT NOT NULL,
    	patient_id INT NOT NULL,
    	disease_id INT NOT NULL,
    	bill_id INT NOT NULL,
    	PRIMARY KEY (appointment_id),
   	  FOREIGN KEY (doctor_id) REFERENCES Doctor(doctor_id),
    	FOREIGN KEY (patient_id) REFERENCES Patient(patient_id),
    	FOREIGN KEY (disease_id) REFERENCES Disease(disease_id),
    	FOREIGN KEY (bill_id) REFERENCES Bill(bill_id)
);

CREATE TABLE Patient( 
    	patient_id INT NOT NULL,
    	patient_f_name VARCHAR(255) NOT NULL,
    	patient_l_name VARCHAR(255) NOT NULL,
    	patience_gender VARCHAR(255) NOT NULL,
    	PRIMARY KEY (patient_id)
);

CREATE TABLE Disease( 
    	disease_id INT NOT NULL,
    	disease_name VARCHAR(255) NOT NULL,
    	PRIMARY KEY (disease_id)
);

CREATE TABLE Bill(
   	    bill_id INT NOT NULL,
    	bill_price INT NOT NULL,
    	PRIMARY KEY (bill_id)
);

ALTER TABLE Ambulance_service RENAME TO Ambulance
ALTER TABLE Medical_treatments RENAME TO Medicaments

ALTER TABLE Ambulance DROP COLUMN ambulance_service_name
```
