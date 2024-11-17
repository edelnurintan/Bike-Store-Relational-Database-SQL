# Bike-Store-Relational-Database-SQL
![bike](https://github.com/edelnurintan/Bike-Store-Relational-Database-SQL/blob/main/sepeda.png)

## Deskripsi
Dalam proyek ini, dilakukan pembangunan, pengelolaan, dan analisis sistem database untuk mendukung proses bisnis, mencakup manajemen produk, stok, pelanggan, dan penjualan. Proyek ini melibatkan pembuatan struktur tabel yang dilengkapi dengan **Primary Key**, **Foreign Key**, serta pengelolaan relasi antar tabel menggunakan **SQL** untuk memastikan integritas dan konsistensi data. Selain itu, berbagai query dirancang untuk keperluan analisis data penjualan dan pembuatan laporan operasional yang relevan.

## Dataset https://www.kaggle.com/datasets/dillonmyrick/bike-store-sample-database

Pada dataset ini terdapat 8 tabel yang berisi informasi tentang penjualan sepeda

### Table Brands
- brand_id
- brand_name
### Table Categories
- category_id
- category_name
### Table Customers
- customer_id
- first_name
- last_name
- phone
- email
- street
- city
- state
- zip_code
### Table Order Item
- order_id
- item_id
- product_id
- quantity
- list_price
- discount
### Table Orders
- order_id
- customer_id
- order_status
- order_date
- required_date
- shipped_date
- store_id
- staff_id
### Table Products
- product_id
- product_name
- brand_id
- category_id
- model_year
- list_price
### Table Staffs
- staff_id
- first_name
- last_name
- email
- phone
- active
- store_id
- manager_id
### Table Stocks 
- store_id
- product_id
- quantity
### Table Store
- store_id
- store_name
- phone
- email
- street
- city
- state
- zip_code
