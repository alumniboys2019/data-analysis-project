# data-analysis-project #
Vision: Programming is more than writing code. The ultimate goal of the projects in this course is that you learn to formulate a programming problem of your own choice, and find your own way to solve it, and present the results. The bullets below are minimum requirements, but otherwise it is very much up to you, what you will like to do with your project. I hope to see some creative ideas!








#Kode
import os
import glob
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import statsmodels.api as sm
import statsmodels.formula.api as smf

from pandas import DataFrame as df
from scipy.stats import trim_mean, kurtosis
from scipy.stats.mstats import mode, gmean, hmean

#sti til mappe der skal arbejdes i 
os.chdir("/Users/Christofferku/Desktop/atp-matches-dataset/")

#hvis filen vi danner i forvejen findes slettes den så der kan køres en ny
if os.path.exists("Tennis_mod.csv"):
    os.remove("Tennis_mod.csv")
else:
    print('File does not exists')

#alle filer med format csv medtages og samles i tennis_total
extension = 'csv'
all_filenames = [i for i in glob.glob('*.{}'.format(extension))]
Tennis_total = pd.concat([pd.read_csv(f) for f in all_filenames ])

#Vælger hvilke kolonner i tennis_total vi vil have med og danner det endelige dataset Tennis_mod
keep_col = ['tourney_id','tourney_name','surface','draw_size','winner_ht', 'winner_age', 'winner_rank', 'winner_rank_points'] 
Tennis_mod=Tennis_total[keep_col]

#fjerner rækker med blanke celler
filter = Tennis_mod["tourney_id"] != ""
Tennis_mod = Tennis_mod[filter]
filter = Tennis_mod["tourney_name"] != ""
Tennis_mod = Tennis_mod[filter]
filter = Tennis_mod["surface"] != ""
Tennis_mod = Tennis_mod[filter]
filter = Tennis_mod["draw_size"] != 0
Tennis_mod = Tennis_mod[filter]
filter = Tennis_mod["winner_ht"] != 0
Tennis_mod = Tennis_mod[filter]
filter = Tennis_mod["winner_age"] != 0
Tennis_mod = Tennis_mod[filter]
filter = Tennis_mod["winner_rank"] != 0
Tennis_mod = Tennis_mod[filter]
filter = Tennis_mod["winner_rank_points"] != 0
Tennis_mod = Tennis_mod[filter]

#Laver tennis_mod til csv som gemmes i samme mappe med stien
Tennis_mod.to_csv( "Tennis_mod.csv", index=False, encoding='utf-8-sig')

#et stk. printet samlet tabel
print(Tennis_mod)

#et stk. deskriptiv analyse
DataDescribe=Tennis_mod.describe()
print(DataDescribe)

#et stk. OLS
results = smf.ols('winner_rank ~ winner_age + winner_ht', data=Tennis_mod).fit()
print(results.summary())

#et stk. plot
plt.style.use('seaborn')
Tennis_mod.plot(x='winner_rank_points', y='winner_rank', kind='scatter')
plt.show()

#bumbum.
