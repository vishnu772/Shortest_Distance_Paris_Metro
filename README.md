# Shortest_Distance_Paris_Metro
#Algorithm to find the shortest_distance and shortest time of Paris Metro


# -*- coding: utf-8 -*-
"""
Created on Thu Dec 14 15:31:02 2017

@author: Welcome
"""

# -*- coding: utf-8 -*-
"""
Created on Wed Dec 13 23:24:33 2017

@author: Welcome
"""

import pandas as pd
import numpy as np
from collections import deque


vert=pd.read_excel('C:\\Users\\Welcome\\Desktop\\CHAITU\\python\\paris metro\\All_Metro_Stations.xlsx')
Station_Vertices = pd.DataFrame(vert.Vertices.str.split(' ',1).tolist(),
                                   columns = ['Metro_Id','Station_Name'])

Station_Vertices[Station_Vertices.Metro_Id=='0000']


edg=pd.read_excel('C:\\Users\\Welcome\\Desktop\\CHAITU\\python\\paris metro\\All_Edges.xlsx')
All_Edges = pd.DataFrame(edg['[Edges]'].str.split(' ',2).tolist(),
                                   columns = ['Station_ID1','Station_ID2','Seconds'])

All_Edges.Seconds=All_Edges.Seconds.astype('float')


vrt=pd.Series.unique(All_Edges.Station_ID1)

Adj_mat=pd.DataFrame(data=None,columns=vrt,index=vrt)

Adj_mat.fillna(0,inplace=True)


pp=All_Edges.head()

#Funtion to load the Data to Data Frame
def Add_Metro_lines(data_f,vertex_1,vertex_2,Time_in_sec):
    data_f.loc[vertex_1][vertex_2]=Time_in_sec
    data_f.loc[vertex_2][vertex_1]=Time_in_sec

# Loading Data to DataFrame
    
def Load_Data_to_Data_Frame():
    for row_index,row in All_Edges.iterrows():
        v1=row['Station_ID1']
        v2=row['Station_ID2']
        w=row['Seconds']
        Add_Metro_lines(Adj_mat,v1,v2,w)
    
Load_Data_to_Data_Frame()


#Distance_d=pd.DataFrame(data=None,columns=['min_distance'],index=vrt)
#Distance_d['previ']=''
#Distance_d.fillna(0,inplace=True)


def algorithms(Start_node):
     que=deque()
     que.append(Start_node)
     source=Start_node
     visited=[]
     Distance_d=pd.DataFrame(data=None,columns=['min_distance'],index=vrt)
     Distance_d['previ']=''
     Distance_d.fillna(0,inplace=True)
     while que:
         v1=que.pop()
         ad_vert=list(Adj_mat[Adj_mat[v1]>0][v1].sort_values().index)
         for i in ad_vert:
             if i!=source and i not in visited:
                 que.append(i)
                 visited.append(i)
                 if Distance_d.loc[i,'min_distance']==0:
                     Distance_d.loc[i,'min_distance']=Adj_mat[v1][i]+Distance_d.loc[v1,'min_distance']
                     Distance_d.loc[i,'previ']=v1
                 else:
                     if Distance_d.loc[i,'min_distance']>Adj_mat[v1][i]+Distance_d.loc[v1,'min_distance']:
                         Distance_d.loc[i,'min_distance']=Adj_mat[v1][i]+Distance_d.loc[v1,'min_distance']
                         Distance_d.loc[i,'previ']=v1
                         
         
     return Distance_d
    
    
    
    
    







def paths(Datafra,sour,dest):
    list_of_station=[]
    Station_name=[]
    
    total_time=Datafra.loc[dest]['min_distance']
    qued=deque()
    qued.append(dest)
    list_of_station.append(dest)
    while qued:
        s=qued.pop()
        if Datafra.loc[s]['previ']!=sour:
            list_of_station.append(Datafra.loc[s]['previ'])
            qued.append(Datafra.loc[s]['previ'])
        else:
            list_of_station.append(Datafra.loc[s]['previ'])
    for i in list_of_station:
        Station_name.append(Station_Vertices.loc[int(i)]['Station_Name'])
    print(total_time)    
    return list_of_station        
    
    
    



list(Adj_mat.loc[Adj_mat.loc['0',]>0,].index)    


list(Adj_mat[Adj_mat['0']>0]['0'].index)
list(Adj_mat[Adj_mat['159']>0]['159'].index)
    
    
    
    
    
algorithms('1')    

Distance_d.drop('0', axis=1, inplace=True)
Distance_d.loc['0','min_distance']=10
Distance_d.loc['0','previ']='323'

Adj_mat['0']['159']


