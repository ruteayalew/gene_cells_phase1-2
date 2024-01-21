# gene_cells_phase1-2

Novel data mining task using pandas, scikit-learn preprocessing, and scikit-learn decomposition libraries to implement Phase I &amp; II of CRISP-DM in assistance of clustering high-dimensional data from a high-throughput experiment.

# # Data Background
Each file comprises measurements of 100 different genes (rows) in 100 different cells (columns). The experiment was repeated 100 times, on the same equipment, generating 100 separate files. The larger research goal is to create a predictive model for kidney disease. This initial project is the first step of implementing Python code to aggregate 100 files into one and save the aggregated file with the averaged results; then use PCA analysis to reduce dimensionality. This single file is the final output to be used for clustering in a future extension of this project. 

# # Process

Step 1. Decompressed the zip data file and iterated through the directory to concatenate all files into one data frame. 

    # define directory name to locate files
    mydir = '/Users/ruteayalew/Downloads/assignment1/data'
    
    # iterate through directory to concatenate all files into one dataframe
    concat_df = pd.DataFrame();
    for file in os.listdir(mydir):
    filename = os.fsdecode(file)
    if filename == '.DS_Store': # catch invisible file to ignore
        continue;
    df = pd.read_csv(mydir + '/' + filename, encoding="utf-8")
    concat_df = pd.concat([concat_df, df])


Step 2: Evaluated the data for missing values and variance.

    # find columns with missing values
    pd.set_option("display.max_rows",101)
    concat_df.isnull().sum()

Reflection:
The data quality is acceptable with only 2 missing values out of 100,000 in total. Before aggregating the files into one, I identified the missing data points and imputed them with 0. I acknowledge that filling the values with the globalized variable of 0 DOES bias the data mining results. A preferred method would have been to use a linear regression model to predict the values so as not to skew mean calculations later on since 0 is an extreme value and means are sensitive to that. Due to the significant variance of Gene measurements between cells, I normalized averaged values.

    # fill missing values with mean of column
    mean_value = concat_df['Cell99'].mean()
    concat_df['Cell99'].fillna(value=mean_value, inplace = True)

Step 3: Aggregate by averages using the index to produce averaged results saved to a new file. 

    # group concatenated data frame by index to calculate the mean by index
    avg_df = concat_df.groupby(concat_df.index)
    avg_df = avg_df.mean(numeric_only=True)
    
    # typecast float averages as integers
    avg_df.astype(int)

    # return Gene labels to final dataframe and save to output directory 
    result = []
    counter = 1
    for value in avg_df['Cell1']:
        result.append('Gene'+str(counter))
        counter = counter + 1
        avg_df['ID'] = result
        avg_df.set_index('ID', inplace=True)
    avg_df.to_csv('/Users/ruteayalew/Downloads/assignment1/output/avg_data.csv')

    # normalize
    normalized_df = (avg_df-avg_df.mean())/avg_df.std()

  Reflection:
  The gene measurements varied considerably between cells so normalization was indeed the best practice in this case.

Step 4: Prepare for clustering by reducing the dimension of the dataset using PCA and saved in output directory

    # reduce the dimension of data set using PCA and save in output directory
    pca = PCA(n_components = 5)
    pca.fit(normalized_df)
    data_pca = pca.transform(normalized_df)
    pc_list = ['PC' + str(i) for i in range(1, 6)]
    data_pca = pd.DataFrame(data_pca,columns=(pc_list))
    data_pca.to_csv('/Users/ruteayalew/Downloads/assignment1/output/aggregated_csv')

  Reflection:
  I used 5 principal components for no reason other than keeping it above 2, below 10, and a number that can proportionally represent the 100 Cells recorded. 
  
  
  

