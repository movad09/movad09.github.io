
Xiaohan Pei
Sukey Xu
Mounica Vaddadi 


```python
import numpy as np
import json
import bqplot
```


```python
characters = json.load(open('shared/sp18-is590dv/data/star_wars/people.json'))
```


```python
chars = ["Boba Fett", "Yoda", "Jabba Desilijic Tiure", "Darth Vader", "Obi-Wan Kenobi",
         "Beru Whitesun lars", "Mon Mothma"]
```


```python
films = json.load(open("shared/sp18-is590dv/data/star_wars/films.json"))
```


```python
char_input = json.load(open("shared/sp18-is590dv/data/star_wars/people.json"))
characters = {}
for character in char_input:
    characters[character['pk']] = character['fields']
```


```python
film_info = []
film_names = []
for film in films:
    film_in = {}
    
    film_name = film["fields"]["title"]
    
    char = []
    for character in film["fields"]["characters"]:
        if characters[character]["name"] in chars:
            char.append(characters[character]["name"])
    film_in[film_name] = char
    film_names.append(film_name)
    film_info.append(film_in)
film_info

```




    [{'A New Hope': ['Darth Vader',
       'Beru Whitesun lars',
       'Obi-Wan Kenobi',
       'Jabba Desilijic Tiure']},
     {'The Empire Strikes Back': ['Darth Vader',
       'Obi-Wan Kenobi',
       'Yoda',
       'Boba Fett']},
     {'Return of the Jedi': ['Darth Vader',
       'Obi-Wan Kenobi',
       'Jabba Desilijic Tiure',
       'Yoda',
       'Boba Fett',
       'Mon Mothma']},
     {'The Phantom Menace': ['Obi-Wan Kenobi', 'Jabba Desilijic Tiure', 'Yoda']},
     {'Attack of the Clones': ['Beru Whitesun lars',
       'Obi-Wan Kenobi',
       'Yoda',
       'Boba Fett']},
     {'Revenge of the Sith': ['Darth Vader',
       'Beru Whitesun lars',
       'Obi-Wan Kenobi',
       'Yoda']}]




```python
film_names
```




    ['A New Hope',
     'The Empire Strikes Back',
     'Return of the Jedi',
     'The Phantom Menace',
     'Attack of the Clones',
     'Revenge of the Sith']




```python
for film in films:
    film_chars = film["fields"]["characters"]
    print(film["fields"]["title"])
    for character in film_chars:
        if characters[character]["name"] in chars: print(characters[character]["name"])
    print()
    
```

    A New Hope
    Darth Vader
    Beru Whitesun lars
    Obi-Wan Kenobi
    Jabba Desilijic Tiure
    
    The Empire Strikes Back
    Darth Vader
    Obi-Wan Kenobi
    Yoda
    Boba Fett
    
    Return of the Jedi
    Darth Vader
    Obi-Wan Kenobi
    Jabba Desilijic Tiure
    Yoda
    Boba Fett
    Mon Mothma
    
    The Phantom Menace
    Obi-Wan Kenobi
    Jabba Desilijic Tiure
    Yoda
    
    Attack of the Clones
    Beru Whitesun lars
    Obi-Wan Kenobi
    Yoda
    Boba Fett
    
    Revenge of the Sith
    Darth Vader
    Beru Whitesun lars
    Obi-Wan Kenobi
    Yoda
    



```python
node_data = []
for char in chars:
    node_char = {}
    node_char['label'] = char
    node_char['name'] = char
    char_films = []
    for film_in in film_info:
        if char in list(film_in.values())[0]:
            char_films.append(list(film_in.keys())[0])
    node_char['movies'] = ', '.join(char_films)
    node_char['shape'] = 'circle'
    node_data.append(node_char)
node_data
```




    [{'label': 'Boba Fett',
      'name': 'Boba Fett',
      'movies': 'The Empire Strikes Back, Return of the Jedi, Attack of the Clones',
      'shape': 'circle'},
     {'label': 'Yoda',
      'name': 'Yoda',
      'movies': 'The Empire Strikes Back, Return of the Jedi, The Phantom Menace, Attack of the Clones, Revenge of the Sith',
      'shape': 'circle'},
     {'label': 'Jabba Desilijic Tiure',
      'name': 'Jabba Desilijic Tiure',
      'movies': 'A New Hope, Return of the Jedi, The Phantom Menace',
      'shape': 'circle'},
     {'label': 'Darth Vader',
      'name': 'Darth Vader',
      'movies': 'A New Hope, The Empire Strikes Back, Return of the Jedi, Revenge of the Sith',
      'shape': 'circle'},
     {'label': 'Obi-Wan Kenobi',
      'name': 'Obi-Wan Kenobi',
      'movies': 'A New Hope, The Empire Strikes Back, Return of the Jedi, The Phantom Menace, Attack of the Clones, Revenge of the Sith',
      'shape': 'circle'},
     {'label': 'Beru Whitesun lars',
      'name': 'Beru Whitesun lars',
      'movies': 'A New Hope, Attack of the Clones, Revenge of the Sith',
      'shape': 'circle'},
     {'label': 'Mon Mothma',
      'name': 'Mon Mothma',
      'movies': 'Return of the Jedi',
      'shape': 'circle'}]




```python
list(film_info[0].keys())[0]
```




    'A New Hope'




```python
link_data = []
for film_in in film_info:
    char_list = list(film_in.values())[0]
    for char in char_list:
        for link_char in char_list:
            if link_char != char:
                link_dict = {}
                link_dict['source'] = chars.index(char)
                link_dict['target'] = chars.index(link_char)
                link_data.append(link_dict)
```


