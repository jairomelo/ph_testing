# Lesson From HTML to List of Words (part 2)

```
f = open('helloworld.txt','wb')
f.write('hello world')
f.close()
```
error: TypeError: a bytes-like object is required, not 'str'

solution: don't write text file as byte file

```
f = open('helloworld.txt','w')
f.write('hello world')
f.close()
```


## html-to-list1.py

*You should get something like the following.*
```
['324.', '\xc2\xa0', 'BENJAMIN', 'BOWSEY', '(a', 'blackmoor', ')', 'was', 'indicted', 'for', 'that', 'he', 'together', 'with', 'five', 'hundred', 'other', 'persons', 'and', 'more,', 'did,', 'unlawfully,', 'riotously,', 'and', 'tumultuously', 'assemble', 'on', 'the', '6th', 'of', 'June', 'to', 'the', 'disturbance', 'of', 'the', 'public', 'peace', 'and', 'did', 'begin', 'to', 'demolish', 'and', 'pull', 'down', 'the', 'dwelling', 'house', 'of', '\xc2\xa0', 'Richard', 'Akerman', ',', 'against', 'the', 'form', 'of', 'the', 'statute,', '&amp;c.', '\xc2\xa0', 'ROSE', 'JENNINGS', ',', 'Esq.', 'sworn.', 'Had', 'you', 'any', 'occasion', 'to', 'be', 'in', 'this', 'part', 'of', 'the', 'town,', 'on', 'the', '6th', 'of', 'June', 'in', 'the', 'evening?', '-', 'I', 'dined', 'with', 'my', 'brother', 'who', 'lives', 'opposite', 'Mr.', "Akerman's", 'house.', 'They', 'attacked', 'Mr.', "Akerman's", 'house', 'precisely', 'at', 'seven', "o'clock;",  'they', 'were', 'preceded', 'by', 'a', 'man', 'better', 'dressed', 'than', 'the', 'rest,', 'who']
```

Tested on Windows and Linux. The console result doesn't show ASCII characters. You can use this example instead:

```
['324.', 'BENJAMIN', 'BOWSEY', '(a', 'blackmoor', ')', 'was', 'indicted', 'for', 'that', 'he', 'together', 'with', 'five', 'hundred', 'other', 'persons', 'and', 'more,', 'did,', 'unlawfully,', 'riotously,', 'and', 'tumultuously', 'assemble', 'on', 'the', '6th', 'of', 'June', 'to', 'the', 'disturbance', 'of', 'the', 'public', 'peace', 'and', 'did', 'begin', 'to', 'demolish', 'and', 'pull', 'down', 'the', 'dwelling', 'house', 'of', 'Richard', 'Akerman', ',', 'against', 'the', 'form', 'of', 'the', 'statute,', '&amp;c.', 'ROSE', 'JENNINGS', ',', 'Esq.', 'sworn.', 'Had', 'you', 'any', 'occasion', 'to', 'be', 'in', 'this', 'part', 'of', 'the', 'town,', 'on', 'the', '6th', 'of', 'June', 'in', 'the', 'evening?', '-', 'I', 'dined', 'with', 'my', 'brother', 'who', 'lives', 'opposite', 'Mr.', "Akerman\\'s", 'house.', 'They', 'attacked', 'Mr.', "Akerman\\'s", 'house', 'precisely', 'at', 'seven', "o\\'clock;", 'they', 'were', 'preceded', 'by', 'a', 'man', 'better', 'dressed', 'than', 'the', 'rest,', 'who', 'went', 'up', 'to']
```

# Normalizing Textual Data with Python

** Suggestion **

* Original code:

```
#html-to-list1.py
import urllib.request, urllib.error, urllib.parse, obo

url = 'http://www.oldbaileyonline.org/browse.jsp?id=t17800628-33&div=t17800628-33'

response = urllib.request.urlopen(url)
html = str(response.read())
text = obo.stripTags(html).lower() #add the string method here.
wordlist = text.split()

print(wordlist)
```

I suggest to change the code for this:

```
import urllib.request, urllib.error, urllib.parse, obo

url = 'http://www.oldbaileyonline.org/browse.jsp?id=t17800628-33&div=t17800628-33'

response = urllib.request.urlopen(url)
html = str(response.read().decode('utf-8')) # decode the result to utf-8
text = obo.stripTags(html).lower() #add the string method here.
wordlist = text.split()

print(wordlist[:10]) # read a small number of elements.
```

Function `decode()` helps to avoid errors in non-english language pages.

