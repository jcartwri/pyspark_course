# Подключение к JupyterHub

В этом документе вы узнаете, как работать с кластером через браузер в JupyterHub используя Apache Spark.

Для работы с Apache Spark мы будем пользоваться JupyterHub. Это обычный Jupyter Notebook только для многопользовательского режима.

Для подключения к нему достаточно ввести свой логин от ЛК и пароль на той мастер-ноде, к которой относитесь вы:

- [https://spark-master-3.newprolab.com](https://spark-master-3.newprolab.com)
- [https://spark-master-4.newprolab.com](https://spark-master-4.newprolab.com)

## Для получения спарк контекста достаточно исполнить

```python
import os
import sys
os.environ["PYSPARK_PYTHON"]='/opt/anaconda/envs/bd9/bin/python'
os.environ["SPARK_HOME"]='/usr/hdp/current/spark2-client'
os.environ["PYSPARK_SUBMIT_ARGS"]='--num-executors 2 pyspark-shell'

spark_home = os.environ.get('SPARK_HOME', None)

sys.path.insert(0, os.path.join(spark_home, 'python'))
sys.path.insert(0, os.path.join(spark_home, 'python/lib/py4j-0.10.7-src.zip'))
exec(open(os.path.join(spark_home, 'python/pyspark/shell.py')).read())
```

Вы увидите

```bash
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 2.4.5
      /_/

Using Python version 3.6.5 (default, Apr 29 2018 16:14:56)
SparkSession available as 'spark'.
```

## Информация о спарк контексте

```python
sc
```

Вы увидите

```bash
SparkContext

Spark UI

Version
v2.4.5
Master
yarn
AppName
pyspark-shell
```

## После завершения работы правильно закрыть контекст

```python
sc.stop()
```
