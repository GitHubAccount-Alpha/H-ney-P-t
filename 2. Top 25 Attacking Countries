import pandas as pd
import os
import numpy as np
import matplotlib.pyplot as plt

main_dir='C:\data\\'
os.chdir(main_dir)
df=pd.read_csv('AllTraffic_1.csv',sep='\t',lineterminator='\r',dtype='str',encoding='utf8')

###Project###

country=df[~df['Tor_flag'].isin(['N'])]
country=country.groupby(['country']).size().to_frame('number_of_attacks').reset_index().sort_values(by=['number_of_attacks','country'],ascending=[False,True])
fix, ax = plt.subplots()
country[0:25].plot('country','number_of_attacks',kind='bar',color='#1f77b4',figsize=(10,7),ax=ax)
ax.set_title('Top 25 Countries\n', fontweight="bold", fontsize=30) 
ax.set_xlabel('\nCountry', fontweight="bold", size=16) 
ax.set_ylabel('Number of Attacks', fontweight="bold", size=16) 
ax.set_xticklabels(ax.get_xticklabels(), fontsize=14, rotation=45, ha="right")
for p in ax.patches: ax.annotate(np.round(p.get_height(),decimals=2), (p.get_x()+p.get_width()/2., p.get_height()), ha='center', va='center', xytext=(0, 10), textcoords='offset points')
ax.minorticks_on()
ax.tick_params(which='both',top=True,left=True,right=True,bottom=True) 
