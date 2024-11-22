# risa_risdiyanti-web-scraping-assignment
Assignment Web Scraping
Tujuan --> belajar melakukan web scraping pada web: https://www.detik.com/terpopuler/oto
Proses

from bs4 import BeautifulSoup
import requests
import pandas as pd
--> mendeklarasi BeautifulSoup agar dapat digunakan untuk membentuk html string ke content html
--> mendeklarasi pandas agar dapat digunakan

from types import resolve_bases
url = 'https://www.detik.com/terpopuler/oto'
response = requests.get(url)
--> mengambil content dari url web

soup = BeautifulSoup(response.text, 'html.parser')
--> melakukan generate content yang memiliki type .text ke html menggunakan BeautifulSoup yang disimpan ke variable Soup

list_content = soup.select('.list-content__item')
--> melakukan pengambilan content berdasarkan class '.list-content__item' yang disimpan kedalam variabel list_content

result = []
for item in list_content:
    data = dict()

    #data collection
    data['tilte'] = item.select_one('.media__title').text
    data['url'] = item.select_one ('.media__title a').get('href')
    data['image'] = item.select_one('.media__image img').get('src')
    data['url_image'] = item.select_one('.media__image a').get('href')
    result.append(data)
--> melakukan pengambilan data berdasarkan class yang akan disimpan kedalam variabel result yang memiliki type list

out = pd.DataFrame(result).to_json(orient='records')[1:-1].replace('},{', '} {').replace('\/','//').replace('\n','').replace('} {', '}, {')
--> melakukan convert value list menjadi data frame menggunakan pandas
--> dan melakukan convert data frame menjadi json string, yang disimpan kedalam variabel out

with open('df.json', 'w') as f:
  f.write('['+out+']')
--> melakukan penyimpanan file json kedalam backend colab

from google.colab import files
files.download('df.json')
--> melakukan deklarasi google.colab file agar dapat digunakan
--> melakukan download file yang sudah disimpan didalam google.colab
