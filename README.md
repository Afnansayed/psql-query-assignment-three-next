# Vehicle Rental System

A comprehensive SQL database system for managing vehicle rentals, including user management, vehicle inventory, and booking operations.

## Project Overview

This project demonstrates a complete vehicle rental management system built using PostgreSQL. It includes database schema design, data population, and various SQL queries showcasing different SQL concepts including JOINs, subqueries, aggregations, and filtering.

## Database Schema

The system consists of three main tables:

### 1. Users Table
- Stores customer and admin information
- Fields: `user_id`, `name`, `phone`, `email`, `password`, `role`
- Role types: `admin`, `customer`

### 2. Vehicles Table
- Manages vehicle inventory
- Fields: `vehicle_id`, `name`, `type`, `model`, `registration_number`, `rental_price`, `status`
- Vehicle types: `car`, `bike`, `truck`
- Status types: `available`, `rented`, `maintenance`

### 3. Bookings Table
- Tracks rental bookings
- Fields: `booking_id`, `user_id`, `vehicle_id`, `start_date`, `end_date`, `status`, `total_cost`
- Booking status: `pending`, `confirmed`, `completed`, `cancelled`
- Foreign keys reference Users and Vehicles tables with CASCADE delete

## SQL Queries and Concepts

### Query 1: JOIN - Retrieve Booking Information

**Objective:** Retrieve booking information along with customer name and vehicle name.

**Concepts Used:** INNER JOIN

**Query:**
```sql
select 
  b.booking_id,
  u.name as customer_name,
  v.name as vehicle_name,
  b.start_date,
  b.end_date,
  b.status
from
  bookings b 
  inner join users u on b.user_id = u.user_id
  inner join vehicles v on b.vehicle_id = v.vehicle_id;
```

**Explanation:** This query joins three tables (bookings, users, and vehicles) to display comprehensive booking information including customer and vehicle details in a single result set.

---

### Query 2: EXISTS - Find Unbooked Vehicles

**Objective:** Find all vehicles that have never been booked.

**Concepts Used:** NOT EXISTS

**Query:**
```sql
select v.*
from vehicles v
where not exists (
  select 1 
  from bookings b 
  where b.vehicle_id = v.vehicle_id
)
order by vehicle_id;
```

**Explanation:** Uses the NOT EXISTS subquery to identify vehicles that don't have any corresponding booking records, effectively finding vehicles that have never been rented.

---

### Query 3: WHERE - Filter Available Vehicles

**Objective:** Retrieve all available vehicles of a specific type (e.g., cars).

**Concepts Used:** SELECT, WHERE

**Query:**
```sql
select * 
from vehicles 
where type = 'car' 
  and status = 'available';
```

**Explanation:** Simple filtering query that returns only vehicles that are both cars and currently available for rental.

---

### Query 4: GROUP BY and HAVING - Popular Vehicles

**Objective:** Find the total number of bookings for each vehicle and display only those vehicles that have more than 2 bookings.

**Concepts Used:** GROUP BY, HAVING, COUNT

**Query:**
```sql
select
  v.name as vehicles_name,
  count(*) as total_bookings
from bookings b
inner join vehicles v on b.vehicle_id = v.vehicle_id
group by v.name
having count(*) > 2;
```

**Explanation:** This query aggregates booking data by vehicle, counts the number of bookings per vehicle, and filters to show only vehicles with more than 2 bookings, helping identify popular rental vehicles.

## Setup Instructions

### Using Beekeeper Studio

1. **Open Beekeeper Studio** and connect to your PostgreSQL server

2. **Create the Database:**
   - Click on the "New Query" tab
   - Execute the following command:
   ```sql
   create database vehicle_rental_system;
   ```

3. **Connect to the New Database:**
   - In the left sidebar, click on your connection name
   - Click "Edit Connection" or create a new connection
   - Set the database name to `vehicle_rental_system`
   - Click "Connect"

4. **Run the SQL Script:**
   - Open the `queries.sql` file in a text editor
   - Copy all the contents
   - In Beekeeper Studio, open a new query tab
   - Paste the entire SQL script
   - Click "Run" or press `Ctrl+Enter` (Windows/Linux) or `Cmd+Enter` (Mac) to execute

5. **View the Results:**
   - After execution, you'll see the tables created in the left sidebar
   - You can browse the data by clicking on each table
   - The query results will appear at the bottom of the query window

## Sample Data

The database includes sample data for:
- 3 users (2 customers, 1 admin)
- 4 vehicles (2 cars, 1 bike, 1 truck)
- 4 bookings with various statuses

## Technologies Used

- **Database:** PostgreSQL
- **SQL Concepts:** INNER JOIN, NOT EXISTS, WHERE clauses, GROUP BY, HAVING, Aggregate functions (COUNT)
- **Features:** ENUM types, Foreign keys with CASCADE delete, Primary keys with SERIAL

## Key Features

- User role management (admin/customer)
- Vehicle inventory tracking with status management
- Booking system with multiple status states
- Referential integrity with foreign key constraints
- Custom ENUM types for data validation
  
## License

This project is for educational purposes.
