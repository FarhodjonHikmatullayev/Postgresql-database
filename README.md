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
   Ma'lumotlarni qator shaklidan ustun shakliga, ustun shaklidan qator shakliga o'zgartirish uchun ishlatiladi

       \x
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
            email VARCHAR(60) NOT NULL UNIQUE,
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

        SELECT 'str1' || 'str2';  # ikkala strni qo'shib chiqaradi 'str1str2' shaklida
        SELECT CONCAT('str1', 'str2');  # bu ham strlarni qo'shish uchun ishlatiladi
        SELECT UPPER('str');  # katta harf qilib beradi 'STR' shaklida
        SELECT LOWER('STR');  # kichik harf qilib beradi 'str' shaklida
        SELECT INITCAP('PostgreSQL database');  # 'Postgresql Database' shaklida qaytaradi Capitalize() funksiyasidek
        SELECT LENGTH('str');  # string uzunligini qaytaradi ya'ni 3 qaytadi
        SELECT MD5('str');  # stringni shiflash uchun ishlatiladi
        SELECT id, STRPOS(last_name, 'yev') FROM students;  # studentlar familiyasida 'yev' borlarini chiqaradi

        SELECT INITCAP(CONCAT(first_name, ' ', last_name)) FROM students;  # example
        SELECT INITCAP(CONCAT_WS(' ', first_name, last_name, birthday)) FROM students;  'First_name Last_name Birthday' shaklida chiqadi, bunda birinchi argument oradagi tashlanadigan joylarni bildiradi
    AS (ALIAS) - ustunni nomlash

        SELECT 49 * 0.20 AS foiz;  # 49 ning 20 foizini topadi va column namega foiz deb yozadi
14. COALESCE va NULLIF
    COALESCE - birinchi null bo'lmagan qiymatni qaytaradi

        SELECT COALESCE(null, 5, 78)  # 5 qiymatini qaytaradi
        SELECT COALESCE(email, 'Email kiritilmagan') FROM users;  # null qiymatning o'rniga boshqa yozuvni chiqarish uchun ishlatilgan
    NULLIF - 2 qiymatlar bir biriga teng bo'lsa null, teng bo'lmasa 1 - sining qiymatini qaytaradi

        SELECT NULLIF(4, 4);  # null qaytaradi
        SELECT NULLIF(6, 9);  # 6 ni qaytaradi
        SELECT 5 / NULLIF(0, 0);  # sonni 0 ga bo'lgandagi errordan qochish, null qaytaradi
        SELECT COALESCE(5 / NULLIF(0, 0), 0);  # bu 5 ni 0 ga bo'lganda error emas, 0 qaytaradi
15. TIMESTAMP va DATE
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
16. CHECK - shartni tekshirish uchun qo'llaniladi undan keyingi qavslar ichiga shart yoziladi

        ALTER TABLE users ADD CONSTRAINT gender_constraint CHECK (gender='female' OR gender='male');  # endi insert qilinayotganda gender male yoki female bo'lishligiga tekshiradi
17. DELETE - ma'lumotlarni jadvaldan o'chirib yuborish uchun ishlatiladi
    Tabledagi barcha ma'lumotlarni o'chirish

        DELETE FROM db_name;
    Tanlangan qatorlarni o'chirish

        DELETE FROM users WHERE name = 'Bexruz';
18. UPDATE - jadvaldagi ma'lumotlarni o'zgartirish
    SINTAKSIS

        UPDATE table_name SET coll_name = 'yangi qiymat' WHERE shart;
    For example

        UPDATE users SET name = 'Farhod';  # users jadvalidagi barcha namelarni Farhodga o'zgartirib chiqadi
        UPDATE users SET name = 'Farhod' WHERE name = 'Bexruz';  # users jadvalidagi Bexruz ismli ma'lumotlarning ismlarini Farhodga o'zgartirib chiqadi
        UPDATE users SET name = 'Farhod', male = 'Male' WHERE name = 'Aziza';
19. FOREIGN KEY, JOIN va RELATIONSHIPs
    Foreign key (for example) - bu yerda car jadvalini users jadvalidan oldin yaratib olishimiz kerak chunki users jadvalida car_id fieldi orqali car jadvalini foreign key qilyapmiz

        CREATE TABLE car(
             id BIGSERIAL NOT NULL PRIMARY KEY,
             make VARCHAR(100) NOT NULL,
             model VARCHAR(100) NOT NULL,
             price NUMERIC(19,2) NOT NULL
        );

        CREATE TABLE users(
             id BIGSERIAL NOT NULL PRIMARY KEY,
             first_name VARCHAR(50) NOT NULL,
             last_name VARCHAR(50) NOT NULL,
             email VARCHAR(100),
             birthday DATE NOT NULL,
             country VARCHAR(60) NOT NULL,
             car_id BIGINT REFERENCES car(id),   # car jadvalining id fieldiga yo'naltiryapmiz
             UNIQUE(car_id)   # car_id ni UNIQUE bo'lishini istadik shuning uchun CONSTRAINT qo'shdik
        );
