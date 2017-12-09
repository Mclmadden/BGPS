import numpy as np
import pandas as pd
from matplotlib import pyplot as plt
from astropy.table import Table 

#Import data table and convert to Panda DataFrame
t = Table.read("BGPS.txt", format = "cds")
df = t.to_pandas()

# Here we just want to plot the distances of clumps that actually have well-resolved
# distances, and not include all the others where it's 50/50 whether they are at
# near or far distance. We can do this via the method "query" on df:
# https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.query.html
# https://pandas.pydata.org/pandas-docs/stable/indexing.html
# This is the bread-and-butter of pandas, querying and transforming data, along with
# merging different datasets.
good_df = df.query('KDAR in ["N", "F"]')

# Next we create an instance of the Figure and Axis classes in matplotlib, the
# essential entities that contain all the stuff (Figure), and an Axis which is
# basically the white square part with the plot, ticks, and labels. A Figure
# instance can contain multiple Axis objects to make sub-plots (such as 2x2 for
# a grid of four plots in the same Figure). It's implicit that if you don't pass
# any arguments to subplots that you only want one Axis object in the figure
# (number of plot rows = 1, number of plot columns = 1).
fig, ax = plt.subplots()

bins = np.linspace(0, 20, 51) #Units in kiloparsec --> 20 instead of 2e4

ax.hist(good_df.dML, bins=bins) #dist_ml actually called dML
plt.xlabel("Distance (kpc)")
plt.ylabel("Entries")
plt.title("Well-Constrained Maximum Likelihood Distances")
plt.show()

# Save the figure to a PDF file, assuming you have a folder in your current
# directory called "plots" from which to write a new file into.
#fig.savefig('plots/resolved_distances_hist.pdf')
