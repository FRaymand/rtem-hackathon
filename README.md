# rtem-hackathon
Code for automatic fault detection (based on RTEM Hackathon data)
In order to maximize ease of adoption and implementation, the code has been developed in the form of a series of functions which allow the user to have near-complete control on the sensor-building-type combinations to study for faults, as well as the parameters involved in these calculations. Below, a brief description of the functions inside the code is given. The complete code is given in a github repository for which the link is given after this section..
Info_desk (metadata) 
Gives basic information, e.g. number of buildings and point types, as well as a list of  the most common point types
Tells user the available types of buildings and asks which one the user wants to study
--------------------------------------------------------------------------------------------------------
more_info(metadata , bdgs_selected) 
Tells the user the top 20 sensor types of the buildings the user has selected along with their count. If a user prompts, they can see the whole list.

dict_builder(metadata, bdgs_selected)  
Asks user which type of sensor they want to study in the chosen building type
Builds a dictionary of the desired point/building combination sensor data, where keys are building names and values are dataframes. This eases indexing

df_builder (metadata, dict_data)
Determine the first and last timestamp of data in the built dictionary
Make a dataframe with a timestamp range, spanning between the two dates found with a frequency of 5 minutes
Take the dataframe of every building within dictionary, and merge it with the previous dataframe until a final dataframe is constructed that includes all data points of the sensor-building combination
In case of too many data points or NANs, resample the data to a larger period
For each sensor within the dataframe, determine start and end times of recorded data and write them to a separate file names startendtimes, for later access

nan_detector (df, startendtimes, max_nan = 0.99)  
Adds the sensor to a list, if the data recorded have more NANs that a threshold
The threshold for maximum NAN content is 0.99 by default but user replaceable

constant_detector (df, startendtimes, max_constant = 0.99)
For each sensor, constructs the differential dataframe for data, and if there are more zeros than the max_constant threshold, adds them to a list

negligible_detector (df, startendtimes, min_days = 1) 
Adds the sensors with less than min_days of data points to a list

outlier_detector ( df, startendtimes, min_outlier=0.5, upr_parameter=1.5, lwr_parameter=1.5 )
Utilizes the IQR method to determine an acceptable upper and  lower bound for data using given parameters
Adds the sensors that have outlier data for more than 0.5 times of all their data points to a list 

show_results(df, startendtimes, filtered_list, constant_list, negligible_list, outlier_list)
Combines all previously created lists into one dataframe that shows problemartic sensors and the reason for them being in the list
Plots 5 of the problematic sensors data by random, to give the user a better visual understanding of the issue

