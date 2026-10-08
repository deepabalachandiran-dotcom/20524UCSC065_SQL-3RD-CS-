CREATE TABLE hospital_patients (
    patient_id NUMBER PRIMARY KEY,
    patient_name VARCHAR2(50) NOT NULL,
    department VARCHAR2(30) NOT NULL,
    admission_date DATE NOT NULL,
    discharge_date DATE,
    bill_amount NUMBER(10,2) NOT NULL,
    payment_status VARCHAR2(20) NOT NULL
);
INSERT INTO hospital_patients VALUES
(101, 'Arun', 'Cardiology', TO_DATE('02/01/26','DD/MM/YY'), TO_DATE('07/01/26','DD/MM/YY'), 45000, 'Paid');

INSERT INTO hospital_patients VALUES
(102, 'Meena', 'Neurology', TO_DATE('03/01/26','DD/MM/YY'), TO_DATE('10/01/26','DD/MM/YY'), 62000, 'Pending');

INSERT INTO hospital_patients VALUES
(103, 'Karthik', 'Orthopedics', TO_DATE('05/01/26','DD/MM/YY'), TO_DATE('09/01/26','DD/MM/YY'), 38000, 'Paid');

INSERT INTO hospital_patients VALUES
(104, 'Divya', 'Pediatrics', TO_DATE('08/01/26','DD/MM/YY'), TO_DATE('12/01/26','DD/MM/YY'), 28000, 'Paid');

INSERT INTO hospital_patients VALUES
(105, 'Suresh', 'General Medicine', TO_DATE('10/01/26','DD/MM/YY'), TO_DATE('15/01/26','DD/MM/YY'), 22000, 'Pending');

INSERT INTO hospital_patients VALUES
(106, 'Priya', 'Cardiology', TO_DATE('12/01/26','DD/MM/YY'), TO_DATE('18/01/26','DD/MM/YY'), 55000, 'Paid');

INSERT INTO hospital_patients VALUES
(107, 'Rahul', 'Neurology', TO_DATE('15/01/26','DD/MM/YY'), TO_DATE('22/01/26','DD/MM/YY'), 70000, 'Pending');

INSERT INTO hospital_patients VALUES
(108, 'Anitha', 'Orthopedics', TO_DATE('18/01/26','DD/MM/YY'), TO_DATE('23/01/26','DD/MM/YY'), 42000, 'Paid');

INSERT INTO hospital_patients VALUES
(109, 'Vijay', 'Pediatrics', TO_DATE('20/01/26','DD/MM/YY'), TO_DATE('24/01/26','DD/MM/YY'), 30000, 'Paid');

INSERT INTO hospital_patients VALUES
(110, 'Nisha', 'General Medicine', TO_DATE('22/01/26','DD/MM/YY'), TO_DATE('28/01/26','DD/MM/YY'), 26000, 'Pending');

INSERT INTO hospital_patients VALUES
(111, 'Ramesh', 'Cardiology', TO_DATE('25/01/26','DD/MM/YY'), TO_DATE('31/01/26','DD/MM/YY'), 60000, 'Paid');

INSERT INTO hospital_patients VALUES
(112, 'Lakshmi', 'Neurology', TO_DATE('27/01/26','DD/MM/YY'), TO_DATE('02/02/26','DD/MM/YY'), 58000, 'Pending');

INSERT INTO hospital_patients VALUES
(113, 'Deepak', 'Orthopedics', TO_DATE('30/01/26','DD/MM/YY'), TO_DATE('05/02/26','DD/MM/YY'), 47000, 'Paid');

INSERT INTO hospital_patients VALUES
(114, 'Swetha', 'Pediatrics', TO_DATE('02/02/26','DD/MM/YY'), TO_DATE('06/02/26','DD/MM/YY'), 32000, 'Pending');

INSERT INTO hospital_patients VALUES
(115, 'Manoj', 'General Medicine', TO_DATE('04/02/26','DD/MM/YY'), TO_DATE('09/02/26','DD/MM/YY'), 24000, 'Paid');

INSERT INTO hospital_patients VALUES
(116, 'Harini', 'Cardiology', TO_DATE('07/02/26','DD/MM/YY'), TO_DATE('15/02/26','DD/MM/YY'), 68000, 'Pending');

INSERT INTO hospital_patients VALUES
(117, 'Gokul', 'Neurology', TO_DATE('10/02/26','DD/MM/YY'), TO_DATE('17/02/26','DD/MM/YY'), 72000, 'Paid');

INSERT INTO hospital_patients VALUES
(118, 'Keerthi', 'Orthopedics', TO_DATE('12/02/26','DD/MM/YY'), TO_DATE('18/02/26','DD/MM/YY'), 44000, 'Pending');

INSERT INTO hospital_patients VALUES
(119, 'Santhosh', 'Pediatrics', TO_DATE('15/02/26','DD/MM/YY'), TO_DATE('19/02/26','DD/MM/YY'), 29000, 'Paid');