20. INNER JOIN - jadvallar orasida relationship o'rnatilganda relationship o'rnatilgan fieldlarning qiymatlari mavjudlarini chiqarish uchun ishlatiladi, ya'ni a va b tablelarning kesishmasi qaytadi


        SELECT * FROM users
        JOIN car ON users.car_id = car.id;

    Yana ham tushunarliroq qilish

        SELECT users.first_name, car.make, car.model, car.price FROM USERS
        JOIN car ON users.car_id = car.id;
21. LEFT JOIN - o'zaro relationshipga ega a va b jadvallarining faqat a sidagi ma'lumotlar to'liq qaytadi
    FOR EXAMPLE

        SELECT * FROM users
        LEFT JOIN car ON users.car_id = car.id;   # bu yerda users jadvali to'liq chiqariladi
22. RIRGHT JOIN - o'zaro relationshipga ega a va b jadvallarining faqat b sidagi ma'lumotlar to'liq qaytariladi
    FOR EXAMPLE

        SELECT * FROM users
        RIGHT JOIN car on users.car_id = car.id;   # bu yerda car jadvali to'liq chiqariladi
23. Ma'lumotlarni CSV fayliga import qilib olish

        \copy (SELECT * FROM users LEFT JOIN car ON car.id = users.car_id) TO 'C:\Users\User\Desctip\result.csv' DELIMITER ',' CSV HEADER;
24. ALTER TABLE
    1. Ustun qo'shish

           ALTER TABLE table_name
           ADD COLUMN column_name data_type;
       Fore example

           ALTER TABLE employees
           ADD COLUMN age INTEGER;
    2. Ustunni o'chirish


           ALTER TABLE table_name
           DROP COLUMN column_name;
       Fore example

           ALTER TABLE employees
           DROP COLUMN age;
    3. Ustun tipini o'zgartirish

           ALTER TABLE table_name
           ALTER COLUMN column_name SET DATA TYPE new_data_type;
       Fore example

           ALTER TABLE employees
           ALTER COLUMN age SET DATA TYPE SMALLINT;
    4. Ustun nomini o'zgartirish

           ALTER TABLE table_name
           RENAME COLUMN old_column_name TO new_column_name;
       Fore example

           ALTER TABLE employees
           RENAME COLUMN age to employee_age;
    5. Jadval nomini o'zgartirish


           ALTER TABLE old_table_name
           RENAME TO new_table_name;
       Fore example

           ALTER TABLE employees
           RENAME TO staff;
    6. Ustunlarga cheklovlar qo'shish

           ALTER TABLE table_name
           ADD CONSTRAINT constraint_name CHECK (condition);
       Fore example

           ALTER TABLE employees
           ADD CONSTRAINT check_age CHECK (age >= 18);
    7. Ustunlardan cheklovlarni o'chirish

           ALTER TABLE table_name
           DROP CONSTRAINT constraint_name;
       Fore example

           ALTER TABLE employees
           DROP CONSTRAINT check_age;
25. Sub query examples
    Sinflar jadvali va jadvalda harbir klassdagi o'quvchilar soni ham chiqariladi 
    
        SELECT *, {
            SELECT COUNT(*) FROM students WHERE students.class_id = classes.id
        } FROM classes;
    Barcha o'quvchiga ega bo'lgan sinflarni chiqarish

        SELECT * FROM classes
        WHERE id IN {SELECT class_id FROM students};
26. UNION - jadvallarni vertikal birlashtirish
    Example (bu yerda asosiy qoida: ustunlarning ma'lumot tiplari va soni bir xil bo'lishi kerak)

        SELECT id, first_name FROM students
        UNION
        SELECT id, name FROM classes;
    Example 2(Agar bir xil qiymatlarga ega qatorlar har ikkala to'plamda ham kelsa ular faqat bir marta chiqariladi)

        SELECT id, first_name FROM students WHERE class_id = 1
        UNION
        SELECT id, first_name FROM students WHERE class_id = 2;

27. Backup and Restore databases
    Bin ga o'tib olish

        cd C:\Program Files\PostgreSQL\16\bin
    .dump file yuklab olish
    
        pg_dump -Fc -h host -U user_name -f file_name.dump db_name
        pg_dump -Fc -h 127.0.0.1 -U postgres -f team.dump team  # example
    Restore qilamiz

        pg_restore -d new_db_name -h host -U new_db_user dump_file
        pg_restore -d team3 -h 127.0.0.1 -U postgres team.dump # example
        
       
       
    
    
              

        
    
       

    
    
       

       
   
            
   

   
   
   
            
      
