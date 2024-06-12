# HotelManagementSystem
CREATE DATABASE HotelMS
USE HotelMS;

-- Create Hotel Table
CREATE TABLE Hotel (
    HotelID INT PRIMARY KEY,
    Name VARCHAR(255) NOT NULL,
    Address VARCHAR(255) NOT NULL,
    Stars INT,
    Phone VARCHAR(15) NOT NULL,
    Email VARCHAR(255),
    CheckinTime TIME NOT NULL,
    CheckoutTime TIME NOT NULL
);

-- Create RoomType Table
CREATE TABLE RoomType (
    TypeID INT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL,
    Description VARCHAR(255),
    PricePerNight DECIMAL(10, 2),
    Capacity INT NOT NULL
);

-- Create Room Table
CREATE TABLE Room (
    RoomID INT PRIMARY KEY,
    HotelID INT NOT NULL,
    TypeID INT NOT NULL,
    Status VARCHAR(20) NOT NULL,
    FOREIGN KEY (HotelID) REFERENCES Hotel(HotelID),
    FOREIGN KEY (TypeID) REFERENCES RoomType(TypeID)
);

-- Create Guest Table
CREATE TABLE Guest (
    GuestID INT PRIMARY KEY,
    FirstName VARCHAR(100) NOT NULL,
    LastName VARCHAR(100) NOT NULL,
    DateOfBirth DATE NOT NULL,
    Phone VARCHAR(20) NOT NULL,
    Address VARCHAR(255) NOT NULL,
    Email VARCHAR(255)
);

-- Create Booking Table
CREATE TABLE Booking (
    BookingID INT PRIMARY KEY,
    GuestID INT NOT NULL,
    RoomID INT NOT NULL,
    CheckinDate DATE NOT NULL,
    CheckoutDate DATE NOT NULL,
    TotalPrice DECIMAL(10, 2),
    FOREIGN KEY (GuestID) REFERENCES Guest(GuestID),
    FOREIGN KEY (RoomID) REFERENCES Room(RoomID)
);

-- Create Payment Table
CREATE TABLE Payment (
    PaymentID INT PRIMARY KEY,
    BookingID INT NOT NULL,
    Amount DECIMAL(10, 2),
    PaymentDate DATE,
    PaymentMethod VARCHAR(50) NOT NULL,
    FOREIGN KEY (BookingID) REFERENCES Booking(BookingID)
);

-- Create CreditCard Table
CREATE TABLE CreditCard (
    PaymentID INT PRIMARY KEY,
    CardNumber VARCHAR(20) UNIQUE,
    CardHolderName VARCHAR(100),
    ExpiryDate DATE NOT NULL,
    FOREIGN KEY (PaymentID) REFERENCES Payment(PaymentID)
);

-- Create OnlineCheckout Table
CREATE TABLE OnlineCheckout (
    PaymentID INT PRIMARY KEY,
    TransactionID INT NOT NULL,
    PaymentStatus VARCHAR(20),
    PaymentGateway VARCHAR(20),
    FOREIGN KEY (PaymentID) REFERENCES Payment(PaymentID)
);

-- Create CashPayment Table
CREATE TABLE CashPayment (
    PaymentID INT PRIMARY KEY,
    CashReceivedBy VARCHAR(20),
    ReceiptNumber INT UNIQUE,
    FOREIGN KEY (PaymentID) REFERENCES Payment(PaymentID)
);

-- Create Staff Table
CREATE TABLE Staff (
    StaffID INT PRIMARY KEY,
    HotelID INT NOT NULL,
    FirstName VARCHAR(100) NOT NULL,
    LastName VARCHAR(100),
    DateOfBirth DATE,
    Phone VARCHAR(20) NOT NULL,
    Position VARCHAR(50),
    Salary DECIMAL(10, 2),
    DateOfJoining DATE,
    HireDate DATE,
    FOREIGN KEY (HotelID) REFERENCES Hotel(HotelID)
);

-- Create Receptionist Table
CREATE TABLE Receptionist (
    StaffID INT PRIMARY KEY,
    Shift VARCHAR(20),
    ServiceRating DECIMAL(10, 2),
    FOREIGN KEY (StaffID) REFERENCES Staff(StaffID)
);

