/* 
Soru 1: Invoice Tablosu
Soru:
“USA” ülkesine ait, 2009 yılı içerisinde oluşturulmuş tüm faturaların (Invoice) toplamını
listeleyen bir sorgu yazınız.
Kullanılacak Tablo:
● invoice
*/
-- USA'de 2009 yılına ait faturaların toplamı

-- 1. Aggregate fonksiyon (SUM) ile hesaplanması
SELECT SUM(total) AS totbill_usa_2009 -- SUM(total) ile total sütunundaki tüm değerleri toplanır. 
FROM invoice
WHERE billing_country = 'USA' -- WHERE ile USA ve 2009 yılı filtreleri uygulanır. 
  AND EXTRACT(YEAR FROM invoice_date) = 2009;  
-- invoice_date içindeki tarihleri çıkarmak için EXTRACT kullanılır ve 2009'a ait tarihler filtrelenir.
-- total_2009_usa 103.95 sonucu verir.

--2. Group by ile hesaplanması 
SELECT customer_id, SUM(amount) AS total_per_customer
FROM invoice
GROUP BY customer_id;

/* 
Soru 2: Track, PlaylistTrack ve Playlist Tablolarında JOIN
Soru:
Tüm parça (track) bilgilerini, bu parçaların ait olduğu playlisttrack ve playlist tablolarıyla birleştirerek
(JOIN) listeleyen bir sorgu yazınız.
Kullanılacak Tablolar:
● track
● playlisttrack
● playlist
*/
SELECT * FROM Track;
SELECT * FROM PlaylistTrack;
-- Soru 2: Track, PlaylistTrack ve Playlist tablolarını birleştirme
SELECT
    track.*, -- Tüm parça bilgilerini seçilmesi
    playlisttrack.playlist_id, -- İlişkili playlist ID'si
    playlist.Name AS PlaylistName
FROM
    track
LEFT JOIN
    playlisttrack ON track.track_id = playlisttrack.track_id
LEFT JOIN
    playlist ON playlisttrack.playlist_id = playlist.playlist_id;
--track tablosu ana tablo olarak alındı ve playlisttrack ile playlist tabloları ilişkisel olarak birleştirildi.
--Eğer yalnızca playlist'e eklenmiş parçaları getirmek için INNER JOIN kullanabilirdik.
--Soru özellikle tüm parçaların listelenmesini istediği için LEFT JOIN kullanıldı.

/* 
Soru 3: Track, Album ve Artist Tablolarında JOIN
Soru:
"Let There Be Rock" adlı albüme ait tüm parçaları (Track) listeleyen, sanatçı (Artist) bilgisini
de içeren bir sorgu yazınız. Ayrıca, sonuçları parça süresi (milliseconds) büyükten küçüğe
sıralayınız.
Kullanılacak Tablolar:
● track
● album
● artist
*/

SELECT 
  t.name AS track_name, -- track_name için alias (t) atanması
  ar.name AS artist_name, -- artist_name için alias (ar) atanması
  t.milliseconds
FROM track t
INNER JOIN album al ON t.album_id = al.album_id
INNER JOIN artist ar ON al.artist_id = ar.artist_id
WHERE LOWER(al.title) = LOWER('Let There Be Rock') -- Harf duyarlılığının önüne geçmek için tüm harfleri küçülterek filtreleme. 
ORDER BY t.milliseconds DESC; -- sonuçları parça süresi (milliseconds) büyükten küçüğe (DESC) sıralama
-- JOIN ile tablolar ilişkilendirilir. WHERE ile albüm adı filtrelenir. ORDER BY ile süreye göre sıralanır.