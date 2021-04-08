## Лаба 5. Спрогнозировать отток клиентов банка по историческим данным на языке Scala

### Дедлайн

⏰ 29 марта 2020 года, 23:59.
  
#### Дедлайн Github  
  
Четверг, 01 апреля, 23:59

### Задача

Борьба с оттоком — одна из самых важных задач для розничного банковского бизнеса.

Эта задача может считаться наиболее актуальной для расчётных безрисковых продуктов.

Понятие оттока формализуется во внутренней методике Банка, которая на основе поведенческих характеристик (уровень оборотов и остатков на счетах и т.д.) клиента, определяет можно ли считать клиента "отточным" на отчётную дату.

Способность своевременно и с достаточным уровнем точности определять клиентов, склонных к оттоку, и коммуницировать с ними с наиболее релевантным предложением — это подход, реализуемый в Банке. Результатом такой коммуникации могут быть: выявление причин низкой активности клиента, снятие негатива клиента по отношение к Банку, его продуктам, ре-онбординг текущего продукта либо его замена на более соответствующий потребностям клиента.

Задача сводится к построению классификатора, позволяющего ранжировать клиентов по их склонности к оттоку. Для исходящего обзвона будет отобрано некоторое кол-во клиентов, в соответствие с текущей загруженностью каналов, максимально склонных к оттоку.

### Обработка данных на вход

**Все нужные вам файлы расположены на master в /share/slaba05data**. Вы можете считывать их в своей программе прямо оттуда.

Вам понадобятся два файла:

- `lab05_train.csv` — тренировочная выборка с известными значениями оттока.
- `lab05_test.csv` — проверочная выборка, значения оттока для которой вам и надо предсказать.

Итак, перед вами набор данных, состоящий из 3-х поколений (3 последовательных отчетных месяцев) активных клиентов, часть из которых спустя 2 месяца стали неактивными, т.е. ушли в отток (поле `TARGET` со значением `1`).

