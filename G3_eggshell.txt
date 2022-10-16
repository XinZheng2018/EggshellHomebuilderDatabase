REM Drop tables

DROP TABLE agent CASCADE CONSTRAINTS;

DROP TABLE bank CASCADE CONSTRAINTS;

DROP TABLE buyer CASCADE CONSTRAINTS;

DROP TABLE constructionsheet CASCADE CONSTRAINTS;

DROP TABLE contract CASCADE CONSTRAINTS;

DROP TABLE decoratorchoice CASCADE CONSTRAINTS;

DROP TABLE elevation CASCADE CONSTRAINTS;

DROP TABLE employee CASCADE CONSTRAINTS;

DROP TABLE house CASCADE CONSTRAINTS;

DROP TABLE "House-Room" CASCADE CONSTRAINTS;

DROP TABLE lot CASCADE CONSTRAINTS;

DROP TABLE "Option" CASCADE CONSTRAINTS;

DROP TABLE room CASCADE CONSTRAINTS;

DROP TABLE school CASCADE CONSTRAINTS;

DROP TABLE schooldistrict CASCADE CONSTRAINTS;

DROP TABLE style CASCADE CONSTRAINTS;

DROP TABLE "Style-Elevation" CASCADE CONSTRAINTS;

DROP TABLE subdivision CASCADE CONSTRAINTS;

DROP TABLE "Subdivision-Style" CASCADE CONSTRAINTS;

DROP TABLE task CASCADE CONSTRAINTS;

DROP TABLE taskprogress CASCADE CONSTRAINTS;

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

REM Create tables

CREATE TABLE agent (
    agent_id   CHAR(5) NOT NULL,
    lname      VARCHAR2(20) NOT NULL,
    fname      VARCHAR2(20) NOT NULL,
    street     VARCHAR2(40) NOT NULL,
    city       VARCHAR2(20) NOT NULL,
    state      VARCHAR2(2) NOT NULL,
    zip        VARCHAR2(10) NOT NULL
);

ALTER TABLE agent ADD CONSTRAINT agent_pk PRIMARY KEY ( agent_id );

CREATE TABLE bank (
    bank_id     CHAR(5) NOT NULL,
    bank_name   VARCHAR2(20) NOT NULL,
    contact     VARCHAR2(50) NOT NULL,
    phone       VARCHAR2(50) NOT NULL,
    fax         VARCHAR2(50),
    street      VARCHAR2(40) NOT NULL,
    city        VARCHAR2(20) NOT NULL,
    state       VARCHAR2(2) NOT NULL,
    zip         VARCHAR2(10) NOT NULL
);

ALTER TABLE bank ADD CONSTRAINT bank_pk PRIMARY KEY ( bank_id );

CREATE TABLE buyer (
    buyer_id   CHAR(5) NOT NULL,
    lname      VARCHAR2(20) NOT NULL,
    fname      VARCHAR2(20) NOT NULL,
    street     VARCHAR2(40) NOT NULL,
    city       VARCHAR2(20) NOT NULL,
    state      VARCHAR2(2) NOT NULL,
    zip        VARCHAR2(10) NOT NULL
);

ALTER TABLE buyer ADD CONSTRAINT buyer_pk PRIMARY KEY ( buyer_id );

CREATE TABLE constructionsheet (
    construction_id        CHAR(5) NOT NULL,
    stage                  NUMBER NOT NULL,
    "date"                 DATE NOT NULL,
    est_complete_date      DATE,
    employee_id   CHAR(5),
    house_id         CHAR(5) NOT NULL
);

ALTER TABLE constructionsheet ADD CHECK ( stage BETWEEN 1 AND 7 );

ALTER TABLE constructionsheet ADD CONSTRAINT constructionsheet_pk PRIMARY KEY ( construction_id );

CREATE TABLE contract (
    contract_id            CHAR(5) NOT NULL,
    house_id         CHAR(5) NOT NULL,
    buyer_id         CHAR(5) NOT NULL,
    employee_id   CHAR(5),
    financing_method       VARCHAR2(50) NOT NULL,
    escrow_deposit         NUMBER(10, 2) NOT NULL,
    agent_id         CHAR(5) NOT NULL,
    bank_id           CHAR(5) NOT NULL,
    est_complete_date      DATE,
    signed_date            DATE,
    copy_agreement         CHAR(1) NOT NULL,
    copy_disclosure        CHAR(1) NOT NULL,
    copy_contract          CHAR(1) NOT NULL
);

CREATE UNIQUE INDEX contract__idx ON
    contract (
        house_id
    ASC );

ALTER TABLE contract ADD CONSTRAINT contract_pk PRIMARY KEY ( contract_id );

CREATE TABLE decoratorchoice (
    choice_id              CHAR(5) NOT NULL,
    house_id         CHAR(5),
    employee_id   CHAR(5),
    employee_date          DATE NOT NULL,
    buyer_id         CHAR(5),
    signed_date            DATE,
    option_id       CHAR(5),
    item_desc              VARCHAR2(100),
    item_price             NUMBER(10, 2) NOT NULL,
    room_id           CHAR(5)
);

ALTER TABLE decoratorchoice ADD CONSTRAINT decoratorchoice_pk PRIMARY KEY ( choice_id );

CREATE TABLE elevation (
    elevation_name           VARCHAR2(20) NOT NULL,
    elevation_desc           VARCHAR2(100),
    additional_cost_sketch   NUMBER(10, 2)
);

ALTER TABLE elevation ADD CONSTRAINT elevation_pk PRIMARY KEY ( elevation_name );

CREATE TABLE employee (
    employee_id   CHAR(5) NOT NULL,
    lname         VARCHAR2(20) NOT NULL,
    fname         VARCHAR2(20) NOT NULL,
    title         VARCHAR2(50),
    license_no    VARCHAR2(4)
);

ALTER TABLE employee ADD CONSTRAINT employee_pk PRIMARY KEY ( employee_id );

CREATE TABLE house (
    house_id               CHAR(5) NOT NULL,
    base_price             NUMBER(10, 2) NOT NULL,
    floor_plan             VARCHAR2(50),
    house_size             NUMBER(10, 2),
    photo                  VARCHAR2(50),
    primary_sketches       CHAR(1) NOT NULL
);


ALTER TABLE house ADD CONSTRAINT house_pk PRIMARY KEY ( house_id );

CREATE TABLE "House-Room" (
    house_id   CHAR(5) NOT NULL,
    room_id     CHAR(5) NOT NULL,
    floor            NUMBER NOT NULL,
    "size"           NUMBER(10, 2) NOT NULL,
    comments         VARCHAR2(50),
    num_window       NUMBER NOT NULL,
    ceiling_desc     VARCHAR2(100)
);

ALTER TABLE "House-Room" ADD CONSTRAINT "House-Room_PK" PRIMARY KEY ( house_id,
                                                                      room_id );

CREATE TABLE lot (
    lot_id                       CHAR(5) NOT NULL,
    street                       VARCHAR2(40) NOT NULL,
    city                         VARCHAR2(20) NOT NULL,
    state                        VARCHAR2(2) NOT NULL,
    zip                          VARCHAR2(10) NOT NULL,
    latitude                     NUMBER(10, 2) NOT NULL,
    longitude                    NUMBER(10, 2) NOT NULL,
    subdivision_id   CHAR(5) NOT NULL,
    style_name             VARCHAR2(20) NOT NULL,
    elevation_name     VARCHAR2(20) NOT NULL,
    house_id               CHAR(5)
);

CREATE UNIQUE INDEX lot__idx ON
    lot (
        house_id
    ASC );

ALTER TABLE lot ADD CONSTRAINT lot_pk PRIMARY KEY ( lot_id );

