# BGPS
# Histograms of distances from Bolocam Galactic Plane Survey

# df is the dataframe
# 'kdar' is the example table column name with the flags
# 'dist' is the example table column name for the maximum-likelihood
#   distance
# df.query('dist > 2000 & < glat < 0.2') would yield a new dataframe
# where all the rows (sources) are more than 2 kpc and less than
# galactic longitude of 0.2 degrees.
import numpy as np
from matplotlib import pyplot as plt
import pandas as pd
from astropy.table import Table 

t = Table.read("BGPS.txt", format = "cds") #reads data into cds format
df = t.to_pandas() #converts table object to DataFrame object
good_df = df.query('KDAR in ["N", "F"]') #specific rows in column KDAR
fig, ax = plt.subplots()
bins = np.arange(0, 2, 50)  #50 bins from 0 pc to 20,000 pc
ax.hist(good_df.dML, bins = bins) #histogram 
#plt.interactive(False)
plt.show()
#plt.savefig('hist.pdf') #saves histogram as pdf 
