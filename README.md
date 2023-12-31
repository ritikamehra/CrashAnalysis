# CrashAnalysis
## Goal: 
Develop a spark application that perform the below analysis on given data and store the results for each analysis: 
1. Analytics 1: Find the number of crashes (accidents) in which number of persons killed are male?
2. Analysis 2: How many two wheelers are booked for crashes?
3. Analysis 3: Which state has highest number of accidents in which females are involved?
4. Analysis 4: Which are the Top 5th to 15th VEH_MAKE_IDs that contribute to a largest number of injuries including death
5. Analysis 5: For all the body styles involved in crashes, mention the top ethnic user group of each unique body style
6. Analysis 6: Among the crashed cars, what are the Top 5 Zip Codes with highest number crashes with alcohols as the contributing factor to a crash (Use Driver Zip Code)
7. Analysis 7: Count of Distinct Crash IDs where No Damaged Property was observed and Damage Level (VEH_DMAG_SCL~) is above 4 and car avails Insurance
8. Analysis 8: Determine the Top 5 Vehicle Makes where drivers are charged with speeding related offences, has licensed Drivers, used top 10 used vehicle colours and has car licensed with the Top 25 states with highest number of offences (to be deduced from the data)

## Data Dictionary

![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/5c22a6ab-a623-46c6-8707-737d06139f03)
![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/ebd7afe7-132f-4b28-bf75-c110b149fb92)
![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/c6815411-bea0-43bd-b0ef-699b62ba935a)

## Assumption:
- AWS EMR has been used for implementation.
- Input data is available in a folder called Data and the path is mentioned correctly in config.json.
- Output path is mentioned correctly in config.json and outputs will be written to Output folder in HDFS. 

## Setup:
1. config.json file has to be updated for any changes in the source or output paths. Any changes in the paths will impact the hadoop fs commands mentioned in point 5 and 7.

source_paths: path of input csv files

output_paths: path of output csv files

2. Create EMR Cluster (5.30.1) with Hadoop(2.8.5) and Spark(2.4.5), 1 m4.xlarge Master Node with 40 GiB EBS Storage. 

![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/f387dff2-de01-4d28-900f-8c07f66b9341)
![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/ce40cb50-a370-40e6-8c53-fb6d8d994b95)


3. Once the Cluster is running, copy the Master DNS and login to WinSCP using EC2 Key pair.

![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/f481c469-3deb-47d4-8417-d946e9850747)


4. Unzip Data.zip and place the Data Folder, utilities folder, config.json and main.py file in the /home/hadoop/ directory.

![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/ca1c5b61-f7d6-40c6-88ef-92b879a1c7ed)


5. Login to EMR cluster and run the following command to place Data files in HDFS. Any changes in config file for source_paths key will require change to 'Data/'.

hadoop fs -copyFromLocal Data/ /user/hadoop/

6. Run the following spark command to run the application and store the console output in a file.

spark-submit --py-files utilities/utility.py --files config.json main.py > output.txt

7. Run the following command to copy the output to local system which will be visible in WinSCP. Any changes in config file for output_paths key will require change to 'Output/'.

hadoop fs -copyToLocal Output/   

![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/9af29e4d-0cb4-4507-8fa7-28ef98370c6e)
![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/83027f31-0475-457a-a2b3-08b22aa35d4c)

Files should be visible in WinSCP as below.

![image](https://github.com/ritikamehra/CarCrashAnalysis/assets/54076372/bfd5f619-154b-4cc2-93ad-fd5d9e4b4dd8)

Output files can now be copied to local system.

Refer the link below for details on connecting to EMR cluster and creating .ppk or .pem file.
https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-plan-access-ssh.html