<table>
 <col>
 <col>
 <tr>
  <td>
  <div><span>VARIABLE</span></td>
  <td ><span>DESCRIPTION</span></td>
 </tr>
  <tr>
    <td><span><b>ID</b></span></td>
    <td><span><b>Уникальный ID записи</b></span></td>
 </tr>
 <tr>
  <td><span>AGE</span></td>
  <td><span>Возраст клиента (в месяцах)</span></td>
 </tr>
 <tr>
  <td><span>AMOUNT_RUB_ATM_PRC</span></td>
  <td rowspan=4 class=xl67><span style='white-space:pre-wrap'>Доля суммы транзакций по МСС ко сумме всех транзакций за период (в руб.)</span></td>
 </tr>
 <tr>
  <td><span>AMOUNT_RUB_CLO_PRC</span></td>
 </tr>
 <tr>
  <td><span>AMOUNT_RUB_NAS_PRC</span></td>
 </tr>
 <tr>
  <td><span>AMOUNT_RUB_SUP_PRC</span></td>
 </tr>
 <tr>
  <td><span>APP_CAR</span></td>
  <td><span>Признак наличия автомобиля</span></td>
 </tr>
 <tr>
  <td><span>APP_COMP_TYPE</span></td>
  <td><span>Тип компании (основной
  работы клиента)</span></td>
 </tr>
 <tr>
  <td><span>APP_DRIVING_LICENSE</span></td>
  <td><span>Признак наличия
  водительского удостоверения</span></td>
 </tr>
 <tr>
  <td><span>APP_EDUCATION</span></td>
  <td><span>Образование клиента</span></td>
 </tr>
 <tr>
  <td><span>APP_EMP_TYPE</span></td>
  <td><span>Тип работы (занятость)</span></td>
 </tr>
 <tr>
  <td><span>APP_KIND_OF_PROP_HABITATION</span></td>
  <td><span>Вид собственности жилья</span></td>
 </tr>
 <tr>
  <td><span>APP_MARITAL_STATUS</span></td>
  <td><span>Семейное положение</span></td>
 </tr>
 <tr>
  <td><span>APP_POSITION_TYPE</span></td>
  <td><span>Тип должности</span></td>
 </tr>
 <tr>
  <td><span>APP_REGISTR_RGN_CODE</span></td>
  <td><span>Код региона регистрации</span></td>
 </tr>
 <tr>
  <td><span>APP_TRAVEL_PASS</span></td>
  <td><span>Признак &quot;есть
  заграничный паспорт&quot;</span></td>
 </tr>
 <tr>
  <td><span>AVG_PCT_DEBT_TO_DEAL_AMT</span></td>
  <td><span>Отношение задолженности к
  сумме кредита (среднее по аннуитетам)</span></td>
 </tr>
 <tr>
  <td><span>AVG_PCT_MONTH_TO_PCLOSE</span></td>
  <td><span>Доля оставшегося срока
  кредита (среднее по аннуитетам)</span></td>
 </tr>
 <tr>
  <td><span>CLNT_JOB_POSITION</span></td>
  <td><span>Должность клиента</span></td>
 </tr>
 <tr>
  <td><span>CLNT_JOB_POSITION_TYPE</span></td>
  <td><span>Тип позиции</span></td>
 </tr>
 <tr>
  <td><span>CLNT_SALARY_VALUE</span></td>
  <td><span>Зарплата клиента</span></td>
 </tr>
 <tr>
  <td><span>CLNT_SETUP_TENOR</span></td>
  <td><span>Срок жизни клиента в банке</span></td>
 </tr>
 <tr>
  <td><span>CLNT_TRUST_RELATION</span></td>
  <td><span>Тип созаемщика</span></td>
 </tr>
 <tr>
  <td><span>CNT_ACCEPTS_MTP</span></td>
  <td><span>Число &nbsp;принятый
  предложений в каналах / группах кампаний</span></td>
 </tr>
 <tr>
  <td><span>CNT_ACCEPTS_TK</span></td>
  <td></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_ATM_TENDENCY1M</span></td>
  <td rowspan=10><span>Тренд по кол-ву
  транзакции в разрезе MCC (за 1 или 3 месяца)</span></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_ATM_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_AUT_TENDENCY1M</span></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_AUT_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_CLO_TENDENCY1M</span></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_CLO_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_MED_TENDENCY1M</span></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_MED_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_SUP_TENDENCY1M</span></td>
 </tr>
 <tr>
  <td><span>CNT_TRAN_SUP_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>CR_PROD_CNT_CC</span></td>
  <td rowspan=6><span>Кол-во открытых
  продуктов за отчетный период (по категориям продуктов)</span></td>
 </tr>
 <tr>
  <td><span>CR_PROD_CNT_CCFP</span></td>
 </tr>
 <tr>
  <td><span>CR_PROD_CNT_IL</span></td>
 </tr>
 <tr>
  <td><span>CR_PROD_CNT_PIL</span></td>
 </tr>
 <tr>
  <td><span>CR_PROD_CNT_TOVR</span></td>
 </tr>
 <tr>
  <td><span>CR_PROD_CNT_VCU</span></td>
 </tr>
 <tr>
  <td><span>DEAL_GRACE_DAYS_ACC_AVG</span></td>
  <td rowspan=3><span>Показатели по
  грейсу за отчетный период</span></td>
 </tr>
 <tr>
  <td><span>DEAL_GRACE_DAYS_ACC_MAX</span></td>
 </tr>
 <tr>
  <td><span>DEAL_GRACE_DAYS_ACC_S1X1</span></td>
 </tr>
 <tr>
  <td><span>DEAL_YQZ_IR_MAX</span></td>
  <td rowspan=4><span>Максимальная /
  минимальная процентная ставка по револьверам / аннуитетам за отчетный период</span></td>
 </tr>
 <tr>
  <td><span>DEAL_YQZ_IR_MIN</span></td>
 </tr>
 <tr>
  <td><span>DEAL_YWZ_IR_MAX</span></td>
 </tr>
 <tr>
  <td><span>DEAL_YWZ_IR_MIN</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_ACT_DAYS_ACC_PCT_AVG</span></td>
  <td rowspan=6><span>Показатели по
  времени активности за отчетный период (по кредитным договорам)</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_ACT_DAYS_PCT_AAVG</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_ACT_DAYS_PCT_CURR</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_ACT_DAYS_PCT_TR</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_ACT_DAYS_PCT_TR3</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_ACT_DAYS_PCT_TR4</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_AMT_MONTH</span></td>
  <td rowspan=11><span>Прочие
  продуктовые параметры за отчетный период (кредитным договорам)</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_DELINQ_PER_MAXYQZ</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_DELINQ_PER_MAXYWZ</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_GRACE_DAYS_PCT_MED</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_TENOR_MAX</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_TENOR_MIN</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_USED_AMT_AVG_YQZ</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_USED_AMT_AVG_YWZ</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_YQZ_CHRG</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_YQZ_COM</span></td>
 </tr>
 <tr>
  <td><span>LDEAL_YQZ_PC</span></td>
 </tr>
 <tr>
  <td><span>MAX_PCLOSE_DATE</span></td>
  <td><span>Кол-во месяцев до даты
  планового закрытия (макс. по аннуитетам)</span></td>
 </tr>
 <tr>
  <td><span>MED_DEBT_PRC_YQZ</span></td>
  <td rowspan=2><span>Медиана доли
  задолженности по аннуитетам / револьверам</span></td>
 </tr>
 <tr>
  <td><span>MED_DEBT_PRC_YWZ</span></td>
 </tr>
 <tr>
  <td><span>PACK</span></td>
  <td><span>Пакет услуг</span></td>
 </tr>
 <tr>
  <td><span>PRC_ACCEPTS_A_AMOBILE</span></td>
  <td rowspan=8><span>% отклика в
  каналах / продуктовых группах</span></td>
 </tr>
 <tr>
  <td><span>PRC_ACCEPTS_A_ATM</span></td>
 </tr>
 <tr>
  <td><span>PRC_ACCEPTS_A_EMAIL_LINK</span></td>
 </tr>
 <tr>
  <td><span>PRC_ACCEPTS_A_MTP</span></td>
 </tr>
 <tr>
  <td><span>PRC_ACCEPTS_A_POS</span></td>
 </tr>
 <tr>
  <td><span>PRC_ACCEPTS_A_TK</span></td>
 </tr>
 <tr>
  <td><span>PRC_ACCEPTS_MTP</span></td>
 </tr>
 <tr>
  <td><span>PRC_ACCEPTS_TK</span></td>
 </tr>
 <tr>
  <td><span>REST_AVG_CUR</span></td>
  <td><span>Средние остатки по текущим
  счетам</span></td>
 </tr>
 <tr>
  <td><span>REST_AVG_PAYM</span></td>
  <td><span>Средние остатки по
  зарплатным счетам</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_CC_1M</span></td>
  <td rowspan=11><span>Тренд
  среднемесяцных остатков по продуктам за отчетный период ( за 1 или 3 месяца)</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_CC_3M</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_CUR_1M</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_CUR_3M</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_FDEP_1M</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_FDEP_3M</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_IL_1M</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_IL_3M</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_PAYM_1M</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_PAYM_3M</span></td>
 </tr>
 <tr>
  <td><span>REST_DYNAMIC_SAVE_3M</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_ATM_TENDENCY1M</span></td>
  <td rowspan=10><span>Тренд по сумме
  транзакций по MCC за отчетный период (1 или 3 месяца)</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_ATM_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_AUT_TENDENCY1M</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_AUT_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_CLO_TENDENCY1M</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_CLO_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_MED_TENDENCY1M</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_MED_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_SUP_TENDENCY1M</span></td>
 </tr>
 <tr>
  <td><span>SUM_TRAN_SUP_TENDENCY3M</span></td>
 </tr>
 <tr>
  <td><span>TRANS_AMOUNT_TENDENCY3M</span></td>
  <td><span>отношение суммы транзакций
  за три месяца к 6 месяцем</span></td>
 </tr>
 <tr>
  <td><span>TRANS_CNT_TENDENCY3M</span></td>
  <td><span>отношение числа транзакций
  за три месяца к 6 месяцем</span></td>
 </tr>
 <tr>
  <td><span>TRANS_COUNT_ATM_PRC</span></td>
  <td rowspan=3><span>Отношение
  транзакций по MCC ко всем транзакциям за отчетный период</span></td>
 </tr>
 <tr>
  <td><span>TRANS_COUNT_NAS_PRC</span></td>
 </tr>
 <tr>
  <td><span>TRANS_COUNT_SUP_PRC</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_CC</span></td>
  <td><span>Средние обороты по
  кредитным картам</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_DYNAMIC_CC_1M</span></td>
  <td rowspan=8><span>Тренд по
  среднемесячным оборотам за отчетный период (1 или 3 месяца)</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_DYNAMIC_CC_3M</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_DYNAMIC_CUR_1M</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_DYNAMIC_CUR_3M</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_DYNAMIC_IL_1M</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_DYNAMIC_IL_3M</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_DYNAMIC_PAYM_1M</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_DYNAMIC_PAYM_3M</span></td>
 </tr>
 <tr>
  <td><span>TURNOVER_PAYM</span></td>
  <td><span>Средние обороты по
  зарплатным счетам</div>
  </span></td>
 </tr>
 <tr>
 <td><span><b>TARGET</b></span></td>
  <td><span><b>Фактический индикатор оттока</b></span></td>
 </tr>
