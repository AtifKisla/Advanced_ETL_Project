# 🧩 Advanced ETL Projesi (SSIS)

## Genel Bakış
**Advanced ETL Projesi**, **SQL Server Integration Services (SSIS)** kullanılarak oluşturulmuş tam kapsamlı bir **Veri Ambarı ETL (Extract–Transform–Load)** sürecini göstermektedir.  
Bu proje, ham verinin alınmasından başlayarak fact tabloya kadar tüm aşamaları kapsar ve veri kalitesi, tarihsel izlenebilirlik ile boyutsal bütünlüğü garanti altına alır.

Proje dört ana paketten oluşur; her biri ETL sürecinin kritik bir aşamasını temsil eder.

---

## Proje Yapısı

1. **P1 – Dynamic File Loader**  
   Dizin içindeki CSV veya Excel dosyalarını **dinamik** şekilde SQL Server’a yükler.  
   Dosya adlarında tarih veya desen bazlı değişkenlikleri yönetir.  
   Bağlantı yöneticilerinde ifadeler (expressions) kullanılarak esnek yapı sağlanmıştır.

2. **P2 – Data Cleansing & Validation**  
   Kaynak veriler üzerinde temizlik ve doğrulama işlemleri yapar.  
   - Eksik veya hatalı kayıtları filtreler.  
   - Veri türlerini dönüştürür.  
   - Temizlenmiş verileri `DW_ValidSales` geçici tablosuna yükler.  

3. **P3 – Slowly Changing Dimension (Dim_Product)**  
   **Dim_Product** tablosunu **SCD Type 2** mantığıyla yönetir.  
   - Ürün özelliklerinde zaman içinde gerçekleşen değişimleri izler.  
   - Mevcut kayıtları günceller, eski kayıtları korur.  
   - `StartDate`, `EndDate` ve `IsCurrent` alanlarını kullanarak tarihsel izlenebilirliği sağlar.  
   - Ürün isimlerini Unicode’a dönüştürerek dil ve karakter uyumluluğu sağlar.

4. **P4 – Fact Table Load (Fact_Sales)**  
   **Fact_Sales** tablosunu Lookup ve Derived Column bileşenleriyle yükler.  
   - `Dim_Product` ve `Dim_Date` tablolarına Lookup ile bağlanır, surrogate key değerlerini getirir.  
   - `TotalAmount = Quantity * Price` hesabını Derived Column ile yapar.  
   - Sonuç verilerini fact tabloya aktarır.

---

## ETL Akış Özeti

