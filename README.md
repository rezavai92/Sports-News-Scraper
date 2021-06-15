 # Sports News Scraper from "THE DAILY STAR" 
 
A small web scraping project focused on scraping only the sports section of renowned bangladeshi newspaper  **"The Daily Star"**  .We going to grab following parts from the sports section
 
 - #### Featured Sports News
 - #### Latest News
 - #### Other Sport News
 
In this project we are going to use following libararies: 
 
 - beautifulsoup4 (Please install this on your local machine )
 
 - urlib (built-in)
 


```python
from urllib.request import urlopen
import re
from bs4 import BeautifulSoup
html = urlopen("https://www.thedailystar.net/sports")
soup = BeautifulSoup(html,"html.parser")

```

##### Common Path is required for internal links of the site


```python
common_path = "https://www.thedailystar.net/"
```

### Featured Sports News 

We will store the featured news in a dictionary named featuredSportsNews with following keys/properties :
- link : the link of the news on newspaper's site.
- image : featured image
- headline : headline of the news
- paragraph : text of the news


```python
#featuredNews
try :
    ft_highlight_link = soup.find(class_="highlight-feature").select("a")[0].get("href")
    featuredImg = soup.find(class_ ="highlight-feature").a.picture.img
    ft_highlight_text = soup.find(class_="highlight-text")
    

except Exception as e:
    print(e)

finally :
    
    featuredSportsNews = {

        "link"  : common_path+ft_highlight_link,
        "image" :{ "src": featuredImg.get("data-srcset"),
                  "alt":featuredImg.get("alt"), 
                  "title":featuredImg.get("title") } ,
        "headline" : ft_highlight_text.h2.text,
        "paragraph" : ft_highlight_text.p.text

    }
    #print(featuredSportsNews)
    

```

#### *Print Featured Sports News*


```python
featuredSportsNews
```




    {'link': 'https://www.thedailystar.net//sports/tennis/news/djokovic-the-peoples-champion-2111529',
     'image': {'src': 'https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/medium_8/public/feature/images/2021/06/13/djokovic_french_open_title.jpg?itok=xQ2eThMA&timestamp=1623607203 444w',
      'alt': 'Djokovic: The people’s champion ',
      'title': ''},
     'headline': 'Djokovic: The people’s champion ',
     'paragraph': 'Novak Djokovic may not seem like the intimidating tennis virtuoso athlete from the outset; rather, like somebody one might approach in a room full of strangers to ask for the Wi-Fi password, for instance. Fondly referred to as the Djoker, his demeanour in general, feels so vibrant and likeable that one might feel like cracking an informal joke with him; and he seems humble and humane- way more than an average superstar athlete or a comic superhero, yet he has morphed into a champion hero for the ages and for all ages.'}



### 2. Latest Sports News

This is the part that comes up with a sidebar news on main site . It's a dynamic content that gets changed from time to time .


```python

section_sports = soup.select(".section-sports >div .updates > ul >li ")

#print(section_sports)
section_sports_list = []

for item in section_sports:
    section_sports_list.append({"image" : {
            "src" : item.a.picture.img.get("data-srcset"),
            "title" : item.a.picture.img.get("title"),
            "alt" : item.a.picture.img.get("alt")
        },
                                
        "topic" : {
            "href":common_path+item.select("div .overflow-hidden > a")[0].get("href"),
            "text": item.select("div .overflow-hidden >a")[0].text
            
        },
        "headline" : {
            "text" : item.select("div .overflow-hidden >h5>a")[0].text,
            "href" : common_path+item.select("div .overflow-hidden >h5>a")[0].get("href")
        }                       
                                
                               })

        
```

####  *Print Latest Sports News*


