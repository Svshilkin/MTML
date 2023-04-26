# Лабораторная работа №2. Исследование факторов и организация работы с таблицами данных

Набор данных: data_purch.xlsx.

Статистика о государственных закупках лекарственных препаратов в 2021 году согласно Единой информационной системе в сфере закупок.
Информация  об  одной  процедуре  может  занимать  более  одной  строки.  
Если    в    рамках    процедуры    производилась    закупка    только    одного лекарственного препарата, 
то такая процедура в таблице будет встречаться один раз и занимать одну строку. 
Если в одной процедуре содержалось n закупочных  позиций,  то  информация  об  этой  процедуре  будет  занимать  в таблице n строк.

###### Загрузка данных

![1](https://user-images.githubusercontent.com/114734035/230467335-da78702d-2684-46c8-b08a-b409ebec0cde.jpg)

######  Описание полей:

* first_pub_date – дата публикации процедуры
* final_protocol_pub_date – дата завершения процедуры
* purchase_number – идентификатор процедуры
* collecting_start_date – дата начала сбора заявок от потенциальных поставщиков
* collecting_end_date – дата окончания сбора заявок от потенциальных поставщиков
* placing_base_name – форма проведения торгов (конкурс, аукцион и т.п.)
* cust_reg – регион заказчика процедуры
* lot_price_correct – максимально возможная, объявленная заказчиком, цена за все препараты в закупке
* drug_mnn_ext_code – идентификатор лекарственного препарата
* drug_mnn_name – наименование лекарственного препарата (их меньше, чем drug_mnn_ext_code)
* drug_qty – объем закупаемого лекарственного препарата
* drug_price – цена за единицу объема закупаемого лекарственного препарата
* drug_position_price – цена за всю позицию закупаемого лекарственного препарата
* ftg – класс закупаемого лекарственного препарата
* is_abnd – фиктивная переменная: 1 – закупка не состоялась, 0 – состоялась
* is_znvlp – фиктивная переменная: 1 – в закупке содержится хотя бы 1 препарат из списка ЖВЛП, 0 – не содержатся
* is_narcotic – фиктивная переменная: 1 – в закупке содержится хотя бы 1 препарат, в составе которого есть наркотические вещества, 0 – не содержатся
* is_msp_purchase – фиктивная переменная: 1 – закупка предназначена только для субъектов малого и среднего предпринимательства, 0 – не предназначена
* is_povt – фиктивная переменная: 1 – эту процедуру заказчик вынужден повторить, 0 – не повторная
* is_dif – фиктивная переменная: 1 – в закупке содержится хотя бы 1 препарат, являющийся дефицитным, 0 – не содержатся
* app_amount_absolute_correct – сумма залога
* advance_sum_correct – сумма аванса

По моему мнению, из имеющихся показателей на вероятность незакрытия закупки сильнее всего влияют:
- `cust_reg`: регион заказчика процедуры
- `placing_base_name`: форма проведения торгов
- `lot_price_correct`: максимально возможная, объявленная заказчиком, цена за все препараты в закупке
- `drug_qty`: объём закупаемого лекарственного препарата
- `drug_price`: цена за единицу объема закупаемого лекарственного препарата
- `drug_position_price`: цена за всю позицию закупаемого лекарственного препарата
- `is_znvlp`: содержится ли препарат в списке ЖВЛП
- `is_narcotic` – есть ли в составе наркотические вещества
- `is_dif` – есть ли в закупке хотя бы 1 препарат, являющийся дефицитным
- `app_amount_absolute_correct` – сумма залога
- `advance_sum_correct` – сумма аванса

#### Группировка

![2](https://user-images.githubusercontent.com/114734035/230467373-dac5bdbb-0315-4994-84a7-ab23839f3585.jpg)

![3](https://user-images.githubusercontent.com/114734035/230467395-6c1ffcd0-1127-49b0-afdc-36313ec1f261.jpg)

### Формирование новых факторов

##### Фактор, полученный с помощью метода главных компонент:
- `PCA_1` - фактор полученный при помощи метода главных компонент по 1-ой компоненте.

##### Факторы, полученные с помощью замен и арифметических операций:
- `collecting_duration` - продолжительность сбора заявок (`collecting_end_date` минус `collecting_start_date`).
- Преобразование фактора `placing_base_name` в набор фиктивных переменных.

### Разделение на обучающую и тестовую выборку

За целевую переменную берём *is_abnd*

![4](https://user-images.githubusercontent.com/114734035/230467429-c98e98db-b25e-446a-a59f-3dad34c24a20.jpg)

#### Оценка важности факторов на основе метода "Дерево решений" 

![5](https://user-images.githubusercontent.com/114734035/230467480-d59614a8-5260-4116-bced-5c2f47129f47.jpg)

#### Построение модели логистической регрессии

Точность модели на обучающей выборке: 0.5055459544383346
Точность модели на тестовой выборке: 0.5083582089552239
Точность модели составляет близко к 0.5. Это равносильно случайному угадыванию. Точность модели является крайне низкой.
 
 
 




