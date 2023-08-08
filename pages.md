

# UFC FIGHT PREDICTOR (WebScraping)

### Table of Contents

* [Building](#Building-and-Running-our-Scraper)
* [Organizing](#Organizing)
* [Assigning](#Assigning-everything)
* [Building](#Building-our-Model)
* [Testing](#Testing-our-Model)

Searching through  peoples projects especially on Kaggle, there are not many good models to predict UFC fights...
what I am doing to make mine different is taking in a couple factors other datasets did not

* height
* weight
* reach

And if I cannot get good enough results with that then

* record

_the reason we don't want to use record right now is because it would be a whole nother project to pull their records at the time of the fight... therefore if we use record it would be their record TODAY and not in the past_

Even though UFC fights are somewhat unpredicatable because a good hit is a good hit **My goal
is to make a model more accurate then the models I have already seen**

# Importing Necessary Libraries


```python
from bs4 import BeautifulSoup # Our webscraper
import pandas as pd # For our Data
import requests # To pull the website
import string # because I am lazy and not listing every single letter
```

# Building and Running our Scraper


```python
# Website lists fighters A - Z so to go through each letter we have the alphabet listed
letters = list(string.ascii_lowercase) 
each_row = [] 

for letter in letters:
    # website goes by last name
    website = f"http://ufcstats.com/statistics/fighters?char={letter}&page=all"
    response = requests.get(website)
    
    # building the scraper for all table rows or 'tr'
    soup = BeautifulSoup(response.content, 'html.parser')
    rows = soup.find_all('tr', class_='b-statistics__table-row')
    
    # For each row finding all table data 'td' in the row
    for row in rows:
        name_info = row.find_all('td', class_="b-statistics__table-col")
        
        # For every data point we find assign it accordingly
        if len(name_info) > 1:
            firstname = name_info[0].get_text(strip=True)
            lastname = name_info[1].get_text(strip = True)
            nickname = name_info[2].get_text(strip = True)
            height = name_info[3].get_text(strip = True)
            weight = name_info[4].get_text(strip = True)
            reach = name_info[5].get_text(strip = True)
            stance = name_info[6].get_text(strip = True)
            wins = name_info[7].get_text(strip = True)
            losses = name_info[8].get_text(strip = True)
            draws = name_info[9].get_text(strip = True)
            
            # Add to one large list to reform the row
            complete_row = [firstname, lastname, nickname, height, weight,
                            reach, stance, wins, losses, draws]
            
            # adding all rows to our empty list
            each_row.append(complete_row)
```

# Making our DataFrame


```python
# Making the Dataset and naming everything accordling
ufc_fighters = pd.DataFrame(each_row)
ufc_fighters = ufc_fighters.rename(columns = {0: 'First', 1: 'Last', 2: 'Nickname', 3: 'HT', 4: 'WT', 5: 'Reach', 6: 'Stance', 
                                              7: 'Win', 8: 'Loss', 9: 'Draw'})
ufc_fighters.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Last</th>
      <th>Nickname</th>
      <th>HT</th>
      <th>WT</th>
      <th>Reach</th>
      <th>Stance</th>
      <th>Win</th>
      <th>Loss</th>
      <th>Draw</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Tom</td>
      <td>Aaron</td>
      <td></td>
      <td>--</td>
      <td>155 lbs.</td>
      <td>--</td>
      <td></td>
      <td>5</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Danny</td>
      <td>Abbadi</td>
      <td>The Assassin</td>
      <td>5' 11"</td>
      <td>155 lbs.</td>
      <td>--</td>
      <td>Orthodox</td>
      <td>4</td>
      <td>6</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Nariman</td>
      <td>Abbasov</td>
      <td>Bayraktar</td>
      <td>5' 8"</td>
      <td>155 lbs.</td>
      <td>66.0"</td>
      <td>Orthodox</td>
      <td>28</td>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>David</td>
      <td>Abbott</td>
      <td>Tank</td>
      <td>6' 0"</td>
      <td>265 lbs.</td>
      <td>--</td>
      <td>Switch</td>
      <td>10</td>
      <td>15</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Hamdy</td>
      <td>Abdelwahab</td>
      <td>The Hammer</td>
      <td>6' 2"</td>
      <td>264 lbs.</td>
      <td>72.0"</td>
      <td>Southpaw</td>
      <td>5</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
ufc_fighters.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Last</th>
      <th>Nickname</th>
      <th>HT</th>
      <th>WT</th>
      <th>Reach</th>
      <th>Stance</th>
      <th>Win</th>
      <th>Loss</th>
      <th>Draw</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4073</th>
      <td>Dave</td>
      <td>Zitanick</td>
      <td></td>
      <td>--</td>
      <td>170 lbs.</td>
      <td>--</td>
      <td></td>
      <td>5</td>
      <td>7</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4074</th>
      <td>Alex</td>
      <td>Zuniga</td>
      <td></td>
      <td>--</td>
      <td>145 lbs.</td>
      <td>--</td>
      <td></td>
      <td>6</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4075</th>
      <td>George</td>
      <td>Zuniga</td>
      <td></td>
      <td>5' 9"</td>
      <td>185 lbs.</td>
      <td>--</td>
      <td></td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4076</th>
      <td>Allan</td>
      <td>Zuniga</td>
      <td>Tigre</td>
      <td>5' 7"</td>
      <td>155 lbs.</td>
      <td>70.0"</td>
      <td>Orthodox</td>
      <td>13</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4077</th>
      <td>Virgil</td>
      <td>Zwicker</td>
      <td>RezDog</td>
      <td>6' 2"</td>
      <td>205 lbs.</td>
      <td>74.0"</td>
      <td></td>
      <td>15</td>
      <td>6</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



Our dataset seems to have worked exactly how we wanted it to thankfully

# Exporting


```python
ufc_fighters.to_csv('ufc_fighters.csv', index = False)
```

# Second Scraper for More Data

First we need to get each link to every fight in the past 10ish years


```python
# Defining the website used and giving an empty list for all websites of each fight to be used
website = "http://www.ufcstats.com/statistics/events/completed?page=all"
websites = []

# Once again requesting rhe website and scraping all 'a' divs since they contain urls
response = requests.get(website)
soup = BeautifulSoup(response.content, 'html.parser')
links = soup.find_all('a', class_ = "b-link b-link_style_black")

# The section the URL is in is the HREF so we find all of those then add it to the list
for link in links:
    url = link['href']
    websites.append(url)
```

Now that we have the link to every fight we need to find the link to every fight


```python
# Now that we have the links to every fight we scarpe every fight for its info
fights = []

# For every fight we scrape the fighters
for site in websites:
    response = requests.get(site)
    soup = BeautifulSoup(response.content, 'html.parser')
    fighters = soup.find_all('a', class_ = 'b-link b-link_style_black')
    
    # Extracting who fought in each fight (the way UFC ordered it was odd numbers won and even number lost)
    for fight in fighters:
        text = fight.text.strip()
        fights.append(text)
```


```python
# Two fighter in each fight therefore divide by 2 is amount of fights
len(fights)/2
```




    7245.0



# Making the Dataset


```python
# Making empty lists and n variable
winners = []
losers = []
n = 1

# The data was ordered that all odd numbers listed in the fight was the winner
for fighters in fights:
    if n%2 == 0:
        losers.append(fighters)
    else:
        winners.append(fighters)
    n += 1        
```


```python
# If equal it most likely worked
len(winners) == len(losers)
```




    True




```python
# To lastly verify the last fighter that won should be ""Scott Morris""
# And the last to lose should be ""Sean Daugherty""
winners[-1], losers[-1]
```




    ('Scott Morris', 'Sean Daugherty')



**SUCCESS**

# Making our Winners and Fighters Database


```python
# Turn into dictionary
ufc_fights = {
    'Winners': winners,
    'Losers': losers
}

# Convert to pandas because pandas is our friend and easier to use
ufc_fights = pd.DataFrame(ufc_fights)
ufc_fights.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Winners</th>
      <th>Losers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Tom Aspinall</td>
      <td>Marcin Tybura</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Julija Stoliarenko</td>
      <td>Molly McCann</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Nathaniel Wood</td>
      <td>Andre Fili</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Paul Craig</td>
      <td>Andre Muniz</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Fares Ziam</td>
      <td>Jai Herbert</td>
    </tr>
  </tbody>
</table>
</div>




```python
ufc_fights.to_csv('fights.csv')
```

Next Step is to organize our data

# Organizing

We have all the fights ever and all the fighters ever, so next we need to append all the fighters statistics to each fight and then make it easier for PyTorch to understand.


```python
import pandas as pd # Our beloved to work with data
import numpy as np # In case we need to do some math
```

# Making the Columns

### For the Fighters Dataset

* We need the win percentage instead of their record
* We need their first and last name in one column
* We do not need their nickname, win, loss, draw columns, and eventually not their first and last name columns so drop those


```python
fighters = pd.read_csv('fighters.csv')
```


```python
# Win Percentage Calulcations
fighters['WinPercentage'] = fighters['Win'] / (fighters['Win'] + fighters['Loss'] + fighters['Draw'])
fighters['Experience'] = fighters['Win'] + fighters['Loss'] + fighters['Draw']
fighters['DrawPercentage'] = fighters['Draw'] / (fighters['Win'] + fighters['Loss'] + fighters['Draw'])
fighters['LossPercentage'] = fighters['Loss'] / (fighters['Win'] + fighters['Loss'] + fighters['Draw'])
# Getting rid of unnecessary columns
fighters.drop(columns=['Draw', 'Loss', 'Win', 'Nickname'], inplace=True)
```


```python
fighters.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>First</th>
      <th>Last</th>
      <th>HT</th>
      <th>WT</th>
      <th>Reach</th>
      <th>WinPercentage</th>
      <th>Experience</th>
      <th>DrawPercentage</th>
      <th>LossPercentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Tom</td>
      <td>Aaron</td>
      <td>--</td>
      <td>155 lbs.</td>
      <td>--</td>
      <td>0.625</td>
      <td>8</td>
      <td>0.0</td>
      <td>0.375</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Danny</td>
      <td>Abbadi</td>
      <td>5' 11"</td>
      <td>155 lbs.</td>
      <td>--</td>
      <td>0.400</td>
      <td>10</td>
      <td>0.0</td>
      <td>0.600</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Nariman</td>
      <td>Abbasov</td>
      <td>5' 8"</td>
      <td>155 lbs.</td>
      <td>66.0"</td>
      <td>0.875</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.125</td>
    </tr>
    <tr>
      <th>3</th>
      <td>David</td>
      <td>Abbott</td>
      <td>6' 0"</td>
      <td>265 lbs.</td>
      <td>--</td>
      <td>0.400</td>
      <td>25</td>
      <td>0.0</td>
      <td>0.600</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Hamdy</td>
      <td>Abdelwahab</td>
      <td>6' 2"</td>
      <td>264 lbs.</td>
      <td>72.0"</td>
      <td>1.000</td>
      <td>5</td>
      <td>0.0</td>
      <td>0.000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Combining first and last names
fighters['Name'] = fighters['First'] + ' ' + fighters['Last']
fighters.drop(columns=['First', 'Last'], inplace=True)
fighters.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>HT</th>
      <th>WT</th>
      <th>Reach</th>
      <th>WinPercentage</th>
      <th>Experience</th>
      <th>DrawPercentage</th>
      <th>LossPercentage</th>
      <th>Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>--</td>
      <td>155 lbs.</td>
      <td>--</td>
      <td>0.625</td>
      <td>8</td>
      <td>0.0</td>
      <td>0.375</td>
      <td>Tom Aaron</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5' 11"</td>
      <td>155 lbs.</td>
      <td>--</td>
      <td>0.400</td>
      <td>10</td>
      <td>0.0</td>
      <td>0.600</td>
      <td>Danny Abbadi</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5' 8"</td>
      <td>155 lbs.</td>
      <td>66.0"</td>
      <td>0.875</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.125</td>
      <td>Nariman Abbasov</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6' 0"</td>
      <td>265 lbs.</td>
      <td>--</td>
      <td>0.400</td>
      <td>25</td>
      <td>0.0</td>
      <td>0.600</td>
      <td>David Abbott</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6' 2"</td>
      <td>264 lbs.</td>
      <td>72.0"</td>
      <td>1.000</td>
      <td>5</td>
      <td>0.0</td>
      <td>0.000</td>
      <td>Hamdy Abdelwahab</td>
    </tr>
  </tbody>
</table>
</div>



### For the fights dataset

* Need to rearrange the order sometimes so the left column isnt always the winner and also so that we can keep track of that


```python
fights = pd.read_csv('fights.csv')
fights.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>Winners</th>
      <th>Losers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Tom Aspinall</td>
      <td>Marcin Tybura</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Julija Stoliarenko</td>
      <td>Molly McCann</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Nathaniel Wood</td>
      <td>Andre Fili</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Paul Craig</td>
      <td>Andre Muniz</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Fares Ziam</td>
      <td>Jai Herbert</td>
    </tr>
  </tbody>
</table>
</div>



# OUTLIERS

Before we move on I want to remove a couple big upsets in the UFC that will definitely skew our model. I am going to remove the following fights:

* Serra vs St. Pierre 1
* Holm vs Rousey
* Weidman vs Silva
* Pena vs Nunes
* Werdum vs Emilianenko
* Dillashaw vs Barao
* Diaz vs McGregor

This list may increase depending on how the model performs.


```python
fights_to_remove = [
    ('Matt Serra', 'Georges St. Pierre'),
    ('Holly Holm', 'Ronda Rousey'),
    ('Chris Weidman', 'Anderson Silva'),
    ('Julianna Pena', 'Amanda Nunes'),
    ('Fabrico Werdum', 'Fedor Emilianenko'),
    ('T.J. Dillashaw', 'Renan Barao'),
    ('Nate Diaz', 'Conor McGregor')
]

# Filter out the fights to be removed
filtered_fights = fights[~fights.apply(lambda row: (row['Winners'], row['Losers']) in fights_to_remove, axis=1)]
```


```python
# Creating a new column for the winner
fighter_1 = []
fighter_2 = []
winner = []

# Randomizing and keeping track of the winner
for index, row in fights.iterrows():
    # random number either 0 or 1
    number = np.random.randint(0, 2)
    
    # If its zero then we do not need to switch the winner
    if number == 0:
        fighter_1.append(row['Winners'])
        fighter_2.append(row['Losers'])
        winner.append(0)
        
    # If its one then we need to switch the order  
    else:
        fighter_1.append(row['Losers'])
        fighter_2.append(row['Winners'])
        winner.append(1)

# Making a new dataframe with the new columns
organized = pd.DataFrame({'Fighter 1': fighter_1, 'Fighter 2': fighter_2, 'Winner': winner})
organized.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Fighter 1</th>
      <th>Fighter 2</th>
      <th>Winner</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Marcin Tybura</td>
      <td>Tom Aspinall</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Julija Stoliarenko</td>
      <td>Molly McCann</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Andre Fili</td>
      <td>Nathaniel Wood</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Paul Craig</td>
      <td>Andre Muniz</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jai Herbert</td>
      <td>Fares Ziam</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



#### SUCCESS

# Assigning everything

We in short need to add all the players STATS to each fight, which sounds very intimidating (at least it did to me) BUT maybe it is not.


```python
fighters_1 = fighters.add_suffix('_1')
fighters_2 = fighters.add_suffix('_2')
```


```python
# I think its working? I am not 100% confident but I think it is
attempt_1 = pd.merge(organized, fighters_1, left_on='Fighter 1', right_on='Name_1')
attempt_1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Fighter 1</th>
      <th>Fighter 2</th>
      <th>Winner</th>
      <th>HT_1</th>
      <th>WT_1</th>
      <th>Reach_1</th>
      <th>WinPercentage_1</th>
      <th>Experience_1</th>
      <th>DrawPercentage_1</th>
      <th>LossPercentage_1</th>
      <th>Name_1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Marcin Tybura</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>249 lbs.</td>
      <td>78.0"</td>
      <td>0.75</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.25</td>
      <td>Marcin Tybura</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Marcin Tybura</td>
      <td>Blagoy Ivanov</td>
      <td>0</td>
      <td>6' 3"</td>
      <td>249 lbs.</td>
      <td>78.0"</td>
      <td>0.75</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.25</td>
      <td>Marcin Tybura</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Marcin Tybura</td>
      <td>Alexandr Romanov</td>
      <td>0</td>
      <td>6' 3"</td>
      <td>249 lbs.</td>
      <td>78.0"</td>
      <td>0.75</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.25</td>
      <td>Marcin Tybura</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Marcin Tybura</td>
      <td>Greg Hardy</td>
      <td>0</td>
      <td>6' 3"</td>
      <td>249 lbs.</td>
      <td>78.0"</td>
      <td>0.75</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.25</td>
      <td>Marcin Tybura</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Marcin Tybura</td>
      <td>Augusto Sakai</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>249 lbs.</td>
      <td>78.0"</td>
      <td>0.75</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.25</td>
      <td>Marcin Tybura</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7246</th>
      <td>Robert Lucarelli</td>
      <td>Orlando Wiet</td>
      <td>1</td>
      <td>6' 2"</td>
      <td>245 lbs.</td>
      <td>--</td>
      <td>0.00</td>
      <td>1</td>
      <td>0.0</td>
      <td>1.00</td>
      <td>Robert Lucarelli</td>
    </tr>
    <tr>
      <th>7247</th>
      <td>Thaddeus Luster</td>
      <td>Frank Hamaker</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>210 lbs.</td>
      <td>--</td>
      <td>0.00</td>
      <td>1</td>
      <td>0.0</td>
      <td>1.00</td>
      <td>Thaddeus Luster</td>
    </tr>
    <tr>
      <th>7248</th>
      <td>David Levicki</td>
      <td>Johnny Rhodes</td>
      <td>1</td>
      <td>6' 5"</td>
      <td>275 lbs.</td>
      <td>--</td>
      <td>0.25</td>
      <td>4</td>
      <td>0.0</td>
      <td>0.75</td>
      <td>David Levicki</td>
    </tr>
    <tr>
      <th>7249</th>
      <td>Ray Wizard</td>
      <td>Patrick Smith</td>
      <td>1</td>
      <td>--</td>
      <td>--</td>
      <td>--</td>
      <td>0.00</td>
      <td>1</td>
      <td>0.0</td>
      <td>1.00</td>
      <td>Ray Wizard</td>
    </tr>
    <tr>
      <th>7250</th>
      <td>Sean Daugherty</td>
      <td>Scott Morris</td>
      <td>1</td>
      <td>6' 0"</td>
      <td>175 lbs.</td>
      <td>--</td>
      <td>0.00</td>
      <td>2</td>
      <td>0.0</td>
      <td>1.00</td>
      <td>Sean Daugherty</td>
    </tr>
  </tbody>
</table>
<p>7251 rows × 11 columns</p>
</div>




```python
attempt_2 = pd.merge(attempt_1, fighters_2, left_on='Fighter 2', right_on='Name_2')
attempt_2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Fighter 1</th>
      <th>Fighter 2</th>
      <th>Winner</th>
      <th>HT_1</th>
      <th>WT_1</th>
      <th>Reach_1</th>
      <th>WinPercentage_1</th>
      <th>Experience_1</th>
      <th>DrawPercentage_1</th>
      <th>LossPercentage_1</th>
      <th>Name_1</th>
      <th>HT_2</th>
      <th>WT_2</th>
      <th>Reach_2</th>
      <th>WinPercentage_2</th>
      <th>Experience_2</th>
      <th>DrawPercentage_2</th>
      <th>LossPercentage_2</th>
      <th>Name_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Marcin Tybura</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>249 lbs.</td>
      <td>78.0"</td>
      <td>0.750000</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.250000</td>
      <td>Marcin Tybura</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.812500</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.187500</td>
      <td>Tom Aspinall</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jake Collier</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>230 lbs.</td>
      <td>78.0"</td>
      <td>0.590909</td>
      <td>22</td>
      <td>0.0</td>
      <td>0.409091</td>
      <td>Jake Collier</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.812500</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.187500</td>
      <td>Tom Aspinall</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Curtis Blaydes</td>
      <td>Tom Aspinall</td>
      <td>0</td>
      <td>6' 4"</td>
      <td>265 lbs.</td>
      <td>80.0"</td>
      <td>0.809524</td>
      <td>21</td>
      <td>0.0</td>
      <td>0.190476</td>
      <td>Curtis Blaydes</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.812500</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.187500</td>
      <td>Tom Aspinall</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alan Baudot</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>243 lbs.</td>
      <td>79.0"</td>
      <td>0.666667</td>
      <td>12</td>
      <td>0.0</td>
      <td>0.333333</td>
      <td>Alan Baudot</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.812500</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.187500</td>
      <td>Tom Aspinall</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andrei Arlovski</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>240 lbs.</td>
      <td>77.0"</td>
      <td>0.607143</td>
      <td>56</td>
      <td>0.0</td>
      <td>0.392857</td>
      <td>Andrei Arlovski</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.812500</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.187500</td>
      <td>Tom Aspinall</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7242</th>
      <td>David Levicki</td>
      <td>Johnny Rhodes</td>
      <td>1</td>
      <td>6' 5"</td>
      <td>275 lbs.</td>
      <td>--</td>
      <td>0.250000</td>
      <td>4</td>
      <td>0.0</td>
      <td>0.750000</td>
      <td>David Levicki</td>
      <td>6' 0"</td>
      <td>210 lbs.</td>
      <td>--</td>
      <td>0.666667</td>
      <td>3</td>
      <td>0.0</td>
      <td>0.333333</td>
      <td>Johnny Rhodes</td>
    </tr>
    <tr>
      <th>7243</th>
      <td>Patrick Smith</td>
      <td>Scott Morris</td>
      <td>0</td>
      <td>6' 2"</td>
      <td>225 lbs.</td>
      <td>--</td>
      <td>0.540541</td>
      <td>37</td>
      <td>0.0</td>
      <td>0.459459</td>
      <td>Patrick Smith</td>
      <td>5' 10"</td>
      <td>210 lbs.</td>
      <td>--</td>
      <td>0.666667</td>
      <td>3</td>
      <td>0.0</td>
      <td>0.333333</td>
      <td>Scott Morris</td>
    </tr>
    <tr>
      <th>7244</th>
      <td>Sean Daugherty</td>
      <td>Scott Morris</td>
      <td>1</td>
      <td>6' 0"</td>
      <td>175 lbs.</td>
      <td>--</td>
      <td>0.000000</td>
      <td>2</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>Sean Daugherty</td>
      <td>5' 10"</td>
      <td>210 lbs.</td>
      <td>--</td>
      <td>0.666667</td>
      <td>3</td>
      <td>0.0</td>
      <td>0.333333</td>
      <td>Scott Morris</td>
    </tr>
    <tr>
      <th>7245</th>
      <td>Robert Lucarelli</td>
      <td>Orlando Wiet</td>
      <td>1</td>
      <td>6' 2"</td>
      <td>245 lbs.</td>
      <td>--</td>
      <td>0.000000</td>
      <td>1</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>Robert Lucarelli</td>
      <td>5' 10"</td>
      <td>170 lbs.</td>
      <td>--</td>
      <td>0.166667</td>
      <td>6</td>
      <td>0.0</td>
      <td>0.833333</td>
      <td>Orlando Wiet</td>
    </tr>
    <tr>
      <th>7246</th>
      <td>Thaddeus Luster</td>
      <td>Frank Hamaker</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>210 lbs.</td>
      <td>--</td>
      <td>0.000000</td>
      <td>1</td>
      <td>0.0</td>
      <td>1.000000</td>
      <td>Thaddeus Luster</td>
      <td>--</td>
      <td>--</td>
      <td>--</td>
      <td>1.000000</td>
      <td>1</td>
      <td>0.0</td>
      <td>0.000000</td>
      <td>Frank Hamaker</td>
    </tr>
  </tbody>
</table>
<p>7247 rows × 19 columns</p>
</div>




```python
fight_data = attempt_2.drop(columns=['Name_1', 'Name_2'])
fight_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Fighter 1</th>
      <th>Fighter 2</th>
      <th>Winner</th>
      <th>HT_1</th>
      <th>WT_1</th>
      <th>Reach_1</th>
      <th>WinPercentage_1</th>
      <th>Experience_1</th>
      <th>DrawPercentage_1</th>
      <th>LossPercentage_1</th>
      <th>HT_2</th>
      <th>WT_2</th>
      <th>Reach_2</th>
      <th>WinPercentage_2</th>
      <th>Experience_2</th>
      <th>DrawPercentage_2</th>
      <th>LossPercentage_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Marcin Tybura</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>249 lbs.</td>
      <td>78.0"</td>
      <td>0.750000</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.250000</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jake Collier</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>230 lbs.</td>
      <td>78.0"</td>
      <td>0.590909</td>
      <td>22</td>
      <td>0.0</td>
      <td>0.409091</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Curtis Blaydes</td>
      <td>Tom Aspinall</td>
      <td>0</td>
      <td>6' 4"</td>
      <td>265 lbs.</td>
      <td>80.0"</td>
      <td>0.809524</td>
      <td>21</td>
      <td>0.0</td>
      <td>0.190476</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alan Baudot</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>243 lbs.</td>
      <td>79.0"</td>
      <td>0.666667</td>
      <td>12</td>
      <td>0.0</td>
      <td>0.333333</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andrei Arlovski</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>240 lbs.</td>
      <td>77.0"</td>
      <td>0.607143</td>
      <td>56</td>
      <td>0.0</td>
      <td>0.392857</td>
      <td>6' 5"</td>
      <td>256 lbs.</td>
      <td>78.0"</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
  </tbody>
</table>
</div>



# Cleaning

* replace all '--' values with 'NaN'
* convert feet to inches
* get rid of any row that has NaN value


```python
# Getting rid of the '--' values cause there are stupid and annoying
for column in fight_data.columns:
    fight_data[column] = fight_data[column].replace({'--': np.nan})
```


```python
# Height Conversion Function
def convert_height(height):
    
    # Ignore it if it already happened
    if type(height) == float:
        return height
    
    # Otherwise strip the feet and inches and convert it to inches
    else:
        feet_inches = height.split("'")
        feet = float(feet_inches[0])
        inches = float(feet_inches[1].replace('"', ''))
        return (feet * 12) + inches
```


```python
# Doing the easier stuff that I know will work first whicfh means no more words in the numbers
fight_data['WT_1'] = fight_data['WT_1'].str.replace(' lbs.', '').astype(float)
fight_data['WT_2'] = fight_data['WT_2'].str.replace(' lbs.', '').astype(float)

fight_data['Reach_1'] = fight_data['Reach_1'].str.replace('"', '').astype(float)
fight_data['Reach_2'] = fight_data['Reach_2'].str.replace('"', '').astype(float)
```


```python
fight_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Fighter 1</th>
      <th>Fighter 2</th>
      <th>Winner</th>
      <th>HT_1</th>
      <th>WT_1</th>
      <th>Reach_1</th>
      <th>WinPercentage_1</th>
      <th>Experience_1</th>
      <th>DrawPercentage_1</th>
      <th>LossPercentage_1</th>
      <th>HT_2</th>
      <th>WT_2</th>
      <th>Reach_2</th>
      <th>WinPercentage_2</th>
      <th>Experience_2</th>
      <th>DrawPercentage_2</th>
      <th>LossPercentage_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Marcin Tybura</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>249.0</td>
      <td>78.0</td>
      <td>0.750000</td>
      <td>32</td>
      <td>0.0</td>
      <td>0.250000</td>
      <td>6' 5"</td>
      <td>256.0</td>
      <td>78.0</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jake Collier</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>230.0</td>
      <td>78.0</td>
      <td>0.590909</td>
      <td>22</td>
      <td>0.0</td>
      <td>0.409091</td>
      <td>6' 5"</td>
      <td>256.0</td>
      <td>78.0</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Curtis Blaydes</td>
      <td>Tom Aspinall</td>
      <td>0</td>
      <td>6' 4"</td>
      <td>265.0</td>
      <td>80.0</td>
      <td>0.809524</td>
      <td>21</td>
      <td>0.0</td>
      <td>0.190476</td>
      <td>6' 5"</td>
      <td>256.0</td>
      <td>78.0</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Alan Baudot</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>243.0</td>
      <td>79.0</td>
      <td>0.666667</td>
      <td>12</td>
      <td>0.0</td>
      <td>0.333333</td>
      <td>6' 5"</td>
      <td>256.0</td>
      <td>78.0</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andrei Arlovski</td>
      <td>Tom Aspinall</td>
      <td>1</td>
      <td>6' 3"</td>
      <td>240.0</td>
      <td>77.0</td>
      <td>0.607143</td>
      <td>56</td>
      <td>0.0</td>
      <td>0.392857</td>
      <td>6' 5"</td>
      <td>256.0</td>
      <td>78.0</td>
      <td>0.8125</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.1875</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Making all the heights into inches
fight_data['HT_1'] = fight_data['HT_1'].apply(convert_height)
fight_data['HT_2'] = fight_data['HT_2'].apply(convert_height)
```


```python
# LASTY NO NULL VALUES
fight_data.dropna(inplace=True)
fight_data.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Fighter 1</th>
      <th>Fighter 2</th>
      <th>Winner</th>
      <th>HT_1</th>
      <th>WT_1</th>
      <th>Reach_1</th>
      <th>WinPercentage_1</th>
      <th>Experience_1</th>
      <th>DrawPercentage_1</th>
      <th>LossPercentage_1</th>
      <th>HT_2</th>
      <th>WT_2</th>
      <th>Reach_2</th>
      <th>WinPercentage_2</th>
      <th>Experience_2</th>
      <th>DrawPercentage_2</th>
      <th>LossPercentage_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7000</th>
      <td>Jess Liaudin</td>
      <td>David Bielkheden</td>
      <td>1</td>
      <td>69.0</td>
      <td>170.0</td>
      <td>72.0</td>
      <td>0.645161</td>
      <td>31</td>
      <td>0.0</td>
      <td>0.354839</td>
      <td>70.0</td>
      <td>155.0</td>
      <td>71.0</td>
      <td>0.666667</td>
      <td>36</td>
      <td>0.000000</td>
      <td>0.333333</td>
    </tr>
    <tr>
      <th>7002</th>
      <td>Jason Lambert</td>
      <td>Terry Martin</td>
      <td>0</td>
      <td>75.0</td>
      <td>185.0</td>
      <td>75.0</td>
      <td>0.666667</td>
      <td>39</td>
      <td>0.0</td>
      <td>0.333333</td>
      <td>68.0</td>
      <td>185.0</td>
      <td>71.0</td>
      <td>0.676471</td>
      <td>34</td>
      <td>0.000000</td>
      <td>0.323529</td>
    </tr>
    <tr>
      <th>7007</th>
      <td>Rameau Thierry Sokoudjou</td>
      <td>Kazuhiro Nakamura</td>
      <td>0</td>
      <td>70.0</td>
      <td>205.0</td>
      <td>78.0</td>
      <td>0.500000</td>
      <td>36</td>
      <td>0.0</td>
      <td>0.500000</td>
      <td>71.0</td>
      <td>205.0</td>
      <td>70.0</td>
      <td>0.617647</td>
      <td>34</td>
      <td>0.000000</td>
      <td>0.382353</td>
    </tr>
    <tr>
      <th>7009</th>
      <td>James Irvin</td>
      <td>Mike Kyle</td>
      <td>1</td>
      <td>74.0</td>
      <td>205.0</td>
      <td>75.0</td>
      <td>0.629630</td>
      <td>27</td>
      <td>0.0</td>
      <td>0.370370</td>
      <td>76.0</td>
      <td>205.0</td>
      <td>77.0</td>
      <td>0.594595</td>
      <td>37</td>
      <td>0.027027</td>
      <td>0.378378</td>
    </tr>
    <tr>
      <th>7052</th>
      <td>Phil Baroni</td>
      <td>Matt Lindland</td>
      <td>1</td>
      <td>69.0</td>
      <td>170.0</td>
      <td>72.0</td>
      <td>0.454545</td>
      <td>33</td>
      <td>0.0</td>
      <td>0.545455</td>
      <td>72.0</td>
      <td>185.0</td>
      <td>74.0</td>
      <td>0.709677</td>
      <td>31</td>
      <td>0.000000</td>
      <td>0.290323</td>
    </tr>
  </tbody>
</table>
</div>



**SUCCESS**

# Caluclate the differences

Lastly to make it easier on our neural network we just want to see the difference from fighter_1 minus fighter_2


```python
HeightDiff = fight_data['HT_1'] - fight_data['HT_2']
WeightDiff = fight_data['WT_1'] - fight_data['WT_2']
ReachDiff = fight_data['Reach_1'] - fight_data['Reach_2']
WinDiff = fight_data['WinPercentage_1'] - fight_data['WinPercentage_2']
ExpDiff = fight_data['Experience_1'] - fight_data['Experience_2']
DrawDiff = fight_data['DrawPercentage_1'] - fight_data['DrawPercentage_2']
LossDiff = fight_data['LossPercentage_1'] - fight_data['LossPercentage_2']
```


```python
final_data = pd.DataFrame({'HeightDiff': HeightDiff, 'WeightDiff': WeightDiff, 'ReachDiff': ReachDiff,
                           'WinDiff': WinDiff, 'ExpDiff': ExpDiff, 'DrawDiff': DrawDiff, 'LossDiff':LossDiff, 'Winner': fight_data['Winner']})
final_data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>HeightDiff</th>
      <th>WeightDiff</th>
      <th>ReachDiff</th>
      <th>WinDiff</th>
      <th>ExpDiff</th>
      <th>DrawDiff</th>
      <th>LossDiff</th>
      <th>Winner</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-2.0</td>
      <td>-7.0</td>
      <td>0.0</td>
      <td>-0.062500</td>
      <td>16</td>
      <td>0.000000</td>
      <td>0.062500</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.0</td>
      <td>-26.0</td>
      <td>0.0</td>
      <td>-0.221591</td>
      <td>6</td>
      <td>0.000000</td>
      <td>0.221591</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1.0</td>
      <td>9.0</td>
      <td>2.0</td>
      <td>-0.002976</td>
      <td>5</td>
      <td>0.000000</td>
      <td>0.002976</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-2.0</td>
      <td>-13.0</td>
      <td>1.0</td>
      <td>-0.145833</td>
      <td>-4</td>
      <td>0.000000</td>
      <td>0.145833</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-2.0</td>
      <td>-16.0</td>
      <td>-1.0</td>
      <td>-0.205357</td>
      <td>40</td>
      <td>0.000000</td>
      <td>0.205357</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>7000</th>
      <td>-1.0</td>
      <td>15.0</td>
      <td>1.0</td>
      <td>-0.021505</td>
      <td>-5</td>
      <td>0.000000</td>
      <td>0.021505</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7002</th>
      <td>7.0</td>
      <td>0.0</td>
      <td>4.0</td>
      <td>-0.009804</td>
      <td>5</td>
      <td>0.000000</td>
      <td>0.009804</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7007</th>
      <td>-1.0</td>
      <td>0.0</td>
      <td>8.0</td>
      <td>-0.117647</td>
      <td>2</td>
      <td>0.000000</td>
      <td>0.117647</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7009</th>
      <td>-2.0</td>
      <td>0.0</td>
      <td>-2.0</td>
      <td>0.035035</td>
      <td>-10</td>
      <td>-0.027027</td>
      <td>-0.008008</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7052</th>
      <td>-3.0</td>
      <td>-15.0</td>
      <td>-2.0</td>
      <td>-0.255132</td>
      <td>2</td>
      <td>0.000000</td>
      <td>0.255132</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>6177 rows × 8 columns</p>
</div>




```python
final_data['Winner'].value_counts()
```




    Winner
    0    3128
    1    3049
    Name: count, dtype: int64




```python
final_data.to_csv('ufc_fights.csv', index=False)
```

# SUCCESS

It appears all has worked and the data is ready to be trained! now lets hope I get good results

# Building a Predictor

Now that our data is in here and working the next step is to build the MODEL so we can put it to practice


```python
# Importing PyTorch libraries and the traintest split function
import torch
import torch.nn as nn
import torch.optim as optim
import torch.nn.functional as F
from torch.utils.data import TensorDataset, DataLoader
from sklearn.model_selection import train_test_split

# Importing libraries for plotting, data, and math
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

# Importing Data and Splitting

First step of any machine learning model is to import the data you want to work with and split it into our training and testing data. We will be using the train_test_split function from sklearn to do this.


```python
# Importing our data
data = pd.read_csv('ufc_fights.csv')
data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>HeightDiff</th>
      <th>WeightDiff</th>
      <th>ReachDiff</th>
      <th>WinDiff</th>
      <th>ExpDiff</th>
      <th>DrawDiff</th>
      <th>LossDiff</th>
      <th>Winner</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-2.0</td>
      <td>-7.0</td>
      <td>0.0</td>
      <td>-0.062500</td>
      <td>16</td>
      <td>0.0</td>
      <td>0.062500</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-2.0</td>
      <td>-26.0</td>
      <td>0.0</td>
      <td>-0.221591</td>
      <td>6</td>
      <td>0.0</td>
      <td>0.221591</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1.0</td>
      <td>9.0</td>
      <td>2.0</td>
      <td>-0.002976</td>
      <td>5</td>
      <td>0.0</td>
      <td>0.002976</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-2.0</td>
      <td>-13.0</td>
      <td>1.0</td>
      <td>-0.145833</td>
      <td>-4</td>
      <td>0.0</td>
      <td>0.145833</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-2.0</td>
      <td>-16.0</td>
      <td>-1.0</td>
      <td>-0.205357</td>
      <td>40</td>
      <td>0.0</td>
      <td>0.205357</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Defining the features and the target
X = data[['HeightDiff', 'WeightDiff', 'ReachDiff', 'WinDiff', 'ExpDiff', 'DrawDiff', 'LossDiff']]
y = data['Winner']
y = torch.eye(2)[y]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=128)
X_train.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>HeightDiff</th>
      <th>WeightDiff</th>
      <th>ReachDiff</th>
      <th>WinDiff</th>
      <th>ExpDiff</th>
      <th>DrawDiff</th>
      <th>LossDiff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3250</th>
      <td>-1.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>-0.076531</td>
      <td>-21</td>
      <td>0.035714</td>
      <td>0.040816</td>
    </tr>
    <tr>
      <th>1261</th>
      <td>1.0</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>0.002179</td>
      <td>-10</td>
      <td>-0.037037</td>
      <td>0.034858</td>
    </tr>
    <tr>
      <th>22</th>
      <td>-1.0</td>
      <td>4.0</td>
      <td>-4.0</td>
      <td>0.047297</td>
      <td>-17</td>
      <td>0.000000</td>
      <td>-0.047297</td>
    </tr>
    <tr>
      <th>619</th>
      <td>5.0</td>
      <td>0.0</td>
      <td>9.0</td>
      <td>-0.067766</td>
      <td>11</td>
      <td>0.000000</td>
      <td>0.067766</td>
    </tr>
    <tr>
      <th>114</th>
      <td>-4.0</td>
      <td>-10.0</td>
      <td>1.0</td>
      <td>0.020678</td>
      <td>8</td>
      <td>0.000000</td>
      <td>-0.020678</td>
    </tr>
  </tbody>
</table>
</div>




```python
y_train
```




    tensor([[1., 0.],
            [0., 1.],
            [1., 0.],
            ...,
            [1., 0.],
            [1., 0.],
            [0., 1.]])



# Making PyTorch understand our data

Because PyTorch is a library that can use Tensors we are going to convert all of our data into tensors to speed up the process of training our model and also so we dont have to do it later.


```python
# Assuming you have already defined X_train_tensor, X_test_tensor, y_train, and y_test

# Convert training data into float tensors
X_train_tensor = torch.tensor(X_train.values, dtype=torch.float32)
X_test_tensor = torch.tensor(X_test.values, dtype=torch.float32)

# Convert target labels into long tensors (integers)
y_train_tensor = y_train.clone().detach().to(torch.float32)
y_test_tensor = y_test.clone().detach().to(torch.float32)

# Define the train and test datasets
train_dataset = TensorDataset(X_train_tensor, y_train_tensor)
test_dataset = TensorDataset(X_test_tensor, y_test_tensor)

# Create the train and test data loaders
batch_size = 32
train_loader = DataLoader(train_dataset, batch_size=batch_size, shuffle=True)
test_loader = DataLoader(test_dataset, batch_size=batch_size, shuffle=False)
```

# Making the Neural Network Architecture

Given that our data is not very complex we will be using a simple neural network with 2 hidden layers and 1 output layer. We will be using the ReLU activation function for our hidden layers and the Sigmoid activation function for our output layer.


```python
class Net(nn.Module):
    """
    Defining the neural network architecture with 2 hidden layers and 3 fully connected layers
    
    forward: like all neural networks, this function takes in the input and passes it through the layers
    
    loss
    """
    def __init__(self, input_size=7, hidden_1=14, hidden_2 = 4, output_size=2):
        super(Net, self).__init__()
        self.fc1 = nn.Linear(input_size, hidden_1)
        self.fc2 = nn.Linear(hidden_1, hidden_2)
        self.fc3 = nn.Linear(hidden_2, output_size)
        self.sigmoid = nn.Sigmoid() 
        self.relu = nn.ReLU()

    def forward(self, x):
        x = self.relu(self.fc1(x))
        x = self.relu(self.fc2(x))
        x = self.fc3(x)
        output = self.sigmoid(x)
        return output
    
    def loss(self, y_pred, y_true):
        # Binary cross entropy loss function
        criterion = nn.BCEWithLogitsLoss()
        return criterion(y_pred, y_true)

# setting this variable for the future
net = Net()
```

# Optimizer Function and Setting Device

Setting the device so it will use the GPU is there is one, and also using the Adam optimizer due to our smaller dataset, and speed.


```python
def optimizer(lr = 0.01):
    
    # Adam optimizer
    optimizer = optim.Adam(net.parameters(), lr=lr)
    return optimizer

device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
net.to(device)

optimizer_ = optimizer()
```

# Training Loop

Now that we have most of our functions made we can now make our training loop. This is where we will be training our model and seeing how well it does.


```python
def training(numb_epochs=10):
    # Saving all epoch losses to visualize later
    epoch_losses = []
    
    # Training the model for how many times we say to
    for epoch in range(numb_epochs):
        # Saving the running loss
        running_loss = 0.0
        
        for inputs, labels in train_loader:
            # Moving the data to the GPU (unless I am on my laptop)
            inputs, labels = inputs.to(device), labels.to(device)
            # zero the gradients
            optimizer_.zero_grad()
            # Forward pass
            outputs = net(inputs)
            # Squeeze outputs to match the shape of labels
            outputs = outputs.squeeze()
            # Calculating the loss  
            loss = net.loss(outputs, labels)
            # Backward pass to compute the gradients
            loss.backward()
            # Updating Parameters
            optimizer_.step()
            # Update Running loss for this epoch
            running_loss += loss.item()
        
        # Calculate the average loss for this epoch
        epoch_loss = running_loss / len(train_loader)
        epoch_losses.append(epoch_loss)
        
        # Print the average loss for this epoch
        print(f"Epoch {epoch + 1}, Loss: {epoch_loss}")
    
    # Plotting loss values
    plt.plot(range(1, numb_epochs + 1), epoch_losses, marker='o')
    plt.xlabel('Epoch')
    plt.ylabel('Loss')
    plt.title('Training Loss vs. Epochs')
    plt.grid(True)
    plt.show()
    
    # DONE
    print("Training finished.")

```

# Starting the Training


```python
numb_epochs = 100
training(numb_epochs)
```

    Epoch 1, Loss: 0.6960421523740215
    Epoch 2, Loss: 0.6770576946197018
    Epoch 3, Loss: 0.6634053349494934
    Epoch 4, Loss: 0.6594270813849664
    Epoch 5, Loss: 0.6599538503154632
    Epoch 6, Loss: 0.6601998675254084
    Epoch 7, Loss: 0.6590805845875893
    Epoch 8, Loss: 0.658729374793268
    Epoch 9, Loss: 0.6588746559235358
    Epoch 10, Loss: 0.6586478283328395
    Epoch 11, Loss: 0.6584276272404579
    Epoch 12, Loss: 0.6578421181248081
    Epoch 13, Loss: 0.658924062021317
    Epoch 14, Loss: 0.6573653559530935
    Epoch 15, Loss: 0.6582988177576373
    Epoch 16, Loss: 0.6574487282383826
    Epoch 17, Loss: 0.6565920618272597
    Epoch 18, Loss: 0.6570995734583946
    Epoch 19, Loss: 0.655734952803581
    Epoch 20, Loss: 0.6558390786570888
    Epoch 21, Loss: 0.6562255113355575
    Epoch 22, Loss: 0.6565302256614931
    Epoch 23, Loss: 0.6557546577145976
    Epoch 24, Loss: 0.6563633703416394
    Epoch 25, Loss: 0.6561134895970745
    Epoch 26, Loss: 0.6555369903964381
    Epoch 27, Loss: 0.6559365549395161
    Epoch 28, Loss: 0.6559232150354694
    Epoch 29, Loss: 0.6552975189301276
    Epoch 30, Loss: 0.6554438683294481
    Epoch 31, Loss: 0.6552443604315481
    Epoch 32, Loss: 0.6547331956125075
    Epoch 33, Loss: 0.6544960506500737
    Epoch 34, Loss: 0.6553468100486263
    Epoch 35, Loss: 0.655958439073255
    Epoch 36, Loss: 0.654805641020498
    Epoch 37, Loss: 0.6543483441875827
    Epoch 38, Loss: 0.6546426450052569
    Epoch 39, Loss: 0.6550539524324479
    Epoch 40, Loss: 0.6557241086036928
    Epoch 41, Loss: 0.6549835997243081
    Epoch 42, Loss: 0.6542556128194256
    Epoch 43, Loss: 0.6544483873151964
    Epoch 44, Loss: 0.6541911786602389
    Epoch 45, Loss: 0.654813555748232
    Epoch 46, Loss: 0.6544728194513628
    Epoch 47, Loss: 0.6534645626621861
    Epoch 48, Loss: 0.6554040024357457
    Epoch 49, Loss: 0.6550358580004785
    Epoch 50, Loss: 0.6536857474234796
    Epoch 51, Loss: 0.6546803324453292
    Epoch 52, Loss: 0.6535027661631184
    Epoch 53, Loss: 0.6543305208606105
    Epoch 54, Loss: 0.6538356669487492
    Epoch 55, Loss: 0.655351197719574
    Epoch 56, Loss: 0.655071142027455
    Epoch 57, Loss: 0.6542219208132836
    Epoch 58, Loss: 0.6536662878528718
    Epoch 59, Loss: 0.6544296337712195
    Epoch 60, Loss: 0.6536508529416977
    Epoch 61, Loss: 0.6537258932667394
    Epoch 62, Loss: 0.6541884645338981
    Epoch 63, Loss: 0.6544564312504184
    Epoch 64, Loss: 0.6534918454385573
    Epoch 65, Loss: 0.6538110552295562
    Epoch 66, Loss: 0.6543524388344057
    Epoch 67, Loss: 0.65304282634489
    Epoch 68, Loss: 0.6545931266200158
    Epoch 69, Loss: 0.6527835930547407
    Epoch 70, Loss: 0.6538929473969244
    Epoch 71, Loss: 0.6527688672465662
    Epoch 72, Loss: 0.6527104600783318
    Epoch 73, Loss: 0.6536579189761993
    Epoch 74, Loss: 0.6555801929966096
    Epoch 75, Loss: 0.6531758346865254
    Epoch 76, Loss: 0.6531255252899663
    Epoch 77, Loss: 0.6543074669376496
    Epoch 78, Loss: 0.6542929791635083
    Epoch 79, Loss: 0.6541048907464551
    Epoch 80, Loss: 0.6532756359346451
    Epoch 81, Loss: 0.6538565362653425
    Epoch 82, Loss: 0.6536649700134031
    Epoch 83, Loss: 0.6533131880144919
    Epoch 84, Loss: 0.6531403164709768
    Epoch 85, Loss: 0.6523044582336179
    Epoch 86, Loss: 0.653483130854945
    Epoch 87, Loss: 0.6540397328715171
    Epoch 88, Loss: 0.652951310142394
    Epoch 89, Loss: 0.6526842382646376
    Epoch 90, Loss: 0.6535423863318659
    Epoch 91, Loss: 0.6547728146276166
    Epoch 92, Loss: 0.6532103084748792
    Epoch 93, Loss: 0.652385974314905
    Epoch 94, Loss: 0.6541423432288631
    Epoch 95, Loss: 0.6540405081164452
    Epoch 96, Loss: 0.6543812974806755
    Epoch 97, Loss: 0.6547023180992373
    Epoch 98, Loss: 0.6539551565724034
    Epoch 99, Loss: 0.652438376411315
    Epoch 100, Loss: 0.6530195059314851
    


    
![png](output_17_1.png)
    


    Training finished.
    

# Evaluate the Model

We got the model to train wooooooooo now we have to see if it is actually any good


```python
def evaluate_model(model, test_loader, criterion):
    model.eval()
    total_loss = 0.0
    correct = 0
    total = 0

    with torch.no_grad():
        for inputs, labels in test_loader:
            inputs, labels = inputs.to(device), labels.to(device)
            outputs = model(inputs)
            loss = criterion(outputs, torch.argmax(labels, dim=1))
            total_loss += loss.item()

            _, predicted = torch.max(outputs.data, 1)
            total += labels.size(0)
            correct += (predicted == torch.argmax(labels, dim=1)).sum().item() 

    test_loss = total_loss / len(test_loader)
    test_accuracy = 100 * correct / total

    print(f"Test Loss: {test_loss:.4f}, Test Accuracy: {test_accuracy:.2f}%")
```

# DISCLAIMER

**The test accuracy looks very low, however we must also realize that this is not as simple as training dogs and cat images and the MNIST dataset thing**

**UFC fights have upsets and injuries, and sadly rigging so we ARE NOT EXPECTING really high performance like 90% in this model it is just UNREALISTIC**


```python
criterion = nn.CrossEntropyLoss()
evaluate_model(net, test_loader, criterion)
```

    Test Loss: 0.6015, Test Accuracy: 70.15%