</table>

### Обработка данных на выход

Для предоставленного вам тестового набора данных `lab05_test.csv` построить бинарный классификатор вероятного оттока клиентов. Критерий качества оценки – [ROC AUC](http://scikit-learn.org/stable/modules/model_evaluation.html#roc-metrics).

Выходной файл должен быть расположен в корне вашей домашней директории в файле `lab05.csv`. Файл должен содержать два поля, разделённых табом, а также шапку: `id target`.

- Поле `id` — идентично тому, что представлено в `lab05_test.csv`.
- Поле `target` — **вероятность** в пределах `[0, 1]` того, что данный клиент окажется отточным. Соответственно, `0` означает, что вы полностью уверены, что клиент останется с банком, а `1` — то, что клиент точно окажется в оттоке.

**Для успешного прохождения лабы вы должны получить ROC AUC не меньше 0.75.**

#### Подсказки

- Работу удобно выполнять в Jupyter Notebook Apache Toree - Scala
- Используйте Spark Scala Api.
- Не забывайте чистить датасет.
- Обратите внимание, что задача в условии — на классификацию. Это повлияет на выбор алгоритма обучения.
- Не забывайте валидироваться на ваших имеющихся данных.
- Если все совсем плохо, напишите код сначала на привычном Python, а потом перепешите на Scala
- Иногда удобно переходить из Python в Scala написав код через Sql запросы.

### Проверка

Проверка осуществляется [автоматическим скриптом](http://lk.newprolab.com/lab/slaba05) из Личного кабинета.