CREATE TABLE "Option" (
    option_id          CHAR(5) NOT NULL,
    option_no          VARCHAR2(4) NOT NULL,
    option_desc        VARCHAR2(100) NOT NULL,
    stage              NUMBER NOT NULL,
    cost               NUMBER(10, 2) NOT NULL,
    category           VARCHAR2(20) NOT NULL,
    style_name   VARCHAR2(20) NOT NULL,
    revision_date      DATE
);

ALTER TABLE "Option" ADD CHECK ( stage BETWEEN 1 AND 7 );

ALTER TABLE "Option" ADD CONSTRAINT option_pk PRIMARY KEY ( option_id );

CREATE TABLE room (
    room_id     CHAR(5) NOT NULL,
    room_name   VARCHAR2(20)
);

ALTER TABLE room ADD CONSTRAINT room_pk PRIMARY KEY ( room_id );

CREATE TABLE school (
    school_id                           CHAR(5) NOT NULL, 
    school_district_id   CHAR(5) NOT NULL,
    school_name                         VARCHAR2(20),
    school_type                         VARCHAR2(20)
);

ALTER TABLE school ADD CONSTRAINT school_pk PRIMARY KEY ( school_id );

CREATE TABLE schooldistrict (
    school_district_id   CHAR(5) NOT NULL
);

ALTER TABLE schooldistrict ADD CONSTRAINT schooldistrict_pk PRIMARY KEY ( school_district_id );

CREATE TABLE style (
    style_name   VARCHAR2(20) NOT NULL
);

ALTER TABLE style ADD CONSTRAINT style_pk PRIMARY KEY ( style_name );

CREATE TABLE "Style-Elevation" (
    style_name           VARCHAR2(20) NOT NULL,
    elevation_name   VARCHAR2(20) NOT NULL
);

ALTER TABLE "Style-Elevation" ADD CONSTRAINT "Style-Elevation_PK" PRIMARY KEY ( style_name,
                                                                                elevation_name );

CREATE TABLE subdivision (
    subdivision_id                      CHAR(5) NOT NULL,
    subdivision_name                    VARCHAR2(20) NOT NULL,
    map                                 VARCHAR2(50) NOT NULL, 
    school_district_id   CHAR(5) NOT NULL
);

ALTER TABLE subdivision ADD CONSTRAINT subdivision_pk PRIMARY KEY ( subdivision_id );

CREATE TABLE "Subdivision-Style" (
    subdivision_id   CHAR(5) NOT NULL,
    style_name             VARCHAR2(20) NOT NULL
);

ALTER TABLE "Subdivision-Style" ADD CONSTRAINT "Subdivision-Style_PK" PRIMARY KEY ( style_name,
                                                                                    subdivision_id );

CREATE TABLE task (
    task_id     CHAR(5) NOT NULL,
    task_desc   VARCHAR2(100)
);

ALTER TABLE task ADD CONSTRAINT task_pk PRIMARY KEY ( task_id );

CREATE TABLE taskprogress ( 
    construction_id   CHAR(5) NOT NULL,
    task_id                        CHAR(5) NOT NULL,
    percent_complete                    NUMBER(10, 2) NOT NULL
);

ALTER TABLE taskprogress ADD CONSTRAINT taskprogress_pk PRIMARY KEY ( construction_id,
                                                                      task_id );

ALTER TABLE constructionsheet
    ADD CONSTRAINT constructionsheet_employee_fk FOREIGN KEY ( employee_id )
        REFERENCES employee ( employee_id );

ALTER TABLE constructionsheet
    ADD CONSTRAINT constructionsheet_house_fk FOREIGN KEY ( house_id )
        REFERENCES house ( house_id );

ALTER TABLE contract
    ADD CONSTRAINT contract_agent_fk FOREIGN KEY ( agent_id )
        REFERENCES agent ( agent_id );

ALTER TABLE contract
    ADD CONSTRAINT contract_bank_fk FOREIGN KEY ( bank_id )
        REFERENCES bank ( bank_id );

ALTER TABLE contract
    ADD CONSTRAINT contract_buyer_fk FOREIGN KEY ( buyer_id )
        REFERENCES buyer ( buyer_id );

ALTER TABLE contract
    ADD CONSTRAINT contract_employee_fk FOREIGN KEY ( employee_id )
        REFERENCES employee ( employee_id );

ALTER TABLE contract
    ADD CONSTRAINT contract_house_fk FOREIGN KEY ( house_id )
        REFERENCES house ( house_id );

ALTER TABLE decoratorchoice
    ADD CONSTRAINT decoratorchoice_buyer_fk FOREIGN KEY ( buyer_id )
        REFERENCES buyer ( buyer_id );

ALTER TABLE decoratorchoice
    ADD CONSTRAINT decoratorchoice_employee_fk FOREIGN KEY ( employee_id )
        REFERENCES employee ( employee_id );

ALTER TABLE decoratorchoice
    ADD CONSTRAINT decoratorchoice_house_fk FOREIGN KEY ( house_id )
        REFERENCES house ( house_id );

ALTER TABLE decoratorchoice
    ADD CONSTRAINT decoratorchoice_option_fk FOREIGN KEY ( option_id )
        REFERENCES "Option" ( option_id );

ALTER TABLE decoratorchoice
    ADD CONSTRAINT decoratorchoice_room_fk FOREIGN KEY ( room_id )
        REFERENCES room ( room_id );


ALTER TABLE "House-Room"
    ADD CONSTRAINT "House-Room_House_FK" FOREIGN KEY ( house_id )
        REFERENCES house ( house_id );

ALTER TABLE "House-Room"
    ADD CONSTRAINT "House-Room_Room_FK" FOREIGN KEY ( room_id )
        REFERENCES room ( room_id );

ALTER TABLE lot
    ADD CONSTRAINT lot_elevation_fk FOREIGN KEY ( elevation_name )
        REFERENCES elevation ( elevation_name );

ALTER TABLE lot
    ADD CONSTRAINT lot_house_fk FOREIGN KEY ( house_id )
        REFERENCES house ( house_id );

ALTER TABLE lot
    ADD CONSTRAINT lot_style_fk FOREIGN KEY ( style_name )
        REFERENCES style ( style_name );

ALTER TABLE lot
    ADD CONSTRAINT lot_subdivision_fk FOREIGN KEY ( subdivision_id )
        REFERENCES subdivision ( subdivision_id );

ALTER TABLE "Option"
    ADD CONSTRAINT option_style_fk FOREIGN KEY ( style_name )
        REFERENCES style ( style_name );

ALTER TABLE school
    ADD CONSTRAINT school_schooldistrict_fk FOREIGN KEY ( school_district_id )
        REFERENCES schooldistrict ( school_district_id );

ALTER TABLE "Style-Elevation"
    ADD CONSTRAINT "Style-Elevation_Elevation_FK" FOREIGN KEY ( elevation_name )
        REFERENCES elevation ( elevation_name );

ALTER TABLE "Style-Elevation"
    ADD CONSTRAINT "Style-Elevation_Style_FK" FOREIGN KEY ( style_name )
        REFERENCES style ( style_name );

ALTER TABLE subdivision
    ADD CONSTRAINT subdivision_schooldistrict_fk FOREIGN KEY ( school_district_id )
        REFERENCES schooldistrict ( school_district_id );

ALTER TABLE "Subdivision-Style"
    ADD CONSTRAINT "Subdivision-Style_Style_FK" FOREIGN KEY ( style_name )
        REFERENCES style ( style_name );


ALTER TABLE "Subdivision-Style"
    ADD CONSTRAINT "Subdivision-Style_Subdivision_FK" FOREIGN KEY ( subdivision_id )
        REFERENCES subdivision ( subdivision_id );


ALTER TABLE taskprogress
    ADD CONSTRAINT taskprogress_constructionsheet_fk FOREIGN KEY ( construction_id )
        REFERENCES constructionsheet ( construction_id );

ALTER TABLE taskprogress
    ADD CONSTRAINT taskprogress_task_fk FOREIGN KEY ( task_id )
        REFERENCES task ( task_id );

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
REM The following constraints are made according to our business rule