It could be a useful advice to reserve computational resources at print long results. A good practice consist on print only the first results to reserve memory. [check McKinney's 'Python for Data Analysis' (2018), "Reading Text Files in Pieces", for a deep explanation of this.]

Those small changes could teach some relevant computational practice to those who do this lessons.

If you agree with this change, you also will need to modify the next code:

```
#html-to-list1.py
import urllib.request, urllib.error, urllib.parse, obo

url = 'http://www.oldbaileyonline.org/browse.jsp?id=t17800628-33&div=t17800628-33'

response = urllib.request.urlopen(url)
html = response.read()
text = obo.stripTags(html).lower()
wordlist = obo.stripNonAlphaNum(text)

print(wordlist)
```

for this

```
#html-to-list1.py
import urllib.request, urllib.error, urllib.parse, obo

url = 'http://www.oldbaileyonline.org/browse.jsp?id=t17800628-33&div=t17800628-33'

response = urllib.request.urlopen(url)
html = response.read().decode('utf-8')
text = obo.stripTags(html).lower()
wordlist = obo.stripNonAlphaNum(text)

print(wordlist[:10])
```

# Counting Word Frequencies with Python

** Suggestion **

To make `obo.py` more readable and pythonic it's possible to include stopwords into a function on a separate file, for instance `stopwords.py`.

```
#stopwords.py

def stopwords():
    stopwords = ['a', 'about', 'above', 'across', 'after', 'afterwards']
    stopwords += ['again', 'against', 'all', 'almost', 'alone', 'along']
    stopwords += ['already', 'also', 'although', 'always', 'am', 'among']
    stopwords += ['amongst', 'amoungst', 'amount', 'an', 'and', 'another']
    stopwords += ['any', 'anyhow', 'anyone', 'anything', 'anyway', 'anywhere']
    stopwords += ['are', 'around', 'as', 'at', 'back', 'be', 'became']
    stopwords += ['because', 'become', 'becomes', 'becoming', 'been']
    stopwords += ['before', 'beforehand', 'behind', 'being', 'below']
    stopwords += ['beside', 'besides', 'between', 'beyond', 'bill', 'both']
    stopwords += ['bottom', 'but', 'by', 'call', 'can', 'cannot', 'cant']
    stopwords += ['co', 'computer', 'con', 'could', 'couldnt', 'cry', 'de']
    stopwords += ['describe', 'detail', 'did', 'do', 'done', 'down', 'due']
    stopwords += ['during', 'each', 'eg', 'eight', 'either', 'eleven', 'else']
    stopwords += ['elsewhere', 'empty', 'enough', 'etc', 'even', 'ever']
    stopwords += ['every', 'everyone', 'everything', 'everywhere', 'except']
    stopwords += ['few', 'fifteen', 'fifty', 'fill', 'find', 'fire', 'first']
    stopwords += ['five', 'for', 'former', 'formerly', 'forty', 'found']
    stopwords += ['four', 'from', 'front', 'full', 'further', 'get', 'give']
    stopwords += ['go', 'had', 'has', 'hasnt', 'have', 'he', 'hence', 'her']
    stopwords += ['here', 'hereafter', 'hereby', 'herein', 'hereupon', 'hers']
    stopwords += ['herself', 'him', 'himself', 'his', 'how', 'however']
    stopwords += ['hundred', 'i', 'ie', 'if', 'in', 'inc', 'indeed']
    stopwords += ['interest', 'into', 'is', 'it', 'its', 'itself', 'keep']
    stopwords += ['last', 'latter', 'latterly', 'least', 'less', 'ltd', 'made']
    stopwords += ['many', 'may', 'me', 'meanwhile', 'might', 'mill', 'mine']
    stopwords += ['more', 'moreover', 'most', 'mostly', 'move', 'much']
    stopwords += ['must', 'my', 'myself', 'name', 'namely', 'neither', 'never']
    stopwords += ['nevertheless', 'next', 'nine', 'no', 'nobody', 'none']
    stopwords += ['noone', 'nor', 'not', 'nothing', 'now', 'nowhere', 'of']
    stopwords += ['off', 'often', 'on','once', 'one', 'only', 'onto', 'or']
    stopwords += ['other', 'others', 'otherwise', 'our', 'ours', 'ourselves']
    stopwords += ['out', 'over', 'own', 'part', 'per', 'perhaps', 'please']
    stopwords += ['put', 'rather', 're', 's', 'same', 'see', 'seem', 'seemed']
    stopwords += ['seeming', 'seems', 'serious', 'several', 'she', 'should']
    stopwords += ['show', 'side', 'since', 'sincere', 'six', 'sixty', 'so']
    stopwords += ['some', 'somehow', 'someone', 'something', 'sometime']
    stopwords += ['sometimes', 'somewhere', 'still', 'such', 'system', 'take']
    stopwords += ['ten', 'than', 'that', 'the', 'their', 'them', 'themselves']
    stopwords += ['then', 'thence', 'there', 'thereafter', 'thereby']
    stopwords += ['therefore', 'therein', 'thereupon', 'these', 'they']
    stopwords += ['thick', 'thin', 'third', 'this', 'those', 'though', 'three']
    stopwords += ['three', 'through', 'throughout', 'thru', 'thus', 'to']
    stopwords += ['together', 'too', 'top', 'toward', 'towards', 'twelve']
    stopwords += ['twenty', 'two', 'un', 'under', 'until', 'up', 'upon']
    stopwords += ['us', 'very', 'via', 'was', 'we', 'well', 'were', 'what']
    stopwords += ['whatever', 'when', 'whence', 'whenever', 'where']
    stopwords += ['whereafter', 'whereas', 'whereby', 'wherein', 'whereupon']
    stopwords += ['wherever', 'whether', 'which', 'while', 'whither', 'who']
    stopwords += ['whoever', 'whole', 'whom', 'whose', 'why', 'will', 'with']
    stopwords += ['within', 'without', 'would', 'yet', 'you', 'your']
    stopwords += ['yours', 'yourself', 'yourselves']
    return stopwords
```

Then, we can import it into obo and just declare the function in `removeStopwords()`:

```
from stopwords import stopwords

# Given a list of words, remove any that are
# in a list of stop words.

def removeStopwords(wordlist, stopwords):
    return [w for w in wordlist if w not in stopwords()]
```

With this method you can reduce `obo.py` lenght to ~60 lines instead of >100

# Output Data as an HTML File with Python

## Python string formatting

** Suggestion **

In this lesson you teach the old way to string format (with `%s`). Although this operator is still valid (it doesn't rise any error), since the release of Python3 is prefered to use `.format()` function to do this task. This method is more flexible and powerful than the 'old way' (it works like template literals in JavaScript), besides is more readable and suitable for current programming in Python. 

This is the original code:

```
frame = 'This fruit is a %s'
print(frame)
-> This fruit is a %s

print(frame % 'banana')
-> This fruit is a banana

print(frame % 'pear')
-> This fruit is a pear
```

With this new method, it could be represented in this way:

```
fruit = 'banana'
fruit2 = 'pear'

print('This fruit is a {}'.format(fruit))
-> This fruit is a banana
print('This fruit is a {}'.format(fruit2))
-> This fruit is a pear
```

Also, this example:

```
frame2 = 'These are %s, those are %s'
print(frame2)
-> These are %s, those are %s

print(frame2 % ('bananas', 'pears'))
-> These are bananas, those are pears
```

Can be change for this single line:

```
print('These are {}s and these are {}s'.format(fruit, fruit2))
-> These are bananas, those are pears
```

## Self-documenting data file

### Windows Instructions

```
# Given name of calling program, a url and a string to wrap,
# output string in html body with basic metadata
# and open in Firefox tab.

def wrapStringInHTMLWindows(program, url, body):
    import datetime
    from webbrowser import open_new_tab

    now = datetime.datetime.today().strftime("%Y%m%d-%H%M%S")

    filename = program + '.html'
    f = open(filename,'wb')

    wrapper = """<html>
    <head>
    <title>%s output - %s</title>
    </head>
    <body><p>URL: <a href=\"%s\">%s</a></p><p>%s</p></body>
    </html>"""

    whole = wrapper % (program, now, url, url, body)
    f.write(whole)
    f.close()

    open_new_tab(filename)
```

error: TypeError: a bytes-like object is required, not 'str'
solution: don't write file as bytes

```
f = open(filename,'w')
```

Suggestion:

To avoid write a script for Mac and other for Windows (and reach a cross-platform coding) just use `os` method `getcwd()` to get the current directory (I also modify the code to works with the .format() approach, and encode the file in utf-8):

```
# Given name of calling program, a url and a string to wrap,
# output string in html body with basic metadata
# and open in Firefox tab.

def wrapStringInHTML(program, url, body):
    import os
    import datetime
    from webbrowser import open_new_tab

    now = datetime.datetime.today().strftime("%Y%m%d-%H%M%S")

    filename = program + '.html'
    f = open(filename,'w', encoding='utf-8') # Utf-8 encoding as a good practice.

    '''
    This is a way to use the .format() function
    '''
    wrapper = """<html>
    <head>
    <title>{} output - {}</title>
    </head>
    <body><p>URL: <a href=\"{}\">{}</a></p><p>{}</p></body>
    </html>""".format(program, now, url, url, body)
    f.write(wrapper)
    f.close()
    '''
    Using os.getcwd() to achieve crossplatform code.
    '''
    open_new_tab(os.getcwd() + filename)
```

Or it can be written more simply, avoiding using the wraper variable:

```
# Given name of calling program, a url and a string to wrap,
# output string in html body with basic metadata
# and open in Firefox tab.

def wrapStringInHTML(program, url, body):
    import datetime
    from webbrowser import open_new_tab

    now = datetime.datetime.today().strftime("%Y%m%d-%H%M%S")

    filename = program + '.html'
    f = open(filename,'w', encoding='utf-8')
    f.write("""<html>
    <head>
    <title>{} output - {}</title>
    </head>
    <body><p>URL: <a href=\"{}\">{}</a></p><p>{}</p></body>
    </html>""".format(program, now, url, url, body))
    f.close()
    '''
    Using os.getcwd() to achieve crossplatform code.
    '''
    open_new_tab(os.getcwd() + filename)
```


With this cross-platfom approach you'll need to change `html-to-freq-3.py` code for this:

```
# html-to-freq-3.py
import obo

# create sorted dictionary of word-frequency pairs
url = 'http://www.oldbaileyonline.org/browse.jsp?id=t17800628-33&div=t17800628-33'
text = obo.webPageToText(url)
fullwordlist = obo.stripNonAlphaNum(text)
wordlist = obo.removeStopwords(fullwordlist, obo.stopwords)
dictionary = obo.wordListToFreqDict(wordlist)
sorteddict = obo.sortFreqDict(dictionary)

# compile dictionary into string and wrap with HTML
outstring = ""
for s in sorteddict:
    outstring += str(s)
    outstring += "<br />"
obo.wrapStringInHTML("html-to-freq-3", url, outstring)

```

# Output Keywords in Context in an HTML File with Python
## Putting it All Together

Just related with the abovementioned issue. This code will work just fine as it is. Users will not need to change it to choose their OS.

```
# html-to-kwic.py

import obo

# create dictionary of n-grams
n = 7
url = 'http://www.oldbaileyonline.org/browse.jsp?id=t17800628-33&div=t17800628-33'

text = obo.webPageToText(url)
fullwordlist = ('# ' * (n//2)).split()
fullwordlist += obo.stripNonAlphaNum(text)
fullwordlist += ('# ' * (n//2)).split()
ngrams = obo.getNGrams(fullwordlist, n)
worddict = obo.nGramsToKWICDict(ngrams)

# output KWIC and wrap with html
target = 'black'
outstr = '<pre>'
if target in worddict:
    for k in worddict[target]:
        outstr += obo.prettyPrintKWIC(k)
        outstr += '<br />'
else:
    outstr += 'Keyword not found in source'

outstr += '</pre>'
obo.wrapStringInHTML('html-to-kwic', url, outstr)
```

# 16. Installing Python Modules with pip

I think it could be helpful to add three aditional items:

## Update Python Modules

`pip` doesn't automatically update your modules. The way to do it is easy, just write the following instructions:

```
pip install --upgrade beautifulsoup4
```

A similar method could be used to upgrade `pip`

```
python -m pip install --upgrade pip
```

## Install specific package versions

Sometimes you'll need a specific version to work with some applications. To install a specific package version you only need to declare it in this way:

```
# To install an specific version

pip install pandas==1.1.5

# To install an older or equal version

pip install pandas<=1.2.0

# To install a newer or equal version

pip install pandas>=1.2.0

# To install a package between a range of versions (you need to put versions between quotation marks)

pip install "pandas>=1.2.0,<1.2.3"

```

## Batch installation of packages

You can install multiple packages simultaneously in the same line:

```
pip install pandas==0.23.1 bokeh==0.13.0 pyproj==1.9.5.1
```

Also, if you want to share your code with other people, you can guarantee they'll have the right packages writing a 'requirements.txt' file. To do that, only need to write a text file with each package for line, like this:

```
pandas==0.23.1 
bokeh==0.13.0 
pyproj==1.9.5.1
```

To install all packages, only need to do is to run this instructions from your shell:

```
pip install -r requirements.txt
```

# 17. Visualizing Data with Bokeh and Pandas

Package error:

```
WARNING: Discarding https://files.pythonhosted.org/packages/29/72/5c1888c4948a0c7b736d10e0f0f69966e7c0874a660222ed0a2c2c6daa9f/pyproj-1.9.5.1.tar.gz#sha256=53fa54c8fa8a1dfcd6af4bf09ce1aae5d4d949da63b90570ac5ec849efaf3ea8 (from https://pypi.org/simple/pyproj/). Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.
ERROR: Could not find a version that satisfies the requirement pyproj==1.9.5.1
ERROR: No matching distribution found for pyproj==1.9.5.1
```

pyproj==1.9.5.1 is too outdated and raise a lot of errors. I recomend delete this section:

> To get the exact versions used to write this tutorial (note: these may not be the most recent versions of each python package) you can pass the following version numbers to `pip`.
```
> pip install pandas==0.23.1 bokeh==0.13.0 pyproj==1.9.5.1
```


## Categorical Data and Bar Charts: Munitions Dropped by Country

People that run this code with versions of `pandas`>1.0 will raise a "FutureWarning" after run this line:

`grouped = df.groupby('COUNTRY_FLYING_MISSION')['TOTAL_TONS', 'TONS_HE', 'TONS_IC', 'TONS_FRAG'].sum()`

To solve this flag, just need to use double brackets:

```
grouped = df.groupby('COUNTRY_FLYING_MISSION')[['TOTAL_TONS', 'TONS_HE', 'TONS_IC', 'TONS_FRAG']].sum()
```

It's necesary to replace all the coincidences through the lesson where `groupby()` function is used.

## Spatial Data: Mapping Target Locations

Resolve this issue was a bit hard. There is a few points where the program will raise errors.
```
#target_locations.py
from bokeh.tile_providers import CARTODBPOSITRON
```

Now, it's necesary import the module `get_provider` from bokeh:

`from bokeh.tile_providers import get_provider`


Tiles need to be invoqued in this way:

```
provider = get_provider('CARTODBPOSITRON')
p.add_tile(provider)
```

pyproj library changes completely the way it transforms data. This function raise an error:

```
from pyproj import Proj, transform

#helper function to convert lat/long to easting/northing for mapping
#this relies on functions from the pyproj library
def LongLat_to_EN(long, lat):
    try:
      easting, northing = transform(
        Proj(init='epsg:4326'), Proj(init='epsg:3857'), long, lat)
      return easting, northing
    except:
      return None, None
```

It's necesary to chage it for this one:

```
from pyproj import Transformer

# helper function to convert lat/long to easting/northing for mapping
# this relies on functions from the pyproj library
def LongLat_to_EN(long, lat):
    try:
      transformer = Transformer.from_crs('epsg:4326', 'epsg:3857')
      easting, northing = transformer.transform(long, lat)
      return easting, northing
    except:
      return None, None
```

Full code for this exercise will be like this:

```
# target_locations.py
import pandas as pd
from bokeh.plotting import figure, output_file, show
from bokeh.models import ColumnDataSource, Range1d
from bokeh.layouts import layout
from bokeh.palettes import Spectral3
from bokeh.tile_providers import get_provider
from pyproj import Transformer
output_file('mapping_targets.html')

# helper function to convert lat/long to easting/northing for mapping
# this relies on functions from the pyproj library


def LongLat_to_EN(long, lat):
    try:
        transformer = Transformer.from_crs('epsg:4326', 'epsg:3857')
        easting, northing = transformer.transform(long, lat)
        return easting, northing
    except:
        return None, None


df = pd.read_csv("thor_wwii.csv")


df['E'], df['N'] = zip(
    *df.apply(lambda x: LongLat_to_EN(x['TGT_LONGITUDE'], x['TGT_LATITUDE']), axis=1))


grouped = df.groupby(['E', 'N'])[['TONS_IC', 'TONS_FRAG']].sum().reset_index()

filter = grouped['TONS_FRAG'] != 0
grouped = grouped[filter]

source = ColumnDataSource(grouped)

left = -2150000
right = 18000000
bottom = -5300000
top = 11000000

p = figure(x_range=Range1d(left, right), y_range=Range1d(bottom, top))

provider = get_provider('CARTODBPOSITRON')
p.add_tile(provider)

p.circle(x='E', y='N', source=source, line_color='grey', fill_color='yellow')

p.axis.visible = False

show(p)

```

After those changes, lesson works just fine.

