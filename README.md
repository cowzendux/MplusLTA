# MplusLTA
SPSS Python Extension function that will use Mplus to run a latent transition analysis from within SPSS

This program allows users to conduct a Latent Transition Analysis that they want to test on an SPSS data set. The program then converts the active data set to Mplus format, writes a program that will perform the analysis in Mplus, then loads the important parts of the Mplus output into the SPSS output window.

This and other SPSS Python Extension functions can be found at http://www.stat-help.com/python.html

## Usage:
**MplusLTA(inpfile, modellabel, runModel, viewOutput, suppressSPSS, nameList, profileList, groupList, starts, stiterations, optseed, svalues, estimator, useobservations, categorical, censored, count, nominal, idvariable, modelDatasetName, meanDatasetName, datasetLabels, savedata, saveCprob, processors, waittime)**
* "inpfile" is a string identifying the directory and filename of Mplus input file to be created by the program. This filename must end with .inp . The data file will automatically be saved to the same directory. This argument is required.
* "modellabel" is a string that indicates what label should be added to the output at the top of your model. If this is not specified, the label defaults to "MplusLTA"
* "runModel" is a boolean argument indicating whether or not you want the program to actually run the program it creates based on the model you define. You may choose to not run the model when you want to use the program to load an existing output file into SPSS. However, when doing this, you should first load the corresponding data set so that the function can determine the appropriate translation between the Mplus variable names and SPSS variable names. By default, the model is run.
* "viewOutput" is a boolean argument indicating whether or not you want the program to read the created output into SPSS. You may choose not to read the output into SPSS when you know that it will take a very long time to run and you do not want to tie up SPSS while you are waiting for Mplus to finish. If you choose not to view the output, then the program will also not create a dataset for the coefficients. By default, the output is read into SPSS.
* "suppressSPSS" is a boolean argument indicating whether or not you want the program to supress SPSS output while running the model. Typically this output is not useful and merely clogs up the output window. However, if your model is not running correctly, the SPSS output can help you see where the errors are. Setting this argument to True will not suppress the Mplus output. By default, the SPSS output is not suppressed.
* "nameList" is a list of strings identifying the names of the different LPAs you want conducted as part of the LTA. This is commonly the name of different time points.
* "profileList" is a list of lists identifying the SPSS variables that you want included in your different LPAs. For each LPA, you identify the included variables as a list of strings.
* "groupList" is a list of numbers identifying the number of groups you want the analysis to create in each profile analysis. The order of this list matches the order of profiles in profileList.
* "starts" is an optional argument indicating the number of random starts that you want to use in the analysis. The Mplus default is 20, but it is not unreasonable to increase this to 1000, 2000, or 5000 if you have problems with estimation. The number of final stage optimizations is set to be .20*the number of random starts.
* "stiterations" is an optional argument indicating the number of initial stage iterations. The Mplus default is 10, but it is not unreasonable to increase this to 20, 40, or 100 if you have problems with estimation.
* "optseed" is a number indicating the seed that should be used to define randomization in the analysis. If this argument is omitted then Mplus will choose one or more random seeds for the analysis.
* "svalues" is a list of lists that allows you to swap the order of the profiles in the analysis. There is one list of numbers for each LPA included in the LTA. Withing each LPA's list, the number in the first list is the new number for the original first profile, the number in the second slot is the new number for the original second profile, etc. This is usually combined with a defined optseed so that you know the ordering of the original profiles.
* "estimator" is a string specifying the estimation method to be used. Valid values are ML, MLM, MLMV, MLR, MLF, MUML, WLS, WLSM, WLSMV, ULS, ULSMV, GLS, and BAYES. If this argument is omitted, the Mplus default will be used, which depends on the data and model types you are using (most commonly MLR).
* "useobservations" is a string specifying a selection criterion that must be met for observations to be included in the analysis. This is an optional argument that defaults to None, indicating that all observations are to be included in the analysis. This should not be used if you have a cluster variable - in that case, use "subpopulation".
* "categorical" is an optional argument that identifies a list of variables that should be treated as categorical by Mplus. Note that what Mplus calls categorical is typically called "ordinal" in other places. Use the "nominal" command described below for true categorical variables.
* "censored" is an optional argument that identifies a list of variables that should be treated as censored by Mplus.
* "count" is an optional argument that identifies a list of variables that should be treated as count variables (i.e., for Poisson regression) by Mplus.
* "nominal" is an optional argument that identifies a list of variables that should be treated as nominal variables by Mplus.
* "idvariable" is an optional argument that identifies an identifier variable for your data set. This is needed if you are saving latent scores and want to merge them into another data set.
* "auxiliary" is an optional argument that identifies a list of variables that are used to assist with estimating missing values but which are not to be included in the model. This defaults to None, which would indicate that there are no auxiliary variables in the analysis.
* "modelDatasetName" is an optional argument that identifies the name of an SPSS dataset that should be used to save information about the overall model.
* "meanDatasetName" is an optional argument that identifies the name of an SPSS dataset that should be used to save the variable means for each group.
* "datasetLabels" is an optional argument that identifies a list of labels that would be added to the dataset. This can be useful if you are appending the results from multiple analyses to the same dataset.
* "savedata" is an optional argument that allows you to save the data file used in the analysis to a file. The value of this argument should be set to the name of the data file that should be created. The saved file will be placed in the same directory as the .inp file. This defaults to None, which does not save the data file.
* "saveCprob" is an optional boolean argument that determines whether the class or profile probabilities are included in the saved data file.
* "processors" is an optional argument that specifies how many logical processors Mplus should use when running the analysis. You should not specify more processors than are available in your machine. If this argument is omitted, Mplus will use 1 processor.
* "waittime" is an optional argument that specifies how many seconds the program should wait after running the Mplus program before it tries to read the output file. This defaults to 5. You should be sure that you leave enough time for Mplus to finish the analyses before trying to import them into SPSS