-- 1. House price >= $10000
ALTER TABLE house
ADD CONSTRAINT CHK_base_price
CHECK (base_price >= 10000);

-- 2. House size >= 50 and <10000
ALTER TABLE house
ADD CONSTRAINT CHK_house_size
CHECK (house_size >= 50 AND house_size<10000);

-- 3. Room size >= 5 and <=200
ALTER TABLE "House-Room"
ADD CONSTRAINT CHK_room_size
CHECK ("size" >= 5 AND "size"<=200);

-- 4. Elevation cost >=0 and <100000
ALTER TABLE elevation
ADD CONSTRAINT CHK_elevation_cost
CHECK (additional_cost_sketch >= 0 AND additional_cost_sketch <100000);

-- 5. Stage can only be integer number 1 to 7
-- stage datatype is already defined in create table clause
ALTER TABLE "Option"
ADD CONSTRAINT CHK_option_stage
CHECK (stage BETWEEN 1 AND 7);

-- 6. Option cost >0 and <100000000
ALTER TABLE "Option"
ADD CONSTRAINT CHK_option_cost
CHECK (cost > 0 AND cost <100000000);

-- 7. The date when employee created decorator choice should between 2005-01-01 and 2022-10-10
ALTER TABLE decoratorchoice
ADD CONSTRAINT CHK_employee_date
CHECK (employee_date BETWEEN '01-JAN-2005' AND '10-OCT-2022');

-- 8. The date when the client signed decortorchoice should between 2005-01-01 and 2022-10-10
ALTER TABLE decoratorchoice
ADD CONSTRAINT CHK_decorator_signed
CHECK (signed_date BETWEEN '01-JAN-2005' AND '10-OCT-2022');

-- 9. The decorator item price should between 0 and 100000000
ALTER TABLE decoratorchoice
ADD CONSTRAINT CHK_item_price
CHECK (item_price > 0 AND item_price <100000000);

-- 10. The escrow deposit in contract should between 10000 and 100000000
ALTER TABLE contract
ADD CONSTRAINT CHK_escrow_deposit
CHECK (escrow_deposit >= 10000 AND escrow_deposit <100000000);

-- 11. The contract sign date should between 2005-01-01 and 2022-10-10
ALTER TABLE contract
ADD CONSTRAINT CHK_contract_signed
CHECK (signed_date BETWEEN '01-JAN-2005' AND '10-OCT-2022');

-- 12. The stage in construction sheet can only be integer number 1 to 7
-- stage datatype is already defined in create table clause
ALTER TABLE constructionsheet
ADD CONSTRAINT CHK_construction_stage
CHECK (stage BETWEEN 1 AND 7);

-- 13. The construction sheet filled date should between 2005-01-01 and 2022-10-10
ALTER TABLE constructionsheet
ADD CONSTRAINT CHK_contraction_filled
CHECK ("date" BETWEEN '01-JAN-2005' AND '10-OCT-2022');

-- 14. Task progress should between 0 to 100 (100%)
ALTER TABLE taskprogress
ADD CONSTRAINT CHK_task_progress
CHECK (percent_complete BETWEEN 0 AND 100);


        
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

REM Populate tables

-- inserting records into Agent

INSERT INTO Agent VALUES
('EA001', 'Priscilla', 'Davies', '44 Spruce Drive', 'Pittsburgh', 'PA', '15212');
INSERT INTO Agent VALUES
('EA002', 'Esha', 'Ayers', '4140 Pine Street', 'Pittsburgh', 'PA', '15219');
INSERT INTO Agent VALUES
('EA003', 'Catriona', 'Hahn', '4529 Cantebury Drive', 'New York', 'NY', '10038');
INSERT INTO Agent VALUES
('EA004', 'Vanesa', 'Lyon', '3245 Tenmile Road', 'Boston', 'MA', '02210');
INSERT INTO Agent VALUES
('EA005', 'Isis', 'Lister', '1338 Locust View Drive', 'San Francisco', 'CA', '94108');



-- inserting records into Bank

INSERT INTO Bank VALUES
('BA001', 'Bank of America', 'Glenn Irwin', '800-432-1000', '800-432-1000', '100 N Tryon St', 'Charlotte', 'NC', '28202');
INSERT INTO Bank VALUES
('BA002', 'Wells Fargo', 'Jacob Salter', '800-869-3557', '800-869-3557', '420 Montgomery St', 'San Francisco', 'CA', '94104');
INSERT INTO Bank VALUES
('BA003', 'PNC Bank', 'Levi Bernard', '800-272-6868', '800-272-6868', '249 Fifth Ave', 'Pittsburgh', 'PA', '15222');
INSERT INTO Bank VALUES
('BA004', 'TD Bank', 'Randall Gibbons', '800-457-2387', '800-457-2387', '357 Kings Hwy N', 'Cherry Hill', 'NJ', '08034');
INSERT INTO Bank VALUES
('BA005', 'Chase', 'Rodney Sanchez', '800-935-9935', '800-935-9935', '270 Park Avenue', 'New York', 'NY', '10017');





-- inserting records into Buyer

INSERT INTO Buyer VALUES
('B0001', 'Blake', 'Gillespie', '1364 Platinum Drive', 'Pittsburgh', 'PA', '15219');
INSERT INTO Buyer VALUES
('B0002', 'Gage', 'Bravo', '4691 Shinn Avenue', 'Pittsburgh', 'PA', '15212');
INSERT INTO Buyer VALUES
('B0003', 'Yasir', 'Hurst', '3817 Beechwood Drive', 'Pittsburgh', 'PA', '15222');
INSERT INTO Buyer VALUES
('B0004', 'Tj', 'Anderson', '2179 Beechwood Drive', 'Pittsburgh', 'PA', '15222');
INSERT INTO Buyer VALUES
('B0005', 'Ingrid', 'Tierney', '4437 Lady Bug Drive', 'New York', 'NY', '10014');
INSERT INTO Buyer VALUES
('B0006', 'Antony', 'Fenton', '1368 Morningview Lane', 'New York', 'NY', '10013');
INSERT INTO Buyer VALUES
('B0007', 'Arsalan', 'Caldwell', '1272 Lynn St', 'Boston', 'MA', '02114');
INSERT INTO Buyer VALUES
('B0008', 'Tatiana', 'Barajas', '3235 Ferguson St', 'Boston', 'MA', '02210');
INSERT INTO Buyer VALUES
('B0009', 'Jethro', 'Cowan', '429 Locust View Drive', 'San Francisco', 'CA', '94108');
INSERT INTO Buyer VALUES
('B0010', 'Eleni', 'Aldred', '3824 Harrison St', 'San Francisco', 'CA', '94143');



-- inserting records into Employee

INSERT INTO Employee VALUES
('E0001', 'Alexander', 'Barlow', 'Sales Agent', '0001');
INSERT INTO Employee VALUES
('E0002', 'Dora', 'Downs', 'Construction Manager', '0002');
INSERT INTO Employee VALUES
('E0003', 'Cruz', 'Goddard', 'Construction Manager', '0003');
INSERT INTO Employee VALUES
('E0004', 'Jessie', 'Paterson', 'Assistant', '0004');
INSERT INTO Employee VALUES
('E0005', 'Jorden', 'Samuels', 'Sales Agent', '0005');
INSERT INTO Employee VALUES
('E0006', 'Ayse', 'Benitez', 'Construction Manager', '0006');
INSERT INTO Employee VALUES
('E0007', 'Yassin', 'Drummond', 'Sales Agent', '0007');
INSERT INTO Employee VALUES
('E0008', 'Gavin', 'Tait', 'Construction Manager', '0008');
INSERT INTO Employee VALUES
('E0009', 'Zidane', 'Sadler', 'Sales Agent', '0009');
INSERT INTO Employee VALUES
('E0010', 'Cleo', 'Harrell', 'Construction Manager', '0010');



