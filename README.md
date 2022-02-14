# imdb-top-1000-movies
IMDb (also known as the Internet Movie Database) is an online database of information related to movies, it combines movie plot Metastore ratings, critic and user ratings, and release year.
Content
This dataset includes IMDB top 1,000 movies of all time with attributes such as Title, Ratings, Year, Genre, etc.

Acknowledgements
The data in this dataset has been scraped using Python BeautifulSoup from the publicly available website https://www.imdb.com.

* imopritng useful libraries:
  
* call url using get func:


* call html parser

* formate the html content using:

#all conatint in h3 tag where class name is lister-item-header

# Creating a loop for all 100 pages:


* creating loop for each page where data want to extract:


import pandas as pd
import requests
from bs4 import BeautifulSoup

response=requests.get("https://www.imdb.com/search/title/?count=100&groups=top_1000&sort=user_rating").text

soup=BeautifulSoup(response,'lxml')

print(soup.prettify())

for i in soup.find_all('h3'):
    print(i.text.strip())
    
info =soup.find_all('div',class_='lister-item-content') #searching on whole description box and save into info.

len(info)

for j in range(1,100):
    url ='https://www.imdb.com/search/title/?groups=top_1000&sort=user_rating,desc&count=100&start=201&ref_={}'.format(j)
    response=requests.get("https://www.imdb.com/search/title/?count=100&groups=top_1000&sort=user_rating").text
    soup=BeautifulSoup(response,'lxml')
    info =soup.find_all('div',class_='lister-item-content')
    
    Movie_Name =[]
    Year=[]
    Rating=[]
    genre=[]
    for i in info:
        #print(i.find('a').text.strip()) # print what do you want
        Movie_Name.append(i.find('a').text.strip())
        #print(i.find('span',class_="lister-item-year text-muted unbold").text)
        Year.append(i.find('span',class_="lister-item-year text-muted unbold").text.strip())
        #print(i.find('div',class_="inline-block ratings-imdb-rating").text.strip())
        Rating.append(i.find('div',class_="inline-block ratings-imdb-rating").text.strip())
        #print(i.find('span',class_='genre').text.strip())
        genre.append(i.find('span',class_='genre').text.strip())

    #creating dataframe:
    d={'Movie_Name':Movie_Name, 'Year':Year, 'Rating':Rating, 'genre':genre }
    df=pd.DataFrame(d)
    final=final.append(df,ignore_index=True)
    
