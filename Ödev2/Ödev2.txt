select * from invoice;
/*
1. Invoice tablosunda, tüm değerleri NULL olan kayıtların sayısını bulmanız
isteniyor.
Bu işlemi tek bir sorgu ile yapmalısınız. Sorguyu yazdıktan sonra, PostgreSQL’in
sol alt kısmındaki Row sayısını, SQL sorgunuzda yorum satırında belirtmeniz
gerekmektedir.*/
-- Tüm kolon Null olan değer sorgusu için 
SELECT COUNT(*)  --COUNT(*)Tüm sütunları NULL olan satırları sayar
FROM invoice 
WHERE invoice_id IS NULL 
  AND customer_id IS NULL 
  AND invoice_date IS NULL 
  AND billing_address IS NULL 
  AND billing_city IS NULL 
  AND billing_state IS NULL 
  AND billing_country IS NULL 
  AND billingpostal_code IS NULL 
  AND total IS NULL;
-- Tüm Sütunları NULL olan satır sayısı 0 dır. 
-- Total rows:1

-- NULL Değer içeren Satırları Getirmek için sorgu 
SELECT *
FROM Invoice
WHERE invoice_id IS NULL 
  or customer_id IS NULL 
  or invoice_date IS NULL 
  or billing_address IS NULL 
  or billing_city IS NULL 
  or billing_state IS NULL 
  or billing_country IS NULL 
  or billingpostal_code IS NULL 
  or total IS NULL;
-- Total Rows: 209 adet null değer içeren satır bulunmaktadır. 
-- billing_state kolonu neredeyse tamamen null değeri içermektedir.

/*2. Koordinasyondaki kişiler, Total değerlerinde bir hata olduğunu belirtiyorlar.
Bu değerlerin iki katını görmek ve eski versiyonlarıyla birlikte karşılaştırmak için bir
sorgu yazmanız isteniyor. Ayrıca, verilerin daha rahat takip edilebilmesi için,
tablonun yeni versiyonuna ait kolona göre büyükten küçüğe sıralama yapılması
isteniyor.*/
-- Fatura verilerini çeken ve toplam tutarı iki katına çıkararak sıralayan sorgu
SELECT 
    invoice_id,           -- Her faturanın benzersiz kimliği
    total AS eski_total,  -- Orijinal toplam tutar (yeniden adlandırıldı)
    total * 2 AS yeni_total -- Toplamın iki katı (yeni sütun oluşturuldu)
FROM 
    invoice                -- Verilerin alındığı tablo
ORDER BY 
    yeni_total DESC;       -- Büyükten küçüğe sıralama (DESC = descending)


/* 3. Adres kolonundaki verileri, soldan 3 karakter ve sağdan 4 karakter alarak
birleştirmeniz ve "Açık Adres" olarak yazmanız isteniyor. Ayrıca, bu yeni açık adresi
2013 yılı ve 8. ay’a göre filtrelemeniz gerekiyor.
*/
--Extract ile Filtreleme Yapılması
SELECT 
  LEFT(billing_address, 3) || RIGHT(billing_address, 4) AS "Açık Adres" 
FROM invoice 
WHERE 
  EXTRACT(YEAR FROM invoice_date) = 2013 
  AND EXTRACT(MONTH FROM invoice_date) = 8;
-- || Operatörüile PostgreSQL'de stringleri birleştirmek için kullanırız.
-- LEFT(billing_address,3) ile Adresin ilk 3 karakterini alırız.
-- RIGHT(billing_address,4) ile Adresin son 4 karakterini alırız.
-- AS ile "Açık Adres" birleştirilen kolon için alias belirlenmesine yarar.
-- Tarihi filtrelemek için EXTRACT kullanılır invoice_date kolonundan 2013 yılına ait kayıtlar getirilir.
-- invoice_date kolonundan 8. ay yani ağustos ayına dair kayıtlar getirlir.

-- Tarih Aralığı ile Filtreleme yapılması
SELECT 
  LEFT(billing_address, 3) || RIGHT(billing_address, 4) AS "Açık Adres" 
FROM invoice 
WHERE 
  invoice_date >= '2013-08-01' 
  AND invoice_date < '2013-09-01';
-- invoice_date'in 2013 1 Ağustos ve Eylül 1 arasındaki kayıtlarını getirir.