--style table
INSERT INTO style VALUES
('Modern');
INSERT INTO style VALUES
('Renaissance');
INSERT INTO style VALUES
('Mediterranean');

--elevation table
INSERT INTO elevation VALUES
('A', 'base design', 0.00);
INSERT INTO elevation VALUES
('B', 'One more window', 5000.00);
INSERT INTO elevation VALUES
('C', 'Different type of roof', 15000.00);


--style-elevation table
INSERT INTO "Style-Elevation" VALUES
('Modern', 'A');
INSERT INTO "Style-Elevation" VALUES
('Modern', 'B');
INSERT INTO "Style-Elevation" VALUES
('Renaissance', 'B');
INSERT INTO "Style-Elevation" VALUES
('Renaissance', 'C');
INSERT INTO "Style-Elevation" VALUES
('Mediterranean', 'A');
INSERT INTO "Style-Elevation" VALUES
('Mediterranean', 'B');
INSERT INTO "Style-Elevation" VALUES
('Mediterranean', 'C');


--school district table
INSERT INTO schooldistrict VALUES
('SD001');
INSERT INTO schooldistrict VALUES
('SD002');
INSERT INTO schooldistrict VALUES
('SD003');
INSERT INTO schooldistrict VALUES
('SD004');


--school table
INSERT INTO school VALUES
('SC001', 'SD001', 'Linden School', 'elementary school');
INSERT INTO school VALUES
('SC002', 'SD001', 'South Hill School', 'middle school');
INSERT INTO school VALUES
('SC003', 'SD002', 'Edgewood School', 'elementary school');
INSERT INTO school VALUES
('SC004', 'SD002', 'East Side School', 'middle school');
INSERT INTO school VALUES
('SC005', 'SD002', 'Pace High School', 'high school');
INSERT INTO school VALUES
('SC006', 'SD003', 'Liberty School', 'elementary school');
INSERT INTO school VALUES
('SC007', 'SD004', 'Kelly School', 'elementary school');


--subdivision table
INSERT INTO subdivision VALUES
('S0001','Rolling Hills','https://www.eggshellhome.com/picture=s0001','SD001');
INSERT INTO subdivision VALUES
('S0002', 'Palm Springs', 'https://www.eggshellhome.com/picture=s0002', 'SD001');
INSERT INTO subdivision VALUES
('S0003','Alexander Woods','https://www.eggshellhome.com/picture=s0003','SD002');
INSERT INTO subdivision VALUES
('S0004','Beverley Woods','https://www.eggshellhome.com/picture=s0004','SD003');
INSERT INTO subdivision VALUES
('S0005','Victory Lakes','https://www.eggshellhome.com/picture=s0005','SD004');


--subdivision-style table
INSERT INTO "Subdivision-Style" VALUES
('S0001', 'Modern');
INSERT INTO "Subdivision-Style" VALUES
('S0002', 'Renaissance');
INSERT INTO "Subdivision-Style" VALUES
('S0002', 'Mediterranean');
INSERT INTO "Subdivision-Style" VALUES
('S0003', 'Modern');
INSERT INTO "Subdivision-Style" VALUES
('S0003', 'Renaissance');
INSERT INTO "Subdivision-Style" VALUES
('S0003', 'Mediterranean');
INSERT INTO "Subdivision-Style" VALUES
('S0004', 'Modern');
INSERT INTO "Subdivision-Style" VALUES
('S0004', 'Renaissance');
INSERT INTO "Subdivision-Style" VALUES
('S0004', 'Mediterranean');
INSERT INTO "Subdivision-Style" VALUES
('S0005', 'Modern');
INSERT INTO "Subdivision-Style" VALUES
('S0005', 'Renaissance');


--Option

INSERT INTO "Option" VALUES
('O0001', '1027', 'Wire for ceiling fan', 7, 350.00, 'Electrical', 'Modern', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0002', '1028', 'Phone jack', 4, 250.00, 'Electrical', 'Modern', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0003', '1028', 'Phone jack', 7, 250.00, 'Electrical', 'Renaissance', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0004', '1029', 'Electric Outlet', 4, 400.00, 'Electrical', 'Mediterranean', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0005', '1029', 'Electric Outlet', 7, 400.00, 'Electrical', 'Modern', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0006', '2010', 'Sink in garage', 1, 450.00, 'Plumbing', 'Modern', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0007', '2010', 'Sink in garage', 1, 650.00, 'Plumbing', 'Renaissance', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0008', '2010', 'Sink in garage', 7, 550.00, 'Plumbing', 'Modern', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0009', '2011', 'Fixture upgrade', 1, 350.00, 'Plumbing', 'Mediterranean', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0010', '2011', 'Fixture upgrade', 1, 350.00, 'Plumbing', 'Renaissance', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0011', '2011', 'Fixture upgrade', 7, 650.00, 'Plumbing', 'Modern', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0012', '4114', 'Carpet level-4', 7, 800.00, 'Interior', 'Renaissance', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0013', '4116', 'Carpet level-6', 7, 1200.00, 'Interior', 'Mediterranean', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0014', '4117', 'Cabinet Upgrade', 7, 2000.00, 'Interior', 'Modern', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0015', '4118', 'Kitchen Countertop', 7, 3000.00, 'Interior', 'Renaissance', '25-Oct-2000');
INSERT INTO "Option" VALUES
('O0016', '4114', 'Carpet level-4', 7, 800.00, 'Interior', 'Mediterranean', '25-Oct-2000');



--house table
INSERT INTO house VALUES
('H0001', 850000.00, NULL, 510.00, NULL, 1);
INSERT INTO house VALUES
('H0002', 1395000.00, NULL, 1200.00,  NULL, 1);
INSERT INTO house VALUES
('H0003', 875000.00, NULL,800.00, NULL,0);
INSERT INTO house VALUES
('H0004', 1200000.00, NULL,1000.00, NULL, 1);
INSERT INTO house VALUES
('H0005', 1400000.00, NULL,1353.00,  NULL,1);
INSERT INTO house VALUES
('H0006', 1189000.00, NULL,1131.00, NULL,0);
INSERT INTO house VALUES
('H0007', 775000.00, NULL,550.00, NULL,1);
INSERT INTO house VALUES
('H0008', 949000.00, NULL,800.00, NULL,0);
INSERT INTO house VALUES
('H0009', 990000.00, NULL,1000.00, NULL,1);
INSERT INTO house VALUES
('H0010', 549500.00, NULL,500.00, NULL,1);


-- inserting records into Contract

INSERT INTO Contract VALUES
('C0001', 'H0001', 'B0001', 'E0001', 'Portfolio Loan', 850000.00, 'EA001', 'BA003', '09-Sep-2022', '10-Sep-2021', 1, 1, 1);
INSERT INTO Contract VALUES
('C0002', 'H0002', 'B0002', 'E0001', 'Conventional Loan', 1395000.00, 'EA001', 'BA001', '10-Oct-2022', '11-Oct-2021', 1, 1, 1);
INSERT INTO Contract VALUES
('C0003', 'H0003', 'B0003', 'E0001', 'Hard Money Loan', 875000.00, 'EA002', 'BA003', '10-Nov-2022', '11-Nov-2021', 1, 1, 1);
INSERT INTO Contract VALUES
('C0004', 'H0004', 'B0004', 'E0001', 'Blanket Loan', 1200000.00, 'EA002', 'BA001', '11-Dec-2022', '12-Dec-2021', 1, 1, 1);
INSERT INTO Contract VALUES
('C0005', 'H0005', 'B0005', 'E0005', '80/15/5', 1400000.00, 'EA003', 'BA005', '11-Jan-2023', '12-Jan-2022', 1, 1, 1);
INSERT INTO Contract VALUES
('C0006', 'H0006', 'B0006', 'E0005', '80/15/5', 1189000.00, 'EA003', 'BA005', '11-Feb-2023', '12-Feb-2022', 1, 1, 1);
INSERT INTO Contract VALUES
('C0007', 'H0007', 'B0007', 'E0007', 'Blanket Loan', 775000.00, 'EA004', 'BA004', '14-Mar-2023', '15-Mar-2022',1, 1, 1);
INSERT INTO Contract VALUES
('C0008', 'H0008', 'B0008', 'E0007', 'Hard Money Loan', 949000.00, 'EA004', 'BA004', '14-Apr-2023', '15-Apr-2022', 1, 1, 1);
INSERT INTO Contract VALUES
('C0009', 'H0009', 'B0009', 'E0009', 'Conventional Loan', 990000.00, 'EA005', 'BA002', '15-May-2023', '16-May-2022', 1, 1, 1);
INSERT INTO Contract VALUES
('C0010', 'H0010', 'B0010', 'E0009', 'Portfolio Loan', 549500.00, 'EA005', 'BA002', '15-Jun-2023', '16-Jun-2022', 1, 1, 1);



