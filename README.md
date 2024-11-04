# **Project Based Virtual Internship: Big Data Analytics - Kimia Farma**

## **Introduction**
Project Based Virtual Internship merupakan inisiatif kolaboratif antara Rakamin Academy dan Kimia Farma, dirancang untuk mendukung pengembangan diri dan akselerasi karier bagi individu yang berminat dalam bidang Big Data Analytics di Kimia Farma. Program ini menawarkan akses ke pembelajaran dasar melalui Article Review dan Company Coaching Video selama 3 minggu, yang bertujuan untuk memperkenalkan peserta pada kompetensi dan keterampilan yang diperlukan dalam posisi Big Data Analytics. Selain itu, peserta akan menjalani evaluasi mingguan berupa soal-soal yang berkaitan dengan materi di minggu tersebut. Pada minggu ke-empat, peserta harus mengerjakan tugas akhir yang akan berfungsi sebagai portofolio peserta dalam program ini. <br>
<br>

**Objectives**
- Meng-import dataset ke dalam BigQuery
- Membuat tabel analisa di BigQuery
- Membuat dashboard visualisasi analisis kinerja bisnis Kimia Farma 2020 - 2023
<br>

**Dataset** <br>
Dataset yang disediakan terdiri dari tabel-tabel berikut:
- kf_final_transaction.csv
- kf_inventory.csv
- kf_kantor_cabang.csv
- kf_product.csv
<br>

##  **Design Datamart**
### Tabel Analisa
Tabel analisa merupakan tabel yang dibuat berdasarkan hasil aggregasi dari ke-empat tabel yang sudah di-import sebelumnya di BigQuery. Hasil tabel ini akan digunakan untuk sumber pembuatan dashboard laporan penjualan.

<details>
  <summary> Query </summary>
    <br>
    
```sql
CREATE TABLE kimia_farma.data_train AS (
  SELECT
    ft.transaction_id,
    ft.date,
    ft.branch_id,
    kc.branch_name,
    kc.kota,
    kc.provinsi,
    kc.rating AS rating_cabang,
    ft.customer_name,
    p.product_id,
    p.product_name,
    ft.price AS actual_price,
    ft.discount_percentage,
    CASE
      WHEN p.price <= 50000 THEN p.price * 0.10
      WHEN p.price <= 180000 THEN p.price * 0.15
      WHEN p.price <= 300000 THEN p.price * 0.20
      WHEN p.price < 500000 THEN p.price * 0.25
      ELSE p.price * 0.30
    END AS persentase_gross_laba,
    (p.price (1 - (ft.discount_percentage / 100))) AS nett_sales,
    (p.price (1 - (ft.discount_percentage / 100))) -
      (CASE
          WHEN p.price <= 50000 THEN p.price * 0.10
          WHEN p.price <= 180000 THEN p.price * 0.15
          WHEN p.price <= 300000 THEN p.price * 0.20
          WHEN p.price < 500000 THEN p.price * 0.25
          ELSE p.price * 0.30
      END) AS nett_profit,
    ft.rating AS rating_transaksi
  FROM
  kimia_farma.kf_final_transaction AS ft
  JOIN
  kimia_farma.kf_inventory AS i ON ft.branch_id = i.branch_id AND ft.product_id = i.product_id
  JOIN
  kimia_farma.kf_kantor.cabang AS kc ON ft.branch_id = kc.branch_id
  JOIN
  kimia_farma.kf_product AS p ON ft.product_id = p.product_id);
);
```
    
<br>
</details>
<br>

<p align="center">
    <kbd> <img width="750" alt="sample tabel analisa" src="https://drive.google.com/uc?id=13_QY52vET61oJp8HfhRpOKN8QogJTDP-"> </kbd> <br>
    Sampel Hasil Tabel Analisa
</p>
<br>

## **Data Visualization**

Visualization: LookerStudio - [Lihat dashboard](https://lookerstudio.google.com/reporting/68d00f14-a556-4e29-b0c0-34da2e229b1c) <br>
