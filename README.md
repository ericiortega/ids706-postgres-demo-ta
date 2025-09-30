# PostgreSQL Dev Container Tutorial

Introduce to PostgreSQL and basic SQL command for IDS 706

This guide will help you set up a PostgreSQL development environment using Docker Compose and VS Code Dev Containers. You’ll learn how to initialize your database, connect to it, and verify your setup.

---

## 1. Project Structure

```
PostgreSQL_intro/
├── .devcontainer/
│   ├── devcontainer.json
│   └── docker-compose.yml
|   └── .env
├── init.sql
└── (your project files, e.g., py file)
```

---

## 2. Configuration Files

### `.devcontainer/devcontainer.json`

### `.devcontainer/docker-compose.yml`
---

## 3. Database Initialization Script

Create an `init.sql` file in your project root:

```sql
CREATE TABLE restaurants (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  address TEXT,
  distance_miles NUMERIC,
  rating NUMERIC,
  cuisine TEXT,
  avg_cost NUMERIC,
  personal_rank INT
);

INSERT INTO restaurants (name, address, distance_miles, rating, cuisine, avg_cost, personal_rank) VALUES
('NuvoTaco', '2512 University Dr, Durham, NC 27707', 1.2, 4.8, 'Mexican', 12.50, 3),
('Pizzeria Toro', '105 E Chapel Hill St, Durham, NC 27701', 1.8, 4.5, 'Italian', 18.00, 4),
('Bullock’s BBQ', '3330 Quebec Dr, Durham, NC 27705', 2.1, 4.4, 'Southern/BBQ', 15.00, 5),
('Guglhupf Bakery', '2706 Durham-Chapel Hill Blvd, Durham, NC 27707', 1.9, 4.6, 'German Café', 20.00, 6),
('M Sushi', '311 Holland St, Durham, NC 27701', 2.2, 4.7, 'Japanese', 35.00, 1);
('Sushi Love', '2812 Erwin Rd #204, Durham, NC 27705', 0.6, 4.1, 'Japanese', 25.00, 2);
```

---

## 4. Building and Starting the Dev Container

1. **Open your project in VS Code.**
2. **Reopen in Container:**  
   Press Shift+Command+P and select `Dev Containers: Reopen in Container`.
3. The container will build and initialize the database using `init.sql`.

---

## 5. Connecting to PostgreSQL

### Using the Terminal

1. **Open a terminal in VS Code.**
2. **Connect to the database:**
   ```sh
   psql -h localhost -U vscode -d duke_restaurants
   ```
   Enter password: `vscode`

### Useful SQL Commands

- **List tables:**
  ```sql
  \dt
  ```
- **View data:**
  ```sql
  SELECT * FROM restaurants;
  ```

### Example Queries

#### Find Nearby Restaurants (within 2 miles)
```sql
SELECT name, distance_miles FROM restaurants
WHERE distance_miles <= 2.0;
```

#### Best Rated Places
```sql
SELECT name, rating FROM restaurants
ORDER BY rating DESC;
```

#### Calculate Cost with Tax (7.5%)
```sql
SELECT name, avg_cost, avg_cost * 1.075 AS cost_with_tax
FROM restaurants;
```
---

#### Python interaction

```python

import psycopg2

# Connect to the database
conn = psycopg2.connect(
    dbname="duke_restaurants",
    user="vscode",
    password="vscode",
    host="localhost",
    port="5432"
)

cur = conn.cursor()

# Define search parameters
cuisine_type = 'Mexican'
min_rating = 4.0

# Execute search query
cur.execute("""
    SELECT name, address, rating, cuisine, avg_cost
    FROM restaurants
    WHERE cuisine = %s AND rating >= %s
    ORDER BY rating DESC;
""", (cuisine_type, min_rating))

# Fetch and print results
results = cur.fetchall()
for row in results:
    print(row)

cur.close()
conn.close()
```


## 7. Troubleshooting

- **Database not initialized?**  
  The `init.sql` script only runs if the database volume is empty (first run).  
  To reinitialize, stop the container, remove the volume, and rebuild.

- **psql: command not found?**  
  Install it with:
  ```sh
  apt-get update && apt-get install -y postgresql-client
  ```

---
## 9. Using a PostgreSQL GUI in VS Code

You can use a graphical interface to manage your PostgreSQL database directly within VS Code.

### Steps

1. **Install the PostgreSQL Extension**
   - Open the Extensions sidebar (`Ctrl+Shift+X` or `Cmd+Shift+X` on Mac).
   - Search for `PostgreSQL` and install the extension by Chris Kolkman.

2. **Connect to Your Database**
   - Click the **PostgreSQL** icon in the Activity Bar (left side of VS Code).
   - Click **"Add New Connection"**.
   - Enter the following details:
     - **Host:** `localhost`
     - **Port:** `5432`
     - **Role:** `vscode`
     - **Password:** `vscode`
     - **Database:** `duke_restaurants`
   - Click **Connect**.

3. **Browse and Query**
   - You can now browse tables, view data, and run SQL queries using the GUI inside VS Code.

---

## 8. References

- [PostgreSQL Docker Image](https://hub.docker.com/_/postgres)
- [VS Code Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)

---
```# PostgreSQL Dev Container Tutorial

This guide will help you set up a PostgreSQL development environment using Docker Compose and VS Code Dev Containers. You’ll learn how to initialize your database, connect to it, and verify your setup.