--room table
INSERT INTO room VALUES
('R0001', 'Living Room');
INSERT INTO room VALUES
('R0002', 'Master Bedroom');
INSERT INTO room VALUES
('R0003', 'Bedroom');
INSERT INTO room VALUES
('R0004', 'Kitchen');
INSERT INTO room VALUES
('R0005', 'Bathroom 1');
INSERT INTO room VALUES
('R0006', 'Bathroom 2');
INSERT INTO room VALUES
('R0007', 'Study');

--house-room table
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0001', 'R0001', 1, 200, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0001', 'R0002', 1, 170, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0001', 'R0005', 1, 60, 0);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0002', 'R0001', 1, 200, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0002', 'R0002', 1, 180, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0002', 'R0003', 2, 150, 0);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0002', 'R0007', 2, 110, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0003', 'R0001', 1, 200, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0003', 'R0002', 2, 200, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0003', 'R0004', 1, 80, 0);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0003', 'R0005', 2, 80, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0004', 'R0001', 1, 200, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0004', 'R0002', 2, 190, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0004', 'R0003', 2, 120, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0004', 'R0004', 1, 80, 0);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0004', 'R0005', 2, 80, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0005', 'R0001', -1, 200, 3);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0005', 'R0002', 1, 180, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0005', 'R0003', 2, 120, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0005', 'R0004', 1, 80, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0005', 'R0005', 1, 60, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0005', 'R0007', 2, 110, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0006', 'R0001', 1, 190, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0006', 'R0002', 2, 170, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0006', 'R0004', 2, 90, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0006', 'R0005', 1, 80, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0006', 'R0006', 2, 50, 0);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0006', 'R0007', 3, 120, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0007', 'R0002', 1, 200, 3);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0007', 'R0004', 1, 100, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0007', 'R0005', 1, 80, 0);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0008', 'R0001', 1, 200, 3);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0008', 'R0002', 2, 170, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0008', 'R0005', 2, 70, 0);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0009', 'R0001', 1, 200, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0009', 'R0002', 2, 180, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0009', 'R0003', 2, 150, 1);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0009', 'R0005', 1, 60, 0);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0010', 'R0002', 1, 180, 2);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0010', 'R0005', 1, 60, 0);
INSERT INTO "House-Room"(house_id, room_id, floor, "size", num_window) VALUES
('H0010', 'R0007', 2, 110, 1);


--decorator choice table
INSERT INTO DecoratorChoice VALUES
('DC001', 'H0001', 'E0001', '09-Sep-2021', 'B0001', '11-Sep-2021', 'O0002', 'Phone jack', 350, 'R0001');
INSERT INTO DecoratorChoice VALUES
('DC002', 'H0001', 'E0001', '14-Oct-2021', 'B0001', '18-Oct-2021', 'O0005', 'Electric Outlet', 430, 'R0002');
INSERT INTO DecoratorChoice VALUES
('DC003', 'H0003', 'E0001', '13-Nov-2021', 'B0003', '15-Nov-2021', 'O0004', 'Electric Outlet', 500, 'R0002');
INSERT INTO DecoratorChoice VALUES
('DC004', 'H0004', 'E0001', '11-Dec-2021', 'B0004', '18-Dec-2021', 'O0009', 'Fixture upgrade', 450, 'R0004');
INSERT INTO DecoratorChoice VALUES
('DC005', 'H0005', 'E0005', '12-Jan-2022', 'B0005', '13-Jan-2022', 'O0006', 'Sink in garage', 480, 'R0005');
INSERT INTO DecoratorChoice VALUES
('DC006', 'H0006', 'E0005', '21-Feb-2022', 'B0006', '23-Feb-2022', 'O0010', 'Fixture upgrade', 450, 'R0001');
INSERT INTO DecoratorChoice VALUES
('DC007', 'H0002', 'E0001', '15-Oct-2021', 'B0002', '18-Oct-2021', 'O0003', 'Phone jack', 400, 'R0002');
INSERT INTO DecoratorChoice VALUES
('DC008', 'H0010', 'E0009', '20-Jun-2022', 'B0010', '22-Jun-2022', 'O0007', 'Sink in garage', 480, 'R0002');



-- inserting records into Construction Sheet

INSERT INTO ConstructionSheet VALUES
('CS001', 1, '01-Sep-2021', '22-Oct-2021', 'E0002', 'H0001');
INSERT INTO ConstructionSheet VALUES
('CS002', 2, '23-Oct-2021', '13-Dec-2021', 'E0002', 'H0001');
INSERT INTO ConstructionSheet VALUES
('CS003', 3, '14-Dec-2021', '03-Feb-2022', 'E0002', 'H0001');
INSERT INTO ConstructionSheet VALUES
('CS004', 4, '04-Feb-2022', '27-Mar-2022', 'E0002', 'H0001');
INSERT INTO ConstructionSheet VALUES
('CS005', 5, '28-Mar-2022', '18-May-2022', 'E0002', 'H0001');
INSERT INTO ConstructionSheet VALUES
('CS006', 6, '19-May-2022', '09-Jul-2022', 'E0002', 'H0001');
INSERT INTO ConstructionSheet VALUES
('CS007', 7, '10-Jul-2022', '30-Aug-2022', 'E0002', 'H0001');
INSERT INTO ConstructionSheet VALUES
('CS008', 1, '11-Oct-2021', '01-Dec-2021', 'E0003', 'H0002');
INSERT INTO ConstructionSheet VALUES
('CS009', 2, '02-Dec-2021', '22-Jan-2022', 'E0003', 'H0002');
INSERT INTO ConstructionSheet VALUES
('CS010', 3, '23-Jan-2022', '15-Mar-2022', 'E0003', 'H0002');
INSERT INTO ConstructionSheet VALUES
('CS011', 4, '16-Mar-2022', '06-May-2022', 'E0003', 'H0002');
INSERT INTO ConstructionSheet VALUES
('CS012', 5, '07-May-2022', '27-Jun-2022', 'E0003', 'H0002');
INSERT INTO ConstructionSheet VALUES
('CS013', 6, '28-Jun-2022', '18-Aug-2022', 'E0003', 'H0002');
INSERT INTO ConstructionSheet VALUES
('CS014', 7, '19-Aug-2022', '09-Oct-2022', 'E0003', 'H0002');
INSERT INTO ConstructionSheet VALUES
('CS015', 1, '11-Nov-2021', '01-Jan-2022', 'E0002', 'H0003');
INSERT INTO ConstructionSheet VALUES
('CS016', 2, '02-Jan-2022', '22-Feb-2022', 'E0002', 'H0003');
INSERT INTO ConstructionSheet VALUES
('CS017', 3, '23-Feb-2022', '15-Apr-2022', 'E0002', 'H0003');
INSERT INTO ConstructionSheet VALUES
('CS018', 4, '16-Apr-2022', '06-Jun-2022', 'E0002', 'H0003');
INSERT INTO ConstructionSheet VALUES
('CS019', 1, '12-Dec-2021', '01-Feb-2022', 'E0003', 'H0004');
INSERT INTO ConstructionSheet VALUES
('CS020', 2, '02-Feb-2022', '25-Mar-2022', 'E0003', 'H0004');
INSERT INTO ConstructionSheet VALUES
('CS021', 3, '26-Mar-2022', '16-May-2022', 'E0003', 'H0004');
INSERT INTO ConstructionSheet VALUES
('CS022', 1, '12-Jan-2022', '04-Mar-2022', 'E0006', 'H0005');
INSERT INTO ConstructionSheet VALUES
('CS023', 2, '05-Mar-2022', '25-Apr-2022', 'E0006', 'H0005');
INSERT INTO ConstructionSheet VALUES
('CS024', 3, '26-Apr-2022', '16-Jun-2022', 'E0006', 'H0005');
INSERT INTO ConstructionSheet VALUES
('CS025', 1, '12-Feb-2022', '04-Apr-2022', 'E0006', 'H0006');
INSERT INTO ConstructionSheet VALUES
('CS026', 2, '05-Apr-2022', '26-May-2022', 'E0006', 'H0006');
INSERT INTO ConstructionSheet VALUES
('CS027', 1, '15-Mar-2022', '05-May-2022', 'E0008', 'H0007');
INSERT INTO ConstructionSheet VALUES
('CS028', 2, '06-May-2022', '26-Jun-2022', 'E0008', 'H0007');
INSERT INTO ConstructionSheet VALUES
('CS029', 1, '15-Apr-2022', '05-Jun-2022', 'E0008', 'H0008');
INSERT INTO ConstructionSheet VALUES
('CS030', 1, '16-May-2022', '06-Jul-2022', 'E0010', 'H0009');
INSERT INTO ConstructionSheet VALUES
('CS031', 1, '16-Jun-2022', '06-Aug-2022', 'E0010', 'H0010');





