from tkinter import *
import pygubu
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
s = open('Details.txt','w')
class Application:
    def __init__(self, master):

        #1: Create a builder
        self.master =master
        self.builder = builder = pygubu.Builder()
        #2: Load an ui file
        builder.add_from_file('Python_ent.ui')

        #3: Create the widget using a master as parent
        self.mainwindow = builder.get_object('Frame_1',master)
        

        
        builder.connect_callbacks(self)
    def print_event(self):
        text_box=self.builder.get_object('Entry_1')
        label_print = self.builder.get_object('Label_1')
        ent_text = text_box.get()
        chrome_opts = Options()
        chrome_opts.add_argument('--headless')
        driver = webdriver.Chrome("./chromedriver",options=chrome_opts)
        driver.get('https://www.google.com')
        driver.find_element_by_xpath('//*[@id="tsf"]/div[2]/div/div[1]/div/div[1]/input').send_keys(ent_text)
        driver.find_element_by_xpath('//*[@id="tsf"]/div[2]/div/div[3]/center/input[1]').click()
        s = open('Details.txt','w')
        
        for i in range(1,7):
           
           driver.find_element_by_xpath('//*[@id="pnnext"]').click()
           search_list=driver.find_element_by_id('search')
           t_vals=search_list.find_elements_by_xpath('//*[@id="rso"]/div/div/div[3]/div/div/div[2]/div')
           for x in t_vals:
              s.write(x.text)
        s.close()
        label_print.configure(text='Data saved')

        
        


if __name__ == '__main__':
    root = Tk()
    app = Application(root)
    root.mainloop()