```python
link_data
```




    [{'source': 3, 'target': 5},
     {'source': 3, 'target': 4},
     {'source': 3, 'target': 2},
     {'source': 5, 'target': 3},
     {'source': 5, 'target': 4},
     {'source': 5, 'target': 2},
     {'source': 4, 'target': 3},
     {'source': 4, 'target': 5},
     {'source': 4, 'target': 2},
     {'source': 2, 'target': 3},
     {'source': 2, 'target': 5},
     {'source': 2, 'target': 4},
     {'source': 3, 'target': 4},
     {'source': 3, 'target': 1},
     {'source': 3, 'target': 0},
     {'source': 4, 'target': 3},
     {'source': 4, 'target': 1},
     {'source': 4, 'target': 0},
     {'source': 1, 'target': 3},
     {'source': 1, 'target': 4},
     {'source': 1, 'target': 0},
     {'source': 0, 'target': 3},
     {'source': 0, 'target': 4},
     {'source': 0, 'target': 1},
     {'source': 3, 'target': 4},
     {'source': 3, 'target': 2},
     {'source': 3, 'target': 1},
     {'source': 3, 'target': 0},
     {'source': 3, 'target': 6},
     {'source': 4, 'target': 3},
     {'source': 4, 'target': 2},
     {'source': 4, 'target': 1},
     {'source': 4, 'target': 0},
     {'source': 4, 'target': 6},
     {'source': 2, 'target': 3},
     {'source': 2, 'target': 4},
     {'source': 2, 'target': 1},
     {'source': 2, 'target': 0},
     {'source': 2, 'target': 6},
     {'source': 1, 'target': 3},
     {'source': 1, 'target': 4},
     {'source': 1, 'target': 2},
     {'source': 1, 'target': 0},
     {'source': 1, 'target': 6},
     {'source': 0, 'target': 3},
     {'source': 0, 'target': 4},
     {'source': 0, 'target': 2},
     {'source': 0, 'target': 1},
     {'source': 0, 'target': 6},
     {'source': 6, 'target': 3},
     {'source': 6, 'target': 4},
     {'source': 6, 'target': 2},
     {'source': 6, 'target': 1},
     {'source': 6, 'target': 0},
     {'source': 4, 'target': 2},
     {'source': 4, 'target': 1},
     {'source': 2, 'target': 4},
     {'source': 2, 'target': 1},
     {'source': 1, 'target': 4},
     {'source': 1, 'target': 2},
     {'source': 5, 'target': 4},
     {'source': 5, 'target': 1},
     {'source': 5, 'target': 0},
     {'source': 4, 'target': 5},
     {'source': 4, 'target': 1},
     {'source': 4, 'target': 0},
     {'source': 1, 'target': 5},
     {'source': 1, 'target': 4},
     {'source': 1, 'target': 0},
     {'source': 0, 'target': 5},
     {'source': 0, 'target': 4},
     {'source': 0, 'target': 1},
     {'source': 3, 'target': 5},
     {'source': 3, 'target': 4},
     {'source': 3, 'target': 1},
     {'source': 5, 'target': 3},
     {'source': 5, 'target': 4},
     {'source': 5, 'target': 1},
     {'source': 4, 'target': 3},
     {'source': 4, 'target': 5},
     {'source': 4, 'target': 1},
     {'source': 1, 'target': 3},
     {'source': 1, 'target': 5},
     {'source': 1, 'target': 4}]




```python
link_data1 = []
for link in link_data:
    if link not in link_data1:
        link_data1.append(link)
link_data = link_data1
link_data
```




    [{'source': 3, 'target': 5},
     {'source': 3, 'target': 4},
     {'source': 3, 'target': 2},
     {'source': 5, 'target': 3},
     {'source': 5, 'target': 4},
     {'source': 5, 'target': 2},
     {'source': 4, 'target': 3},
     {'source': 4, 'target': 5},
     {'source': 4, 'target': 2},
     {'source': 2, 'target': 3},
     {'source': 2, 'target': 5},
     {'source': 2, 'target': 4},
     {'source': 3, 'target': 1},
     {'source': 3, 'target': 0},
     {'source': 4, 'target': 1},
     {'source': 4, 'target': 0},
     {'source': 1, 'target': 3},
     {'source': 1, 'target': 4},
     {'source': 1, 'target': 0},
     {'source': 0, 'target': 3},
     {'source': 0, 'target': 4},
     {'source': 0, 'target': 1},
     {'source': 3, 'target': 6},
     {'source': 4, 'target': 6},
     {'source': 2, 'target': 1},
     {'source': 2, 'target': 0},
     {'source': 2, 'target': 6},
     {'source': 1, 'target': 2},
     {'source': 1, 'target': 6},
     {'source': 0, 'target': 2},
     {'source': 0, 'target': 6},
     {'source': 6, 'target': 3},
     {'source': 6, 'target': 4},
     {'source': 6, 'target': 2},
     {'source': 6, 'target': 1},
     {'source': 6, 'target': 0},
     {'source': 5, 'target': 1},
     {'source': 5, 'target': 0},
     {'source': 1, 'target': 5},
     {'source': 0, 'target': 5}]




```python
tooltip = bqplot.Tooltip(fields=["name",'movies'])

graph = bqplot.Graph(node_data=node_data, link_data=link_data, tooltip = tooltip, link_type = 'line', link_distance = 250,charge= -500)

fig = bqplot.Figure(marks = [graph])
display(fig)
```


    Figure(fig_margin={'top': 60, 'bottom': 60, 'left': 60, 'right': 60}, layout=Layout(min_width='125px'), marks=…

