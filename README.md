[![Build Status](https://travis-ci.com/databricks/spark-pandas.svg?token=Rzzgd1itxsPZRuhKGnhD&branch=master)](https://travis-ci.com/databricks/spark-pandas)

# Koalas: Pandas APIs on Apache Spark

The Koalas project makes data scientists more productive when interacting with big data, by augmenting Apache Spark's Python DataFrame API to be compatible with Pandas'.

Pandas is the de facto standard (single-node) dataframe implementation in Python, while Spark is the de facto standard for big data processing. With this package, data scientists can:
 - Be immediately productive with Spark, with no learning curve, if one is already familiar with Pandas.
 - Have a single codebase that works both with Pandas (tests, smaller datasets) and with Spark (distributed datasets).


## Dependencies

 - Spark 2.4. Some older versions of Spark may work too but they are not officially supported.
 - A recent version of Pandas. It is officially developed against 0.23+ but some other versions may work too.
 - Python 3.5+ if you want to use type hints in UDFs. Work is ongoing to also support Python 2.


## Get Started

Pending publication on the PyPI repository, a compiled package can be installed by using
this URL:

```bash
pip install https://github.com/databricks/spark-pandas/releases/download/v0.0.6/databricks_koalas-0.0.6-py3-none-any.whl
```

After installing the package, you can import the package:

```py
import databricks.koalas
```

That's it. Now you have turned all the Spark Dataframes 
that will be created from now on into API-compliant Pandas 
dataframes.

Example:

```py
import pandas as pd
pdf = pd.DataFrame({'x':range(3), 'y':['a','b','b'], 'z':['a','b','b']})

df = spark.from_pandas(pdf)

# Rename the columns
df.columns = ['x', 'y', 'z1']

# Do some operations in place:
df['x2'] = df.x * df.x
```


## Project Status

This project is currently in beta and is rapidly evolving.
You should expect the following differences:

 - some functions may be missing (see the [What is available](#what-is-available) section)

 - some behaviour may be different, in particular in the treatment of nulls: Pandas uses
   Not a Number (NaN) special constants to indicate missing values, while Spark has a
   special flag on each value to indicate missing values. We would love to hear your use
   case if you find differences.
   
 - because Spark is lazy in nature, some operations like creating new columns only get 
   performed when Spark needs to print or write the dataframe.



## Development Guide

### Environment Setup

We recommend setting up a Conda environment for development:
```
conda create --name koalas-dev-env python=3.6
source activate koalas-dev-env
conda install -c conda-forge pyspark=2.4 pandas pyarrow=0.10 decorator flake8 nose
```

### Running Tests

To run all the tests, similar to our CI pipeline:
```
./dev/run-tests.sh
```

To run a specific test file:
```
python databricks/koalas/tests/test_dataframe.py
```

To run a specific test method:
```
python databricks/koalas/tests/test_dataframe.py DataFrameTest.test_Dataframe
```

### Contributions

Please create a GitHub issue if your favorite function is not yet supported.

We also document all the functions that are not yet supported in the [missing directory](https://github.com/databricks/spark-pandas/tree/master/databricks/koalas/missing). In most cases, it is very easy to add new functions by simply wrapping the existing Pandas or Spark functions. Pull requests welcome!
