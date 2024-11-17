# Bike-Store-Relational-Database-SQL
Dalam proyek ini, dilakukan pembangunan, pengelolaan, dan analisis sistem database untuk mendukung proses bisnis, mencakup manajemen produk, stok, pelanggan, dan penjualan. Proyek ini melibatkan pembuatan struktur tabel yang dilengkapi dengan **Primary Key**, **Foreign Key**, serta pengelolaan relasi antar tabel menggunakan **SQL** untuk memastikan integritas dan konsistensi data. Selain itu, berbagai query dirancang untuk keperluan analisis data penjualan dan pembuatan laporan operasional yang relevan.
![bike](https://github.com/edelnurintan/Bike-Store-Relational-Database-SQL/blob/main/sepeda.png)

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
  
Karena dataset sudah tersedia dalam format CSV, maka proses pembuatan tabel baru pada SQL (CREATE TABLE) tidak diperlukan. Langkah selanjutnya adalah langsung menetapkan Primary Key dan Foreign Key pada tabel yang diimpor.

## TABLE BRANDS (Membuat Primary Key)
``` sql
ALTER TABLE brands
ALTER COLUMN brand_id INT NOT NULL;

ALTER TABLE brands
ADD CONSTRAINT PK_brands PRIMARY KEY (brand_id);
```
## TABLE CATEGORIES (Membuat Primary Key)
``` sql
ALTER TABLE categories
ALTER COLUMN category_id INT NOT NULL;

ALTER TABLE categories
ADD CONSTRAINT PK_categories PRIMARY KEY (category_id);
```
## TABLE CUSTOMERS (Membuat Primary Key)
``` sql
ALTER TABLE customers
ALTER COLUMN customer_id INT NOT NULL;

ALTER TABLE customers
ADD CONSTRAINT PK_customer PRIMARY KEY (customer_id);
```
## TABLE ORDER_ITEM  (Membuat Foreign Key dengan tabel Orders)
``` sql
ALTER TABLE order_items
ALTER COLUMN order_id INT NOT NULL;

ALTER TABLE order_items
ADD CONSTRAINT FK_order_item FOREIGN KEY (order_id) REFERENCES orders (order_id);
```
##  TABLE ORDERS (Membuat PK dan FK dengan table customers,orders,orders dan staffs)
``` sql
ALTER TABLE orders
ALTER COLUMN order_id INT NOT NULL;
ALTER TABLE orders
ADD CONSTRAINT PK_orders PRIMARY KEY (order_id);

ALTER TABLE orders
ALTER COLUMN customer_id INT;
ALTER TABLE orders
ADD CONSTRAINT FK_order_customer FOREIGN KEY (customer_id) REFERENCES customers (customer_id);

ALTER TABLE orders
ALTER COLUMN store_id INT;
ALTER TABLE orders
ADD CONSTRAINT FK_orders_store FOREIGN KEY (store_id) REFERENCES stores (store_id);

ALTER TABLE orders
ALTER COLUMN staff_id INT;
ALTER TABLE orders
ADD CONSTRAINT FK_order_staff FOREIGN KEY (staff_id) REFERENCES staffs (staff_id);
```
## TABLE PRODUCTS (Membuat Foreign Key dengan table Brands dan Categories)
``` sql
ALTER TABLE products
ALTER COLUMN product_id INT NOT NULL;

ALTER TABLE products
ALTER COLUMN brand_id INT ;
ALTER TABLE products
ADD CONSTRAINT FK_product FOREIGN KEY (brand_id) REFERENCES brands (brand_id);

ALTER TABLE products
ALTER COLUMN category_id INT ;
ALTER TABLE products
ADD CONSTRAINT FK_categories FOREIGN KEY (category_id) REFERENCES categories (category_id);
```
## TABLE STAFFS (Membuat PK dan FK dengan table stores)
``` sql
ALTER TABLE staffs
ALTER COLUMN staff_id INT NOT NULL;
ALTER TABLE staffs
ADD CONSTRAINT PK_staff PRIMARY KEY (staff_id);

ALTER TABLE staffs
ALTER COLUMN store_id INT NOT NULL;
ALTER TABLE staffs
ADD CONSTRAINT FK_staff FOREIGN KEY (store_id) REFERENCES stores (store_id);
```
## TABLE STOCKS (Menambahkan FK dengan tabel stores)
``` sql
ALTER TABLE stocks
ALTER COLUMN store_id INT NOT NULL ;
ALTER TABLE stocks
ADD CONSTRAINT FK_stocks FOREIGN KEY (store_id) REFERENCES stores (store_id);
```
## TABLE STORES (Membuat Primary Key)
``` sql
ALTER TABLE stores 
ALTER COLUMN store_id INT NOT NULL;
ALTER TABLE stores
ADD CONSTRAINT PK_store PRIMARY KEY (store_id);
```
# Membuat Relasi Antar Table yang Diperlukan
## Menampilkan semua pesanan yang dilakukan setiap pelangggan
``` sql
SELECT
	customers.customer_id,
	customers.first_name,
	orders.order_id,
	products.product_id,
	products.product_name,
	order_items.quantity
FROM
	customers
INNER JOIN
	orders
	ON customers.customer_id = orders.customer_id
INNER JOIN
	order_items
	ON orders.order_id = order_items.order_id
INNER JOIN
	products
	ON order_items.product_id = products.product_id;
```
## Menampilkan Produk Berdasarkan Kategori Tertentu
``` sql
SELECT
	products.product_id,
	products.product_id,
	products.model_year,
	products.list_price,
	categories.category_id,
	category_name
FROM
	products
INNER JOIN
	categories
	ON products.category_id = categories.category_id
WHERE
	categories.category_name IN ('Mountain Bikes','Electric Bikes');
```
## Menampilkan total pesanan per pelanggan
``` sql
SELECT 
	customers.customer_id,
	customers.first_name,
	SUM(order_items.quantity*order_items.list_price) as Total_harga
FROM
	customers
LEFT JOIN
	orders
	ON customers.customer_id = orders.customer_id
LEFT JOIN
	order_items
	ON orders.order_id = order_items.order_id
LEFT JOIN
	products
	ON order_items.product_id = products.product_id
GROUP BY
	customers.customer_id,
	customers.first_name;
```
## Mencari sisa stock terendah 
``` sql
SELECT 
	products.product_name,
	stocks.quantity as sisa_stok
FROM
	stocks
LEFT JOIN
	products
	ON stocks.product_id = products.product_id
WHERE
	stocks.product_id <10
ORDER BY
	sisa_stok ASC;
```
## Mencari pelanggaan yang melakukan pesanan lebih dari sekali
``` sql
SELECT 
	customers.customer_id,
	customers.first_name,
	COUNT(DISTINCT orders.order_id) as Total_order
FROM
	customers
INNER JOIN
	orders
	ON customers.customer_id = orders.customer_id
GROUP BY
	customers.customer_id,
	customers.first_name
HAVING 
	COUNT(DISTINCT orders.order_date)>1;
```


