import tkinter as tk
import re
NAMES = '''Dante Toribio, Willian Soto, María Rosarío, Dan Wilson'''
LIST_NAMES = NAMES.split(',')
name = 'W'
LIST_NAMES_NEW = [re.search( rf'.*{name}.*',NAME, re.IGNORECASE).group(0).lstrip()\
      for NAME in LIST_NAMES if re.search( rf'.*{name}.*',NAME, re.IGNORECASE)]

class App(tk.Tk):
    def __init__(self):
        super().__init__()
        self.w_old = None
        self.list = tk.Listbox(self) # create empty list
        self.list.grid(row=2, column=1, pady=5, padx=5)

        self.frame_search = tk.LabelFrame(self, text='SEARCH', cursor = 'arrow', labelanchor = 'n' )
        self.frame_search.grid(row=0, column=0, pady=5, padx = 5, columnspan=3)

        # 1. ENTRY
        self.var_search = tk.StringVar()
        self.var_search.trace(mode='w', callback=self.show_message)
        self.entry_search = tk.Entry(self.frame_search , textvariable=self.var_search)
        self.entry_search.grid(row=1, column=0,columnspan=3, pady=5, padx = 5)

        # 2. RADIO BUTTON
        self.var_but = tk.StringVar()
        self.var_but.set('A')
        self.rad_but =tk.Radiobutton(self.frame_search, text='Vainilla', value='Vainilla', variable=self.var_but)
        self.rad_but.grid(row=0, column=0,columnspan=1, pady=5, padx = 5)

        self.rad_but_1 =tk.Radiobutton(self.frame_search, text='Strong', value='Strong', variable=self.var_but)
        self.rad_but_1.grid(row=0, column=1,columnspan=1, pady=5, padx = 5)


        # 3. LABEL FRAME FOR SELECT
        self.frame_select = tk.LabelFrame(self, text='SELECTED', labelanchor = 'n')
        self.frame_select.grid(row=0, column=3,columnspan=3, pady=5, padx=5 ) #,rowspan=1)

        texts = ['dataframe.csv', 'dataframe.xlsx', 'dataframe.txt']
        self.labels_select = [tk.Label(self.frame_select, text=text) for text in texts]

        for label, i in zip(self.labels_select, range(0,4)):
            label.grid(row=i, column=3, pady=5, padx=5)
        
        self.bt_down = tk.Button(self, text='Download', command=self.to_download, relief=tk.GROOVE)
        self.bt_down.grid(row=3, column=3, pady=5, padx=5, ) #, rowspan=1)
        

    def show_message(self, *args):
        value = self.var_search.get()
        LIST_NAMES_NEW = [re.search( rf'.*{value}.*',NAME, re.IGNORECASE).group(0).lstrip()\
              for NAME in LIST_NAMES if re.search( rf'.*{value}.*',NAME, re.IGNORECASE)]
        w = tuple(LIST_NAMES_NEW)
        if self.w_old:
            self.list.delete(0, tk.END)
            self.list.insert(0,*w)
        else :
            self.list.insert(0,*w)
        if len(value)==0:
            self.list.delete(0, tk.END)
        self.w_old = w
        #print(w, value)


    def to_download(self):
        data_str = """They are 333
        so I don't need of them"""
        with open('data_frame.txt', 'w') as writer:
            writer.write(data_str)

if __name__ == '__main__':
    app = App()
    app.mainloop()