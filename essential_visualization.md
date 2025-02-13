# Essential Visualization in Python

## Table of Contents
- [Essential Visualization in Python](#essential-visualization-in-python)
  - [Table of Contents](#table-of-contents)
  - [matplotlib basics](#matplotlib-basics)
  - [pandas \& seaborn](#pandas--seaborn)
  - [Other tools for visualization](#other-tools-for-visualization)
  - [References](#references)

## matplotlib basics

```python
import matplotlib.pyplot as plt

# plots reside in a figure, cureate figure first
fig = plt.figure()
# add a subplot to the figure
ax1 = fig.add_subplot(2, 2, 1)  # 2x2 grid, 1st subplot
ax2 = fig.add_subplot(2, 2, 2)
ax3 = fig.add_subplot(2, 2, 3)

# draw
ax1.hist(np.random.standard_normal(100), bins=20, color="black", alpha=0.3)  # histogram
ax2.scatter(np.arange(30), np.arange(30) + 3 * np.random.standard_normal(30)) # scatter plot
ax3.plot(np.random.standard_normal(30).cumsum())  # line plot

# more ways to create subplots
## case 1
fig = plt.figure()       # create a figure
ax = fig.add_subplot()   # a single subplot
## case 2
fig, ax = plt.subplots() # same as above
## case 3
fig, axes = plt.subplots(2, 3) # 2x3 grid of subplots
axes[0, 1].hist(np.random.standard_normal(100), bins=20, color="black", alpha=0.3)

# plot settings
ax.set_xticks([0, 250, 500, 750, 1000])
ax.set_xticklabels(["one", "two", "three", "four", "five"], rotation=30, fontsize=8)
ax.set_xlabel("Stages")
ax.set_title("My first matplotlib plot")

# legends
fig, ax = plt.subplots()
ax.plot(np.random.randn(1000).cumsum(), color="black", label="one")
ax.plot(np.random.randn(1000).cumsum(), color="black", linestyle="dashed", label="two")
ax.plot(np.random.randn(1000).cumsum(), color="black", linestyle="dotted", label="three")

# annotations
ax.text(x, y, "Hello world!", family="monospace", fontsize=10)

# save figure
fig.savefig("figpath.svg")

# change figure size (global)
plt.rcParams["figure.figsize"] = (20,3)  # unit: inches
plt.rcParams["figure.figsize"] = plt.rcParamsDefault["figure.figsize"]

# change figure size (local)
# before plotting
fig = plt.figure(figsize=(10, 5))
fig, ax = plt.subplots(figsize=(10, 5))
# after plotting
fig.set_size_inches(10, 5)
fig.set_size_inches((10, 5))
```

## pandas & seaborn
```python
import pandas as pd
import seaborn as sns

s = pd.Series(np.random.standard_normal(10).cumsum(), index=np.arange(0, 100, 10))
s2 = pd.Series(np.random.standard_normal(100))

df = pd.DataFrame(
    np.random.standard_normal((6, 4)),
    index=["one", "two", "three", "four", "five", "six"],
    columns=pd.Index(["A", "B", "C", "D"], name="Genus"))

iris = sns.load_dataset("iris")

# simple line plot
s.plot()  # by default, index is x-axis
df.plot()

# bar plot
s.plot(kind="bar")
df.plot(kind="bar")
df.plot.bar()  # same as above

fig, axes = plt.subplots(2, 1)
s.plot.bar(ax=axes[0], color="black", alpha=0.7)   # horizontal bar plot
s.plot.barh(ax=axes[1], color="black", alpha=0.7)  # vertical bar plot

df.plot.bar(stacked=True)  # stacked bar plot

sns.barplot(x='sepal_length', y='species', data=iris, orient='h')  # seaborn bar plot
# summary statistics are calculated automatically
sns.set_style("whitegrid")  # change style
sns.set_palette("pastel")   # change color palette

# histogram
s2.hist(bins=20, color="black", alpha=0.3)
iris['sepal_length'].hist(bins=20, color="black", alpha=0.3)
sns.histplot(data=iris, x='sepal_length', bins=20, color="black", alpha=0.6)
sns.histplot(data=s2)

# density plot
s2.plot.kde()
s2.plot.density()

# scatter plot
ax = sns.regplot(x='sepal_length', y='sepal_width', data=iris, fit_reg=False)
ax.set_title("Iris dataset")

# pairplot for exploring relationships
sns.pairplot(trans_data, diag_kind="kde", plot_kws={"alpha": 0.2})

# facet grid
sns.FacetGrid(iris, col="species").map(sns.kdeplot, "sepal_length")
sns.FacetGrid(iris, row="species").map(sns.kdeplot, "sepal_length")
sns.catplot(x='sepal_length', y='sepal_width', col='species', kind='point', data=iris)

# boxplot
sns.catplot(x='sepal_length', y='species', kind='box', data=iris)
```

## Other tools for visualization
- [Plotly](https://plotly.com/python/)
- [Bokeh](https://docs.bokeh.org/en/latest/index.html)
- [Altair](https://altair-viz.github.io/)
- [plotnine](https://plotnine.readthedocs.io/en/stable/)
- [ggplot](http://yhat.github.io/ggpy/)

## References
- [Python for Data Analysis, 3rd Edition, by Wes McKinney](https://wesmckinney.com/book/plotting-and-visualization)
- Matplotlib official [tutorials](https://matplotlib.org/stable/tutorials/index.html) and [user guide](https://matplotlib.org/stable/users/index.html)
