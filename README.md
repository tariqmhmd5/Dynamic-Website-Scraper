# Dynamic Website Scraper
### Scraping data from ViewFinder from .aspx Page
In this Repo I scrape some data from School website for given school code.

**Website:**

> http://schoolreportcards.in/SRC-New/AdvanceSearch/AdvanceSearch.aspx

> Sample School code for demo 07030626712 paste this on search box

### Concept
First with the help of automation script search for school data from website with school affiliation code then with the help of PyAutoGUI the cursor automaticallly go to the desired location and copy the data from there and put it to a file 

# Prerequisite

*   School Code File
*   Python Environment
*   Python Packages listed below
---

# Python Packages

*   PyAutoGui
*   Pyperclip
*   Numpy
*   Pandas
*   Selenium
---

Import all Packages
```
import pandas as pd
import numpy as np
import pyautogui
import pyperclip
from selenium import webdriver
from pandas import ExcelWriter
from pandas import ExcelFile
import time
import warnings
warnings.filterwarnings("ignore")
```
```
data=pd.read_excel("School Code.xlsx")    #import file with affiliation number in it
```
```
aff_num=list(data['AffiliationNo'])  # create python list of aff num or school code
```
```
codes=[]            #filter out the affiliation numbers 
for i in aff_num:
    a=str(i)
    if a.isnumeric():
        a='0'+str(i)
        codes.append(a)
    else:
        codes.append(a)
```


```
pyautogui.position()   #use to know the location of Mouse pointer to grab the data
```


```
df=pd.DataFrame(np.nan, index=range(0,len(codes)), columns=['Block', 'Village','Pin'])  #Make empty data frame to input the Data
```


```
driver=webdriver.Chrome()  #open browser
driver.get('http://schoolreportcards.in/SRC-New/AdvanceSearch/AdvanceSearch.aspx') #open site
i=0   #pointer to point data in dataframe
for code in codes:   # iterate through the school codes
    try:
        sch_code=driver.find_element_by_xpath('//*[@id="txtSearchSchool"]') #search school by school code
        sch_code.clear()
        sch_code.send_keys(code)
        driver.find_element_by_xpath('/html/body/form/div[3]/div[2]/div[2]/div[2]/div/div[2]/a[2]/img').click()
        time.sleep(7)  #time taken by internet to open the site reduce it if internet is fast
        driver.switch_to.window(driver.window_handles[1]) #turn browser focus to the new tab opened
        
        pyautogui.click(x=331, y=564,clicks=2)   # click on data to select
        pyautogui.hotkey('ctrl', 'c')  #copy data
        time.sleep(1)
        con = pyperclip.paste()  #paste data to con variable
        
        if len(con)==7:    # now all the conditions to grab the data and paste it into data frame
    
            pyautogui.click(x=454, y=541,clicks=3)
            pyautogui.hotkey('ctrl', 'c')
            time.sleep(1)
            block = pyperclip.paste()
        
            pyautogui.click(x=473, y=564,clicks=3)
            pyautogui.hotkey('ctrl', 'c')
            time.sleep(1)
            village = pyperclip.paste()
        
            pyautogui.click(x=551, y=622,clicks=3)
            pyautogui.hotkey('ctrl', 'c')
            time.sleep(1)
            pin = pyperclip.paste()
        
            df.loc[:,'Block'][i] = block
            df.loc[:,'Village'][i] = village
            df.loc[:,'Pin'][i] = pin
        
        elif len(con)==5:
            pyautogui.click(x=454, y=541,clicks=3)
            pyautogui.hotkey('ctrl', 'c')
            time.sleep(1)
            block = pyperclip.paste()
        
            pyautogui.click(x=473, y=564,clicks=3)
            pyautogui.hotkey('ctrl', 'c')
            time.sleep(1)
            village = pyperclip.paste()
        
            pyautogui.click(x=551, y=622,clicks=3)
            pyautogui.hotkey('ctrl', 'c')
            time.sleep(1)
            pin = pyperclip.paste()
        
            df.loc[:,'Block'][i] = block
            df.loc[:,'Village'][i] = village
            df.loc[:,'Pin'][i] = pin
        
        else:
            pyautogui.click(x=454, y=541,clicks=3)
            pyautogui.hotkey('ctrl', 'c')
            time.sleep(1)
            block = pyperclip.paste()
        
            pyautogui.click(x=473, y=564,clicks=3)
            pyautogui.hotkey('ctrl', 'c')
            time.sleep(1)
            village = pyperclip.paste()
        
            pyautogui.click(x=551, y=622,clicks=3)
            pyautogui.hotkey('ctrl', 'c')
            time.sleep(1)
            pin = pyperclip.paste()
        
            df.loc[:,'Block'][i] = block
            df.loc[:,'Village'][i] = village
            df.loc[:,'Pin'][i] = pin
        
        
        driver.close()
        driver.switch_to.window(driver.window_handles[0])
        
    except Exception as e:
        print(e)
        pass
    finally:
        i+=1
```


```
writer = ExcelWriter('Output File.xlsx')
df.to_excel(writer,'Sheet1')
writer.save()
```