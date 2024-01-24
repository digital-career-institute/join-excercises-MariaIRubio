# java-DB-AdvancedSQL-join-excercises

Join excercises


1. Select all product names for suppliers from USA.
```sql
SELECT 
  product_id,
  product_name,
  s.country
FROM
  products p
LEFT JOIN 
  suppliers s ON p.supplier_id = s.supplier_id
WHERE 
  s.country = 'USA';
```
3. Select order id, order date, employee first and last name for employee whose last name starts with 'D' and for order that was shipped to Germany.
```sql
SELECT DISTINCT
  o.order_id, 
  o.order_date, 
  e.first_name, 
  e.last_name,
  o.ship_country
FROM 
  orders o
LEFT JOIN 
  employees e ON o.employee_id = e.employee_id
WHERE
  e.last_name LIKE 'D%'
AND 
  o.ship_country = 'Germany';
```
4. Select all orders shipped by United Package for orders sooner than '1996-08-12'
```sql
SELECT *
FROM 
  orders 
WHERE 
  ship_via IN (SELECT shipper_id FROM shippers WHERE company_name = 'United Package')
AND 
  order_date < '1996-08-12';
```
6. Select shippers company name that doesn't have any orders assigned.
```sql
SELECT 
  company_name,
  order_id
FROM
  shippers s
LEFT JOIN 
  orders o ON s.shipper_id = o.ship_via
WHERE
  order_id IS NULL;
```
8. Select order ids that contains product from category 'Beverages' ordered '1996-08-14'.
```sql
SELECT 
  od.order_id, 
  p.product_name,
  c.category_name,
  o.order_date
FROM 
  order_details od
LEFT JOIN
  products p ON od.product_id = p.product_id
LEFT JOIN 
  categories c ON p.category_id = c.category_id
JOIN
  orders o ON od.order_id = o.order_id
WHERE
  c.category_name = 'Beverages'
AND
  o.order_date  = '1996-08-14';
```
9. Select product names and availability of products supplied by 'Lyngbysild' from category 'Seafood'
```sql
SELECT 
  p.product_name,
  p.units_in_stock,
  c.category_name,
  s.company_name
FROM 
  products p
LEFT JOIN 
  categories c ON p.category_id = c.category_id
LEFT JOIN
  suppliers s ON p.supplier_id = s.supplier_id
WHERE
  s.company_name = 'Lyngbysild'
AND 
  c.category_name = 'Seafood';
```
10. Select last names of employees assigned to 'Northern' region.
```sql
SELECT DISTINCT
  e.last_name,
  r.region_description
FROM 
  employees e
LEFT JOIN 
  employee_territories et ON e.employee_id = et.employee_id
LEFT JOIN
  territories t ON et.territory_id = t.territory_id
LEFT JOIN 
  region r ON t.region_id = r.region_id
WHERE
  region_description = 'Northern';
```
11. Select employee id and his boss last name for those who have orders with date later than '1996-07-18'.
if an employee doesn't have a boss don't include him.
```sql
SELECT DISTINCT
  (e.employee_id),
  e.last_name,
  e.first_name,
  e2.last_name AS boss_last_name,
  COUNT(o.order_date) AS total_orders
FROM 
  employees e
RIGHT JOIN 
  employees e2 ON e.employee_id = e2.employee_id
LEFT JOIN 
  orders o ON e.employee_id = o.employee_id
WHERE
  o.order_date > '1996-07-18'
GROUP BY
  (e.employee_id),
  e.last_name,
  e.first_name,
  e2.last_name
ORDER BY
e.employee_id; 
```
12. List the employees in the warehouse with orders that are not shipped yet.
```sql
SELECT DISTINCT
  e.employee_id,
  e.first_name, 
  e.last_name,
  o.*
FROM 
  employees e
LEFT JOIN 
  orders o ON e.employee_id = o.employee_id
WHERE
  o.shipped_date IS NULL;
```
13. Calculate the price for each product with its name in each order after discount is applied. 
```sql
SELECT 
  od.*,
  p.product_name,
  ROUND(CAST((od.unit_price - od.discount)*od.quantity AS NUMERIC),2) AS final_price
FROM 
  products p
LEFT JOIN
  order_details od ON p.product_id = od.product_id
ORDER BY
final_price DESC;
```
