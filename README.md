# gene_cells_phase1-2

Novel data mining task following Phase I &amp; II of CRISP-DM to assist in clustering of high-dimensional data from a high-throughput experiment.

# # Data Background
Each file comprises measurements of 100 different genes (rows) in 100 different cells (columns). The experiment was repeated 100 times, on the same equipment, generating 100 separate files. The larger research goal is to create a predictive model for kidney disease. This initial project is the first step of implementing Python code to aggregate 100 files into one and save the aggregated file with the averaged results; then use PCA analysis to reduce dimensionality. This single file is the final output to be used for clustering in a future extension of this project. 

# # Process

Step 1. Decompressed the zip data file and iterated through the directory to concatenate all files into one data frame. 

Step 2: Evaluated the data for missing values and variance.

    Reflection:
    The data quality is acceptable with only 2 missing values out of 100,000 in total. Before aggregating the files into one, I identified the missing data points and imputed them with 0. I acknowledge that filling the values with the globalized variable of 0 DOES bias the data mining results. A preferred method would have been to use a linear regression model to predict the values so as not to skew mean calculations later on since 0 is an extreme value and means are sensitive to that. Due to the significant variance of Gene measurements between cells, I normalized averaged values.
    
Step 3: Aggregate by averages using the index to produce averaged results saved to a new file. 

  Reflection:
  The gene measurements varied considerably between cells so normalization was indeed the best practice in this case.

Step 4: Prepare for clustering by reducing the dimension of the dataset using PCA and saved in output directory

  Reflection:
  I used 5 principal components for no reason other than keeping it above 2, below 10, and a number that can proportionally represent the 100 Cells recorded. 
  
  
  

