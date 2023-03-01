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
#### Write a query to extract from the driver table the first and last name into one column using the aliases for that column.
```
SELECT CONCAT(driver_f_name, ', ', driver_l_name) AS full_name FROM driver;
```
#### Write a query to select first fifteen records from a Patient table.
```
SELECT *FROM patient ORDER BY patient_id LIMIT 15;
```
#### Write a query to get all data from the Patient table in descending order by their first name. You should use aliases for presented columns.
```
SELECT CONCAT(patient_id, ' - ', ptn_f_name, ' ', ptn_l_name, ', ', ptn_gender) AS full_info FROM patient ORDER BY ptn_f_name DESC;
```
#### Write a single query to display first and last name and date of birth for all doctors who were born between 1980 and 1985.
```
SELECT dc_f_name, dc_l_name, dc_bith_of_date FROM doctor WHERE dc_bith_of_date >= '01/01/1980' AND dc_bith_of_date <= '12/31/1985';
```
#### Write a query to modify hospital status in a Hospitals table for hospitals, located in Beizhang.
```
UPDATE hospitals SET hospital_status = 'closed' WHERE town = 'Beizhang';
```
#### Write a query to get the number of town available in the Hospitals table.
```
SELECT COUNT(town) FROM hospitals WHERE hospital_status = 'AV';
```
#### Write a query to get the total price for all appointments.
```
SELECT SUM(b.bill_price) AS Total FROM appointment d, bill b WHERE d.bill_id = b.bill_id;
```
#### Write a query to get the maximum price for bills with “Flu” disease.
```
SELECT MAX(b.bill_price) AS maximum FROM appointment d, bill b, disease j WHERE d.bill_id = b.bill_id AND j.disease_name = 'Flu';
```
#### Write a query to get the bills’ minimum price for patient Corine Doherty.
```
SELECT MIN(b.bill_price) AS minimum FROM appointment d, bill b, patient j WHERE d.bill_id = b.bill_id AND d.patient_id = j.patient_id AND j.ptn_f_name = 'Corine' AND j.ptn_l_name = 'Doherty';
```
#### Write a query to get the average price for doctors with specialization_id = 18.
```
SELECT AVG(b.bill_price) AS average FROM appointment d, bill b, doctor j WHERE d.bill_id = b.bill_id AND d.doctor_id = j.doctor_id AND j.splztn_id = 18;
```
#### Write a query to get data from hospitals table excluding the duplicates values from the "town" column.
```
SELECT town FROM hospitals GROUP BY town;
```
#### Write a query to get the number of doctors working for each specialization_id.
```
SELECT b.spzltn_name, COUNT(d.splztn_id) FROM doctor d, specialization b WHERE d.splztn_id = b.splztn_id GROUP BY b.splztn_id; 
```
#### Write a query to display the age of doctors, if the age is less than 35, display the age group as young, from 35 to 60 as middle, from 60 as old.
```
SELECT COUNT(doctor_id) FILTER (WHERE (EXTRACT(YEAR FROM CURRENT_DATE) - EXTRACT(YEAR FROM dc_bith_of_date)) < 35) AS "Young",
    COUNT(doctor_id) FILTER (WHERE (EXTRAXT(YEAR FROM CURRENT_DATE) - EXTRACT(YEAR FROM dc_bith_of_date)) >= 35 
        AND (EXTRAXT(YEAR FROM CURRENT_DATE) - EXTRACT(YEAR FROM dc_bith_of_date)) <= 60) AS "Middle",
    COUNT(doctor_id) FILTER (WHERE (EXTRACT(YEAR FROM CURRENT_DATE) - EXTRACT(YEAR FROM dc_bith_of_date)) > 60) AS "Old"
FROM doctor;
```
#### Write a query to get the first and last name of the patients who has the letter 'n' as the fifth character and 'l' in the first name.
```
SELECT ptn_f_name, ptn_l_name FROM patient WHERE ptn_f_name LIKE '____n%' AND ptn_f_name LIKE '%l'; 
```
#### Write a query to display the last name of doctors whose name contain exactly seven characters.
```
SELECT dc_l_name, dc_f_name FROM doctor WHERE LENGTH(dc_f_name) = 7;
```
#### Write a query to get the difference between the highest and lowest bill’s price.
```
SELECT MAX(bill_price) - MIN(bill_price) FROM bill;
```
#### Write a query to get number of medicaments available for each hospital in descending order from hospital_medicaments table.
```
SELECT COUNT(b.medicament_id), d.hospital_name FROM hospital_medicaments b, hospitals d WHERE d.hospital_id = b.hospital_id GROUP BY d.hospital_id ORDER BY COUNT(b.medicaments_id) DESC; 
```
#### Write a single query to display the first and last name of patient whose bill price is higher than 13000.
```
SELECT b.ptn_f_name, b.ptn_l_name, j.bill_price FROM patient b, appointment d, bill j WHERE b.patient_id = d.patient_id AND d.bill_id = j.bill_id AND j.bill_price > 13000;
```
