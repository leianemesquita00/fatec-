import pandas as pd

data = {'coluna_1':[1.5,2.0,1.7,2.2,1.9],
        'coluna_2':[10.0,12.0,11.5,13.0,11.0],
        'coluna_3':[0,1,0,1,0],
        'coluna_4':[1,0,1,1,0],
        'coluna_5':[20.5,25.5,22.0,27.0,23.0]}

df = pd.DataFrame(data)

print(df)

from google.colab import drive

drive.mount ('/content/drive')

origem = '/content/drive/MyDrive/DATA/COMEXSTAT/'

drive.mount ('/content/drive')

arquivo_1 = origem + 'EXP_2025.csv'
arquivo_2 = origem + 'EXP_2026.csv'
ncm = origem + 'NCM.csv'
pais = origem + 'PAIS.csv'
vias = origem + 'VIA.csv'
urf = origem + 'URF.csv'

exp25 = pd.read_csv(arquivo_1,low_memory=False,sep=";",encoding='UTF-8')
exp26 = pd.read_csv(arquivo_2,low_memory=False,sep=";",encoding='UTF-8')
expncm= pd.read_csv(ncm,low_memory=False,sep=";",encoding='latin-1')
exppais = pd.read_csv(pais,low_memory=False,sep=";",encoding='latin-1')
expvias = pd.read_csv(vias,low_memory=False,sep=";",encoding='UTF-8')
expurt= pd.read_csv(urf,low_memory=False,sep=";",encoding='latin-1')

exp25.info()

exp26.info()

expncm.info()


exp_final = pd.concat([exp25,exp26],ignore_index=True)

exp_final.head(5)

exp_final.tail(5)

exp_final = exp_final.merge(exppais[['CO_PAIS','NO_PAIS']],on="CO_PAIS",how="left")
exp_final = exp_final.merge(expncm[['CO_NCM','NO_NCM_POR']],on='CO_NCM',how='left')

exp_final.head(5)

coluna = 'NO_PAIS'
cp = exp_final[coluna].value_counts().sort_values(ascending=False)
print(f'total de operações{coluna}:')
print(cp)

exp_final.to_csv(origem +'exp_final.csv',index=False)
