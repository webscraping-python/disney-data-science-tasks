# disney data science tasks

1. [Importar librerías ](#schema1)

<hr>

<a name="schema1"></a>

# 1. Importar librerías
~~~python
from bs4 import BeautifulSoup as bs
import requests
~~~
<hr>

<a name="schema2"></a>

# 2 Cargar la pagina web y convertir a un objeto BeautifulSoup

Primero vamos a scrapear una sola película

~~~python
url = "https://en.wikipedia.org/wiki/Toy_Story_3"
r = requests.get(url)

r
<Response [200]>
~~~

Convertir a un objeto `BeautifulSoup` y buscamos `infobox vevent` que es donde esta la información que queremos scrapear

~~~python
soup = bs(r.content)
info_box = soup.find(class_ = "infobox vevent")
info_box.get_text()
'Toy Story 3Theatrical release posterDirected byLee UnkrichProduced byDarla K. AndersonScreenplay byMichael ArndtStory by\nJohn Lasseter\nAndrew Stanton\nLee Unkrich\nStarring\nTom Hanks\nTim Allen\nJoan Cusack\nDon Rickles\nWallace Shawn\nJohn Ratzenberger\nEstelle Harris\nNed Beatty\nMichael Keaton\nJodi Benson\nJohn Morris\nMusic byRandy NewmanCinematography\nJeremy Lasky\nKim White\nEdited byKen SchretzmannProductioncompanies \nWalt Disney Pictures\nPixar Animation Studios\nDistributed byWalt Disney StudiosMotion PicturesRelease date\nJune\xa012,\xa02010\xa0(2010-06-12) (Taormina Film Fest)\nJune\xa018,\xa02010\xa0(2010-06-18) (United States)\nRunning time103 minutes[1]CountryUnited StatesLanguageEnglishBudget$200\xa0million[1]Box office$1.067\xa0billion[1]'


info_rows = info_box.find_all('tr')
~~~

<hr>

<a name="schema3"></a>


# 3. Creamos un diccionario con las información

~~~python
movie_info = {}
~~~

Creamos una función llamada `get_content_value` para obtener los datos que estan contenidos en una lista `li`
~~~python

def get_content_value(row_data):
    if row_data.find("li"):
        return [li.get_text(" ", strip=True).replace("\xa0", " ") for li in row_data.find_all("li")]
    else:
        return row_data.get_text(" ", strip=True).replace("\xa0", " ")
~~~
Con este `for` iteramos por todas las filas de `info_rows` y buscamos la info

~~~python
for index, row in enumerate(info_rows):
    print(index)
    if index == 0:
        movie_info['title'] = row.find("th").get_text(" ", strip=True)
    elif index == 1:
        continue
    else:
        print(row)
        content_key = row.find("th").get_text(" ", strip=True)   
        content_value = get_content_value(row.find("td"))
        movie_info[content_key] = content_value
        
movie_info
{'title': 'Toy Story 3',
 'Directed by': 'Lee Unkrich',
 'Produced by': 'Darla K. Anderson',
 'Screenplay by': 'Michael Arndt',
 'Story by': ['John Lasseter', 'Andrew Stanton', 'Lee Unkrich'],
 'Starring': ['Tom Hanks',
  'Tim Allen',
  'Joan Cusack',
  'Don Rickles',
  'Wallace Shawn',
  'John Ratzenberger',
  'Estelle Harris',
  'Ned Beatty',
  'Michael Keaton',
  'Jodi Benson',
  'John Morris'],
 'Music by': 'Randy Newman',
 'Cinematography': ['Jeremy Lasky', 'Kim White'],
 'Edited by': 'Ken Schretzmann',
 'Production companies': ['Walt Disney Pictures', 'Pixar Animation Studios'],
 'Distributed by': 'Walt Disney Studios Motion Pictures',
 'Release date': ['June 12, 2010 ( 2010-06-12 ) ( Taormina Film Fest )',
  'June 18, 2010 ( 2010-06-18 ) (United States)'],
 'Running time': '103 minutes [1]',
 'Country': 'United States',
 'Language': 'English',
 'Budget': '$200 million [1]',
 'Box office': '$1.067 billion [1]'} 
~~~




















<hr>

<a name="schema7"></a>

# 7. Documentación
https://www.youtube.com/watch?v=Ewgy-G9cmbg&t=93s

https://github.com/KeithGalli/disney-data-science-tasks


https://en.wikipedia.org/wiki/List_of_Walt_Disney_Pictures_films