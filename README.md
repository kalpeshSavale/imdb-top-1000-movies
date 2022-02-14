# imdb-top-1000-movies
IMDb (also known as the Internet Movie Database) is an online database of information related to movies, it combines movie plot Metastore ratings, critic and user ratings, and release year.
Content
This dataset includes IMDB top 1,000 movies of all time with attributes such as Title, Ratings, Year, Genre, etc.

Acknowledgements
The data in this dataset has been scraped using Python BeautifulSoup from the publicly available website https://www.imdb.com.

* imopritng useful libraries:

      import pandas as pd
      import requests
      from bs4 import BeautifulSoup
  
* call url using get function and convert into text:

      response=requests.get("https://www.imdb.com/search/title/?count=100&groups=top_1000&sort=user_rating").text


* call html parser

      soup=BeautifulSoup(response,'lxml')

* formate the html content using:

      print(soup.prettify())


#all conatint in h3 tag where class name is lister-item-header


      soup.find_all('h3')[0]
      
      
# Creating  FOR loop for all h3 tag 

      for i in soup.find_all('h3'):
          print(i.text.strip())
          
  
#searching on whole description box and save into info:
       
       info =soup.find_all('div',class_='lister-item-content')


# checking a length of h3 tag data where 1 page conataine a 100 movies data:

       len(info)


# try code foor 1 page where 100 movies data will be extarcted:

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
      df
      
      
      
# creating same loop for all 100 pages and store in list we created:
      
     # Creating a loop for all 100 pages:
      final=pd.DataFrame()

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