-- Create Manager Table
CREATE TABLE Manager (
    StaffID INT PRIMARY KEY,
    OfficeNumber VARCHAR(10),
    YearsOfExperience INT,
    FOREIGN KEY (StaffID) REFERENCES Staff(StaffID)
);
-- Populate Hotel Table
INSERT INTO Hotel (HotelID, Name, Address, Stars, Phone, Email, CheckinTime, CheckoutTime) 
VALUES
(1, 'Sunrise Hotel', '123 Beach Ave', 5, '123-456-7890', 'info@sunrise.com', '14:00', '12:00'),
(2, 'Mountain View', '456 Hill St', 4, '987-654-3210', 'contact@mountainview.com', '15:00', '11:00'),
(3, 'City Inn', '789 City Rd', 3, '456-789-0123', 'info@cityinn.com', '13:00', '11:00'),
(4, 'Country Lodge', '101 Country Ln', 4, '321-654-9870', 'contact@countrylodge.com', '14:00', '12:00'),
(5, 'Ocean Breeze', '202 Ocean Blvd', 5, '123-321-1234', 'info@oceanbreeze.com', '14:00', '11:00'),
(6, 'Desert Oasis', '303 Desert Rd', 4, '555-987-6543', 'info@desertoasis.com', '15:00', '11:00'),
(7, 'Forest Retreat', '404 Forest Dr', 3, '555-123-4567', 'info@forestretreat.com', '13:00', '12:00'),
(8, 'Urban Stay', '505 Urban St', 4, '555-765-4321', 'contact@urbanstay.com', '14:00', '12:00'),
(9, 'Lakeside Resort', '606 Lakeside Ave', 5, '555-456-7890', 'info@lakesideresort.com', '15:00', '11:00'),
(10, 'Hilltop Hotel', '707 Hilltop Rd', 3, '555-789-0123', 'info@hilltophotel.com', '14:00', '12:00');

-- Populate Room Table
INSERT INTO Room (RoomID, HotelID, TypeID, Status)
VALUES
(101, 1, 1, 'Available'),
(102, 2, 2, 'Available'),
(103, 3, 3, 'Available'),
(104, 4, 4, 'Available'),
(105, 1, 5, 'Available'),
(106, 2, 6, 'Available'),
(107, 3, 7, 'Available'),
(108, 4, 8, 'Available'),
(109, 1, 9, 'Available'),
(110, 2, 10, 'Available'),
(111, 3, 1, 'Available'),
(112, 4, 2, 'Available'),
(113, 1, 3, 'Available'),
(114, 2, 4, 'Available'),
(115, 3, 5, 'Available');

-- Populate RoomType Table
INSERT INTO RoomType (TypeID, Name, Description, PricePerNight, Capacity) 
VALUES
(1, 'Single', 'Single bed room', 100.00, 1),
(2, 'Double', 'Double bed room', 150.00, 2),
(3, 'Suite', 'Luxury suite', 300.00, 4),
(4, 'Twin', 'Two single beds', 120.00, 2),
(5, 'Family', 'Family room with multiple beds', 200.00, 4),
(6, 'Deluxe Suite', 'Deluxe luxury suite', 400.00, 4),
(7, 'Studio', 'Studio room with kitchenette', 180.00, 2),
(8, 'Penthouse', 'Penthouse with city view', 500.00, 6),
(9, 'Presidential Suite', 'Presidential luxury suite', 600.00, 6),
(10, 'Economy', 'Economy room with basic amenities', 80.00, 2);

