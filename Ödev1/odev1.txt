SELECT * from employees;
SELECT * from departments;

--a. Belirli Kolonları Seçme
SELECT FirstName, LastName, Salary FROM Employees;
 
-- b.DISTINCT Komutu ile Tekrarları Önleme
-- yalnızca çalışanı bulunan departmanları listelemek için join kullanılarak departments'dan departmentname kolonu
-- employeesdeki departmentid ile eşleştirilir
-- alias olarak employees yerine e atanır.
SELECT DISTINCT d.departmentname 
FROM employees e  
JOIN departments d ON e.departmentid = d.departmentid; 

/* c. Belirli Bir Departmana Ait Çalışanları Listeleme
Sadece IT departmanında çalışanların bilgilerini getiren bir SQL sorgusu yazınız. 
*/
-- 1. Geçici yol 
SELECT e.* 
FROM employees e 
JOIN departments d ON e.DepartmentID = d.DepartmentID 
WHERE d.DepartmentName = 'IT';
-- employeeden tüm kolonlar seçilir (alias olarak e verildi), departments tablosu employee tablosunda departmentID ler eşleşecek şekilde INNER JOIN ile birleştirilir.
-- department tablosundan departmentName kolonun IT olduğu satırlar WHERE koşul ifadesi ile seçilir. 
-- Böylece sadece IT çalışanlarına ait bilgiler getirilir.

-- 2. kalıcı değişiklik olarak departmentname kolonunun employees tablosuna eklenmesi
ALTER TABLE employees 
ADD COLUMN departmentname VARCHAR(50);
-- yeni tabloyu veri ile doldurma 
UPDATE employees e
SET departmentname = d.departmentname
FROM departments d
WHERE e.departmentid = d.departmentid;

/* d. Maaşa Göre Sıralama
Çalışanları maaşlarına göre büyükten küçüğe sıralayan bir SQL sorgusu yazınız.*/
SELECT * FROM employees ORDER BY salary DESC;

/* e. Kolonları Birleştirme (Concatenation)
Çalışanların FirstName ve LastName alanlarını birleştirerek, tam adlarını içeren yeni bir kolon
oluşturan bir SQL sorgusu yazınız.*/
-- 1. yol 
SELECT FirstName || ' ' || LastName AS FullName FROM employees;
-- iki veri tipini birleştirmek için string kullanıldı 
-- kolon ismine alias verdiğimiz için AS kullanıldı.
-- Bir çok veri tipinde kullanılabilir.
--2. yol
SELECT CONCAT(firstname, ' ', lastname) AS fullname
FROM employees;
/* sadece string veri tipleriyle kullanılabilir.
employees tablosundan firstname ve lastname kolonlarının arasına boşluk bırakarak concat ile  birleştirilir alias olarak fullname
adında bir kolon altında gösterir. */