-- inserting records into Task

INSERT INTO Task VALUES
('T0001', 'Prepare Construction Site and Pour Foundation');
INSERT INTO Task VALUES
('T0002', 'Complete Rough Framing');
INSERT INTO Task VALUES
('T0003', 'Complete Rough Plumbing, Electrical and HVAC');
INSERT INTO Task VALUES
('T0004', 'Install Insulation');
INSERT INTO Task VALUES
('T0005', 'Start Exterior Finishes and Install Exterior Walkways');
INSERT INTO Task VALUES
('T0006', 'Install Hard Surface Flooring and Countertops');
INSERT INTO Task VALUES
('T0007', 'Finish Flooring and Exterior Landscaping');
INSERT INTO Task VALUES
('T0008', 'Complete Drywall and Interior Fixtures');
INSERT INTO Task VALUES
('T0009', 'Finish Mechanical Trims and Install Bathroom Fixtures');




-- inserting records into TaskProgress

INSERT INTO TaskProgress VALUES
('CS001','T0001', 100);
INSERT INTO TaskProgress VALUES
('CS002','T0002',  100);
INSERT INTO TaskProgress VALUES
('CS003','T0003',  100);
INSERT INTO TaskProgress VALUES
('CS004','T0004',  100);
INSERT INTO TaskProgress VALUES
('CS005','T0005',  100);
INSERT INTO TaskProgress VALUES
('CS006','T0006', 100);
INSERT INTO TaskProgress VALUES
('CS006','T0007',  100);
INSERT INTO TaskProgress VALUES
('CS007','T0008',  100);
INSERT INTO TaskProgress VALUES
('CS007','T0009',  100);
INSERT INTO TaskProgress VALUES
('CS008', 'T0001', 100);
INSERT INTO TaskProgress VALUES
('CS009','T0002',  100);
INSERT INTO TaskProgress VALUES
('CS010','T0003',  100);
INSERT INTO TaskProgress VALUES
('CS011','T0004',  100);
INSERT INTO TaskProgress VALUES
('CS012','T0005',  100);
INSERT INTO TaskProgress VALUES
('CS013','T0006',  100);
INSERT INTO TaskProgress VALUES
('CS013','T0007',  100);
INSERT INTO TaskProgress VALUES
('CS014','T0008',  100);
INSERT INTO TaskProgress VALUES
('CS014','T0009',  100);
INSERT INTO TaskProgress VALUES
('CS015','T0001',  100);
INSERT INTO TaskProgress VALUES
('CS016','T0002',  100);
INSERT INTO TaskProgress VALUES
('CS017','T0003',  100);
INSERT INTO TaskProgress VALUES
('CS018','T0004',  90);
INSERT INTO TaskProgress VALUES
('CS019','T0001',  100);
INSERT INTO TaskProgress VALUES
('CS020','T0002',  100);
INSERT INTO TaskProgress VALUES
('CS021','T0003',  80);
INSERT INTO TaskProgress VALUES
('CS022','T0001',  100);
INSERT INTO TaskProgress VALUES
('CS023','T0002',  100);
INSERT INTO TaskProgress VALUES
('CS024','T0003',  85);
INSERT INTO TaskProgress VALUES
('CS025','T0001',  100);
INSERT INTO TaskProgress VALUES
('CS026','T0002',  90);
INSERT INTO TaskProgress VALUES
('CS027','T0001',  100);
INSERT INTO TaskProgress VALUES
('CS028','T0002',  75);
INSERT INTO TaskProgress VALUES
('CS029','T0001',  88);
INSERT INTO TaskProgress VALUES
('CS030','T0001',  95);
INSERT INTO TaskProgress VALUES
('CS031','T0001',  83);



--lot table
INSERT INTO lot VALUES
('L0001', '1364 Jacobs St', 'Pittsburgh', 'PA', '15201', 40.50, -80.02, 'S0001', 'Modern', 'A', 'H0001');
INSERT INTO lot VALUES
('L0002', '4200 Stiles St', 'Pittsburgh', 'PA', '15213', 40.35, -79.92, 'S0002', 'Renaissance', 'B', 'H0002');
INSERT INTO lot VALUES
('L0003', '2556 Losh Lane', 'Pittsburgh', 'PA', '15213', 40.44, -79.97, 'S0002', 'Mediterranean', 'C', 'H0003');
INSERT INTO lot VALUES
('L0004', '1152 Beechwood Drive', 'Pittsburgh', 'PA', '15213', 40.53, -79.99, 'S0002', 'Mediterranean', 'A', 'H0004');
INSERT INTO lot VALUES
('L0005', '1102 Dancing Dove Lane', 'New York', 'NY', '10013', 40.78, -73.93, 'S0003', 'Modern', 'A', 'H0005');
INSERT INTO lot VALUES
('L0006', '4717 My Drive', 'New York', 'NY', '10013', 40.81, -73.92, 'S0003', 'Renaissance', 'C', 'H0006');
INSERT INTO lot VALUES
('L0007', '127 Marlborough St.', 'Boston', 'MA', '02116', 42.35, -71.08, 'S0004', 'Renaissance', 'B', 'H0007');
INSERT INTO lot VALUES
('L0008', '5 Spring Park Ave', 'Boston', 'MA', '02130', 42.32, -71.11, 'S0004', 'Mediterranean', 'C', 'H0008');
INSERT INTO lot VALUES
('L0009', '2676 Union St.', 'San Francisco', 'CA', '94123', 38.00, -122.00, 'S0005', 'Modern', 'A', 'H0009');
INSERT INTO lot VALUES
('L0010', '438 Steiner St.', 'San Francisco', 'CA', '94117', 37.79, -122.44, 'S0005', 'Renaissance', 'B', 'H0010');
INSERT INTO lot VALUES
('L0011', '4711 Hanover Street', 'New York', 'NY', '10013', 40.78, -73.93, 'S0003', 'Mediterranean', 'A', NULL);
INSERT INTO lot VALUES
('L0012', '4280 Kovar Road', 'Boston', 'MA', '02110', 42.43, -70.97, 'S0004', 'Modern', 'A', NULL);
COMMIT;

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

