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
5. 
            
   

   
   
   
            
      
