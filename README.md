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
        SELECT name, email, COUNT(*) FROM users GROUP BY name, email;
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
           SELECT model, MAX(price) FROM car GROUP BY model;  # car jadvalidagi barcha modellarning eng max narxlari va model nomini chiqarib ko'rsatadi
    5. SUM() - yig'indini hisoblash funksiyasi

           SELECT SUM(price) FROM car;  # car jadvalidagi varcha pricelar yig'indisini chiqaradi
           SELECT model, SUM(price) FROM car GROUP BY model;  # modellar bo'yicha guruhlab o'sha modeldagi mashinalarning narxlari yig'indilari va model nomlarini birga chiqaradi
12. Arifmetik operatsiyalar va ALIAS

        SELECT 3 + 1;
        SELECT 3 - 1;
        SELECT 4 * 7;
        SELECT 7 / 2; # butun qismni chiqaradi
        SELECT 7!;  # factorial
        SELECT 10 % 3; # qoldiqni chiqaradi
        SELECT 100 * .10  # foizni aniqlash (100 ning 10 foizini aniqladi)
    AS (ALIAS) - ustunni nomlash

        SELECT 49 * 0.20 AS foiz;  # 49 ning 20 foizini topadi va column namega foiz deb yozadi
13. COALESCE va NULLIF
    COALESCE - birinchi null bo'lmagan qiymatni qaytaradi

        SELECT COALESCE(null, 5, 78)  # 5 qiymatini qaytaradi
        SELECT COALESCE(email, 'Email kiritilmagan') FROM users;  # null qiymatning o'rniga boshqa yozuvni chiqarish uchun ishlatilgan
    NULLIF - 2 qiymatlar bir biriga teng bo'lsa null, teng bo'lmasa 1 - sining qiymatini qaytaradi

        SELECT NULLIF(4, 4);  # null qaytaradi
        SELECT NULLIF(6, 9);  # 6 ni qaytaradi
        SELECT 5 / NULLIF(0, 0);  # sonni 0 ga bo'lgandagi errordan qochish, null qaytaradi
        SELECT COALESCE(5 / NULLIF(0, 0), 0);  # bu 5 ni 0 ga bo'lganda error emas, 0 qaytaradi
14. TIMESTAMP va DATE
    NOW() - hozirgi vaqtni qaytaradi

        SELECT NOW();  # 2025-02-01 16:04:15.816876+05
        SELECT NOW()::DATE;  # 2025-02-01
        SELECT NOW()::TIME;  # 16:06:59.948165
    INTERVAL - bu orqali vaqt orqaga qaytarish va oldinga siljitish mumkin

        SELECT NOW() - INTERVAL '1 YEAR';  # 1 yil oldingi vaqtni qaytaradi
        SELECT NOW() + INTERVAL '5 MONTH';  # 5 oydan keyingi vaqtni qaytaradi
        SELECT (NOW() + INTERVAL '4 MONTH')::DATE;  # 4 oydan keyingi sanani qaytaradi
    EXTRACT() - asr, yil, oy, sana, minut, soat va sekund ni olsa bo'ladi (DOW - haftaning nechanchi kuni ekanini qaytaradi)

        SELECT EXTRACT(YEAR FROM NOW());  # hozirgi yilni qaytaradi
        SELECT EXTRACT(SECOND FROM (NOW() + INTERVAL '34 SECOND'));  # hozirgi vaqtdan 34 sekund keyingi sekundni qaytaradi
        SELECT EXTRACT(DOW FROM NOW()) AS hafta_kuni;  # hozir haftaning nechanchi kuniligini chiqaradi
        SELECT EXTRACT(CENTURY FROM NOW()) AS asr;  # hozirgi asrni qaytaradi
    AGE(date, date) - yoshni hisoblash uchun ishlatiladi

        SELECT name, surname, email, AGE(NOW(), birthday) AS age FROM users;
        SELECT AGE(NOW(), '2004-05-02') AS yoshim;  # 20 years 8 mons 30 days 16:28:20.743028
15. PRIMARY KEY - birlamchi kalit
    
       

    
    
       

       
   
            
   

   
   
   
            
      