REM 2 Sequences
-- Create sequence for table style
ALTER TABLE style ADD style_id NUMBER(4,0);

CREATE SEQUENCE style_id_seq
INCREMENT BY 1;

UPDATE style
SET style_id = style_id_seq.NEXTVAL;

COMMIT;


-- Create sequence for table elevation
ALTER TABLE elevation ADD elevation_id NUMBER(4,0);

CREATE SEQUENCE elevation_id_seq
INCREMENT BY 1;

UPDATE elevation
SET elevation_id = elevation_id_seq.NEXTVAL;

COMMIT;


------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

REM 2 Views
-- View for construction sheet
CREATE OR REPLACE VIEW construction_stage AS
SELECT * FROM constructionsheet
WHERE stage = 1 OR stage = 4 OR stage = 7;

--View for Option
CREATE OR REPLACE VIEW option_view AS
SELECT option_id, option_no, option_desc, stage, category, style_name FROM "Option";


------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

REM 2 Procedures

--procedure 1
--Show ongoing tasks (percent_complete < 100) for all construction managers 

CREATE OR REPLACE
PROCEDURE ongoing_task 
AS TYPE ongoing_type IS RECORD
                (
                construction_id1 constructionsheet.construction_id%TYPE,
                employee_id1 constructionsheet.employee_id%TYPE,
                fullname employee.fname%TYPE,
		house_id1 constructionsheet.house_id%TYPE,
                task_id1 taskprogress.task_id%TYPE,
                percent_complete1 taskprogress.percent_complete%TYPE,
                task_desc1 task.task_desc%TYPE);
ongoing_task_record ongoing_type;               

CURSOR task_cursor IS
        SELECT c.construction_id, c.employee_id, CONCAT(CONCAT(e.fname,' '),
                e.lname) AS fullname, c.house_id, tp.task_id, tp.percent_complete, t.task_desc
        FROM constructionsheet c, taskprogress tp, task t, employee e
        WHERE c.construction_id = tp.construction_id 
        AND tp.task_id = t.task_id
        AND c.employee_id = e.employee_id
        AND tp.percent_complete < 100;
       
BEGIN
    OPEN task_cursor;
    LOOP
        FETCH task_cursor INTO ongoing_task_record;
        EXIT WHEN task_cursor%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE('+                                                                  +');
        DBMS_OUTPUT.PUT_LINE('Employee ID: '||ongoing_task_record.employee_id1);
        DBMS_OUTPUT.PUT_LINE('Employee Name: '||ongoing_task_record.fullname);
        DBMS_OUTPUT.PUT_LINE('Construction ID: '||ongoing_task_record.construction_id1);
        DBMS_OUTPUT.PUT_LINE('House ID: '||ongoing_task_record.house_id1);
        DBMS_OUTPUT.PUT_LINE('Task ID: '||ongoing_task_record.task_id1);
        DBMS_OUTPUT.PUT_LINE('Task Description: '||ongoing_task_record.task_desc1);
        DBMS_OUTPUT.PUT_LINE('Percent Complete: '||ongoing_task_record.percent_complete1);

    END LOOP;
    CLOSE task_cursor;

END;
/


-- procedure 2: 
-- Show base price, elevation cost, each decorator choice cost,
-- and the total cost of the chosen house 
-- take input parameter = house_id


CREATE OR REPLACE 
PROCEDURE house_price_query(houseid1 IN house.house_id%TYPE)

AS
house_id house.house_id%TYPE;
house_price house.base_price%TYPE;
elevation_price elevation.Additional_cost_sketch%TYPE;
decoration_count NUMBER(2) :=1;  -- use to number decoration 1, 2, ...
total_cost NUMBER(15); -- sum the total cost
is_empty boolean := true; -- to check is the cursor (decoration record is empty)


-- Use cursor because a house could have several kinds of decoration
-- Note: a house can only have 1 base price and elevation price

CURSOR c1(houseid2 house.house_id%TYPE)
IS 
SELECT o.option_desc, o.cost
FROM house h, "Option" o, decoratorchoice d
WHERE h.house_id(+) = d.house_id
AND d.option_id = o.option_id
AND h.house_id = houseid2;


BEGIN
	-- if a price is null, change it to 0
	SELECT h.house_id, COALESCE(h.base_price,0), COALESCE(e.Additional_cost_sketch,0)
	INTO house_id, house_price, elevation_price
	FROM house h, lot l, elevation e
	WHERE e.elevation_name = l.elevation_name
	AND l.house_id = h.house_id
	AND h.house_id = houseid1
	GROUP BY h.house_id, h.base_price, e.Additional_cost_sketch;

	DBMS_OUTPUT.PUT_LINE('House ID:         '||house_id);
	DBMS_OUTPUT.PUT_LINE('House Base Price: $'||house_price);
	DBMS_OUTPUT.PUT_LINE('Elevation Price:  $'||elevation_price);
	total_cost := house_price + elevation_price; -- do the sum

	-- get all the decorations and print them out 
	FOR item in c1(houseid1)
	LOOP
		DBMS_OUTPUT.PUT_LINE('Decoration '||decoration_count||':     '||item.option_desc);
		DBMS_OUTPUT.PUT_LINE('  Price:            $'||item.cost);
		total_cost := total_cost + item.cost; -- do the sum
		decoration_count := decoration_count+1; -- add the decoration number
		is_empty := false; -- if the cursor is not empty, change to false

	END LOOP;
	
	IF is_empty THEN 
	DBMS_OUTPUT.PUT_LINE('No decoration yet');
	END IF;

	DBMS_OUTPUT.PUT_LINE('Total:            $'|| total_cost);

EXCEPTION
	WHEN no_data_found THEN
	DBMS_OUTPUT.PUT_LINE('NO such house');
	ROLLBACK;

END;
/

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

REM 1 Function

-- Average base price of house under selected style (input parameter = style_name)
CREATE OR REPLACE FUNCTION avg_price_by_style (style_n VARCHAR)
RETURN NUMBER 
IS 
    avg_price NUMBER;
BEGIN
    SELECT AVG(base_price)
    INTO avg_price
    FROM style JOIN lot USING(style_name)
                 JOIN house h USING(house_id)
    GROUP BY style_name
    HAVING style_name = style_n;
    
    RETURN avg_price;
END avg_price_by_style;
/

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 REM 1 Package

CREATE OR REPLACE PACKAGE c_package AS 
	PROCEDURE fire_employee(employeeid IN employee.employee_id%TYPE);
	PROCEDURE hire_employee(employeeid IN employee.employee_id%TYPE,
				lastname IN employee.lname%TYPE,
				firstname IN employee.fname%TYPE,
				title IN employee.title%TYPE,
				license IN employee.license_no%TYPE);
END c_package;
/

CREATE OR REPLACE PACKAGE BODY c_package AS


-- first procedure starts

	PROCEDURE hire_employee(employeeid IN employee.employee_id%TYPE,
				lastname IN employee.lname%TYPE,
				firstname IN employee.fname%TYPE,
				title IN employee.title%type,
				license IN employee.license_no%TYPE)
	IS
	BEGIN
		INSERT INTO employee
		VALUES(employeeid, lastname, firstname, title, license);
		DBMS_OUTPUT.PUT_LINE('Employee hired: '||employeeid||' '||firstname||' '||lastname);

	END hire_employee;
-- first procedure ends