## Example 1 - Random starts: 
**MplusLTA(inpfile = "D:/Personality/Mplus/model.inp,  
modellabel = "3-group LPA",  
nameList = ["Time1",  "Time2"],  
profileList = [ ["OpennessT1", "ConcientiousnessT1", "ExtraversionT1", "AgreeablenessT1", "NeuroticismT1"],  
["OpennessT2", "ConcientiousnessT2", "ExtraversionT2", "AgreeablenessT2", "NeuroticismT2"] ],  
groups = [3, 2],  
starts = 500,  
stiterations = 20,  
modelDatasetName = "Personality",  
savedata = "D:/Personality/Mplus/model.txt",  
saveCprob = True,  
waittime = 20)**  
* This program would conduct a transition analysis for the Big 5 personality scales across two timepoints.
* Five variables are being examined at each time point.
* The profile analysis at time 1 will create 3 groups, while the profile analysis at time 2 will create 2 groups.
* It would use 500 random starts and 20 initial stage iterations.
* Information about the profile analysis would be saved to an SPSS data set named "Personality".
* The full data set including the assigned profile as well as the probabilities associated with each profile would be saved to the file "D:/Personality/Mplus/model.txt".

## Example 2 - Running a specific random seed: 
**MplusLTA(inpfile = "D:/Personality/Mplus/model.inp,  
modellabel = "3-group LPA",  
nameList = ["Time1",  "Time2"],  
profileList = [ ["OpennessT1", "ConcientiousnessT1", "ExtraversionT1", "AgreeablenessT1", "NeuroticismT1"],  
$~~~~$["OpennessT2", "ConcientiousnessT2", "ExtraversionT2", "AgreeablenessT2", "NeuroticismT2"] ],  
groups = [3, 2],  
optseed = 76609,  
svalues = [ [3, 1, 2], [1, 2] ],  
modelDatasetName = "Personality",  
savedata = "D:/Personality/Mplus/model.txt",  
saveCprob = True,  
waittime = 20)**  
* This program would conduct a transition analysis for the Big 5 personality scales across two timepoints.
* Five variables are being examined at each time point.
* The profile analysis at time 1 will create 3 groups, while the profile analysis at time 2 will create 2 groups.
* Instead of using random starts, this program conducts a single run using seed 76609.
* The order of the profiles at time 1 is changed so that what was the last profile is now the first, what was the first profile is now the second, and what was the second profile is now the third. The order of the profiles at time 2 stays the same.
* Typically you would first identify what random seed you want to run, then run the program initially without having anything in svalues to understand what the original profiles look like, and then rerun it using the svales argument to set the profile order to be what you want.
* Information about the profile analysis would be saved to an SPSS data set named "Personality".
* The full data set including the assigned profile as well as the probabilities associated with each profile would be saved to the file "D:/Personality/Mplus/model.txt".
