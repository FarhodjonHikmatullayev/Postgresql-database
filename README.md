# Postgresql-database
1. Ma'lumotlar ba'zasini yaratish
   
       CREATE DATABASE database_name;
   Ma'lumotlar ba'zasiga ulanish

       \c database_name
   Ma'lumotlar ba'zasidagi barcha tablelarni ko'rish

       \d dataabase_name
   Barcha yaratilgan ma'lumotlar bazalarini ko'rish

       \l
   Ma'lumotlar ba'zasini o'chirib yuborish

       DROP DATABASE database_name;
2. Table yaratish va uni o'chirish
   Table yaratish
   
       CREATE TABLE table_name (name_column data_type adds);
   For example

       CREATE TABLE users (
            id INT,
            name VARCHAR(60),
            email VARCHAR(60),
            birthday TIMESTAMP,
       );
   
            
      