-- second procedure starts
	PROCEDURE fire_employee(employeeid IN employee.employee_id%TYPE)
	IS
	constructor_manager_ornot NUMBER;
	sells_manager_ornot NUMBER;
	decorator_choice_ornot NUMBER;
	no_number EXCEPTION;

	BEGIN
		SELECT count(*) INTO decorator_choice_ornot
		FROM employee e, decoratorchoice d
		WHERE e.employee_id = d.employee_id
		AND e.employee_id = employeeid;

		SELECT count(*) INTO constructor_manager_ornot
		FROM employee e, constructionsheet c
		WHERE e.employee_id = c.employee_id
		AND e.employee_id = employeeid;

		SELECT count(*) INTO sells_manager_ornot
		FROM employee e, contract c
		WHERE e.employee_id = c.employee_id
		AND e.employee_id = employeeid;

		IF constructor_manager_ornot != 0 THEN 
		DBMS_OUTPUT.PUT_LINE('You can not fire a construction manager!');
		ELSIF sells_manager_ornot != 0 THEN 
		DBMS_OUTPUT.PUT_LINE('You can not fire a sales manager!');
		ELSIF decorator_choice_ornot != 0 THEN
		DBMS_OUTPUT.PUT_LINE('You can not fire a decorator choice manager!');
		ELSE 
		DBMS_OUTPUT.PUT_LINE('FIRE '||employeeid);
		DELETE FROM employee                      
		WHERE employee.employee_id = employeeid;
 			IF SQL%NOTFOUND THEN
     			RAISE no_number;
 			END IF;
		END IF;

	EXCEPTION
	WHEN no_number THEN
	DBMS_OUTPUT.PUT_LINE('NO such employee');
	ROLLBACK;

	END fire_employee;

--second procedure ends

END c_package;
/


------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

REM 2 Triggers

REM Trigger1: check contract's estimate completion date

CREATE OR REPLACE TRIGGER 
contract_completion_date
BEFORE INSERT OR UPDATE ON contract /* contract table*/
FOR EACH ROW
DECLARE 
    unacceptable_est_completion EXCEPTION; 
BEGIN 
    IF (TO_DATE(:new.est_complete_date) < add_months(TO_DATE(:new.signed_date), 12))
    THEN 
        DBMS_OUTPUT.PUT_LINE('Contract Added/Updated'); 
        DBMS_OUTPUT.PUT_LINE('New Estimated Complete Date: ' || :new.est_complete_date); 
        DBMS_OUTPUT.PUT_LINE('New Signed Date: ' || :new.signed_date); 
        DBMS_OUTPUT.PUT_LINE('Old Estimated Complete Date: ' || :old.est_complete_date); 
        DBMS_OUTPUT.PUT_LINE('Old Signed Date: ' || :old.signed_date); 
    ELSE 
        RAISE unacceptable_est_completion; 
    END IF; 
EXCEPTION
    WHEN unacceptable_est_completion THEN RAISE_APPLICATION_ERROR (-20326,'Error in new estimated completion date');
END contract_completion_date; 
/

ALTER TRIGGER contract_completion_date ENABLE; 



REM Trigger2

CREATE OR REPLACE TRIGGER 
constructionsheet_stage
BEFORE INSERT OR UPDATE ON constructionsheet
FOR EACH ROW
DECLARE 
    v_max_stage constructionsheet.stage%TYPE; 
    unacceptable_stage EXCEPTION; 
BEGIN 
    SELECT MAX(stage) INTO v_max_stage 
    FROM constructionsheet 
    WHERE :new.house_id = constructionsheet.house_id; 
    
    IF :new.stage = v_max_stage + 1 THEN
        DBMS_OUTPUT.PUT_LINE('ConstructionSheet Added/Updated'); 
        DBMS_OUTPUT.PUT_LINE('New Stage: ' || :new.stage); 
        DBMS_OUTPUT.PUT_LINE('Old Stage: ' || :old.stage); 
    ELSE 
        RAISE unacceptable_stage; 
    END IF; 
EXCEPTION
    WHEN unacceptable_stage THEN RAISE_APPLICATION_ERROR (-20326,'Error in new stage');
END constructionsheet_stage; 
/



ALTER TRIGGER constructionsheet_stage ENABLE; 


------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
REM Scheduled Job

BEGIN
DBMS_SCHEDULER.CREATE_JOB (
job_name => 'task_schedule',
job_type => 'PLSQL_BLOCK',
job_action => 'UPDATE taskprogress
        				SET percent_complete = percent_complete+1
        				WHERE percent_complete <100;',
start_date => sysdate,
repeat_interval => 'FREQ=DAILY',
enabled     	=> true);
END;
/

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
REM 3 Roles

REM create a role of sales representative 
CREATE ROLE sales_representative;

GRANT select, insert, update, delete ON decoratorchoice TO sales_representative; 
GRANT select, insert, update, delete ON contract TO sales_representative;
GRANT select, insert, update, delete ON buyer TO sales_representative;
GRANT SELECT, insert, update, delete  ON house TO sales_representative;

GRANT SELECT ON subdivision TO sales_representative; 
GRANT SELECT ON "Subdivision-Style" TO sales_representative;
GRANT SELECT ON style TO sales_representative;
GRANT SELECT ON "Option" TO sales_representative;
GRANT SELECT ON "Style-Elevation" TO sales_representative;
GRANT SELECT ON schooldistrict TO sales_representative;
GRANT SELECT ON school TO sales_representative;
GRANT SELECT ON lot TO sales_representative;
GRANT SELECT ON elevation TO sales_representative;
GRANT SELECT ON room TO sales_representative;
GRANT SELECT ON constructionsheet TO sales_representative;
GRANT SELECT ON task TO sales_representative;
GRANT SELECT ON agent TO sales_representative;
GRANT SELECT ON bank TO sales_representative;
GRANT SELECT ON employee TO sales_representative;





REM create role of construction manager

CREATE ROLE construction_mgr;

GRANT select, insert, update, delete ON constructionsheet TO construction_mgr;
GRANT select, insert, update, delete ON  task TO construction_mgr;
GRANT select, insert, update, delete ON room TO construction_mgr;
GRANT select, insert, update, delete ON house TO construction_mgr;
GRANT select, insert, update, delete ON elevation TO construction_mgr;
GRANT select, insert, update, delete ON subdivision TO construction_mgr;
GRANT select, insert, update, delete ON "Subdivision-Style" TO construction_mgr;
GRANT select, insert, update, delete ON style TO construction_mgr;
GRANT select, insert, update, delete ON "Option" TO construction_mgr;
GRANT select, insert, update, delete ON "Style-Elevation" TO construction_mgr;

GRANT select ON lot TO construction_mgr;
GRANT select ON schooldistrict TO construction_mgr;
GRANT select ON  school TO construction_mgr;
GRANT select ON agent TO construction_mgr;
GRANT select ON bank TO construction_mgr;
GRANT select ON decoratorchoice TO construction_mgr;
GRANT select ON  contract TO construction_mgr;
GRANT select ON buyer TO construction_mgr;



REM create role of buyer
CREATE ROLE buyers; 

GRANT SELECT ON subdivision TO buyers; 
GRANT SELECT ON "Subdivision-Style" TO buyers;
GRANT SELECT ON style TO buyers;
GRANT SELECT ON "Style-Elevation" TO buyers;
GRANT SELECT ON schooldistrict TO buyers;
GRANT SELECT ON school TO buyers;
GRANT SELECT ON house TO buyers;
GRANT SELECT ON lot TO buyers;
GRANT SELECT ON elevation TO buyers;
GRANT SELECT ON room TO buyers;
GRANT select ON construction_stage to buyers; 
GRANT SELECT ON option_view to buyers; 

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

REM 2 Alternate Indexes

CREATE INDEX subdivision_idx ON subdivision (subdivision_name);
CREATE INDEX room_idx ON room (room_name);
/

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
REM De-normalization with Materialized View

CREATE MATERIALIZED VIEW construction_progress
REFRESH ON COMMIT
AS
SELECT c.house_id, c.stage, t.task_desc,tp.percent_complete
FROM constructionsheet c, taskprogress tp, task t
WHERE c.construction_id = tp.construction_id AND t.task_id = tp.task_id;
/