-- Populate Guest Table
INSERT INTO Guest (GuestID, FirstName, LastName, DateOfBirth, Phone, Address, Email) 
VALUES
(1, 'John', 'Doe', '1980-05-15', '555-1234', '789 Elm St', 'jdoe@example.com'),
(2, 'Jane', 'Smith', '1990-07-20', '555-5678', '456 Oak St', 'jsmith@example.com'),
(3, 'Emily', 'Jones', '1985-03-10', '555-8765', '123 Maple St', 'ejones@example.com'),
(4, 'Michael', 'Brown', '1975-09-25', '555-4321', '321 Pine St', 'mbrown@example.com'),
(5, 'David', 'Wilson', '1982-01-14', '555-6543', '987 Birch St', 'dwilson@example.com'),
(6, 'Linda', 'Johnson', '1988-08-30', '555-3456', '654 Cedar St', 'ljohnson@example.com'),
(7, 'Robert', 'Taylor', '1995-03-20', '555-7890', '321 Spruce St', 'rtaylor@example.com'),
(8, 'Susan', 'Lee', '1979-11-11', '555-1230', '654 Redwood St', 'slee@example.com'),
(9, 'James', 'Anderson', '1987-06-15', '555-4567', '987 Aspen St', 'janderson@example.com'),
(10, 'Patricia', 'Clark', '1992-09-05', '555-7896', '123 Walnut St', 'pclark@example.com'),
(11, 'Daniel', 'Harris', '1983-12-25', '555-6789', '456 Magnolia St', 'dharris@example.com'),
(12, 'Jessica', 'Lewis', '1991-04-18', '555-2345', '789 Cypress St', 'jlewis@example.com'),
(13, 'Mark', 'Walker', '1984-07-22', '555-5670', '321 Palm St', 'mwalker@example.com'),
(14, 'Karen', 'Hall', '1996-02-28', '555-8901', '654 Fir St', 'khall@example.com'),
(15, 'Steven', 'Allen', '1978-10-10', '555-4323', '987 Elm St', 'sallen@example.com');

-- Populate Booking Table
INSERT INTO Booking (BookingID, GuestID, RoomID, CheckinDate, CheckoutDate, TotalPrice) 
VALUES
(1, 1, 101, '2024-06-01', '2024-06-07', 700.00),
(2, 2, 102, '2024-06-05', '2024-06-10', 750.00),
(3, 3, 103, '2024-06-10', '2024-06-15', 1500.00),
(4, 4, 104, '2024-06-15', '2024-06-20', 750.00),
(5, 5, 105, '2024-06-01', '2024-06-04', 400.00),
(6, 6, 106, '2024-06-02', '2024-06-06', 600.00),
(7, 7, 107, '2024-06-03', '2024-06-07', 1200.00),
(8, 8, 108, '2024-06-04', '2024-06-08', 800.00),
(9, 9, 109, '2024-06-05', '2024-06-09', 900.00),
(10, 10, 110, '2024-06-06', '2024-06-10', 1000.00),
(11, 11, 111, '2024-06-07', '2024-06-11', 1100.00),
(12, 12, 112, '2024-06-08', '2024-06-12', 1200.00),
(13, 13, 113, '2024-06-09', '2024-06-13', 1300.00),
(14, 14, 114, '2024-06-10', '2024-06-14', 1400.00),
(15, 15, 115, '2024-06-11', '2024-06-15', 1500.00);

-- Populate Payment Table
INSERT INTO Payment (PaymentID, BookingID, Amount, PaymentDate, PaymentMethod) 
VALUES
(1, 1, 700.00, '2024-06-01', 'Credit Card'),
(2, 2, 750.00, '2024-06-05', 'Credit Card'),
(3, 3, 1500.00, '2024-06-10', 'Credit Card'),
(4, 4, 750.00, '2024-06-15', 'Credit Card'),
(5, 5, 400.00, '2024-06-01', 'Credit Card'),
(6, 6, 600.00, '2024-06-02', 'Cash'),
(7, 7, 1200.00, '2024-06-03', 'Cash'),
(8, 8, 800.00, '2024-06-04', 'Cash'),
(9, 9, 900.00, '2024-06-05', 'Cash'),
(10, 10, 1000.00, '2024-06-06', 'Online Checkout'),
(11, 11, 1100.00, '2024-06-07', 'Online Checkout'),
(12, 12, 1200.00, '2024-06-08', 'Online Checkout'),
(13, 13, 1300.00, '2024-06-09', 'Online Checkout'),
(14, 14, 1400.00, '2024-06-10', 'Online Checkout'),
(15, 15, 1500.00, '2024-06-11', 'Online Checkout');

-- Populate CreditCard Table
INSERT INTO CreditCard (PaymentID, CardNumber, CardHolderName, ExpiryDate) 
VALUES
(1, '1234-5678-9876-5432', 'John Doe', '2025-05-01'),
(2, '9876-5432-1234-5678', 'Michael Brown', '2026-07-01'),
(3, '9876-5432-1234-5678', 'Robert Taylor', '2024-11-01'),
(4, '2222-3333-4444-5555', 'James Anderson', '2024-10-01'),
(5, '5555-6666-7777-8888', 'Jessica Lewis', '2024-09-01');

