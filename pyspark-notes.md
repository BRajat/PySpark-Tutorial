## PySpark

1. Python helper library to build spark-application-jobs. 

2. Spark is data processing engine in distributed environment, for large data- it implements the idea of in memory parallel computation, lazy execution with optimized DAG.

3. Pyspark has DataFrame a data structure like Pandas DataFrame with some key differences - like slicing is not allowed, we need to use VectorAssembler while using MLib.

4. The entire data in PySpark DataFrame is stored in partitions across multiple nodes of the cluster.

5. Spark can run in Standalone mode or with cluster manager - Apache Mesos and YARN, which monitor and manages the resources of the cluster.

**Transformations and Actions**

1. Transformations functions do not trigger the response from DAG, they wait until the downstream Action function is triggered.
Ex: filter(), map(), flatMap(), aggregate(), reduce()

2. The Action function - starts the execution of Spark Job-
Ex: show(), collect()

## Starting Spark Job

1. A session builder command is used to create a spark Application

```
spark = SparkSession.builder.appName("practise").getOrCreate();
```

2. Loading a spark dataFrame from csv

```
df_spark = spark.csv.read('filepath', inferSchema = True, headers = True)
```

Additional methods- 

- df.select()

- df.withColumn()

- df.drop()


3. Using vector assembler - to vectorize features into a new feature with name NEW_NAME

```
from pyspark.ml.feature import VectorAssembler
featureassembler=VectorAssembler(inputCols=["age","Experience"],outputCol="Independent Features")
```

4. String Indexer - to handle categorical feature- encoding

```
from pyspark.ml.feature import StringIndexer
indexer=StringIndexer(inputCol="sex",outputCol="sex_indexed")
df_r=indexer.fit(df).transform(df)
```

5. Using Mlib library

```
from pyspark.ml.regression import LinearRegression
## train test split
train_data,test_data=finalized_data.randomSplit([0.75,0.25])
regressor=LinearRegression(featuresCol='Independent Features', labelCol='total_bill')
regressor=regressor.fit(train_data)

print(regressor.coefficients)


print(regressor.intercept)

pred_results = regressor.evaluate(test_data)
pred_results.predictions.show()

print(pred_resuls.r2, pred_results.meanAbsoluteError, pred_results.meanSquaredError)

```


