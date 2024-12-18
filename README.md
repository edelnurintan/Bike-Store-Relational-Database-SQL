# BIKE-STORE-RELATIONAL-DATABASE-SQL
Dalam proyek ini, dilakukan pembangunan, pengelolaan, dan analisis sistem database untuk mendukung proses bisnis, mencakup manajemen produk, stok, pelanggan, dan penjualan. Proyek ini melibatkan pembuatan struktur tabel yang dilengkapi dengan **Primary Key**, **Foreign Key**, serta pengelolaan relasi antar tabel menggunakan **SQL** untuk memastikan integritas dan konsistensi data. Selain itu, berbagai query dirancang untuk keperluan analisis data penjualan dan pembuatan laporan operasional yang relevan.
![bike](https://github.com/edelnurintan/Bike-Store-Relational-Database-SQL/blob/main/diag.png)

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

ALTER TABLE order_items
ADD CONSTRAINT FK_product_order FOREIGN KEY (product_id) REFERENCES products (product_id);
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
| customer_id | first_name | order_id | product_id | product_name                                      | quantity |
|-------------|------------|----------|------------|--------------------------------------------------|----------|
| 259         | Johnathan  | 1        | 20         | Electra Townie Original 7D EQ - Women's - 2016  | 1        |
| 259         | Johnathan  | 1        | 8          | Trek Remedy 29 Carbon Frameset - 2016           | 2        |
| 259         | Johnathan  | 1        | 10         | Surly Straggler - 2016                          | 2        |
| 259         | Johnathan  | 1        | 16         | Electra Townie Original 7D EQ - 2016            | 2        |
| 259         | Johnathan  | 1        | 4          | Trek Fuel EX 8 29 - 2016                        | 2        |
| 1212        | Jaqueline  | 2        | 20         | Electra Townie Original 7D EQ - Women's - 2016  | 1        |
| ...         | ...        | ...      | ...        | ...                                              | ...      |

## Menampilkan Produk Berdasarkan Kategori Tertentu
``` sql
SELECT
	products.product_id,
	products.product_id,
	products.model_year,
	products.list_price,
	categories.category_id,
	categories.category_name
FROM
	products
INNER JOIN
	categories
	ON products.category_id = categories.category_id
WHERE
	categories.category_name IN ('Mountain Bikes','Electric Bikes');
```
| product_id | model_year | list_price | category_id | category_name  |
|------------|------------|------------|-------------|----------------|
| 1          | 2016       | 379.99     | 6           | Mountain Bikes |
| 2          | 2016       | 749.99     | 6           | Mountain Bikes |
| 3          | 2016       | 999.99     | 6           | Mountain Bikes |
| 4          | 2016       | 2899.99    | 6           | Mountain Bikes |
| 5          | 2016       | 1320.99    | 6           | Mountain Bikes |
| 6          | 2016       | 469.99     | 6           | Mountain Bikes |
| ...        | ...        | ...        | ...         | ...            |

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
| customer_id | first_name | Total_harga |
|-------------|------------|-------------|
| 1234        | Joshua     | 2269,95     |
| 902         | Elease     | 1343,96     |
| 355         | Arvilla    | 3199,97     |
| 687         | Cecilia    | 8638,92     |
| 23          | Kaylee     | 5139,96     |
| 46          | Monika     | 9589,90     |
| 710         | Ezra       | 1897,99     |
| ...         | ...        | ...         |

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
| product_name                           | sisa_stok |
|----------------------------------------|-----------|
| Trek Remedy 29 Carbon Frameset - 2016 | 0         |
| Surly Ice Cream Truck Frameset - 2016 | 0         |
| Surly Wednesday Frameset - 2016       | 0         |
| Heller Shagamaw Frame - 2016          | 1         |
| Trek Remedy 29 Carbon Frameset - 2016 | 1         |
| Trek Fuel EX 8 29 - 2016              | 2         |
| Heller Shagamaw Frame - 2016          | 3         |
| Ritchey Timberwolf Frameset - 2016    | 5         |
| Surly Wednesday Frameset - 2016       | 6         |
| ...                                    | ...       |

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
| customer_id | first_name | Total_order |
|-------------|------------|-------------|
| 1           | Debra      | 3           |
| 2           | Kasha      | 3           |
| 3           | Tameka     | 3           |
| 4           | Daryl      | 3           |
| 5           | Charolette | 3           |
| 6           | Lyndsey    | 3           |
| 7           | Latasha    | 3           |
| ...         | ...        | ...         |

## Mengidentifikasi Produk yang Paling Banyak Dibeli
``` sql
SELECT 
    products.product_id,
    products.product_name,
    SUM(order_items.quantity) AS total_terjual
FROM 
    products
INNER JOIN 
    order_items 
ON 
    products.product_id = order_items.product_id
GROUP BY 
    products.product_id, products.product_name
ORDER BY 
    total_terjual DESC;
```
| product_id | product_name                                    | total_terjual |
|------------|------------------------------------------------|---------------|
| 6          | Surly Ice Cream Truck Frameset - 2016          | 167           |
| 13         | Electra Cruiser 1 (24-Inch) - 2016             | 157           |
| 16         | Electra Townie Original 7D EQ - 2016           | 156           |
| 7          | Trek Slash 8 27.5 - 2016                       | 154           |
| 23         | Electra Girl's Hawaii 1 (20-Inch) - 2015/2016  | 154           |
| 12         | Electra Townie Original 21D - 2016             | 153           |
| 11         | Surly Straggler 650b - 2016                    | 151           |
| ...        | ...                                            | ...           |

## Membuat Laporan Pesanan Bulanan dari Data Harian
``` sql
SELECT 
    YEAR(order_date) AS order_year, 
    DATENAME(MONTH, order_date) AS order_month_name,
    SUM(order_items.quantity * (order_items.list_price - order_items.discount)) AS total_sales
FROM 
    orders
INNER JOIN 
    order_items 
ON 
    orders.order_id = order_items.order_id
GROUP BY 
    YEAR(order_date), DATENAME(MONTH, order_date)
ORDER BY 
    order_year, DATENAME(MONTH, order_date);
```

| order_year | order_month_name | total_sales  |
|------------|------------------|--------------|
| 2016       | April            | 187204,79    |
| 2016       | August           | 253105,66    |
| 2016       | December         | 223671,37    |
| 2016       | February         | 175743,09    |
| 2016       | January          | 241161,11    |
| 2016       | July             | 222830,89    |
| 2016       | June             | 231100,38    |
| 2016       | March            | 202134,86    |
| 2016       | May              | 228678,79    |
| 2016       | November         | 205295,35    |
| ...        | ...              | ...          |

