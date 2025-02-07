# Summary of essential visualization techniques in Python.

based on the following resources:
- [Python for Data Analysis, 3rd Edition, by Wes McKinney](https://wesmckinney.com/book/plotting-and-visualization)
- Matplotlib official [tutorials](https://matplotlib.org/stable/tutorials/index.html) and [user guide](https://matplotlib.org/stable/users/index.html)

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