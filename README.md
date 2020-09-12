# SQL
INSERT INTO public.user_columns(
 table_name, column_name, data_type, column_name_vi, enum_type, categorial_col, condition_col, timesserial_col, values_col)
	VALUES ('mp_price_material', 'size', 'varchar', 'kích cỡ', 'sql', 1, 1, 0, 0),
		('mp_price_material', 'category', 'varchar', 'phân loại 1', 'sql', 1, 1, 0, 0),
		('mp_price_material', 'standard_size', 'varchar', 'kích cỡ chuẩn hóa', 'sql', 1, 1, 0, 0),
		('mp_price_material', 'standard_category', 'varchar', 'phân loại 1 chuẩn hóa', 'sql', 1, 1, 0, 0),
		('mp_price_material', 'location', 'varchar', 'vị trí nhà máy', 'sql', 1, 1, 0, 0),
		('mp_price_material', 'price', 'float', 'số lượng', 'sql', 0, 0, 0, 1),
		('mp_price_material', 'standard_value', 'float', 'số lượng chuẩn hóa', 'sql', 0, 0, 0, 1),
		('mp_price_material', 'standard_type', 'varchar', 'loại nguyên liệu', 'sql', 1, 1, 0, 0),
		('mp_price_material', 'unit', 'varchar', 'đơn vị tính', 'sql', 1, 1, 0, 0),
		('mp_price_material', 'currency', 'varchar', 'loại tiền', 'sql', 1, 1, 0, 0),
		
		('mp_price_material', 'active_date', 'varchar', 'ngày (year-month-day)', 'sql', 1, 1, 1, 0),
		('mp_price_material', 'day', 'varchar', 'ngày (day/month/year)', 'sql', 1, 0, 1, 0),
		('mp_price_material', 'week', 'varchar', 'tuần (week/month/year)', 'sql', 1, 0, 1, 0),
		('mp_price_material', 'month', 'varchar', 'tháng (month/year)', 'sql', 1, 0, 1, 0),
		('mp_price_material', 'quarter', 'varchar', 'quý (quarter/year)', 'sql', 1, 0, 1, 0),
		('mp_price_material', 'quarter_number', 'quarter', 'tuần (number)', 'sql', 1, 0, 1, 0),
		('mp_price_material', 'month_number', 'month', 'tháng (number)', 'sql', 1, 0, 1, 0),
		('mp_price_material', 'year', 'year', 'năm (number)', 'sql', 1, 0, 1, 0);
---------------------------------------------------------------------------------------------------------
SELECT 
	location,
	standard_type,
	producthierrachy,
	standard_size,  
	unit,
	active_date, 
	plan_delivery_date,
	standard_value, 
	day,
	week, 
	quarter,
	month,
	year,
	month_number, 
	quarter_number
		FROM public.mp_material_ordering;
		------------------------------------------------------------
select distinct standard_size from public.mp_material_ordering
-------------------------------------------------------------
select distinct standard_type from public.mp_material_ordering
-------------------------------------------------------------
select distinct standard_type, commodity, filename from public.mp_material_ordering where standard_type = 'None'
-----------------------------------------------------------------------
select distinct standard_size,table_name, type, low_price, high_price, active_date from tra_pham_urnerbarry_shrimp_prices
where  SUBSTRING(standard_size, '[0-9]/[0-9]') != '' and standard_size not ilike '%+%' 
and standard_size not like '%.00%' and table_name like 't4%' 
create view tra_pham_urnerbarry_shrimp_prices as
SELECT replace(urnerbarry_shrimp_prices_weekly.size::text, '-'::text, '/'::text) AS standard_size,
 urnerbarry_shrimp_prices_weekly.table_name,
 urnerbarry_shrimp_prices_weekly.type,
    urnerbarry_shrimp_prices_weekly.low_price,
    urnerbarry_shrimp_prices_weekly.high_price,
    urnerbarry_shrimp_prices_weekly.date_time AS active_date,
    (((date_part('year'::text, urnerbarry_shrimp_prices_weekly.date_time) || '/'::text) || lpad(date_part('month'::text, urnerbarry_shrimp_prices_weekly.date_time)::text, 2, '0'::text)) || '/'::text) || lpad(date_part('week'::text, urnerbarry_shrimp_prices_weekly.date_time)::text, 2, '0'::text) AS week,
    (date_part('year'::text, urnerbarry_shrimp_prices_weekly.date_time) || '/'::text) || lpad(date_part('month'::text, urnerbarry_shrimp_prices_weekly.date_time)::text, 2, '0'::text) AS month,
    (date_part('year'::text, urnerbarry_shrimp_prices_weekly.date_time) || '/'::text) || date_part('quarter'::text, urnerbarry_shrimp_prices_weekly.date_time) AS quarter,
    date_part('year'::text, urnerbarry_shrimp_prices_weekly.date_time) AS year
   FROM urnerbarry_shrimp_prices_weekly where table_name not like 't1%' and table_name not like 't5%' and SUBSTRING(replace(urnerbarry_shrimp_prices_weekly.size::text, '-'::text, '/'::text), '[0-9]/[0-9]') != '' and replace(urnerbarry_shrimp_prices_weekly.size::text, '-'::text, '/'::text) not ilike '%+%' 
   and replace(urnerbarry_shrimp_prices_weekly.size::text, '-'::text, '/'::text) not like '%.00%' 
-------------------------------
SELECT size, cast(split_part(size, '/',1) as int) as stt  , sum("values") as tong from material where "type_standard" = 'Sú Vỏ'group by size order by stt
------------------------------
SELECT 
	CASE
      WHEN size~E'^\\d+$' THEN
         CAST (size AS INTEGER)
      ELSE
         0
      END as size1
FROM
   public.material_receipt_mpcm_3;
   
select size from public.material_receipt_mpcm_3
-----------------------------------
