# Dashboard-chart-db-explorer
This is a pure Python framework aims to visualize data (stored in sql database) with an online dashboard. It helps define a dashboard with a set of sql queries and their associated definition for the output chart, and allows reusing without having to rewrite the program.   

At present, the framework mainly applies Dash, whose links to its tutorials and other references are attached at the end of this file. 

## Table of contents
* [Functional Design](#Functional-Design)
* [Algorithmic Design](#Algorithmic-Design)
* [Reference](#Reference)

## Functional Design
< __Input Definition__ >  

User that requests the web dashboard need to prepare a `.JSON` file that contains queries, chart_def, and user-defined x y mapping that points from query result table to chart. Here, the index(idx) corresponds to the order (start from 0) of the selected elements in the query. For example, if the query select id and average_vote, then {...,"x":[1],...} means that we take average_vote as the x axis. The users can also choose not to write x y mapping explicitly. In this case, the x y axis will be decidedly automatically in the order of the selected element.  

An example `input.json` file is presented below,
```
{"0chart": [{"query": "...", "chart_def": "table" (, "x": [...],"y" : [...])}, ...],
 "1chart": [{"query": "...", "chart_def": "table" (, "x": [...],"y" : [...])}, ...],
 "2chart": [{"query": "...", "chart_def": "table" (, "x": [...],"y" : [...])}, ...],
 "3markdown": ["...", "..."],
 "4markdown": ["..."],
 ...
 "layout": {"top":[...(e.g. 0,1,2,3,4...)],"body":{"left":[...],("middle":[...],) "right": [...] },"bottom":[...]}
}
```
Here, every field for chart and markdown begin with an unique int (e.g. __0__ in "0chart") as an identity number that can be used to refer to it in the layout part. Every field is considered as a section. It can has several components, which will be presented in sequential order on the page. For example, in `ochart` section, users can include two or more queries, and the result charts will be shown vertically.   

In the layout part, the user can divide the whole page into at most three parts (top/body(left/middle/right side by side)/bottom) and is free to throw away any of them; further, for the body part, the user is free to exclude any of the three sub-parts, i.e. left, middle and right, and these sub-parts are shown horizontally.


< __Run Code__ >   

First install os module locally, and import it. User can get access to a help function called `run(file_dir)`, which receive a parameter called file_dir that points to the prepared json file as shown above. The help function pass the file_dir as an argv.  

```
import os
def run(file_dir): 
    ...
    (command to run the app with input file directory as additional argument)
    ...
    return
```
To generate web dashboard, simply run helpfunction `run(file_dir)`.
```
# run the program
run("input.json") 
```

## Algorithmic Design
The user store the query and chart_def in pairs inside of the JSON file and pass the file directory into the "dashboard generator". The `helpfunc.py` file takes this information, it reads the queries from the file and pass them to the database, and get the query result. The `chart_def.py` file takes the result and the (unchanged) chart_def and transfer the result into chart configurations according to the char_def. The configuration and the chart_def can be turned into dashboard in the `app.py` file once the file is run by the user.

The following is the diagram that depicts the process that is described in the previous paragram.

![This is an image](/algorithm_diagram.png)

Present available chart type includes: table, scatter, line chart, histogram

## Reference
* [Essay That Introduces Dash](https://medium.com/plotly/introducing-dash-5ecf7191b503)
* [Dash Github](https://github.com/plotly/dash/)
* [Dash Online Tutorial](https://dash.plotly.com/)
