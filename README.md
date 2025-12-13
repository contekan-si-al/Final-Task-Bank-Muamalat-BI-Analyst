# Final Task - Digital User Churn Dashboard

## Latar Belakang

Setelah melalui proses pembelajaran selama 4 minggu, aku tentu sudah memahami bagaimana pekerjaan yang dilakukan oleh seorang Business Intelligence Analyst dalam kesehariannya. Sebagai Business Intelligence Analyst Intern yang telah memahami data bisnis di Bank Muamalat, aku diminta untuk membuat *dashboard* dari data mentah yang telah disediakan. Untuk menguji pemahamanku, data tersebut perlu dilakukan proses pengolahan data dari awal hingga akhir. Dengan melakukan proses tersebut, diharapkan aku dapat memahami pekerjaan seorang Business Intelligence Analyst di Bank Muamalat.

## Tahapan Pengerjaan Tugas
1. Mengunduh [*dataset*](https://drive.google.com/file/d/1RwsBQ1FriNfz6qiq0V5nD7gF7jO81To3/view) yang sudah disediakan
2. Mengimport *dataset* yang sudah disediakan ke dalam Google Big Query
   
   Kode untuk mengubah ekstensi .xlsx ke .csv menggunakan python agar mudah unggah di Big Query:
   ```python
   # Simpan ke CSV
   produk.to_csv("C:/Users/AL/Downloads/Compressed/Dataset Task 5/products.csv", index=False) # Ubah lokasi penyimpanan yang sesuai
   pelanggan.to_csv("C:/Users/AL/Downloads/Compressed/Dataset Task 5/customers.csv", index=False)
   pesanan.to_csv("C:/Users/AL/Downloads/Compressed/Dataset Task 5/orders.csv", index=False)
   kat_produk.to_csv("C:/Users/AL/Downloads/Compressed/Dataset Task 5/product_category.csv", index=False)
   ```
4. Membuat `table_master` di Google Big Query
   ```sql
   WITH joined_data AS (
     SELECT
   o.Date AS order_date,
   pc.CategoryName AS category_name,
   p.ProdName AS product_name,
   p.Price AS product_price,
   o.Quantity AS order_qty,
   (o.Quantity * p.Price) AS total_sales,
   c.CustomerEmail AS cust_email,
   c.CustomerCity AS cust_city
   FROM
   `bank_muamalat.orders` AS o
   JOIN
   `bank_muamalat.products` AS p ON o.ProdNumber = p.ProdNumber
   JOIN
   `bank_muamalat.customers` AS c ON o.CustomerID = c.CustomerID
   JOIN
   `bank_muamalat.product_category` AS pc ON p.Category = pc.CategoryID
   )
   SELECT
   order_date,
   category_name,
   product_name,
   product_price,
   order_qty,
   total_sales,
   cust_email,
   cust_city
   FROM joined_data
   ORDER BY order_date ASC;
   ```
5. Membuat [*dashboard*](https://lookerstudio.google.com/reporting/083bf9ae-5bb1-4ec2-9b05-c17f2180313c) menggunakan Looker Studio dari `table_master`