INSERT INTO hospital_patients VALUES
(120, 'Pavithra', 'General Medicine', TO_DATE('18/02/26','DD/MM/YY'), TO_DATE('25/02/26','DD/MM/YY'), 27000, 'Pending');

INSERT INTO hospital_patients VALUES
(121, 'Mohan', 'Cardiology', TO_DATE('20/02/26','DD/MM/YY'), TO_DATE('27/02/26','DD/MM/YY'), 63000, 'Paid');

INSERT INTO hospital_patients VALUES
(122, 'Aishwarya', 'Neurology', TO_DATE('22/02/26','DD/MM/YY'), TO_DATE('01/03/26','DD/MM/YY'), 75000, 'Pending');

INSERT INTO hospital_patients VALUES
(123, 'Surya', 'Orthopedics', TO_DATE('25/02/26','DD/MM/YY'), TO_DATE('03/03/26','DD/MM/YY'), 49000, 'Paid');

INSERT INTO hospital_patients VALUES
(124, 'Janani', 'Pediatrics', TO_DATE('27/02/26','DD/MM/YY'), TO_DATE('02/03/26','DD/MM/YY'), 31000, 'Pending');

INSERT INTO hospital_patients VALUES
(125, 'Ashok', 'General Medicine', TO_DATE('01/03/26','DD/MM/YY'), TO_DATE('08/03/26','DD/MM/YY'), 25000, 'Paid');

COMMIT;
SELECT patient_id,
       patient_name,
       department,
       admission_date,
       bill_amount
FROM hospital_patients
ORDER BY patient_id;
SELECT department,
       COUNT(*) AS total_patients
FROM hospital_patients
GROUP BY department
ORDER BY department;
SELECT patient_id,
       patient_name,
       department,
       bill_amount,
       payment_status
FROM hospital_patients
WHERE payment_status = 'Pending'
ORDER BY patient_id;
SELECT department,
       SUM(bill_amount) AS total_billing_revenue
FROM hospital_patients
GROUP BY department
ORDER BY department;
SELECT patient_id,
       patient_name,
       admission_date,
       discharge_date,
       discharge_date - admission_date AS hospital_stay
FROM hospital_patients
WHERE discharge_date IS NOT NULL
ORDER BY hospital_stay DESC;
CREATE OR REPLACE PROCEDURE generate_patient_bill (
    p_patient_id IN NUMBER,
    p_service_charge IN NUMBER
)
IS
    v_bill NUMBER;
BEGIN
    SELECT bill_amount
    INTO v_bill
    FROM hospital_patients
    WHERE patient_id = p_patient_id;

    v_bill := v_bill + p_service_charge;

    UPDATE hospital_patients
    SET bill_amount = v_bill
    WHERE patient_id = p_patient_id;

    COMMIT;

    DBMS_OUTPUT.PUT_LINE('Patient ID: ' || p_patient_id);
    DBMS_OUTPUT.PUT_LINE('Updated Bill Amount: ' || v_bill);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Patient ID not found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
BEGIN
    generate_patient_bill(101, 5000);
END;
/
CREATE OR REPLACE FUNCTION calculate_stay_days (
    p_patient_id IN NUMBER
)
RETURN NUMBER
IS
    v_admission DATE;
    v_discharge DATE;
BEGIN
    SELECT admission_date, discharge_date
    INTO v_admission, v_discharge
    FROM hospital_patients
    WHERE patient_id = p_patient_id;

    IF v_discharge IS NULL THEN
        RETURN NULL;
    END IF;

    RETURN v_discharge - v_admission;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        RAISE_APPLICATION_ERROR(-20001, 'Patient ID not found');
END;
/
SELECT patient_id,
       patient_name,
       calculate_stay_days(patient_id) AS stay_days
FROM hospital_patients
WHERE patient_id = 101;
CREATE OR REPLACE PROCEDURE update_payment_status (
    p_patient_id IN NUMBER,
    p_new_status IN VARCHAR2
)
IS
    v_department hospital_patients.department%TYPE;
BEGIN

    SELECT department
    INTO v_department
    FROM hospital_patients
    WHERE patient_id = p_patient_id;

    UPDATE hospital_patients
    SET payment_status = p_new_status
    WHERE patient_id = p_patient_id;

    COMMIT;

    DBMS_OUTPUT.PUT_LINE('Payment status updated successfully.');
    DBMS_OUTPUT.PUT_LINE('Patient ID: ' || p_patient_id);
    DBMS_OUTPUT.PUT_LINE('Department: ' || v_department);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Patient ID not found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;
/
BEGIN
    update_payment_status(102, 'Paid');
END;
/
SELECT department,
       COUNT(*) AS total_patients,
       SUM(bill_amount) AS total_billing
FROM hospital_patients
GROUP BY department
ORDER BY department;
BEGIN
    generate_patient_bill(999, 5000);
END;
/
SELECT calculate_stay_days(999) AS stay_days
FROM dual;
BEGIN
    update_payment_status(999, 'Paid');
END;
/