```python
section_sports
```




    [<li>
     <a class="avater" href="/sports/cricket/news/dpl-super-league-be-telecast-live-tv-2111489"><picture>
     <!--[if IE 9]><video style="display: none;"><![endif]-->
     <source data-aspectratio="/" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/kazi-inam-ahmed.gif?itok=W2rIZnTR&amp;timestamp=1623750364 1x" media="(min-width: 1024px)"/>
     <!--[if IE 9]></video><![endif]-->
     <!--[if lt IE 9]>
     <img  class="lazyload" data-aspectratio="" data-src="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/kazi-inam-ahmed.gif?itok=W2rIZnTR&amp;timestamp=1623750364" alt="DPL Super League to be telecast live on TV  " title="" />
     <![endif]-->
     <!--[if !lt IE 9]><!-->
     <img alt="DPL Super League to be telecast live on TV  " class="lazyload" data-aspectratio="" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/kazi-inam-ahmed.gif?itok=W2rIZnTR&amp;timestamp=1623750364" title=""/>
     <!-- <![endif]-->
     </picture></a> <div class="overflow-hidden">
     <a class="sub-head margin-bottom-zero color-green" href="/sports/cricket">Cricket</a> <h5><a href="/sports/cricket/news/dpl-super-league-be-telecast-live-tv-2111489">DPL Super League to be telecast live on TV </a></h5>
     </div>
     </li>,
     <li>
     <a class="avater" href="/sports/football/news/im-fine-eriksen-greets-hospital-bed-2111453"><picture>
     <!--[if IE 9]><video style="display: none;"><![endif]-->
     <source data-aspectratio="/" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/christian-eriksen_0.gif?itok=U8EDt4V0&amp;timestamp=1623744731 1x" media="(min-width: 1024px)"/>
     <!--[if IE 9]></video><![endif]-->
     <!--[if lt IE 9]>
     <img  class="lazyload" data-aspectratio="" data-src="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/christian-eriksen_0.gif?itok=U8EDt4V0&amp;timestamp=1623744731" alt="&#039;I&#039;m fine&#039;: Eriksen greets from hospital bed" title="" />
     <![endif]-->
     <!--[if !lt IE 9]><!-->
     <img alt="'I'm fine': Eriksen greets from hospital bed" class="lazyload" data-aspectratio="" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/christian-eriksen_0.gif?itok=U8EDt4V0&amp;timestamp=1623744731" title=""/>
     <!-- <![endif]-->
     </picture></a> <div class="overflow-hidden">
     <a class="sub-head margin-bottom-zero color-green" href="/sports/football">Football</a> <h5><a href="/sports/football/news/im-fine-eriksen-greets-hospital-bed-2111453">'I'm fine': Eriksen greets from hospital bed</a></h5>
     </div>
     </li>,
     <li>
     <a class="avater" href="/sports/football/news/france-face-germany-portugal-begin-title-defence-2111405"><picture>
     <!--[if IE 9]><video style="display: none;"><![endif]-->
     <source data-aspectratio="/" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/france.gif?itok=_x9ubv4H&amp;timestamp=1623738960 1x" media="(min-width: 1024px)"/>
     <!--[if IE 9]></video><![endif]-->
     <!--[if lt IE 9]>
     <img  class="lazyload" data-aspectratio="" data-src="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/france.gif?itok=_x9ubv4H&amp;timestamp=1623738960" alt="France face Germany as Portugal begin title defence " title="" />
     <![endif]-->
     <!--[if !lt IE 9]><!-->
     <img alt="France face Germany as Portugal begin title defence " class="lazyload" data-aspectratio="" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/france.gif?itok=_x9ubv4H&amp;timestamp=1623738960" title=""/>
     <!-- <![endif]-->
     </picture></a> <div class="overflow-hidden">
     <a class="sub-head margin-bottom-zero color-green" href="/sports/football">Football</a> <h5><a href="/sports/football/news/france-face-germany-portugal-begin-title-defence-2111405">France face Germany as Portugal begin title defence </a></h5>
     </div>
     </li>,
     <li>
     <a class="avater" href="/sports/tennis/news/tsitsipas-learned-grandmothers-death-minutes-final-2111381"><picture>
     <!--[if IE 9]><video style="display: none;"><![endif]-->
     <source data-aspectratio="/" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/14/tsitsipas.jpg?itok=Pbf6aWJL&amp;timestamp=1623737348 1x" media="(min-width: 1024px)"/>
     <!--[if IE 9]></video><![endif]-->
     <!--[if lt IE 9]>
     <img  class="lazyload" data-aspectratio="" data-src="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/14/tsitsipas.jpg?itok=Pbf6aWJL&amp;timestamp=1623737348" alt="Tsitsipas learned of grandmother&#039;s death minutes before final" title="" />
     <![endif]-->
     <!--[if !lt IE 9]><!-->
     <img alt="Tsitsipas learned of grandmother's death minutes before final" class="lazyload" data-aspectratio="" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/14/tsitsipas.jpg?itok=Pbf6aWJL&amp;timestamp=1623737348" title=""/>
     <!-- <![endif]-->
     </picture></a> <div class="overflow-hidden">
     <a class="sub-head margin-bottom-zero color-green" href="/sports/tennis">Tennis</a> <h5><a href="/sports/tennis/news/tsitsipas-learned-grandmothers-death-minutes-final-2111381">Tsitsipas learned of grandmother's death minutes before final</a></h5>
     </div>
     </li>,
     <li>
     <a class="avater" href="/sports/football/news/messis-strike-not-enough-chile-frustrate-argentina-2111345"><picture>
     <!--[if IE 9]><video style="display: none;"><![endif]-->
     <source data-aspectratio="/" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/messi_and_vargas_1.jpg?itok=0vilGT3R&amp;timestamp=1623712802 1x" media="(min-width: 1024px)"/>
     <!--[if IE 9]></video><![endif]-->
     <!--[if lt IE 9]>
     <img  class="lazyload" data-aspectratio="" data-src="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/messi_and_vargas_1.jpg?itok=0vilGT3R&amp;timestamp=1623712802" alt="Messi&#039;s strike not enough as Chile frustrate Argentina" title="" />
     <![endif]-->
     <!--[if !lt IE 9]><!-->
     <img alt="Messi's strike not enough as Chile frustrate Argentina" class="lazyload" data-aspectratio="" data-srcset="https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/very_small_1/public/feature/images/2021/06/15/messi_and_vargas_1.jpg?itok=0vilGT3R&amp;timestamp=1623712802" title=""/>
     <!-- <![endif]-->
     </picture></a> <div class="overflow-hidden">
     <a class="sub-head margin-bottom-zero color-green" href="/sports/football">Football</a> <h5><a href="/sports/football/news/messis-strike-not-enough-chile-frustrate-argentina-2111345">Messi's strike not enough as Chile frustrate Argentina</a></h5>
     </div>
     </li>]



