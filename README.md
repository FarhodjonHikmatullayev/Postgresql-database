# Postgresql-database
1. Ma'lumotlar ba'zasini yaratish
   
       CREATE DATABASE database_name;
   Ma'lumotlar ba'zasiga ulanish

       \c database_name
   Ma'lumotlar ba'zasidagi barcha tablelarni ko'rish

       \d 
   Barcha yaratilgan ma'lumotlar bazalarini ko'rish

       \l
   Barcha komandalar haqida bilish

       \?
   Fayldagi kodlarni o'qib olib uni run qilish

       \i file_path
   Ma'lumotlar ba'zasini o'chirib yuborish

       DROP DATABASE database_name;
3. Table yaratish va uni o'chirish
   Table yaratish
   
       CREATE TABLE table_name (name_column data_type adds);
   Tableni o'chirish

       DROP TABLE table_name;
   For example

       CREATE TABLE users (
            id BIGSERIAL NOT NULL PRIMARY KEY,
            name VARCHAR(60) NOT NULL,
            email VARCHAR(60) NOT NULL,
            birthday DATE NOT NULL
       );
   Data typelar haqida ma'lumot: https://www.postgresql.org/docs/13/datatype.html

   Yaratilgan ma'lumotlar ba'zasidagi barcha ustunlarni ko'rish

       \d table_name
4. INSERT INTO, jadvalga ma'lumot yozish
   Jadvalga ma'lumot qo'shish

       INSERT INTO table_name ( column_names) VALUES (values);
   For example

       INSERT INTO users ( name, email, birthday)
       VALUES ('Bexruz', 'bexruz@gmail.com', DATE '1997-01-01');
5. Tabledan ma'lumotlarni o'qish
   Jadvaldagi barcha ma'lumotlarni o'qish

       SELECT * FROM table_name;
   Jadvaldagi kerakli columnlarni o'qib olish

       SELECT column_name FROM table_name;
   Fore example

       SELECT name, email, birthday FROM users;
6. ORDER BY (ESENDING VA DISENDING)

       SELECT * FROM table_name ORDER BY column_name;
   Fore example (Bu yerda agar birthday bir xil bo'lib qolsa keyin email bo'yicha tartiblaydi)

       SELECT * FROM users ORDER BY birthday, email;
   ASC - O'sish tartibida tartiblash

       SELECT * FROM users ORDER BY birthday ASC;
   DESC - Kamayish tartibida

       SELECT * FROM users ORDER BY birthday DESC;
7. DISTINCT, WHERE, AND va OR
   DISTINCT - jadvaldagi ma'lumotlardan faqat 1 tadan chiqaradi, takrorlanganlarini qabul qilmaydi

       SELECT DISTINCT name FROM users ORDER BY name, birthday DESC;
   WHERE - qandaydir shartga taalluqlilarini ajratib chiqarish

       SELECT * FROM users WHERE name='farhod';
   AND - berilgan shartlarning barchasini rostlarini qaytaradi

       SELECT * FROM users WHERE name='farhod' AND gender='male';
   OR - berilgan shartlarning birortasi rostlarini qaytaradi

       SELECT * FROM users WHERE name='farhod' OR gender='female';
8. LIMIT, OFFSET va FETCH, IN
   LIMIT - (Quyidagi misolda birinchi 10 qator chiqadi)

       SELECT * FROM users LIMIT 10;
   OFFSET - (Quyidagi misolda birinchi 10 ta qator tashlab yuboriladi va undan keyingi 10 qator chiqariladi)

       SELECT * FROM users OFFSET 10 LIMIT 10;
   FETCH - (Quyidagi misolda birinchi 10 ta qator tashlab yuboriladi va undan keyingi 10 qator chiqariladi)

       SELECT * FROM users OFFSET 10 FETCH FIRST 10 ROW ONLY;
   Quyidagi misolda birinchi 10 ta qator tashlab yuboriladi va undan keyingi barcha qatorlarni chiqaradi

       SELECT * FROM users OFFSET 10;
   IN - berilgan ma'lumotni to'plamga tegishlilarini chiqaradi

       SELECT * FROM users WHERE country IN ('Russia', 'Uzbekistan', 'Tajikistan', 'Poland', 'Brasil');
       SELECT * FROM users WHERE country IN ('Russia', 'Uzbekistan', 'Tajikistan', 'Poland', 'Brasil') ORDER BY country ASC;
9. BETWEEN, LIKE va ILIKE
   BETWEEN - ikki qiymat orasidagilarni ajratib chiqaradi

       SELECT * FROM users WHERE birthday
       BETWEEN DATE '2020-05-05' AND '2021-01-01'
       ORDER BY birthday;
   LIKE - berilgan shaklga o'xshash ma'lumotlarni ajratib chiqaradi, katta va kichik harflarni ham ajratadi ('_' belgisi qanday dir belgini bildiradi, undan nechta qo'yilsa shuncha nomalum belgini anglatadi)

       SELECT * FROM users WHERE email LIKE '%.com';
       SELECT * FROM users WHERE email LIKE 'farhod%.com';
       SELECT * FROM users WHERE email LIKE '_____hikmatullayev%.com'
   
   ILIKE - Katta va kichik harflarning ahamiyati yo'q

       SELECT * FROM users WHERE email ILIKE '%.com';
10. GROUP BY va HAVING
    GROUP BY va DISTINCT biroz o'xshash lekin GROUP BY orqali qo'shimcha shart bajarsa ham bo'ladi (Quyidagi 2 qator bir xil natija qaytaradi)

        SELECT DISTINCT name FROM users;
        SELECT name FROM users GROUP BY name;
    GROUP BY uchun farq qiluvchi Example
    
        SELECT name, COUNT(*) FROM users GROUP BY name;  # har bir name va undan nechtadan borligini chiqaradi
        SELECT name, COUNT(*) FROM users GROUP BY name ORDER BY name ASC;  # har bir name va undan nechtadan borligini chiqaradi, name ni o'sish bo'yicha tartiblaydi 
    HAVING - doim GROUP BY bilan ishlatiladi va undan keyin yoziladi,

        SELECT name, COUNT(*) FROM users GROUP BY name HAVING COUNT(*) > 5 ORDER BY name ASC;  # Bu yerda namelarning 5 tadan ko'p takrorlanganlari va ularning takrorlanish sonlarini chiqarib beradi
11. Aggrigate functions - https://www.postgresql.org/docs/9.5/functions-aggregate.html
    1. MIN() - minimal qiymatni chiqaradi

           SELECT MIN(price) FROM car;  # car jadvalidagi eng arzon mashina narxini chiqaradi
    2. MAX() - maksimal qiymatni chiqaradi

           SELECT MAX(price) FROM car;  # car jadvalidagi eng qimmat mashina narxini chiqaradi
    3. AVG() - o'rtacha (arifmetik) qiymatni chiqaradi

           SELECT AVG(price) FROM car;  # car jadvalidagi mashinalar narxining o'rta arifmetigini topib chiqaradi
    4. ROUND() - yaxlitlash funksiyasi

           SELECT ROUND(AVG(price)) FROM car;  # car jadvalidagi mashinalar narxining o'rta arifmetigini topadi va uni yaxlitlab qaytaradi
       
       

    
    
       

       
   
            
   

   
   
   
            
      
