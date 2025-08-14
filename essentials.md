---
hide:
    - nav
title: Essentials
---

# Introduction

We italians, whenever we struggle accomplish a complicated task, are used to compose small "poetries" (2/3 words long, similar to japenese haiku). 
These poetries consist in a sort of dialog we have with God.

We call these italian haiku ***bestemmie***.

<br>
<br>
A couple of weeks ago I started studing [kedro](https://kedro.org/), a package by McKinsey.

I was interested in the way this package handles dataset and the concept of nodes and pipelines to make your code clean and well organised.

In paticular I was amazed by the interactive visualisation tool they've implemented to visualize the structure of the program and the modules that compose it.

For **few weeks** I force my self to use this package; I really wanted to use that visualisation tool. 
I struggled a lot. I soon realized how rigid the package was, how difficult it was to add even a little of customisation and the amount of black box functions I was supposed to call not understanding what they were doing under the hood just to make it works.

<br>
<br>

**The final result is that**:
* I gave up kedro due to frustration.
* I've created my own package to replicate kedro's objects I like the most appling some changes to improve them.
* I can now compose ***bestemmie***  up to 10 words long, engaging multiple deities even from different religions.


<br>
<br>




# The package

In this page I will illustrate what I've implemented guiding with examples and short explainations.

!!! info
    You can find this *tool box* in the github repository [**clarkpy_essentials**](https://github.com/clarkmaio/clarkpy_essentials).
    The package can be installed with the command:

    ```
    pip install git+https://github.com/clarkmaio/clarkpy_essentials.git
    ```


## **ParserManager**
Let's start from a dummy one: the `ParserManager`.

I just do not like to edit parser from a `.py` file

`ParserManager` is just a class that parse a `yaml` file and build a parser using the standard library `argparser`.

Parser keys will be stored in a dictionary ready to be queried.


```yaml
# Example of parser_conf.yml
variable_int:
  default: 9
  help: 'This variable is an integer'

variable_str:
  default: abc
  help: 'This variable is a string'

variable_nargs:
  default: ['a', 'b', 'c']
  nargs: '*'
```


```py
from clarkpy_essentials import ParserManager

PARSER_CONF = `/path_to_parser/parser_conf.yml`
parser_args = ParserManager.load(path = PARSER_CONF)

# parser_args is now just a dictionary
print('variable_int', parser_args['variable_int'])
print('variable_nargs', parser_args['variable_nargs'])
```





## **DataCatalog**
This class is nothing but an huge *if/else* function to handle different type of dataset.

The idea has been stolen directly from [**kedro**](https://docs.kedro.org/en/stable/data/index.html).
In **kedro** the user write down a yaml file to map dataset to a label. The dataset can be then loaded in pipeline simply using the label as input of nodes.

Since I was looking for something more explicit and a tool that could be used outside a **kedro** pipeline I've implemented the `DataCatalog` class.

The most important thing to use this class is to write properly the data catalog.

It is important to specify for each label the parameters:

* `type`: namely the way the dataset will be loaded. Can be `pandas.csv`, `pandas.excel`, `pandas.hdf`, `pandas.parquet`, `polars.csv`, `polars.parquet`,`yaml`, `json`, `torch`, `pickle`
* `filepath`: the path to the file. By default this is a relative path that will be appended to `source_path` defined in the constructor. You can refer to an absolute path using the prefix `@abs:`
* `load_kwargs`: additional paramters that will be passed to loader fucntion as kwargs

```yaml
# Example of a data catalog yml
PandasDataset:
    type: pandas.csv
    filepath: dataset/csv_dataset.csv
    load_kwargs:
        sep: ','

PolarsDataset:
    type: polars.parquet
    filepath: @abs:/home/user/dataset/parquet_dataset.parquet

UrlPandasDataset:
    type: pandas.csv
    filepath: @abs:https://raw.githubusercontent.com/cs109/2014_data/master/countries.csv
    load_kwargs:
        sep: ','
```

```py
from clarkpy_essentials import DataCatalog

# Initialize the datacatalog specifying the catalog.yml path and the source path that will be used to load datasets.
CATALOG_PATH = '/home/user/catalog.yml'
dc = DataCatalog(catalog=CATALOG_PATH)

print('Pandas Dataset', dc('PandasDataset').head(5))
print('Polars Dataset', dc('PolarsDataset').head(5))
print('URL Pandas Dataset', dc('UrlPandasDataset').head(5))
```

You can even add a custom loader function using the method:

```python
def myloaderfunc(filepath: str, **kwargs):
    ...
    load file
    ...
    return file

from clarkpy_essentials import DataCatalog
dc = DataCatalog(catalog=CATALOG_PATH)
dc.add_custom_loader(key='myloader', func=myloaderfunc)
```

Now you can load data with label `'mylader'` using function `myloaderfunc`.


## **Context**

This is a very trivial class. The only purpose it to store senttings and variables as properties of a class.

Whatever you pass to the constructor of this class will be trasformed in a property.

!!! note
    `catalog` is a privileged key that is ment to store a `DataCatalog` instance. It is privileged in the sense that whatever is stored in this key will be treated as `DataCatalog` intance in `Pipeline` (see below).


```python
from clarkpy_essentials import Context

global_vars = {'GLOBAL': 100}
context = Context(parser=parser_args, global_vars=global_vars, catalog=dc)

print(context.parser)
print(context.global_vars)
print(context.catalog)
```



## **Pipelines and nodes**

Again I've tried to copy a feature of kedro package but tring to make it easier to use it.

I really like the way **Pipelines** and **Nodes** work in kedro, in particular the elegant way to pass input/output across different nodes.

You can use mine `Pipeline` and `Node` classes to accomplish the same but you can easily use them in your program without using kedro.


Here a dummy example:

```py
from clarkpy_essentials import Pipeline, Node, Context

# ------------ Node fucntions --------------
def prod(x: float, y: float) -> float:
  return x*y


def sum(x: float, y: float) -> float:
  reutrn x+y


# ------------ Create Context --------------
GLOBAL_VARIABLES = {'var1': 1, 'var2': 100, 'var3': 10}
context = Context(global_variables=GLOBAL_VARIABLES)

# ------------ Initialize Pipeline ---------
pipeline = Pipeline([
    Node(func=prod,
         inputs=['context.global_variables.var1', 'context.global_variables.var2'],
         outputs='output_prod'),
    Node(func=sum,
         inputs=['output_prod', 'context.global_variables.var3'],
         outputs='outpout_sum')
])

# ------------ Run Pipeline ----------------
pipeline_results = pipeline.run(context=context)
print('All pipeline variables', pipeline_results)
```


For more complex example have a look at one of mine [template](https://github.com/clarkmaio/cookiecutter_template).

## **DataTransformer**

When dealing with multiple models it happens that different data processing are needed depending on the model you are using.

To make it easier to control this operations I've created a class that take in input a list of instructions and transform a dataset accordingly.

Ideally you should create instructions (in the form of a `yaml`) for each model.


To use correctly this class you should:

1. Write the functions to transform data like so:

    ```python
    def transformer_1(X, **kwars):
        transform X some how
        return X

    def transformer_2(X, **kwars):
        transform X some how
        return X
    ```

2. Create an instance of `DataTransformer` and map the functions:

    ```python
    dt = DataTransformer({
        't1': trabsformer_1,
        't2': transformer_2 
    })
    ```

3. Create list of instructions.

    Each instruction step must consist in a dictionary containing keys:

    * `type`: label of the function to apply 
    * `kwargs` [optional]: function kwargs

    ```python
    instructions = [
        {
            'type': 't1'
            'kwargs': {
                'kwarg1': ...,
                'kwarg2': ...
            }
        },

        'type': 't2'
            'kargs': {
                'kwarg1': ...,
                'kwarg2': ...
            }
    ]
    ```

4. Transform data
```python
X_transformed = dt.transform(X, instructions=instructions)
```


Here a complete example:

```python
def f1(X):
    return X

def f2(X, alpha):
    return X*alpha

dt = DataTransformer()
dt.add_transformer_catalog({
    'identity': f1,
    'mul': f2
})


instructions = [
    {
        'type': 'identity'
    },

    {
        'type': 'mul',
        'kwargs': {
            'alpha': 2
        }
    }
]


X = [1,2,3,4,5]
X_transformed = dt.transform(X=X, instructions=instructions)
print(X_transformed)
```


## **Decorators**

Just a collection of decorators you can import from `clarkpy_essentials.decorators`.

* `@force_kwargs`:  raise an error if function arguments are not passed through keys.
* `@deepcopy_args`: deep copy args and kwargs passed to a function (usefully to avoid operations on pandas dataframe copy to affect original one).