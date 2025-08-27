# Мини-БД магазина: SCD1/2/3, триггеры, купоны, soft-delete (SQLite)

Учебный проект по SQL/БД. Собрана компактная схема магазина с версионированием (SCD1/2/3), триггерами, купонами и мягким удалением продуктов.

## Схема
- users — SCD3 по стране (prev_country), аудит updated_at/row_version.
- products — SCD1 по имени + SCD2 по цене (в `product_price_history`), SCD3 prev_price, soft-delete (is_deleted).
- product_price_history — история цен (valid_from/valid_to, is_current).
- orders, order_items — позиции заказа, пересчёт сумм.
- coupons, coupon_redemptions — купоны, лимиты и лог редемпшнов.
- v_product_current_price — вью с текущей ценой.

## Триггеры
- products: touch, аудит, SCD2 при смене price, soft-delete (BEFORE DELETE → is_deleted=1).
- users: touch, prev_country при смене country.
- order_items: запрет добавлять soft-deleted товары, пересчёт amount и total заказа.
- orders: пересчёт скидки/total при изменении позиций/купона; валидация лимитов при оплате; редемпшны.

## Как запустить
- В Jupyter: открыть `db_demo.ipynb` и выполнить ячейки DB‑0 → DB‑4.
- Или перенести SQL в `schema.sql` и выполнить в sqlite3: