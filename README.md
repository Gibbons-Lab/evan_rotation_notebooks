# evan_rotation_notebooks
Repository for Evan Pepper's Fall 2020 Rotation

The goal of this project was to essentially see if we can predict a patient’s ability, given a health-based intervention and baseline multi-omic measurements, to bring certain general health-related markers within a healthy range.

If you're reading this, it likely means one of two things (or both). You're either casually looking through what I did during my Fall quarter rotation through the Gibbons lab, or you're interested in running the pipeline I made! Either way, welcome! For the former, have fun! Let me know if you have any questions -- I can be reached at epepper@uw.edu or epepper@systemsbiology.org. For the latter, listed below are the instructions for using the scripts to perform the analysis:

**Step 0:** Select a feature you are interested in analyzing. This feature must exist within the thousands of features measured within the Arivale study. For example, let's choose glycated hemoglobin, abbreviated hba1c.

**Step 1:** Identify what dataframe this feature is in. Here is a list of dataframes and a description of the features held therein:
  chemsitries: clinically relevant analytes (including hba1c)
  metabolomics: measureable blood metabolites
  proteomics: proteins captured from blood sample
  microbiome_diversity: standard diversity metrics for microbiome
  mirobiome_genera: defined genera of microbes found in any participant
  blood_pressure: standard blood pressure metrics
  saliva: measurements of cortisol througout the day
  weight: standard weight metrics, including BMI, waist circumference, and more
  
**Step 2:** Create a folder in your current working directory that contains the jupyter notebooks with the name of the feature you have selected. For example, create a new folder called `<hab1c>`

**Step 3:** Open the notebook titled "explore_health_deltas.ipynb"

**Step 4:** To run the "get_deltas" function, use the following instructions:
  agruments are as follows:
  
    1. name of snapshot containing feature that was initialized at beginning of notebook    
    2. column title for feature of interest
    3. abbreviated title to use throughout, MUST BE SAME AS DIRECTORY NAME
    4. numerical threshold for unhealthy group, use 0.0 to include all samples
    5. if True, unhealthy range is ABOVE threshold value, if False, unhealthy range is BELOW threshold value
  
For example, use: 

    get_deltas(chemistries, 'GLYCOHEMOGLOBIN A1C', 'hba1c', 0.0, True)
    ** note that the third argument is the is the same name as the folder you made that will recieve the intermediate files
    ** this will also generate and save a figure that shows various plots of the deltas and residuals, etc.
    
**Step 5:** Run all cells. This will import the necessary libraries and run through the whole script, generating the intermediate files that will be used in the following multivariate regression analysis.

**Step 6:** Navigate to the notebook titled "get_predictors.ipynb"

**Step 7:** To run the "get_predictions" function, call it beneath the function itself and use, for example: 

    get_prediction('hba1c')
    ** note that the only argument for the function is the abbreviated name you gave the feature, which should be the same name as the folder containing the intermediate files
  
**Step 8:** Let the script run (this should take ~ 1 hour, depending on how many features you have chosen to include in the analysis)

**Step 9:** Observe the results. The final plot will be a summary of the results, where each subplot contains the results from the lasso regression, where the predicted residuals are plotted on the x-axis, and the observed residuals on the y-axis. Also included is a line with a slope of 1 and the actual mean-out-of-sample R2, which is essentially the score for the estimator. If the model is perfect, the scatter plot will have points that fall along the line and have an R2 close to 1.

**Step 10:** Take a look at the csv titled "hba1c_lasso_predictions". This will give you a list of all the features that had non-zero beta-coefficients in the final lasso model. The univariate regression script shows an example of then taking those predictive features and performing a linear regression on each of them against the residuals, one at a time. Then, the R2 from the linear regressions is saved, and a dataframe of the sorted R2's from each regression is printed out.
