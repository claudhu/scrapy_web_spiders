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


## 我們的第一支spider
Spider主要是經由開發者透過程式語言來控制爬蟲進行的方向，你可以單單爬蟲單一網域或者是複合網域，大多數的爬衝規則都是寫在spider之內。此外我們需要在spider定義一個`起頭網址`來提供。並且告訴我們的spider它需要的行動方向，要萃取哪些資料以及即將搜索的下一個頁面為何?

> 要建構Spider之前，你需要在類別之中繼承`scrapy.Spider`，並且定義三個主要的屬性欄位

- `name` :  主要作為系統識別spider的名稱，因此它需要是獨一無二，且不應該跟其它spider的名稱重疊。
- `start_urls` : 你可以提供一個串列型態的列表給spider，而spider就會根據你的需求逐一訪問網頁並且擷取資料，因此在列表中的第一個網址就是起頭的網站。
- `parse()` : 當spider開始進行爬蟲時，它會將網頁的`回應資料`往後送，此時spider內建的parse()，就會協助你解析檔案，得到結構化的資料

> parse()的工作專職於解析網頁的內容，並且將資料整理成item回傳，並且依序的處理`start_urls`的檔案

以下是我們的第一支spider程式碼，請將它儲存並且命名為`dmoz_spider.py`，並且存放在`tutorial/spiders`的資料夾底下


```python
import scrapy

class DmozSpider(scrapy.Spider):
    name = "dmoz"
    allowed_domains = ["dmoz.org"]
    start_urls = [
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Books/",
        "http://www.dmoz.org/Computers/Programming/Languages/Python/Resources/"
    ]

    def parse(self, response):
        filename = response.url.split("/")[-2]
        with open(filename, 'wb') as f:
            f.write(response.body)
```

## 開始爬蟲  
讓我們回到專案資料夾的最頂端，使用指令來讓我們的爬蟲正確執行  
``` bash
scrapy crawl dmoz
```