-- Populate CashPayment Table
INSERT INTO CashPayment (PaymentID, CashReceivedBy, ReceiptNumber) 
VALUES
(6, 'Jane Smith', 123456),
(7, 'Michael Brown', 56789),
(8, 'Linda Johnson', 34567),
(9, 'Susan Lee', 67890);

-- Populate OnlineCheckout Table
INSERT INTO OnlineCheckout (PaymentID, TransactionID, PaymentStatus, PaymentGateway) 
VALUES
(10, 98765, 'Completed', 'PayPal'),
(11, 23456, 'Pending', 'Stripe'),
(12, 45678, 'Pending', 'Stripe'),
(13, 35991, 'Pending', 'Stripe'),
(14, 54921, 'Pending', 'Stripe'),
(15, 18401, 'Pending', 'Stripe');

-- Populate Staff Table
INSERT INTO Staff (StaffID, HotelID, FirstName, LastName, DateOfBirth, Phone, Position, Salary, DateOfJoining, HireDate) 
VALUES
(1, 1, 'Emma', 'Wilson', '1990-06-25', '555-1234', 'Housekeeping', 26000.00, '2017-04-20', '2017-04-20'),
(2, 2, 'Frank', 'Anderson', '1983-03-17', '555-5678', 'Chef', 40000.00, '2016-08-10', '2016-08-10'),
(3, 3, 'Grace', 'Thomas', '1995-09-12', '555-6789', 'Waiter', 28000.00, '2019-11-30', '2019-11-30'),
(4, 4, 'Henry', 'Martinez', '1980-04-08', '555-2345', 'Security', 35000.00, '2018-05-15', '2018-05-15'),
(5, 1, 'Ivy', 'Taylor', '1987-10-30', '555-7890', 'Housekeeping', 27000.00, '2020-02-28', '2020-02-28'),
(6, 2, 'Jack', 'Clark', '1993-07-22', '555-8901', 'Waiter', 29000.00, '2019-06-10', '2019-06-10'),
(7, 3, 'Kate', 'White', '1986-11-15', '555-0123', 'Housekeeping', 25500.00, '2017-09-05', '2017-09-05'),
(8, 4, 'Leo', 'Johnson', '1998-01-05', '555-6789', 'Chef', 38000.00, '2018-10-20', '2018-10-20'),
(9, 1, 'Mia', 'Garcia', '1991-05-18', '555-2345', 'Waiter', 29500.00, '2016-12-15', '2016-12-15'),
(10, 2, 'Noah', 'Brown', '1982-08-28', '555-6789', 'Housekeeping', 26500.00, '2019-03-01', '2019-03-01');

-- Populate Receptionist Table
INSERT INTO Receptionist (StaffID, Shift, ServiceRating) 
VALUES
(1, 'Evening', 4.3),
(2, 'Morning', 4.7),
(3, 'Evening', 4.4);

-- Populate Manager Table
INSERT INTO Manager (StaffID, OfficeNumber, YearsOfExperience) 
VALUES
(4, '102', 8),
(5, '103', 12),
(6, '104', 9),
(7, '105', 7);

-- Creating a new table
CREATE TABLE Service (
    ServiceID INT PRIMARY KEY,
    ServiceName VARCHAR(100),
    Price DECIMAL(10, 2)
);
SELECT * FROM Service;

-- Dropping a table
DROP TABLE Service;

-- Altering a table to add a column
ALTER TABLE Guest ADD Gender VARCHAR(10);
Select * FROM Guest;

-- Updating a table to modify data
UPDATE Room SET Status = 'Occupied' WHERE RoomID = 101;
select * from Room;
-- Using SELECT statements
SELECT * FROM Booking WHERE CheckinDate > '2024-06-15';

-- Using SELECT with ORDER BY
SELECT * FROM Guest ORDER BY LastName ASC;

-- Using SELECT with GROUP BY and aggregation functions
SELECT HotelID, COUNT(*) AS TotalRooms FROM Room GROUP BY HotelID;

-- Using JOIN to retrieve data from multiple tables
SELECT Guest.FirstName, Guest.LastName, Booking.CheckinDate, Booking.CheckoutDate
FROM Guest
JOIN Booking ON Guest.GuestID = Booking.GuestID;

-- Using aggregation functions
SELECT AVG(Amount) AS AveragePayment FROM Payment;