### 3. Other News 

In this part we are going to scrap all of other news that come within a grid below the featured news section .


```python
paneNewsRow = soup.find( id="block-system-main")
others =paneNewsRow.find(class_="pane-news-row").select(".thumb")
#print(others)
otherNews =[]

for news in others :
    otherNews.append({
        "link" : common_path+news.get("href"),
        "image" :{
            "src" : news.img.get("data-srcset"),
            "alt" : news.img.get("alt"),
            "title" : news.img.get("title")
        }
    })


```

####  *Print Other News*


```python
otherNews
```




    [{'link': 'https://www.thedailystar.net//sports/football/news/paraguay-come-behind-beat-bolivia-3-1-2111393',
      'image': {'src': 'https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/medium_3/public/feature/images/2021/06/15/paraguay-bolivia.gif?itok=F2qrRVL8&timestamp=1623738099',
       'alt': 'Paraguay come from behind to beat Bolivia 3-1',
       'title': ''}},
     {'link': 'https://www.thedailystar.net//sports/football/news/morata-jeered-wasteful-spain-frustrated-sweden-2111341',
      'image': {'src': 'https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/medium_3/public/feature/images/2021/06/15/morata.jpg?itok=G6GyClhF&timestamp=1623709346',
       'alt': 'Morata jeered as wasteful Spain frustrated by Sweden',
       'title': ''}},
     {'link': 'https://www.thedailystar.net//sports/news/abahani-ride-munims-blazing-knock-2111149',
      'image': {'src': 'https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/medium_3/public/feature/images/2021/06/15/munim-shahriar.jpg?itok=ZAcTyXFz&timestamp=1623696865',
       'alt': 'Abahani ride on Munim’s blazing knock',
       'title': ''}},
     {'link': 'https://www.thedailystar.net//sports/news/icc-award-inspires-mushfiqur-2111145',
      'image': {'src': 'https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/medium_3/public/feature/images/2021/06/15/mushfiqur-rahim.jpg?itok=4hXL-hZK&timestamp=1623696354',
       'alt': 'ICC award inspires Mushfiqur ',
       'title': ''}},
     {'link': 'https://www.thedailystar.net//sports/news/depleted-bangladesh-against-oman-today-2111141',
      'image': {'src': 'https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/medium_3/public/feature/images/2021/06/15/bangladesh-football-team.jpg?itok=AAvNKAYI&timestamp=1623695246',
       'alt': 'Depleted Bangladesh up  against Oman today',
       'title': ''}},
     {'link': 'https://www.thedailystar.net//sports/news/everything-possible-2111137',
      'image': {'src': 'https://assetsds.cdnedge.bluemix.net/sites/default/files/styles/medium_3/public/feature/images/2021/06/15/novak-djokovic.jpg?itok=N9DW1pmB&timestamp=1623695087',
       'alt': '‘Everything is possible’',
       'title': ''}}]




```python

```
