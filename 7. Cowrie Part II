import pandas as pd
import os
import numpy as np
import matplotlib.pyplot as plt

main_dir='C:\data\\'
os.chdir(main_dir)

########################################
#Project
########################################
df=pd.read_csv('cowrie_1.csv',sep='\t',lineterminator='\r',dtype='str',encoding='utf8')

# Drop last n rows
df.drop(df.tail(1).index,inplace=True)

# Covert fields to numeric
df['unixTimestamp'] = pd.to_numeric(df['unixTimestamp'], errors='coerce')

########################################
# 1. Main Table 
########################################
Table = df[['source_ip','source_ip_num','timestamp','unixTimestamp','cal_date','destination_ip','loggedin','credentials','payload_endTime']]

########################################
# 2. Valid Credentials
########################################
Table_Cred=Table.groupby(['loggedin']).size().to_frame('number_of_attacks').reset_index().sort_values(by=['number_of_attacks','loggedin'],ascending=[False,True])
Table_Cred=Table_Cred.loc[Table_Cred['loggedin'] != 'None']

########################################
# 3. Total Attacks
########################################
Attack=Table.loc[Table['loggedin'] != 'None']

########################################
# 4. Total Scans
########################################
Scan=Table.loc[Table['loggedin'] == 'None']

########################################
# 5.1 Attacks - Loggedin with/without scans
########################################
Attack['scan_status'] = np.select([Attack.source_ip.isin(Scan.source_ip), Attack.source_ip.notnull()],['Scan', 'Noscan'], default=None)

########################################
# 5.2 Attack Credentials - Loggedin without scans 
########################################
Attack_Noscan_Cred=Attack.loc[Attack['scan_status'] == 'Noscan']
Attack_Noscan_Cred=Attack_Noscan_Cred.groupby(['loggedin']).size().to_frame('number_of_attacks').reset_index().sort_values(by=['number_of_attacks','loggedin'],ascending=[False,True])

########################################
# 6. Scans - Valid Credentials 
########################################
Scans_Valid_Cred=pd.DataFrame()

for i in Attack_Noscan_Cred.loggedin:
    Scans_Valid_Cred=Scans_Valid_Cred.append(Scan[Scan['credentials'].str.contains(i,na=False,regex=False)])

Scans_Valid_Cred['UNIQUE']=Scans_Valid_Cred['source_ip']+" "+Scans_Valid_Cred['destination_ip']+" "+Scans_Valid_Cred['payload_endTime']
Scans_Valid_Cred = Scans_Valid_Cred.drop_duplicates(subset=['UNIQUE'], keep='first')
del Scans_Valid_Cred['UNIQUE']

# Sort by time
Scans_Valid_Cred.sort_values('unixTimestamp', ascending=True, inplace=True)

########################################
# 7.1 Attacks initiated after scanning & Removing duplicates by earliest loggin time
########################################
Attacks_After_Scan=Attack[~Attack['scan_status'].isin(['Scan'])]
Attacks_After_Scan=Attacks_After_Scan.sort_values(by=['timestamp'], ascending=True)

Attack_Noscan_Cred = Attack_Noscan_Cred.drop_duplicates(subset=['source_ip'], keep='first')

########################################
# 7.2 Attacks initiated with no scan
########################################
Attacks_After_No_Scan=Attack[~Attack['scan_status'].isin(['Noscan'])]
Attacks_After_No_Scan=Attacks_After_No_Scan.sort_values(by=['timestamp'], ascending=True)

########################################
# 8. Prints all Attacks performed after scanning
########################################
print('All Attacks performed after scanning:')
print(Attack_Noscan_Cred[['source_ip','destination_ip','loggedin','payload_endTime']])

########################################
# 9. Print attack and related scans
########################################
for i in Attacks_After_Scan['source_ip'].unique():
    print('Attack ip:',i, 'Last scanning with valid credentials:', Scans_Valid_Cred[Scans_Valid_Cred['source_ip'] == i])
    
    
for i in Attacks_After_Scan['source_ip'].unique():
    print('Attack ip:',i, 'Last scanning with valid credentials:', Scans_Valid_Cred.source_ip[Scans_Valid_Cred['source_ip'] == i])
    
for i in Attacks_After_Scan['source_ip'].unique():
    print('Attack ip:',i, 'Last scanning with valid credentials:', Scans_Valid_Cred.timestamp[Scans_Valid_Cred['source_ip'] == i])
