import pandas as pd
import os
import numpy as np
import matplotlib.pyplot as plt

main_dir='C:\data\\'
os.chdir(main_dir)
df=pd.read_csv('AllTraffic_1.csv',sep='\t',lineterminator='\r',dtype='str',encoding='utf8')

###Project###
country=df['country'].value_counts()
country=country.to_frame().reset_index().rename(columns={'index':'Country','country':'number_of_attacks'})
print(country[0:25].plot.bar(x='Country',y='number_of_attacks'))
