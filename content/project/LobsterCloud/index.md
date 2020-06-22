---
date: "2020-06-22T00:00:00Z"
image:
  caption: 
  focal_point: Smart
summary: Creating a stylized wordcloud from an academic paper
tags:
- wordcloud
- text manipulation
- natural language processing
- Python
- Jupyter Notebook
- Visualizations
- Data Visualization
title: Lobster Wordcloud from Academic Paper
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
---

# Creating a Word Cloud from an Academic Paper!

#### Word clouds are a fun and creative way to show what the most used words are in a book, document, website or basically anything containing words. I wanted to create a word cloud from one of my papers that were published from my PhD. To take it a step further, I wanted to turn it into a lobster cloud because the paper is on a species of spiny lobster. Here is a walkthrough of how I did this...

#### First, let's see what we will need to import. Academic papers are usually in a column format, so I used the [pdf_layout_scanner](https://pypi.org/project/PDF-Layout-Scanner/) package by Yusuke Shinyama to import the pdf into a format that can be read by the computer. It extracts text from pdf's with multiple columns.


```python
from pdf_layout_scanner import layout_scanner
```

#### Next, we import the [Natural Language Toolkit](https://www.nltk.org/) or nltk library, which we will use to tokenize words from the pdf. We will also remove stop words which are commonly used words such as 'the', 'a', 'in' etc.


```python
from nltk import word_tokenize
from nltk.corpus import stopwords
```

#### Then we want to import the [WordCloud](http://amueller.github.io/word_cloud/) library.


```python
from wordcloud import WordCloud
```

#### Another important library we will need is the [Pillow (PIL)](https://python-pillow.org/). Pillow is used for opening, manipulating and saving different image file formats  


```python
from PIL import Image
```

#### Finally, we will import numpy, pandas and matplotlib


```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```

#### Step 1: Parse the pdf file using layout_scanner


```python
pages = layout_scanner.get_pages('lobster.pdf')
```

    

#### Check how many pages we working with


```python
print(len(pages))
```

    44
    

#### Created a variable called text to store the first 23 pages of the paper (the rest are references, or images)


```python
text = pages[0:23]
```


```python
type(text)
```




    list



#### Our text is currently a list, which we will have to convert into a string. We have 52251 words


```python
text2 = ' '.join(text) #convert list to string simplest method
```


```python
len(text2)
```




    52251



#### After tokenizing and removing stop words we see that the number of words is now reduced to 6711


```python
stop_words = set(stopwords.words('english'))
text_tokens = word_tokenize(text2)

filtered_words = [w for w in text_tokens if not w in stop_words]
len(filtered_words)

```




    6711




```python
filtered_words = ' '.join(filtered_words)
```

#### Now for the fun part! To make the wordcloud in the shape of a lobster, we will need a vector .png image of a lobster. The best resource for finding pictures of biological creatures is [PhyloPic](http://phylopic.org/). Download the image to your working folder and assign it to a variable.


```python
LOB_FILE = 'Spiny2.png'
```

#### Here is an example of how an ordinary word cloud looks


```python
word_cloud = WordCloud().generate(filtered_words)

plt.imshow(word_cloud, interpolation = 'bilinear')
plt.axis('off')
plt.show()
```


![png](LobsterCloud_28_0.png)


#### Now we will use pillow to read the image and do some manipulations. We have to create an image mask from the lobster image which will be a canvas for the wordcloud


```python
icon = Image.open(LOB_FILE)
#creating blank image object using pillow
image_mask = Image.new(mode='RGB', size = icon.size, color = (255, 255, 255))
image_mask.paste(icon, box = icon)
rgb_array = np.array(image_mask) #converts the image object into an array


word_cloud = WordCloud(mask = rgb_array, background_color = 'white',
                      max_words = 1000, colormap = 'ocean', max_font_size = 300)
word_cloud.generate(filtered_words.upper())

plt.figure(figsize=[20, 20])
plt.imshow(word_cloud, interpolation = 'bilinear')
plt.axis('off')
plt.show()
```


![png](LobsterCloud_30_0.png)


#### And there you have it! A beautiful lobster wordcloud created from an academic paper
