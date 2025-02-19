```mermaid
# Koi Order System - Entity Relationship Diagram Documentation

## 1. Entities and Attributes

### 1.1. CUSTOMER
- **customer_id** (PK) - INT, Auto increment
- name - NVARCHAR(100), NOT NULL
- wallet_balance - DECIMAL(10,2), DEFAULT 0
- email - NVARCHAR(100), UNIQUE
- phone - NVARCHAR(20)
- created_at - DATETIME, DEFAULT GETDATE()

### 1.2. MANAGER
- **manager_id** (PK) - INT, Auto increment
- name - NVARCHAR(100), NOT NULL
- email - NVARCHAR(100), UNIQUE
- phone - NVARCHAR(20)
- created_at - DATETIME, DEFAULT GETDATE()

### 1.3. STAFF
- **staff_id** (PK) - INT, Auto increment
- *manager_id* (FK) - INT
- name - NVARCHAR(100), NOT NULL
- role - NVARCHAR(50), CHECK ('Consulting', 'Delivery')
- status - NVARCHAR(20), DEFAULT 'Active'
- email - NVARCHAR(100), UNIQUE
- phone - NVARCHAR(20)
- created_at - DATETIME, DEFAULT GETDATE()

### 1.4. KOI_FARM
- **farm_id** (PK) - INT, Auto increment
- name - NVARCHAR(100), NOT NULL
- location - NVARCHAR(200)
- contact_person - NVARCHAR(100)
- phone - NVARCHAR(20)
- email - NVARCHAR(100)
- created_at - DATETIME, DEFAULT GETDATE()

### 1.5. RESERVATION
- **reservation_id** (PK) - INT, Auto increment
- *customer_id* (FK) - INT, NOT NULL
- *staff_id* (FK) - INT
- departure_date - DATE, NOT NULL
- expected_budget - DECIMAL(10,2)
- status - NVARCHAR(50), CHECK ('Pending', 'Confirmed', 'Cancelled', 'Completed')
- deposit_amount - DECIMAL(10,2)
- total_cost - DECIMAL(10,2)
- created_at - DATETIME, DEFAULT GETDATE()
- updated_at - DATETIME

### 1.6. FISH_ORDER
- **order_id** (PK) - INT, Auto increment
- *reservation_id* (FK) - INT, NOT NULL
- *farm_id* (FK) - INT, NOT NULL
- breed - NVARCHAR(50), NOT NULL
- size - NVARCHAR(20)
- age - INT
- price - DECIMAL(10,2), NOT NULL
- delivery_date - DATE
- status - NVARCHAR(50), CHECK ('Pending', 'Confirmed', 'Delivered', 'Cancelled')
- created_at - DATETIME, DEFAULT GETDATE()
- updated_at - DATETIME

### 1.7. PAYMENT
- **payment_id** (PK) - INT, Auto increment
- *reservation_id* (FK) - INT, NOT NULL
- amount - DECIMAL(10,2), NOT NULL
- payment_type - NVARCHAR(50), CHECK ('Deposit', 'Final Payment')
- payment_date - DATETIME, DEFAULT GETDATE()
- status - NVARCHAR(50), CHECK ('Pending', 'Completed', 'Refunded')
- transaction_id - NVARCHAR(100)
- created_at - DATETIME, DEFAULT GETDATE()

## 2. Relationships

1. **CUSTOMER - RESERVATION** (1:N)
   - One customer can make multiple reservations
   - Each reservation must belong to one customer

2. **MANAGER - STAFF** (1:N)
   - One manager can supervise multiple staff members
   - Each staff member reports to one manager

3. **STAFF - RESERVATION** (1:N)
   - One staff member can handle multiple reservations
   - Each reservation is handled by one staff member

4. **RESERVATION - FISH_ORDER** (1:N)
   - One reservation can contain multiple fish orders
   - Each fish order belongs to one reservation

5. **KOI_FARM - FISH_ORDER** (1:N)
   - One farm can supply multiple fish orders
   - Each fish order comes from one farm

6. **RESERVATION - PAYMENT** (1:N)
   - One reservation can have multiple payments
   - Each payment belongs to one reservation

## 3. Business Rules

1. Customer wallet management:
   - Must maintain sufficient balance for transactions
   - 30% deposit required for fish orders

2. Reservation management:
   - Can be cancelled 48 hours before departure
   - Cancellation fees apply for airfare and hotel

3. Staff assignments:
   - Only managers can assign staff to reservations
   - Consulting staff limited to viewing assigned reservations

4. Order processing:
   - Customer loses deposit on delivery cancellation
   - Deposit refunded for quality issues or farm fault

5. Payment processing:
   - All payments processed through CHIPay system
   - Multiple payment types supported (Deposit, Final Payment)

## 4. Indexes

1. Primary Keys (Automatically indexed):
   - customer_id
   - manager_id
   - staff_id
   - farm_id
   - reservation_id
   - order_id
   - payment_id

2. Foreign Keys:
   - IX_Staff_ManagerId
   - IX_Reservation_CustomerId
   - IX_Reservation_StaffId
   - IX_FishOrder_ReservationId
   - IX_FishOrder_FarmId
   - IX_Payment_ReservationId

## 5. Audit Trail

- All tables include created_at timestamp
- Reservation and Fish_Order include updated_at timestamp
- Automatic triggers update timestamps on modifications
- Payment transactions tracked with transaction_id
