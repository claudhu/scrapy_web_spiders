# 學習使用Scrapy來爬蟲資料

# Requirement  
1. 認識基本的HTML5標籤
2. 具備基礎的Python編成
3. 不錯的資料結構邏輯

# Tools
1. 各種作業系統皆可(推薦使用Linux或者MacOS)
2. 簡單可用的IDE
3. Python編譯器以及套件安裝工具(PIP)
4. 安裝`scrapy爬蟲工具 1.3版`


# Start Now
我們會先假設你已經安裝完了Python以及PIP軟件，如果還沒安裝Python的話，可以直接上[Python 官方網站](https://www,python.org)下載你需要的作業系統版本

## 安裝Scrapy  
``` bash
pip install scrapy ## 透過PIP套件工具安裝Scrapy
```

## 創造一個全新的專案
```bash
scrapy startproject tutorial 
```
這個會生成一個名叫`tutorial`的資料夾

資料夾裡面包含了這些檔案架構  
```bash
tutorial/
    scrapy.cfg
    tutorial/
        __init__.py
        items.py
        pipelines.py
        settings.py
        spiders/
            __init__.py
            ...

```

## 簡單說明一下這些檔案負責的工作!
* `scrapy.cfg` 這個專案的設定檔
* `tutorial` 專案使用的python模組，主要是未來會使用的code都是從這裡import的
* `tutorial/items.py` 專案的`items`檔案
* `tutorial/pipelines.py` 專案的`pipelines`檔案
* `tutorial/spiders/` 這是要讓你存放的資料夾

## 開始定義Item
Item是一種資料乘載的工具，經由爬蟲獲得的資料我們會將其讀取並且使用Item來對於其定義，它工作起來的用途跟dict(字典)很像，但是它可以針對一些為尚未宣告的fields進行處理，並且可以避免一些型態錯誤的問題  

我們經由繼承，賦予原先class的Item具備了一些特殊功能，並且使得爬回來的資料都可以作為物件使用，就像是使用資料庫的ORM功能一樣(如果你目前還不是很了解ORMs的工作，晚點還會在更進一步的說明)

目前我們打算去爬`dmoz.org`這個網站並且擷取這個網站的`網址` `超連結` `描述`，所以我們item的類別之下建立了三個屬性欄位使用，分別對應前面的項目。

```python 
import scrapy

class DmozItem(scrapy.Item):
    title = scrapy.Field()
    link = scrapy.Field()
    desc = scrapy.Field()
```

> 雖然定義的過程有點麻煩，但是為了以後可以更接近物件的操作方式，定義的過程是必須的! 後續開發也將較為友善

