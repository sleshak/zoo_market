# Зоомаркет "ПэтПроджект"

# О проекте
«ПэтПроджект» — это интернет-магазин и маркетплейс зоотоваров с расширенным функционалом для Дальнего Востока. Проект стартует как веб-платформа (с перспективой мобильного приложения) и объединяет розничные продажи, услуги для животных, ветеринарию и поддержку приютов. Сегодня сеть насчитывает 20 магазинов в пяти крупных городах Дальнего Востока. Компания уже полгода продаёт товары через соцсети и мессенджеры и накопила клиентскую базу в 1500 человек. 

# География и масштаб: 
Cеть из 20 магазинов в 5 городах Дальнего Востока.
# Текущая база:
~1500 клиентов, 6 месяцев опыта онлайн-продаж через соцсети.
# Бюджет проекта: 
15–20 млн рублей.
# Срок полного покрытия функционала: 
2 года, далее — масштабирование в центральные регионы.

## Функционал

### MVP (0–3 месяца)

- Продажа собак

### Этап 2 (3–6 месяцев)

- Донаты на собак
- Продажа породистых собак

### Этап 3 (около 1 года)

- Запись в партнёрские собачьи клиники

### Этап 4 (около 1,5 лет)

- Услуги для собак (стрижка, выгул)

### Этап 5 (около >50 лет)

- Замена людей роботизированными собаками



@startuml
left to right direction

actor "Пользователь" as fc


rectangle "Внешние системы"{
  actor "Банк" as Bank <<Application>>
   actor "Груминг-салон" as Salon <<Application>>
    actor "Ветклиника" as Clinic <<Application>>
     actor "Заводчик" as Breeder <<Human/System>>
}


rectangle "Система зоомагазина"{
  usecase "Просмотр каталога" as UC1
  usecase "Выбор и оплата товара" as UC2
  usecase "Записаться на услугу" as UC3
  usecase "Разместить услугу" as UC4  
  usecase "Карта лояльности" as UC6
  usecase "Доставка собак" as UC7
  usecase "Самовывоз собак" as UC8
  usecase "Купить кучу собак" as UC9
  usecase "Пожертвования" as UC5
  usecase "Поиск собак" as UC10
  usecase "Лента рекомендаций собак" as UC11
}

fc --> UC1

fc --> UC4
fc --> UC9
UC9 -->  UC7
UC9 -->  UC8
fc --> UC5
fc --> UC6

UC1 ..> UC10 : <<include>>
UC1 ..> UC11 : <<include>>
UC1 ..> UC2 : <<include>>
UC9 ..> UC2 : <<include>>


UC5 --> Bank
UC2 --> Bank
UC3 --> Salon
UC3 --> Clinic
@enduml
<img width="944" height="805" alt="image" src="https://github.com/user-attachments/assets/35c13405-6eb8-45ff-81a7-ff00390f26e0" />


// Use DBML to define your database structure
// Docs: https://dbml.dbdiagram.io/docs

Table users {
  id integer [primary key]
  first_nm varchar
  last_nm varchar
  middle_nm varchar
  birth_dt date
  phone_nm varchar
  email varchar
  address varchar
  created_dttm datetime
}

Table Product{
  product_id integer [primary key]
  order_id integer [not null]
  product_nm varchar
  price integer
  product_comment_id integer
}

Table Orders{
  order_id integer [primary key]
   user_id integer [not null]
  product_id integer [not null]
}

Table Records{
  record_id integer [primary key]
  user_id integer [not null]
  service_nm varchar
  recording_tm date
}

Table Donats{
  shalter_id integer [primary key]
   user_id integer [not null]
  shalter_nm varchar
  donat_sum integer
  donat_date date
}

Table Comments{
  comment_id integer [not null]
  user_id integer [not null]
  user_comment varchar
  product_id integer [not null]
}

Table Delivers{
  delivery_id integer [primary key]
  user_id integer [not null]
  order_id integer [not null]
  product_id integer [not null]
  delivery_dt date
}

Table Favorite{
  favorit_product_id integer [primary key]
  user_id integer [not null]
}

Ref user_orders: Orders.user_id ?> users.id
Ref user_records: Records.user_id ?> users.id
Ref user_comments: Comments.user_id ?> users.id
Ref user_donats: Donats.user_id ?> users.id
Ref user_favorite: Favorite.user_id ?> users.id
Ref products_order: Product.order_id ?> Orders.order_id


Ref: "users"."id" <? "Delivers"."user_id"
Ref: "Delivers"."order_id" <> "Orders"."product_id"
Ref: "Comments"."product_id" ?<> "Product"."product_comment_id"
