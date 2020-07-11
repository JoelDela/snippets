# Snippet for Altair

## Import Altair

```python
import altair as alt
### if in notebook
alt.renderers.enable("notebook")
```

## Bar Chart
```python
trends_bar=alt.Chart(df).mark_bar().encode(
    x=x_variable,
    y=y_variable,
    color=color_field
)
```
## Histogram

```python
alt.Chart(df).mark_bar().encode(
    x=alt.X('x_variable',bin=True,title="X Variable Binned"),
    y="count(y_variable)"
).properties(
    width=width,
    height=height,
    title="Histogram"
)
``` 

## Scatter Plot 

```python
scatter = alt.Chart(df).mark_circle().encode(
    x="Body Weight",
    y="Brain Weight"
)
``` 

## Line Chart
```python
trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color=color_variable
)
```

## Map
```python
states = alt.topo_feature("https://unpkg.com/world-atlas@1.1.4/world/110m.json",feature="countries")

background = alt.Chart(states).mark_geoshape(
    fill="lightgray",
    stroke="white"
)
```

```python
temp = weather.groupby(["city","date"]).mean().reset_index()

points = alt.Chart(temp).mark_circle().encode(
    latitude="lat:Q",
    longitude="lng:Q",
    color=alt.Color("mean(temp)",scale=alt.Scale(range=["blue","orange","red"]))
)
```
```python
(background+points)
```

## Horizontal concatenation

```python
graph_1 | graph_2
```

## Vertical concatenation

```python
graph_1 & graph_2
```


## Single selection

```python
select_search_term = alt.selection_single(encodings=["x"]) 
# or 
select_search_term = alt.selection(type="single",encodings=["x"])

trends_line=alt.Chart(df).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color=color_variable
).transform_filter(
    select_search_term
)

trends_bar=alt.Chart(df).mark_bar().encode(
    x=x_variable,
    y="sum(value)",
    color=color_variable
).properties(
    selection=select_search_term
)

trends_bar|trends_line
```

## Interval Selection

```python
selection = alt.selection_interval(encodings=["x"])
# or
selection = alt.selection(type="interval",encodings=["x","y"])

histo1 = alt.Chart(df).mark_bar().encode(
    x=alt.X(x_variable,bin=alt.Bin(maxbins=100)),
    y="count()"
).properties(
    width=width,
    title="Histograma 1"
).interactive().transform_filter(
    selection
)

histo2 = alt.Chart(df).mark_bar().encode(
    x=alt.X(x_variable,bin=alt.Bin(maxbins=100)),
    y="count()"
).properties(
    width=width,
    title="Histograma 1"
).interactive().transform_filter(
    selection
)

scatter3 = alt.Chart(df).mark_point().encode(
    x=x_variable,
    y=y_variable
).properties(
    width=width*2,
    title="Relación",
    selection=selection
)

scatter_bb&(histo_body|histo_brain)
```
