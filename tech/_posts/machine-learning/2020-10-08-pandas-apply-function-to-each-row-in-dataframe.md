---
title: "6 Ways to Apply a Function to Every Row in Pandas DataFrame"
excerpt: "Pandas apply function to each row: profile performance of loop, iterrows, itertuples, apply, list comprehension, vectorization."
image: "https://cdn-images-1.medium.com/max/2320/1*zsfQIDGZrdGLsjA5vrafWA.jpeg"
image_thumbnail: "https://cdn-images-1.medium.com/max/2320/1*zsfQIDGZrdGLsjA5vrafWA.jpeg"
tags:
  - "machine-learning"
  - "data-science"
  - "big-data"
canonical_url: "https://towardsdatascience.com/apply-function-to-pandas-dataframe-rows-76df74165ee4"
code: true
---

![](https://cdn-images-1.medium.com/max/2320/1*zsfQIDGZrdGLsjA5vrafWA.jpeg){:width="100%" class="framedimg"}

Applying a function to all rows in a [Pandas](https://pandas.pydata.org/) [DataFrame](https://pandas.pydata.org/docs/reference/frame.html) is one of the most common operations during [data wrangling](https://en.wikipedia.org/wiki/Data_wrangling). There are many ways of doing it. In this article, you will measure the performance of 6 common alternatives. With a companion Colab, you can do it all in your browser. No need to install anything on your machine.

## Problem

Recently, I was analyzing user behavior data for an e-commerce app. Depending on the number of times a user did text and voice searches, I assigned each user to one of four cohorts:

* **No Search:** Users who did no search at all
* **Text Only:** Users who did text searches only
* **Voice Only:** Users who did voice searches only
* **Both:** Users who did both text and voice search

It was a huge data set with 100k to a million users depending upon the chosen time slice. Computing it with Pandas `apply()` function was excruciatingly slow, so I evaluated alternatives. This article is the distilled lessons from that.

I can’t share that dataset. So I am picking another similar problem to show the solutions: [the Eisenhower method](https://en.wikipedia.org/wiki/Time_management#The_Eisenhower_Method).

![The Eisenhower Method: Tasked put into 4 bins depending on importance and urgency.](https://cdn-images-1.medium.com/max/2000/1*U0ISjHVnL8P26S-GcrgINg.png){:width="100%"}

Based on a task’s importance and urgency, the Eisenhower Method assigns it into one of 4 bins. Each bin has an associated action:

* Important and Urgent: **Do** right away
* Important but not Urgent: **Schedule** for later
* Not Important but Urgent: **Delegate** to someone else
* Neither Important nor Urgent: **Delete** time wasters.

We will use the boolean matrix shown in the adjacent figure. Importance and urgency booleans make the binary integer value for each action: DO(3), SCHEDULE(2), DELEGATE(1), DELETE(0).

We will **profile** the **performance** of mapping tasks to one of the actions. We will measure which of the **6 alternatives** take the least amount of time. And we will plot the performance for **up to a million tasks**.

It is a good time to open the companion [Colab](https://colab.research.google.com/github/scgupta/yearn2learn/blob/master/ml/pandas/pandas_apply_fn_on_dataframe.ipynb). If you want to see the code in action, you can execute the cells in the Colab as you read along. Go ahead, execute all the cells in the Setup section.

## Test Data

[Faker](https://faker.readthedocs.io/en/stable/) is a handy library to generate data. In the Colab, it is used for generating a DataFrame with a million tasks. Each task is a row in the DataFrame. It consists of task_name (`str`), due_date (`datetime.date`), and priority (`str`). Priority can be one of the three values: LOW, MEDIUM, HIGH.

![First 5 tasks in the generated DataFrame](https://cdn-images-1.medium.com/max/2000/1*EshpxVJlCG8Jb0gBKc4lvQ.png){:width="100%"}

### Optimize DataFrame Storage

We will minimize the storage size to eliminate its effect on any of the alternatives. The DataFrame with a million tasks is taking 22.9MB:

~~~ python
>>> test_data_set.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000000 entries, 0 to 999999
Data columns (total 3 columns):
 #   Column     Non-Null Count    Dtype
---  ------     --------------    -----
 0   task_name  1000000 non-null  object
 1   due_date   1000000 non-null  object
 2   priority   1000000 non-null  object
dtypes: object(3)
memory usage: 22.9+ MB
~~~


Instead of `str`, priority can be stored as Pandas `categorical` type:

~~~ python
priority_dtype = pd.api.types.CategoricalDtype(
  categories=['LOW', 'MEDIUM', 'HIGH'],
  ordered=True
)

test_data_set['priority'] = test_data_set['priority'].astype(priority_dtype)
~~~


Let’s check out the DataFrame size now:

~~~ python
>>> test_data_set.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1000000 entries, 0 to 999999
Data columns (total 3 columns):
 #   Column     Non-Null Count    Dtype   
---  ------     --------------    -----   
 0   task_name  1000000 non-null  object  
 1   due_date   1000000 non-null  object  
 2   priority   1000000 non-null  category
dtypes: category(1), object(2)
memory usage: 16.2+ MB
~~~


Size is reduced to 16.2MB.

### Eisenhower Action Function

Given importance and urgency, `eisenhower_action` computes an integer value between 0 and 3.

~~~ python
def eisenhower_action(is_important: bool, is_urgent: bool) -> int:
  return 2 * is_important + is_urgent
~~~


For this exercise, we will assume that a task with **HIGH priority** is **important**. If the **due date** is in the **next two days**, then the task is **urgent**.

The Eisenhower Action for a task (i.e. a row in the DataFrame) is computed by using the `due_date` and `priority` columns:

~~~ python
>>> cutoff_date = datetime.date.today() + datetime.timedelta(days=2)

>>> eisenhower_action(
  test_data_set.loc[0].priority == 'HIGH',
  test_data_set.loc[0].due_date <= cutoff_date
)

2
~~~


The integer 2 means that the needed action is to SCHEDULE.

In the rest of the article, we will evaluate 6 alternatives for applying `eisenhower_action` function to DataFrame rows. First, we will measure the time for a sample of 100k rows. Then, we will measure and plot the time for up to a million rows.

## Method 1: Loop Over Rows of Pandas DataFrame

The simplest method to process each row in the good old Python loop. This is obviously the worst way, and nobody in the right mind will ever do it.

~~~ python
def loop_impl(df):
  cutoff_date = datetime.date.today() + datetime.timedelta(days=2)

  result = []
  for i in range(len(df)):
    row = df.iloc[i]
    result.append(
      eisenhower_action(
        row.priority == 'HIGH', row.due_date <= cutoff_date)
    )

  return pd.Series(result)
~~~


As expected, it takes a horrendous amount of time: 37.5 seconds.

~~~ python
%timeit data_sample['action_loop'] = loop_impl(data_sample)

1 loop, best of 3: 37.5 s per loop
~~~


It establishes the worst-case performance upper bound. Since its cost is linear, i.e. O(n), it provides a good baseline to compare other alternatives.

### Line Level Profiling

Let’s find out what is taking so long using the [line_profiler](https://github.com/pyutils/line_profiler), but for a smaller sample of 100 rows:

~~~ python
%lprun -f loop_impl  loop_impl(test_data_sample(100))
~~~


Its output is shown in the following figure:

![The output of the line-level profiler for processing a 100-row DataFrame in Python loop.](https://cdn-images-1.medium.com/max/3840/1*vxs1duzzGjBGuJ7yWpm5Jg.png){:width="100%"}

Extracting a row from DataFrame (line #6) takes 90% of the time. Even if we take out that 90% cost from 37.5s for 100k rows, it would take 3.7s. That is still a lot of time.

## Method 2: Pandas DataFrame `iterrows` Function

Instead of processing each row in a Python loop, let’s try Pandas `[iterrows`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.iterrows.html) function.

~~~ python
def iterrows_impl(df):
  cutoff_date = datetime.date.today() + datetime.timedelta(days=2)

  return pd.Series(
    eisenhower_action(
      row.priority == 'HIGH', row.due_date <= cutoff_date)
    for index, row in df.iterrows()
  )
~~~


It takes 9.59 seconds, approx. one-fourth of the time taken by the loop:

~~~ python
%timeit data_sample['action_iterrow'] = iterrows_impl(data_sample)

1 loop, best of 3: 9.59 s per loop
~~~


## Method 3: Pandas DataFrame `itertuples` Function

Pandas has another method, `[itertuples`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.itertuples.html), that processes rows as tuples.

~~~ python
def itertuples_impl(df):
  cutoff_date = datetime.date.today() + datetime.timedelta(days=2)

  return pd.Series(
    eisenhower_action(
      row.priority == 'HIGH', row.due_date <= cutoff_date)
    for row in df.itertuples()
  )
~~~


Its performance threw a surprise, it took only 178 milliseconds.

~~~ python
%timeit data_sample['action_itertuples'] = itertuples_impl(data_sample)

10 loops, best of 3: 178 ms per loop
~~~


## Method 4: Pandas DataFrame `apply` Function

Pandas `[apply`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html) function is quite versatile and is a popular choice. To make it process the rows, you have to pass `axis=1` argument.

~~~ python
def apply_impl(df):
  cutoff_date = datetime.date.today() + datetime.timedelta(days=2)
  return df.apply(
      lambda row:
        eisenhower_action(
          row.priority == 'HIGH', row.due_date <= cutoff_date),
      axis=1
  )
~~~


This also threw a surprise for me. It took 1.75 seconds. 10x worse than `itertuples`!

~~~ python
%timeit data_sample['action_impl'] = apply_impl(data_sample)

1 loop, best of 3: 1.75 s per loop
~~~


## Method 5: Python List Comprehension

A column in DataFrame is a Series that can be used as a list in a [list comprehension](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions) expression:

~~~ python
[ foo(x) for x in df['x'] ]
~~~


If multiple columns are needed, then `zip` can be used to make a list of tuples.

~~~ python
def list_impl(df):
  cutoff_date = datetime.date.today() + datetime.timedelta(days=2)
  return pd.Series([
    eisenhower_action(priority == 'HIGH', due_date <= cutoff_date)
    for (priority, due_date) in zip(df['priority'], df['due_date'])
  ])
~~~


This also threw a surprise. It took only 70.7 milliseconds, even better than `itertuples`!

~~~ python
%timeit data_sample['action_list'] = list_impl(data_sample)

10 loops, best of 3: 70.7 ms per loop
~~~


## Method 6: Vectorization

The real power of Pandas shows up in vectorization. But it requires unpacking the function as a vector expression using Pandas and NumPy.

~~~ python
def vec_impl(df):
  cutoff_date = datetime.date.today() + datetime.timedelta(days=2)
  return (
    2*(df['priority'] == 'HIGH') + (df['due_date'] <= cutoff_date))
~~~


It gives the best performance: only 19.5 milliseconds.

~~~ python
%timeit data_sample['action_vec'] = vec_impl(data_sample)

100 loops, best of 3: 19.5 ms per loop
~~~


Vectorizing, depending upon the complexity of the function, can take significant efforts. Sometimes, it may not even be feasible.

## Plot Performance Over DataFrame Size

Plotting is helpful in understanding the relative performance of alternatives over input size. [Perfplot](https://pypi.org/project/perfplot/) is a handy tool for that. It requires a setup to generate input of a given size and a list of implementations to compare.

~~~ python
kernels = [
  loop_impl,
  iterrows_impl,
  itertuples_impl,
  apply_impl,
  list_impl,
  vec_impl
]

labels = [str(k.__name__)[:-5] for k in kernels]

perfplot.show(
  setup=lambda n: test_data_sample(n),
  kernels=kernels,
  labels=labels,
  n_range=[2**k for k in range(20)],
  xlabel='N',
  logx=True,
  logy=True
)
~~~

It generates a plot like the one shown below.

![Time taken in applying a function to 100,000 rows of a Pandas DataFrame](https://cdn-images-1.medium.com/max/2000/1*MxiloyVqYTARUpERwe1u7Q.png)

![Processing time for various DataFrame sizes in log-log scale.](https://cdn-images-1.medium.com/max/2000/1*ySLoEdkj3SUeTi3NkQfceQ.png){:width="100%"}

Here are some observations from the plot:

* For this use case, asymptotic performance order stabilizes at about 10k rows in the DataFrame.
* List Comprehension is the best for less than a thousand rows.
* Since all lines in the plot become parallel, the perf difference might not be apparent in the log-log scale plot.
* The `itertuples` is as simple to use as `apply` but with 10x better performance
* List Comprehension is ~2.5x better than `itertuples`, though it can be verbose to write for a complex function
* Vectorization is ~3.5x and ~9x better than List Comprehension and `itertuples` respectively.

## Recommendations

Performing an operation independently to all Pandas rows is a common need. Here are my recommendations:

1. **Pandas `itertuples` function:** Its API is like `apply` function, but offers 10x better performance. It is the easiest and most readable option. It offers reasonable performance. It is now my default choice to start.
2. **List Comprehension:** When needing only 2–3 DataFrame columns, I opt for this alternative to squeeze better performance.
3. **Vectorize DataFrame expression:** I go for this whenever vectorization is straight forward. For performance-critical code, I try my best to vectorize and extract the best performance possible.

Understanding the cost of various alternatives is critical for making an informed choice. Use `timeit`, `line_profiler`, and `perfplot` to measure the performance of these alternatives. Balance performance with ease of use for deciding the best alternative for your use case.

