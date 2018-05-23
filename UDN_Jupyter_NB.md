
# The Undiagnosed Disease Network: description of the database

# Introduction

Identifying the exact diagnosis of atypical clinical presentation is sometimes a medical challenge. Such clinical presentation can be simply the fact of common disease with atypical presentations or several diseases with overlapping symptoms. However, these presentations can also lead to undiagnosed disease. Diagnostic wavering is often the cause of a misdiagnosis causing suffering of the patient and behavior often inappropriate of those around him. Thus, more than half of the patients report a degraded quality of life associating physical or psychological prejudices.

A network of rare disease professionals was therefore needed. That's why, the Undiagnosed Disease Network (UDN) had been set up in 2012 to bring together a network of expert centers specializing in complex diagnostics.

# Step 1 - Install packages

PicsuRe is provided through GitHub. In order to install it, devtools package - available in CRAN (https://cran.r-project.org/) - is required. To install devtools the user must type the following commands in an R session:


```R
#install.packages("devtools", repos = "http://cran.r-project.org")
```


```R
#install.packages("readr", repos = "http://cran.r-project.org")
```


```R
library(readr)
library(devtools)
```


```R
install_github("hms-dbmi/picsuRe", force = TRUE)
```

    Downloading GitHub repo hms-dbmi/picsuRe@master
    from URL https://api.github.com/repos/hms-dbmi/picsuRe/zipball/master
    Installing picsuRe
    '/opt/conda/lib/R/bin/R' --no-site-file --no-environ --no-save --no-restore  \
      --quiet CMD INSTALL  \
      '/tmp/Rtmpnuj8IJ/devtools303c6a353b5d/hms-dbmi-picsuRe-66f21b7'  \
      --library='/opt/conda/lib/R/library' --install-tests 
    



```R
#install_git('https://github.com/hms-dbmi/picsuRe')
library(picsuRe)
```

In order to built table, the user can install 'pander' package typing the following in an R session:


```R
install.packages('pander', repos='http://cran.us.r-project.org')
library(pander)
```

    Updating HTML index of packages in '.Library'
    Making 'packages.html' ... done


# Step 2 - Define your token

To authenticate with PIC-SURE put your token in an text file in your JupyterNotebook's folder. The token will be read from there so the token does not get seen by anyone except you. You can find the token under the user profile (https://udn.hms.harvard.edu/transmart/user).


```R
token <- as.character(read.table("token.txt", sep=",")[1,1])
```

    Warning message:
    In read.table("token.txt", sep = ","): incomplete final line found by readTableHeader on 'token.txt'

# Step 3 - Define your environment

In order to retrieve the data of interest is to set up the environment. As input we need to determine the URL of the UDN database (https://udn-dev.hms.harvard.edu).


```R
env <- "https://udn-dev.hms.harvard.edu"
subset <- "ALL"
```

# Step 4 - Define the path of your variables

In order to retrieve the paths of the location of each variable of interest we define the path of our variables to retrieve the data. For example, we know that age, gender, race, ethnicity variables are under the "Demographics" folder, and so on.


```R
Demographics <- "00_Demographics"
Primary_symptom <- "01_Primary symptom category reported by patient or caregiver"
Sequence_Submitted <- "02_Sequence Submitted"
Type_sequencing <- "02_Type of sequencing"
Clinical_Site <-"03_UDN Clinical Site"
Phenotype <-"04_Clinical symptoms and physical findings (in HPO, from PhenoTips)"
Maternal_ethnicity <-"05_Maternal ethnicity (from PhenoTips)"
Paternal_ethnicity <-"06_Paternal ethnicity (from PhenoTips)"
Family_history <-"08_Family history (from PhenoTips)"
Prenatal_history <-"09_Prenatal and perinatal history (from PhenoTips)"
Medications <-"10_Medications (from PhenoTips)/Name"
Metabolites<-"10b_Metabolite (z-scores)"
Candidate_genes <-"11_Candidate genes"
Candidate_variants <-"12_Candidate variants"
Status <-"13_Status"
Diagnostic <-"14_Disorders (in OMIM, from PhenoTips)"
```

# Step 5 - Retrieve your data

Her we use the picsure function, to do it we inform the arguments: environment, token and variable's name.

# Demographics


```R
system.time(Var_Demographics <- picsure(env, token, Demographics, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /00_Demographics
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Age at symptom onset in years/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Age at UDN Evaluation (in years)/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Current age in years/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Ethnicity/Hispanic or Latino/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Ethnicity/Not Hispanic or Latino/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Ethnicity/N|A/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Ethnicity/Unknown|Not Reported Ethnicity/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Gender/Female/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Gender/Male/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Gender/Other/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Race/American Indian or Alaska Native/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Race/Asian/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Race/Black or African American/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Race/Native Hawaiian or Other Pacific Islander/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Race/Other/
    /i2b2-udn/Demo/00_Demographics/00_Demographics/Race/White/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #417
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 923 observations of 7 variables. Its size is 29.9 Kb



       user  system elapsed 
      1.877   0.040  25.696 



```R
str(Var_Demographics)
```

    'data.frame':	923 obs. of  7 variables:
     $ patient_id                      : int  71897 71893 71894 205059 204192 204190 205055 205056 204859 205050 ...
     $ Age_at_symptom_onset_in_years   : int  28 62 12 11 35 5 35 4 40 13 ...
     $ Age_at_UDN_Evaluation_.in_years.: int  25 62 11 NA 34 3 34 3 38 10 ...
     $ Current_age_in_years            : int  28 62 12 11 35 5 35 4 40 13 ...
     $ Ethnicity                       : Factor w/ 4 levels "Hispanic or Latino",..: 3 3 3 1 3 3 3 3 3 1 ...
     $ Gender                          : Factor w/ 3 levels "Female","Male",..: 2 2 2 2 2 2 2 2 2 1 ...
     $ Race                            : Factor w/ 20 levels "","American Indian or Alaska NativeAsian",..: 20 20 20 20 20 20 20 20 7 18 ...



```R
summary(Var_Demographics)
```


       patient_id     Age_at_symptom_onset_in_years
     Min.   : 71622   Min.   : 0.00                
     1st Qu.:203576   1st Qu.: 6.00                
     Median :204418   Median :14.00                
     Mean   :177032   Mean   :21.04                
     3rd Qu.:205234   3rd Qu.:32.75                
     Max.   :246216   Max.   :80.00                
                      NA's   :1                    
     Age_at_UDN_Evaluation_.in_years. Current_age_in_years
     Min.   : 0.00                    Min.   : 0.00       
     1st Qu.: 5.00                    1st Qu.: 6.00       
     Median :14.00                    Median :14.00       
     Mean   :20.68                    Mean   :21.03       
     3rd Qu.:33.00                    3rd Qu.:32.50       
     Max.   :77.00                    Max.   :80.00       
     NA's   :215                                          
                              Ethnicity      Gender   
     Hispanic or Latino            :130   Female:480  
     N/A                           :  3   Male  :442  
     Not Hispanic or Latino        :676   Other :  1  
     Unknown/Not Reported Ethnicity:114               
                                                      
                                                      
                                                      
                            Race    
     White                    :702  
     Other                    : 61  
     Asian                    : 53  
     Black or African American: 50  
                              : 17  
     AsianWhite               :  9  
     (Other)                  : 31  


## - Age of patients


```R
age <- matrix(c(0,6,14,21.04,32.75,80,1,0,5.00,14.00,20.68,33.00,77.00,215,0,6.00,14.00,21.03,32.50,80.00,0),nrow=3,ncol=7,byrow=TRUE)
```


```R
dimnames(age)=list(c("At symptom (years)","At UDN evaluation (years)","Current (years)"),c("Min","1st Qu","Median","Mean","3rd Qu","Max","NA's"))
```


```R
pander::pander(age, split.cell = 100, split.table = Inf,main="Age of patients (years)")
```

    
    -------------------------------------------------------------------------------------
                &nbsp;               Min   1st Qu   Median   Mean    3rd Qu   Max   NA's 
    ------------------------------- ----- -------- -------- ------- -------- ----- ------
        **At symptom (years)**        0      6        14     21.04   32.75    80     1   
    
     **At UDN evaluation (years)**    0      5        14     20.68     33     77    215  
    
          **Current (years)**         0      6        14     21.03    32.5    80     0   
    -------------------------------------------------------------------------------------
    



```R
par(mfrow=c(2,2))
boxplot(Var_Demographics$Age_at_symptom_onset_in_years,Var_Demographics$Age_at_UDN_Evaluation_.in_years.,Var_Demographics$Current_age_in_years,names=c("Symptom","Evaluation","Current"),ylim=c(-5,80),main="Age of patients (years)",xlab = "", ylab="Age (years)", col="grey", las="1")
hist(Var_Demographics$Age_at_symptom_onset_in_years,nclass=30, ylim=c(0,100),xlim=c(0,80),main="Age of patients at symptom",xlab = "Age (years)", ylab="Number of patients", col="grey", las="1")
hist(Var_Demographics$Age_at_UDN_Evaluation_.in_years.,nclass=30, ylim=c(0,100),xlim=c(0,80),main="Age of patients at UDN Evaluation",xlab = "Age (years)", ylab="Number of patients", col="grey", las="1")
hist(Var_Demographics$Current_age_in_years,nclass=30, ylim=c(0,100),xlim=c(0,80),main="Current Age of patients",xlab = "Age (years)", ylab="Number of patients", col="grey", las="1")
```


![svg](output_31_0.svg)


## - Race of patients


```R
as.data.frame.matrix(table(Var_Demographics$Race,Var_Demographics$Gender))
```


<table>
<thead><tr><th></th><th scope=col>Female</th><th scope=col>Male</th><th scope=col>Other</th></tr></thead>
<tbody>
	<tr><th scope=row></th><td>9</td><td>8</td><td>0</td></tr>
	<tr><th scope=row>American Indian or Alaska NativeAsian</th><td>1</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>American Indian or Alaska NativeAsianBlack or African AmericanNative Hawaiian or Other Pacific Islander</th><td>1</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>American Indian or Alaska NativeNative Hawaiian or Other Pacific IslanderWhite</th><td>1</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>American Indian or Alaska NativeOther</th><td>0</td><td>1</td><td>0</td></tr>
	<tr><th scope=row>American Indian or Alaska NativeWhite</th><td>4</td><td>4</td><td>0</td></tr>
	<tr><th scope=row>Asian</th><td>27</td><td>26</td><td> 0</td></tr>
	<tr><th scope=row>AsianBlack or African American</th><td>0</td><td>1</td><td>0</td></tr>
	<tr><th scope=row>AsianNative Hawaiian or Other Pacific IslanderWhite</th><td>1</td><td>2</td><td>0</td></tr>
	<tr><th scope=row>AsianWhite</th><td>6</td><td>3</td><td>0</td></tr>
	<tr><th scope=row>Black or African American</th><td>23</td><td>27</td><td> 0</td></tr>
	<tr><th scope=row>Black or African AmericanOther</th><td>0</td><td>1</td><td>0</td></tr>
	<tr><th scope=row>Black or African AmericanOtherWhite</th><td>2</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>Black or African AmericanWhite</th><td>3</td><td>1</td><td>0</td></tr>
	<tr><th scope=row>Native Hawaiian or Other Pacific Islander</th><td>1</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>Native Hawaiian or Other Pacific IslanderOther</th><td>1</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>Native Hawaiian or Other Pacific IslanderWhite</th><td>0</td><td>1</td><td>0</td></tr>
	<tr><th scope=row>Other</th><td>28</td><td>33</td><td> 0</td></tr>
	<tr><th scope=row>OtherWhite</th><td>2</td><td>3</td><td>0</td></tr>
	<tr><th scope=row>White</th><td>370</td><td>331</td><td>  1</td></tr>
</tbody>
</table>




```R
race <- matrix(c(7,5,34,32,28,29,2,1,372,334,28,33,9,8),nrow=7,ncol=2,byrow=TRUE)
```


```R
dimnames(race)=list(c("American Indian or Alaska Native","Asian","Black or African American","Native Hawaiian","White","Other","NA"),c("Female (n=480)","Male (n=442)"))
```


```R
pander::pander(race, split.cell = 100, split.table = Inf, main="Race of patients by gender")
```

    
    ----------------------------------------------------------------------
                    &nbsp;                  Female (n=480)   Male (n=442) 
    -------------------------------------- ---------------- --------------
     **American Indian or Alaska Native**         7               5       
    
                  **Asian**                       34              32      
    
        **Black or African American**             28              29      
    
             **Native Hawaiian**                  2               1       
    
                  **White**                      372             334      
    
                  **Other**                       28              33      
    
                    **NA**                        9               8       
    ----------------------------------------------------------------------
    


Here we are looking, for each race of patient, if a significant difference exists betwen the Female and the Male groupe. We performed a Fisher exact test in so far as Chi square test can't be applied in all the categories. 


```R
raceTest <- as.data.frame(race)
head(raceTest)
class(raceTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=480)</th><th scope=col>Male (n=442)</th></tr></thead>
<tbody>
	<tr><th scope=row>American Indian or Alaska Native</th><td>7</td><td>5</td></tr>
	<tr><th scope=row>Asian</th><td>34</td><td>32</td></tr>
	<tr><th scope=row>Black or African American</th><td>28</td><td>29</td></tr>
	<tr><th scope=row>Native Hawaiian</th><td>2</td><td>1</td></tr>
	<tr><th scope=row>White</th><td>372</td><td>334</td></tr>
	<tr><th scope=row>Other</th><td>28</td><td>33</td></tr>
</tbody>
</table>




'data.frame'



```R
raceTest$fisher_test <- NA
raceTest$CI <- NA
raceTest$OR <- NA

head(raceTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=480)</th><th scope=col>Male (n=442)</th><th scope=col>fisher_test</th><th scope=col>CI</th><th scope=col>OR</th></tr></thead>
<tbody>
	<tr><th scope=row>American Indian or Alaska Native</th><td> 7</td><td> 5</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><th scope=row>Asian</th><td>34</td><td>32</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><th scope=row>Black or African American</th><td>28</td><td>29</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><th scope=row>Native Hawaiian</th><td> 2</td><td> 1</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><th scope=row>White</th><td>372</td><td>334</td><td> NA</td><td> NA</td><td> NA</td></tr>
	<tr><th scope=row>Other</th><td>28</td><td>33</td><td>NA</td><td>NA</td><td>NA</td></tr>
</tbody>
</table>




```R
totalFemales <- 480
totalMales   <- 442

for( i in 1:nrow(raceTest)){

mytable <- matrix(c(raceTest$Female[i],totalFemales-raceTest$Female[i], raceTest$Male[i], totalMales-raceTest$Male[i]), ncol = 2)
     
mytest  <- fisher.test( mytable ) 

raceTest$fisher_test[i] <- round( mytest$p.value, 2 )

raceTest$CI[i] <- paste( "[ ", round( mytest$conf.int[1], 2 ) , " , ", round( mytest$conf.int[2], 2 ), " ]")

raceTest$OR[i]<- round ( mytest$estimate, 2 )
    
}

head(raceTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=480)</th><th scope=col>Male (n=442)</th><th scope=col>fisher_test</th><th scope=col>CI</th><th scope=col>OR</th></tr></thead>
<tbody>
	<tr><th scope=row>American Indian or Alaska Native</th><td>7                  </td><td>5                  </td><td>0.78               </td><td>[  0.35  ,  5.21  ]</td><td>1.29               </td></tr>
	<tr><th scope=row>Asian</th><td>34                 </td><td>32                 </td><td>1                  </td><td>[  0.57  ,  1.67  ]</td><td>0.98               </td></tr>
	<tr><th scope=row>Black or African American</th><td>28                </td><td>29                </td><td>0.68              </td><td>[  0.5  ,  1.57  ]</td><td>0.88              </td></tr>
	<tr><th scope=row>Native Hawaiian</th><td>2                   </td><td>1                   </td><td>1                   </td><td>[  0.1  ,  109.04  ]</td><td>1.84                </td></tr>
	<tr><th scope=row>White</th><td>372                </td><td>334                </td><td>0.53               </td><td>[  0.81  ,  1.53  ]</td><td>1.11               </td></tr>
	<tr><th scope=row>Other</th><td>28                 </td><td>33                 </td><td>0.35               </td><td>[  0.44  ,  1.34  ]</td><td>0.77               </td></tr>
</tbody>
</table>




```R
race2 <- matrix(c(7,5,0.78,1.29,34,32,1,0.98,28,29,0.68,0.88,2,1,1,1.84,372,334,0.53,1.11,28,33,0.35,0.77),nrow=7,ncol=4,byrow=TRUE)
dimnames(race2)=list(c("American Indian or Alaska Native","Asian","Black or African American","Native Hawaiian","White","Other","NA"),c("Female (N=480)","Male (N=442)","Fisher test","OR"))
pander::pander(race2, split.cell = 100, split.table = Inf,main="Race of patients by gender")
```

    Warning message:
    In matrix(c(7, 5, 0.78, 1.29, 34, 32, 1, 0.98, 28, 29, 0.68, 0.88, : data length [24] is not a sub-multiple or multiple of the number of rows [7]

    
    -------------------------------------------------------------------------------------------
                    &nbsp;                  Female (N=480)   Male (N=442)   Fisher test    OR  
    -------------------------------------- ---------------- -------------- ------------- ------
     **American Indian or Alaska Native**         7               5            0.78       1.29 
    
                  **Asian**                       34              32             1        0.98 
    
        **Black or African American**             28              29           0.68       0.88 
    
             **Native Hawaiian**                  2               1              1        1.84 
    
                  **White**                      372             334           0.53       1.11 
    
                  **Other**                       28              33           0.35       0.77 
    
                    **NA**                        7               5            0.78       1.29 
    -------------------------------------------------------------------------------------------
    


# Primary symptoms


```R
system.time(Var_Primary_symptom <- picsure(env, token, Primary_symptom, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /01_Primary symptom category reported by patient or caregiver
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Allergies and Disorders of The Immune System/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Cardiology and vascular conditions (heart, artery, vein, and lymph disorders)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Dentistry and craniofacial (bones of head and face)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Dermatology (skin diseases and disorders)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Endocrinology (disorder of the endocrine glands and hormones)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Gastroenterology (disorder of the stomach and intestines)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Gynecology and reproductive medicine/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Hematology (blood diseases and disorders)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Infectious diseases/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Musculoskeletal and orthopedics (structural and functional disorders of muscles, bones, and joints)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Nephrology (kidney diseases and disorders)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Neurology (disorders of the nervous system, including brain and spinal cord)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/N|A/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Oncology (Tumors and cancer)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Ophthalmology (Eye disorders and diseases)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Other/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Psychiatry/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Pulmonology (Lung disorders and diseases)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Rheumatology (immune disorders of the joints, muscles, and ligaments)/
    /i2b2-udn/Demo/01_Primary symptom category reported by patient or caregiver/01_Primary symptom category reported by patient or caregiver/Urology/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #419
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 921 observations of 2 variables. Its size is 10.4 Kb



       user  system elapsed 
      2.078   0.045  25.571 



```R
str(Var_Primary_symptom)
```

    'data.frame':	921 obs. of  2 variables:
     $ patient_id                                                   : int  71897 71893 71894 205059 204192 204190 205055 205056 204859 205050 ...
     $ x01_Primary_symptom_category_reported_by_patient_or_caregiver: Factor w/ 20 levels "Allergies and Disorders of The Immune System",..: 11 12 13 13 13 13 14 10 13 13 ...



```R
summary(Var_Primary_symptom)
```


       patient_id    
     Min.   : 71622  
     1st Qu.:203580  
     Median :204418  
     Mean   :177070  
     3rd Qu.:205234  
     Max.   :246216  
                     
                                                                         x01_Primary_symptom_category_reported_by_patient_or_caregiver
     Neurology (disorders of the nervous system, including brain and spinal cord)                       :428                          
     Musculoskeletal and orthopedics (structural and functional disorders of muscles, bones, and joints):112                          
     Other                                                                                              : 92                          
     Allergies and Disorders of The Immune System                                                       : 48                          
     Cardiology and vascular conditions (heart, artery, vein, and lymph disorders)                      : 43                          
     Gastroenterology (disorder of the stomach and intestines)                                          : 37                          
     (Other)                                                                                            :161                          



```R
symptoms <- matrix(c(233,195,59,53,25,23,20,23,20,17,20,13,14,12,8,10,3,12,6,7,5,4,4,2,3,2,0,3,2,1,1,1,1,1,0,1,43,49,12,13),nrow=20,ncol=2,byrow=TRUE)
dimnames(symptoms)=list(c("Neurology","Orthopedics","Immune system disorders","Cardiology","Gastroenterology","Rheumatology","Endocrinology","Pulmonology","Nephrology","Hematology","Ophthalmology","Dentistry","Dermatology","Oncology","Psychiatry","Gynecology","Infectious diseases","Urology","Other","NA"),c("Female (n=480)","Male (n=442)"))
pander::pander(symptoms, split.cell = 100, split.table = Inf,main="Symptoms of patients by gender")
```

    
    -------------------------------------------------------------
               &nbsp;              Female (n=480)   Male (n=442) 
    ----------------------------- ---------------- --------------
            **Neurology**               233             195      
    
           **Orthopedics**               59              53      
    
     **Immune system disorders**         25              23      
    
           **Cardiology**                20              23      
    
        **Gastroenterology**             20              17      
    
          **Rheumatology**               20              13      
    
          **Endocrinology**              14              12      
    
           **Pulmonology**               8               10      
    
           **Nephrology**                3               12      
    
           **Hematology**                6               7       
    
          **Ophthalmology**              5               4       
    
            **Dentistry**                4               2       
    
           **Dermatology**               3               2       
    
            **Oncology**                 0               3       
    
           **Psychiatry**                2               1       
    
           **Gynecology**                1               1       
    
       **Infectious diseases**           1               1       
    
             **Urology**                 0               1       
    
              **Other**                  43              49      
    
               **NA**                    12              13      
    -------------------------------------------------------------
    


Here we are looking, for each category of symptoms, if a significant difference exists betwen the Female and the Male groupe. We performed a Fisher exact test in so far as Chi square test can't be applied in all the categories.


```R
symptomsTest <- as.data.frame(symptoms)
head(symptomsTest)
class(symptomsTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=480)</th><th scope=col>Male (n=442)</th></tr></thead>
<tbody>
	<tr><th scope=row>Neurology</th><td>233</td><td>195</td></tr>
	<tr><th scope=row>Orthopedics</th><td>59</td><td>53</td></tr>
	<tr><th scope=row>Immune system disorders</th><td>25</td><td>23</td></tr>
	<tr><th scope=row>Cardiology</th><td>20</td><td>23</td></tr>
	<tr><th scope=row>Gastroenterology</th><td>20</td><td>17</td></tr>
	<tr><th scope=row>Rheumatology</th><td>20</td><td>13</td></tr>
</tbody>
</table>




'data.frame'



```R
symptomsTest$ttest <- NA
symptomsTest$CI <- NA
symptomsTest$OR <- NA
head(symptomsTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=480)</th><th scope=col>Male (n=442)</th><th scope=col>ttest</th><th scope=col>CI</th><th scope=col>OR</th></tr></thead>
<tbody>
	<tr><th scope=row>Neurology</th><td>233</td><td>195</td><td> NA</td><td> NA</td><td> NA</td></tr>
	<tr><th scope=row>Orthopedics</th><td>59</td><td>53</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><th scope=row>Immune system disorders</th><td>25</td><td>23</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><th scope=row>Cardiology</th><td>20</td><td>23</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><th scope=row>Gastroenterology</th><td>20</td><td>17</td><td>NA</td><td>NA</td><td>NA</td></tr>
	<tr><th scope=row>Rheumatology</th><td>20</td><td>13</td><td>NA</td><td>NA</td><td>NA</td></tr>
</tbody>
</table>




```R
totalFemales <- 480
totalMales   <- 442

for( i in 1:nrow(symptomsTest)){
    mytable <- matrix(c(symptomsTest$Female[i],totalFemales-symptomsTest$Female[i], 
                 symptomsTest$Male[i], totalMales-symptomsTest$Male[i]), ncol = 2)
    mytest  <- fisher.test( mytable )
    symptomsTest$ttest[i] <- round( mytest$p.value, 2 )
    symptomsTest$CI[i] <- paste( "[ ", round( mytest$conf.int[1], 2 ) , " , ", round( mytest$conf.int[2], 2 ), " ]") 
    symptomsTest$OR[i]<- round ( mytest$estimate, 2 )
    
}
```


```R
symptoms2 <- matrix(c(233,195,0.19,1.19,59,53,0.92,1.03,25,23,1,1,20,23,0.53,0.79,20,17,0.87,1.09,20,13,0.38,1.43,14,12,1,1.08,8,10,0.64,0.73,3,12,0.02,0.23,6,7,0.78,0.79,5,4,1,1.15,4,2,0.69,1.85,3,2,1,1.38,0,3,0.11,0,2,1,1,1.84,1,1,1,0.92,1,1,1,0.92,0,1,0.48,0,43,49,0.32,0.79,12,13,0.69,0.85),nrow=20,ncol=4,byrow=TRUE)
dimnames(symptoms2)=list(c("Neurology","Orthopedics","Immune system disorders","Cardiology","Gastroenterology","Rheumatology","Endocrinology","Pulmonology","Nephrology","Hematology","Ophthalmology","Dentistry","Dermatology","Oncology","Psychiatry","Gynecology","Infectious diseases","Urology","Other","NA"),c("Female (n=480)","Male (n=442)","Fisher test","OR"))
pander::pander(symptoms2, split.cell = 100, split.table = Inf,main="Symptoms of patients by gender")
```

    
    ----------------------------------------------------------------------------------
               &nbsp;              Female (n=480)   Male (n=442)   Fisher test    OR  
    ----------------------------- ---------------- -------------- ------------- ------
            **Neurology**               233             195           0.19       1.19 
    
           **Orthopedics**               59              53           0.92       1.03 
    
     **Immune system disorders**         25              23             1         1   
    
           **Cardiology**                20              23           0.53       0.79 
    
        **Gastroenterology**             20              17           0.87       1.09 
    
          **Rheumatology**               20              13           0.38       1.43 
    
          **Endocrinology**              14              12             1        1.08 
    
           **Pulmonology**               8               10           0.64       0.73 
    
           **Nephrology**                3               12           0.02       0.23 
    
           **Hematology**                6               7            0.78       0.79 
    
          **Ophthalmology**              5               4              1        1.15 
    
            **Dentistry**                4               2            0.69       1.85 
    
           **Dermatology**               3               2              1        1.38 
    
            **Oncology**                 0               3            0.11        0   
    
           **Psychiatry**                2               1              1        1.84 
    
           **Gynecology**                1               1              1        0.92 
    
       **Infectious diseases**           1               1              1        0.92 
    
             **Urology**                 0               1            0.48        0   
    
              **Other**                  43              49           0.32       0.79 
    
               **NA**                    12              13           0.69       0.85 
    ----------------------------------------------------------------------------------
    


# Sequence submitted


```R
system.time(Var_Sequence_Submitted <- picsure(env, token, Sequence_Submitted, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /02_Sequence Submitted
    /i2b2-udn/Demo/02_Sequence Submitted/02_Sequence Submitted/false/
    /i2b2-udn/Demo/02_Sequence Submitted/02_Sequence Submitted/true/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #421
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 624 observations of 2 variables. Its size is 6.1 Kb



       user  system elapsed 
      0.477   0.021   6.758 



```R
str(Var_Sequence_Submitted)
```

    'data.frame':	624 obs. of  2 variables:
     $ patient_id            : int  203587 203584 203583 203580 203581 204962 71893 71790 71791 205059 ...
     $ x02_Sequence_Submitted: Factor w/ 2 levels "false","true": 2 2 2 2 2 2 2 2 2 2 ...



```R
summary(Var_Sequence_Submitted)
```


       patient_id     x02_Sequence_Submitted
     Min.   : 71622   false:  3             
     1st Qu.:203752   true :621             
     Median :204455                         
     Mean   :183946                         
     3rd Qu.:205116                         
     Max.   :246212                         


# Type of Sequencing


```R
system.time(Var_Type_sequencing <- picsure(env, token, Type_sequencing, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /02_Type of sequencing
    /i2b2-udn/Demo/02_Type of sequencing/02_Type of sequencing/Targeted Variant/
    /i2b2-udn/Demo/02_Type of sequencing/02_Type of sequencing/Whole Exome/
    /i2b2-udn/Demo/02_Type of sequencing/02_Type of sequencing/Whole Genome/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #423
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 624 observations of 2 variables. Its size is 6.2 Kb



       user  system elapsed 
      0.571   0.018   7.251 



```R
str(Var_Type_sequencing)
```

    'data.frame':	624 obs. of  2 variables:
     $ patient_id            : int  203587 203584 203583 203580 203581 204962 71790 71893 71791 205059 ...
     $ x02_Type_of_sequencing: Factor w/ 3 levels "Targeted Variant",..: 2 2 2 2 2 2 2 2 2 2 ...



```R
summary(Var_Type_sequencing)
```


       patient_id          x02_Type_of_sequencing
     Min.   : 71622   Targeted Variant:  3       
     1st Qu.:203752   Whole Exome     :311       
     Median :204455   Whole Genome    :310       
     Mean   :183946                              
     3rd Qu.:205116                              
     Max.   :246212                              



```R
sequence <- matrix(c(162,149,311,152,158,310,2,1,3),nrow=3,ncol=3,byrow=TRUE)
dimnames(sequence)=list(c("Whole Exome","Whole Genome","Targeted Variant"),c("Female (n=316)","Male (n=308)","Total (N=624)"))
pander::pander(sequence, split.cell = 100, split.table = Inf,main="Symptoms of patients by gender")
```

    
    ----------------------------------------------------------------------
            &nbsp;          Female (n=316)   Male (n=308)   Total (N=624) 
    ---------------------- ---------------- -------------- ---------------
       **Whole Exome**           162             149             311      
    
       **Whole Genome**          152             158             310      
    
     **Targeted Variant**         2               1               3       
    ----------------------------------------------------------------------
    


Here we are looking, for each type of sequencing, if a significant difference exists betwen the Female and the Male groupe. We performed a Fisher exact test in so far as Chi square test can't be applied in all the categories.


```R
sequenceTest <- as.data.frame(sequence)
head(sequenceTest)
class(sequenceTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=316)</th><th scope=col>Male (n=308)</th><th scope=col>Total (N=624)</th></tr></thead>
<tbody>
	<tr><th scope=row>Whole Exome</th><td>162</td><td>149</td><td>311</td></tr>
	<tr><th scope=row>Whole Genome</th><td>152</td><td>158</td><td>310</td></tr>
	<tr><th scope=row>Targeted Variant</th><td>2</td><td>1</td><td>3</td></tr>
</tbody>
</table>




'data.frame'



```R
sequenceTest$fisher_test <- NA
sequenceTest$CI <- NA
sequenceTest$OR <- NA

head(sequenceTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=316)</th><th scope=col>Male (n=308)</th><th scope=col>Total (N=624)</th><th scope=col>fisher_test</th><th scope=col>CI</th><th scope=col>OR</th></tr></thead>
<tbody>
	<tr><th scope=row>Whole Exome</th><td>162</td><td>149</td><td>311</td><td> NA</td><td> NA</td><td> NA</td></tr>
	<tr><th scope=row>Whole Genome</th><td>152</td><td>158</td><td>310</td><td> NA</td><td> NA</td><td> NA</td></tr>
	<tr><th scope=row>Targeted Variant</th><td> 2</td><td> 1</td><td> 3</td><td>NA</td><td>NA</td><td>NA</td></tr>
</tbody>
</table>




```R
totalFemales <- 316
totalMales   <- 308

for( i in 1:nrow(sequenceTest)){

mytable <- matrix(c(sequenceTest$Female[i],totalFemales-sequenceTest$Female[i], sequenceTest$Male[i], totalMales-sequenceTest$Male[i]), ncol = 2)
     
mytest  <- fisher.test( mytable ) 

sequenceTest$fisher_test[i] <- round( mytest$p.value, 2 )

sequenceTest$CI[i] <- paste( "[ ", round( mytest$conf.int[1], 2 ) , " , ", round( mytest$conf.int[2], 2 ), " ]")

sequenceTest$OR[i]<- round ( mytest$estimate, 2 )
    
}

head(sequenceTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=316)</th><th scope=col>Male (n=308)</th><th scope=col>Total (N=624)</th><th scope=col>fisher_test</th><th scope=col>CI</th><th scope=col>OR</th></tr></thead>
<tbody>
	<tr><th scope=row>Whole Exome</th><td>162                </td><td>149                </td><td>311                </td><td>0.47               </td><td>[  0.81  ,  1.56  ]</td><td>1.12               </td></tr>
	<tr><th scope=row>Whole Genome</th><td>152                </td><td>158                </td><td>310                </td><td>0.47               </td><td>[  0.63  ,  1.22  ]</td><td>0.88               </td></tr>
	<tr><th scope=row>Targeted Variant</th><td>2                   </td><td>1                   </td><td>3                   </td><td>1                   </td><td>[  0.1  ,  115.66  ]</td><td>1.95                </td></tr>
</tbody>
</table>




```R
sequence2 <- matrix(c(2,1,3,1,1.95,162,149,311,0.47,0.88,152,158,310,0.47,0.88),nrow=3,ncol=5,byrow=TRUE)
dimnames(sequence2)=list(c("Targeted Variant","Whole Exome","Whole Genome"),c("Female (n=316)","Male (n=308)","Total (N=624)","Fisher test","OR"))
pander::pander(sequence2, split.cell = 100, split.table = Inf,main="Symptoms of patients by gender")
```

    
    -------------------------------------------------------------------------------------------
            &nbsp;          Female (n=316)   Male (n=308)   Total (N=624)   Fisher test    OR  
    ---------------------- ---------------- -------------- --------------- ------------- ------
     **Targeted Variant**         2               1               3              1        1.95 
    
       **Whole Exome**           162             149             311           0.47       0.88 
    
       **Whole Genome**          152             158             310           0.47       0.88 
    -------------------------------------------------------------------------------------------
    


# UDN Clinical Site


```R
system.time(Var_Clinical_Site <- picsure(env, token, Clinical_Site, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /03_UDN Clinical Site
    /i2b2-udn/Demo/03_UDN Clinical Site/03_UDN Clinical Site/baylor/
    /i2b2-udn/Demo/03_UDN Clinical Site/03_UDN Clinical Site/duke/
    /i2b2-udn/Demo/03_UDN Clinical Site/03_UDN Clinical Site/harvard-affiliate/
    /i2b2-udn/Demo/03_UDN Clinical Site/03_UDN Clinical Site/nih/
    /i2b2-udn/Demo/03_UDN Clinical Site/03_UDN Clinical Site/stanford/
    /i2b2-udn/Demo/03_UDN Clinical Site/03_UDN Clinical Site/ucla/
    /i2b2-udn/Demo/03_UDN Clinical Site/03_UDN Clinical Site/vanderbilt/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #425
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 923 observations of 2 variables. Its size is 8.8 Kb



       user  system elapsed 
      0.909   0.030  11.388 



```R
str(Var_Clinical_Site)
```

    'data.frame':	923 obs. of  2 variables:
     $ patient_id           : int  71897 71893 71894 205059 204192 204190 205055 205056 204859 205050 ...
     $ x03_UDN_Clinical_Site: Factor w/ 7 levels "baylor","duke",..: 4 4 3 5 4 3 1 2 4 4 ...



```R
summary(Var_Clinical_Site)
```


       patient_id           x03_UDN_Clinical_Site
     Min.   : 71622   baylor           :133      
     1st Qu.:203576   duke             :125      
     Median :204418   harvard-affiliate:102      
     Mean   :177032   nih              :241      
     3rd Qu.:205234   stanford         :121      
     Max.   :246216   ucla             : 98      
                      vanderbilt       :103      



```R
clinical_site<- matrix(c(72,61,133,63,62,125,50,52,102,131,110,241,64,57,121,44,54,98,56,47,103),nrow=7,ncol=3,byrow=TRUE)
dimnames(clinical_site)=list(c("Baylor","Duke","Harvard-affiliate","NIH","Stanford","UCLA","Vanderbilt"),c("Female (n=480)","Male (n=442)","Total (N=922)"))
pander::pander(clinical_site, split.cell = 100, split.table = Inf,main="Clinical UDN sites by gender")
```

    
    -----------------------------------------------------------------------
            &nbsp;           Female (n=480)   Male (n=442)   Total (N=922) 
    ----------------------- ---------------- -------------- ---------------
          **Baylor**               72              61             133      
    
           **Duke**                63              62             125      
    
     **Harvard-affiliate**         50              52             102      
    
            **NIH**               131             110             241      
    
         **Stanford**              64              57             121      
    
           **UCLA**                44              54             98       
    
        **Vanderbilt**             56              47             103      
    -----------------------------------------------------------------------
    


Here we are looking, for each clinical site, if a significant difference exists betwen the Female and the Male groupe. We performed a Fisher exact test in so far as Chi square test can't be applied in all the categories.


```R
clinical_siteTest <- as.data.frame(clinical_site)
head(clinical_siteTest)
class(clinical_siteTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=480)</th><th scope=col>Male (n=442)</th><th scope=col>Total (N=922)</th></tr></thead>
<tbody>
	<tr><th scope=row>Baylor</th><td> 72</td><td> 61</td><td>133</td></tr>
	<tr><th scope=row>Duke</th><td> 63</td><td> 62</td><td>125</td></tr>
	<tr><th scope=row>Harvard-affiliate</th><td> 50</td><td> 52</td><td>102</td></tr>
	<tr><th scope=row>NIH</th><td>131</td><td>110</td><td>241</td></tr>
	<tr><th scope=row>Stanford</th><td> 64</td><td> 57</td><td>121</td></tr>
	<tr><th scope=row>UCLA</th><td>44</td><td>54</td><td>98</td></tr>
</tbody>
</table>




'data.frame'



```R
clinical_siteTest$fisher_test <- NA
clinical_siteTest$CI <- NA
clinical_siteTest$OR <- NA

head(clinical_siteTest)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=480)</th><th scope=col>Male (n=442)</th><th scope=col>Total (N=922)</th><th scope=col>fisher_test</th><th scope=col>CI</th><th scope=col>OR</th></tr></thead>
<tbody>
	<tr><th scope=row>Baylor</th><td> 72</td><td> 61</td><td>133</td><td> NA</td><td> NA</td><td> NA</td></tr>
	<tr><th scope=row>Duke</th><td> 63</td><td> 62</td><td>125</td><td> NA</td><td> NA</td><td> NA</td></tr>
	<tr><th scope=row>Harvard-affiliate</th><td> 50</td><td> 52</td><td>102</td><td> NA</td><td> NA</td><td> NA</td></tr>
	<tr><th scope=row>NIH</th><td>131</td><td>110</td><td>241</td><td> NA</td><td> NA</td><td> NA</td></tr>
	<tr><th scope=row>Stanford</th><td> 64</td><td> 57</td><td>121</td><td> NA</td><td> NA</td><td> NA</td></tr>
	<tr><th scope=row>UCLA</th><td>44</td><td>54</td><td>98</td><td>NA</td><td>NA</td><td>NA</td></tr>
</tbody>
</table>




```R
totalFemales <- 480
totalMales   <- 442

for( i in 1:nrow(clinical_siteTest)){

mytable <- matrix(c(clinical_siteTest$Female[i],totalFemales-clinical_siteTest$Female[i], clinical_siteTest$Male[i], totalMales-clinical_siteTest$Male[i]), ncol = 2)
     
mytest  <- fisher.test( mytable ) 

clinical_siteTest$fisher_test[i] <- round( mytest$p.value, 2 )

clinical_siteTest$CI[i] <- paste( "[ ", round( mytest$conf.int[1], 2 ) , " , ", round( mytest$conf.int[2], 2 ), " ]")

clinical_siteTest$OR[i]<- round ( mytest$estimate, 2 )
    
}

head(clinical_siteTest,10)
```


<table>
<thead><tr><th></th><th scope=col>Female (n=480)</th><th scope=col>Male (n=442)</th><th scope=col>Total (N=922)</th><th scope=col>fisher_test</th><th scope=col>CI</th><th scope=col>OR</th></tr></thead>
<tbody>
	<tr><th scope=row>Baylor</th><td>72                 </td><td>61                 </td><td>133                </td><td>0.64               </td><td>[  0.75  ,  1.62  ]</td><td>1.1                </td></tr>
	<tr><th scope=row>Duke</th><td>63                 </td><td>62                 </td><td>125                </td><td>0.7                </td><td>[  0.62  ,  1.38  ]</td><td>0.93               </td></tr>
	<tr><th scope=row>Harvard-affiliate</th><td>50                 </td><td>52                 </td><td>102                </td><td>0.53               </td><td>[  0.57  ,  1.35  ]</td><td>0.87               </td></tr>
	<tr><th scope=row>NIH</th><td>131                </td><td>110                </td><td>241                </td><td>0.41               </td><td>[  0.83  ,  1.54  ]</td><td>1.13               </td></tr>
	<tr><th scope=row>Stanford</th><td>64                </td><td>57                </td><td>121               </td><td>0.92              </td><td>[  0.7  ,  1.55  ]</td><td>1.04              </td></tr>
	<tr><th scope=row>UCLA</th><td>44                 </td><td>54                 </td><td>98                 </td><td>0.14               </td><td>[  0.46  ,  1.13  ]</td><td>0.73               </td></tr>
	<tr><th scope=row>Vanderbilt</th><td>56                 </td><td>47                 </td><td>103                </td><td>0.68               </td><td>[  0.72  ,  1.71  ]</td><td>1.11               </td></tr>
</tbody>
</table>




```R
clinical_site2<- matrix(c(72,61,133,0.64,1.1,63,62,125,0.7,0.93,50,52,102,0.53,0.87,131,110,241,0.41,1.13,64,57,121,0.92,1.04,44,54,98,0.14,0.73,56,47,103,0.68,1.11),nrow=7,ncol=5,byrow=TRUE)
dimnames(clinical_site2)=list(c("Baylor","Duke","Harvard-affiliate","NIH","Stanford","UCLA","Vanderbilt"),c("Female (n=480)","Male (n=442)","Total (N=922)","Fisher test","OR"))
pander::pander(clinical_site2, split.cell = 100, split.table = Inf,main="Clinical UDN sites by gender")
```

    
    --------------------------------------------------------------------------------------------
            &nbsp;           Female (n=480)   Male (n=442)   Total (N=922)   Fisher test    OR  
    ----------------------- ---------------- -------------- --------------- ------------- ------
          **Baylor**               72              61             133           0.64       1.1  
    
           **Duke**                63              62             125            0.7       0.93 
    
     **Harvard-affiliate**         50              52             102           0.53       0.87 
    
            **NIH**               131             110             241           0.41       1.13 
    
         **Stanford**              64              57             121           0.92       1.04 
    
           **UCLA**                44              54             98            0.14       0.73 
    
        **Vanderbilt**             56              47             103           0.68       1.11 
    --------------------------------------------------------------------------------------------
    


# Maternal Ethnicity


```R
system.time(Var_Maternal_ethnicity <- picsure(env, token, Maternal_ethnicity, gabe = TRUE, verbose = TRUE))
```


```R
str(Var_Maternal_ethnicity)
```


```R
summary(Var_Maternal_ethnicity)
```

# Paternal Ethnicity


```R
system.time(Var_Paternal_ethnicity <- picsure(env, token, Paternal_ethnicity, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /06_Paternal ethnicity (from PhenoTips)
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ Ashkenazi/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ English/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ French/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ German/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ Hungarian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ Irish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ Jewish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ MX/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ Scottish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ Sicilian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ Spain/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/ Welsh/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Afghanistan/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/African/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/African American/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/African Americans/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Aleutian Indian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Arabs/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Argentina/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Armenian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Ashkenazi Jewish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Ashkenazi Jews/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Asian Indian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Austrian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Austrians/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Bolivian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/British/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Calatayud/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Canary island/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Caucasian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/caucasian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Cherokee/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Chinese/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Columbia/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Croats/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Czech/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Czechoslovakian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Danish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Denmark/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Dutch/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Eastern European/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Ecuador/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Ecuadorian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Egyptian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/El Salvador/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/English/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Ethiopian/
    
       !!!There is an issue in the database with the path: "/i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/European/"
       -> discarding path. Please contact the developpers regarding this issue!!!
    
    
       !!!There is an issue in the database with the path: "/i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/European/"
       -> discarding path. Please contact the developpers regarding this issue!!!
    
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/European Americans/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/European Caucasian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Finnish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Flemish/
    
       !!!There is an issue in the database with the path: "/i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/French/"
       -> discarding path. Please contact the developpers regarding this issue!!!
    
    
       !!!There is an issue in the database with the path: "/i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/French/"
       -> discarding path. Please contact the developpers regarding this issue!!!
    
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/French Canadian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/German/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/german/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Germans/
    
       !!!There is an issue in the database with the path: "/i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Greek/"
       -> discarding path. Please contact the developpers regarding this issue!!!
    
    
       !!!There is an issue in the database with the path: "/i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Greek/"
       -> discarding path. Please contact the developpers regarding this issue!!!
    
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Guatemalan/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Guerrero/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Hispanic/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/hispanic/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Hispanic - Mexico/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Hispanic-Mexico/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Hungarian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Hungarians/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/indian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Indo-Trinidadian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Irani/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Iranian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Iraqi/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Irish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/irish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Italian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Italians/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Italy/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Japanese/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Japenese/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Jewish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Kazakhstani/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Korean/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Lebanese/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Lumbee/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Mexican/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/mexican/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Mexico/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/michoacan/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Mixed European/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Muhajir (Pakistan)/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Native American/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/native american/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Native American - Cherokee/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Nigerian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/No known Ashkenazi Jewish ancestry/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/No known Jewish ancestry/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Non Jewish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Non-Hispanic/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Northern European/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Northern India/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Northern Scottish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Norwegian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Norwegians/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Not Ashkenzi Jewish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Other unknown/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Pakistan/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/pakistan/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Palestinian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Pennsylvania German/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Phillipino/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Polish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Polynesians/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Portuguese/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Possible distant Ashkenazi Jewish ancestry/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Puerto Rican/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Puerto Rico/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Puerto-Rican/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Romanian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Russian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Scandanavian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Scottish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Seminole Native American/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Sephardic Jewish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Serbian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Serbs/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Sicilian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/South African/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/South American (Colombia)/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/South Korean/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Spain/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Spaniards/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Spanish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Sri Lanka/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Swedes/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Swedish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Swiss/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Taiwanese/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Turkish/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Ukrainian/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/UNKNOWN/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/unknown/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Unknown/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Urkraine/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Venezuelan/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Vietnamese/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Welsh/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Welsh|English/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/Western European/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/White/
    /i2b2-udn/Demo/06_Paternal ethnicity (from PhenoTips)/06_Paternal ethnicity (from PhenoTips)/yemen/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #427
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 479 observations of 2 variables. Its size is 21.6 Kb



       user  system elapsed 
     12.376   0.303 151.171 



```R
str(Var_Paternal_ethnicity)
```

    'data.frame':	479 obs. of  2 variables:
     $ patient_id                             : int  203587 203584 203583 71897 71894 204954 205059 204192 204958 205055 ...
     $ x06_Paternal_ethnicity_.from_PhenoTips.: Factor w/ 236 levels "Afghanistan",..: 59 108 30 213 43 126 137 158 103 151 ...



```R
summary(Var_Paternal_ethnicity)
```


       patient_id     x06_Paternal_ethnicity_.from_PhenoTips.
     Min.   : 71676   Hispanic-Mexico   : 25                 
     1st Qu.:203786   Mexican           : 23                 
     Median :204485   Caucasian         : 16                 
     Mean   :184827   European Caucasian: 15                 
     3rd Qu.:205150   Irish             : 15                 
     Max.   :246213   German            : 14                 
                      (Other)           :371                 


# Family History


```R
system.time(Var_Family_history <- picsure(env, token, Family_history, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /08_Family history (from PhenoTips)
    /i2b2-udn/Demo/08_Family history (from PhenoTips)/08_Family history (from PhenoTips)/Affected Relatives/false/
    /i2b2-udn/Demo/08_Family history (from PhenoTips)/08_Family history (from PhenoTips)/Affected Relatives/true/
    /i2b2-udn/Demo/08_Family history (from PhenoTips)/08_Family history (from PhenoTips)/Consanguinity/false/
    /i2b2-udn/Demo/08_Family history (from PhenoTips)/08_Family history (from PhenoTips)/Consanguinity/true/
    /i2b2-udn/Demo/08_Family history (from PhenoTips)/08_Family history (from PhenoTips)/Miscarriages/false/
    /i2b2-udn/Demo/08_Family history (from PhenoTips)/08_Family history (from PhenoTips)/Miscarriages/true/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #429
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 519 observations of 4 variables. Its size is 10.7 Kb



       user  system elapsed 
      0.907   0.037  11.191 



```R
str(Var_Family_history)
```

    'data.frame':	519 obs. of  4 variables:
     $ patient_id        : int  71796 203587 203584 203583 71897 71791 71894 204954 205059 204958 ...
     $ Affected_Relatives: Factor w/ 3 levels "","false","true": 1 3 1 2 1 2 2 3 2 2 ...
     $ Consanguinity     : Factor w/ 3 levels "","false","true": 2 2 2 2 2 2 2 2 2 2 ...
     $ Miscarriages      : Factor w/ 3 levels "","false","true": 2 2 1 2 2 2 2 1 2 2 ...



```R
summary(Var_Family_history)
```


       patient_id     Affected_Relatives Consanguinity Miscarriages
     Min.   : 71658        : 94               : 19          : 59   
     1st Qu.:203750   false:332          false:482     false:431   
     Median :204437   true : 93          true : 18     true : 29   
     Mean   :183622                                                
     3rd Qu.:205146                                                
     Max.   :246213                                                


# Prenatal and perinatal history


```R
system.time(Var_Prenatal_history <- picsure(env, token, Prenatal_history, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /09_Prenatal and perinatal history (from PhenoTips)
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Assisted Reproduction Donor Egg/false/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Assisted Reproduction Donor Egg/true/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Assisted Reproduction Donor Sperm/false/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Assisted Reproduction Fertility Meds/false/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Assisted Reproduction Fertility Meds/true/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Assisted Reproduction iui/false/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Assisted Reproduction iui/true/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Assisted Reproduction Surrogacy/false/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Assisted Reproduction Surrogacy/true/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Gestation/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/icsi/false/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/icsi/true/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/ivf/false/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/ivf/true/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Maternal Age/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/multipleGestation/false/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/multipleGestation/true/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Obseteric History/Births/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Obseteric History/Gravida/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Obseteric History/Para/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Obseteric History/Term/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Paternal Age/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Preterm/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Sab/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/Tab/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/twinNumber/A/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/twinNumber/B/
    /i2b2-udn/Demo/09_Prenatal and perinatal history (from PhenoTips)/09_Prenatal and perinatal history (from PhenoTips)/twinNumber/Other/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #431
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 770 observations of 20 variables. Its size is 67.6 Kb



       user  system elapsed 
      3.074   0.064  35.837 



```R
str(Var_Prenatal_history)
```

    'data.frame':	770 obs. of  20 variables:
     $ patient_id                          : int  71796 203587 203584 203583 203581 71897 204962 71893 71790 71894 ...
     $ Assisted_Reproduction_Donor_Egg     : Factor w/ 3 levels "","false","true": 1 1 1 2 1 1 1 1 1 1 ...
     $ Assisted_Reproduction_Donor_Sperm   : Factor w/ 2 levels "","false": 1 1 1 2 1 1 1 1 1 1 ...
     $ Assisted_Reproduction_Fertility_Meds: Factor w/ 3 levels "","false","true": 1 1 1 2 1 1 1 1 1 1 ...
     $ Assisted_Reproduction_iui           : Factor w/ 3 levels "","false","true": 1 1 1 2 1 1 1 1 1 1 ...
     $ Assisted_Reproduction_Surrogacy     : Factor w/ 3 levels "","false","true": 1 1 1 2 1 1 1 1 1 1 ...
     $ Gestation                           : int  40 40 35 NA NA 40 NA NA NA NA ...
     $ Maternal_Age                        : int  0 0 0 20 0 0 0 0 0 0 ...
     $ Paternal_Age                        : int  0 0 0 0 0 0 0 0 0 0 ...
     $ Preterm                             : int  NA NA NA NA NA NA NA NA NA NA ...
     $ Sab                                 : int  NA NA NA NA NA NA NA NA NA 1 ...
     $ Tab                                 : int  NA NA NA NA NA NA NA NA NA NA ...
     $ icsi                                : Factor w/ 3 levels "","false","true": 1 1 1 2 1 1 1 1 1 1 ...
     $ ivf                                 : Factor w/ 3 levels "","false","true": 1 1 1 2 1 1 1 1 1 1 ...
     $ multipleGestation                   : Factor w/ 3 levels "","false","true": 2 2 1 2 1 2 1 1 1 2 ...
     $ Births                              : int  NA NA NA NA NA NA NA NA NA 3 ...
     $ Gravida                             : int  NA NA NA 1 NA NA NA NA NA 4 ...
     $ Para                                : int  NA NA NA NA NA NA NA NA NA 3 ...
     $ Term                                : int  NA NA NA NA NA NA NA NA NA NA ...
     $ twinNumber                          : Factor w/ 4 levels "","A","B","Other": 1 1 1 1 1 1 1 1 1 1 ...



```R
summary(Var_Prenatal_history)
```


       patient_id     Assisted_Reproduction_Donor_Egg
     Min.   : 71622        :591                      
     1st Qu.:203665   false:178                      
     Median :204428   true :  1                      
     Mean   :178900                                  
     3rd Qu.:205166                                  
     Max.   :246213                                  
                                                     
     Assisted_Reproduction_Donor_Sperm Assisted_Reproduction_Fertility_Meds
          :593                              :582                           
     false:177                         false:174                           
                                       true : 14                           
                                                                           
                                                                           
                                                                           
                                                                           
     Assisted_Reproduction_iui Assisted_Reproduction_Surrogacy   Gestation    
          :592                      :590                       Min.   :24.00  
     false:175                 false:179                       1st Qu.:38.00  
     true :  3                 true :  1                       Median :40.00  
                                                               Mean   :38.78  
                                                               3rd Qu.:40.00  
                                                               Max.   :42.00  
                                                               NA's   :464    
      Maternal_Age    Paternal_Age      Preterm           Sab              Tab     
     Min.   : 0.00   Min.   : 0.00   Min.   :1.000   Min.   : 1.000   Min.   :1    
     1st Qu.: 0.00   1st Qu.: 0.00   1st Qu.:1.000   1st Qu.: 1.000   1st Qu.:1    
     Median : 0.00   Median : 0.00   Median :1.000   Median : 1.000   Median :1    
     Mean   :11.42   Mean   :10.73   Mean   :1.357   Mean   : 1.791   Mean   :1    
     3rd Qu.:27.00   3rd Qu.:27.75   3rd Qu.:2.000   3rd Qu.: 2.000   3rd Qu.:1    
     Max.   :43.00   Max.   :62.00   Max.   :3.000   Max.   :11.000   Max.   :1    
                                     NA's   :742     NA's   :703      NA's   :760  
        icsi        ivf      multipleGestation     Births         Gravida      
          :590        :580        :453         Min.   :1.000   Min.   : 1.000  
     false:175   false:177   false:293         1st Qu.:1.000   1st Qu.: 1.000  
     true :  5   true : 13   true : 24         Median :2.000   Median : 2.000  
                                               Mean   :2.032   Mean   : 2.437  
                                               3rd Qu.:2.000   3rd Qu.: 3.000  
                                               Max.   :6.000   Max.   :16.000  
                                               NA's   :615     NA's   :509     
          Para            Term        twinNumber 
     Min.   :1.000   Min.   : 1.000        :755  
     1st Qu.:1.000   1st Qu.: 1.000   A    :  6  
     Median :2.000   Median : 2.000   B    :  7  
     Mean   :2.056   Mean   : 1.941   Other:  2  
     3rd Qu.:2.500   3rd Qu.: 2.000              
     Max.   :7.000   Max.   :10.000              
     NA's   :575     NA's   :634                 


# Candidate genes


```R
system.time(Var_Candidate_genes <- picsure(env, token, Candidate_genes, gabe = TRUE, verbose = TRUE))
```


```R
str(Var_Candidate_genes)
```


```R
summary(Var_Candidate_genes)
```

## - Candidate genes status


```R
Candidate_genes_status <-"11_Candidate genes/Status"
```


```R
system.time(Var_Candidate_genes_status <- picsure(env, token, Candidate_genes_status, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /11_Candidate genes/Status
    /i2b2-udn/Demo/11_Candidate genes/11_Candidate genes/Status/candidate/
    /i2b2-udn/Demo/11_Candidate genes/11_Candidate genes/Status/carrier/
    /i2b2-udn/Demo/11_Candidate genes/11_Candidate genes/Status/rejected/
    /i2b2-udn/Demo/11_Candidate genes/11_Candidate genes/Status/solved/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #433
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 334 observations of 2 variables. Its size is 4.4 Kb



       user  system elapsed 
      0.644   0.027   8.181 



```R
str(Var_Candidate_genes_status)
```

    'data.frame':	334 obs. of  2 variables:
     $ patient_id: int  203820 203587 203584 204435 203581 204337 204339 71897 204338 203723 ...
     $ Status    : Factor w/ 9 levels "candidate","candidatecarrier",..: 1 5 4 1 1 1 9 1 1 9 ...



```R
summary(Var_Candidate_genes_status)
```


       patient_id                   Status   
     Min.   : 71658   candidate        :213  
     1st Qu.:203870   solved           : 62  
     Median :204492   candidaterejected: 21  
     Mean   :192198   candidatesolved  : 15  
     3rd Qu.:205077   rejected         : 12  
     Max.   :246202   rejectedsolved   :  6  
                      (Other)          :  5  


## - Candidate genes strategy


```R
Candidate_genes_strategy <-"11_Candidate genes/Strategy"
```


```R
system.time(Var_Candidate_genes_strategy <- picsure(env, token, Candidate_genes_strategy, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /11_Candidate genes/Strategy
    /i2b2-udn/Demo/11_Candidate genes/11_Candidate genes/Strategy/common_mutations/
    /i2b2-udn/Demo/11_Candidate genes/11_Candidate genes/Strategy/deletion/
    /i2b2-udn/Demo/11_Candidate genes/11_Candidate genes/Strategy/familial_mutation/
    /i2b2-udn/Demo/11_Candidate genes/11_Candidate genes/Strategy/sequencing/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #435
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 296 observations of 2 variables. Its size is 4.3 Kb



       user  system elapsed 
      0.641   0.029   8.322 



```R
str(Var_Candidate_genes_strategy)
```

    'data.frame':	296 obs. of  2 variables:
     $ patient_id: int  203587 203584 204435 203581 204337 204338 203723 71894 204954 204341 ...
     $ Strategy  : Factor w/ 10 levels "common_mutations",..: 10 10 10 10 7 10 10 10 10 10 ...



```R
summary(Var_Candidate_genes_strategy)
```


       patient_id                                   Strategy  
     Min.   : 71658   sequencing                        :258  
     1st Qu.:203868   deletionsequencing                : 15  
     Median :204550   common_mutationsdeletionsequencing:  6  
     Mean   :191513   deletion                          :  6  
     3rd Qu.:205096   familial_mutationsequencing       :  3  
     Max.   :246202   common_mutationssequencing        :  2  
                      (Other)                           :  6  


# Candidate variants


```R
system.time(Var_Candidate_variants <- picsure(env, token, Candidate_variants, gabe = TRUE, verbose = TRUE))
```


```R
str(Var_Candidate_variants)
```


```R
summary(Var_Candidate_variants)
```

## - Candidate variants interpretation


```R
system.time(Var_Candidate_variants_interpretation <- picsure(env, token, Candidate_variants_interpretation, gabe = TRUE, verbose = TRUE))
```


```R
str(Var_Candidate_variants_interpretation)
```


```R
summary(Var_Candidate_variants_interpretation)
```


```R
table(Var_Candidate_variants_interpretation$x03_Interpretation)
```


```R
as.data.frame(table(Var_Candidate_variants_interpretation$x03_Interpretation))
```

### a. Variant US


```R
Candidate_variants_interpretation_variant_u_s <-"12_Candidate variants/03 Interpretation/variant_u_s"
```


```R
system.time(Var_Candidate_variants_interpretation_variant_u_s <- picsure(env, token, Candidate_variants_interpretation_variant_u_s, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /12_Candidate variants/03 Interpretation/variant_u_s
    /i2b2-udn/Demo/12_Candidate variants/12_Candidate variants/03 Interpretation/variant_u_s/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #437
    
    Waiting for PIC-SURE to return the query
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 164 observations of 2 variables. Its size is 2.8 Kb



       user  system elapsed 
      0.339   0.020   4.713 



```R
str(Var_Candidate_variants_interpretation_variant_u_s)
```

    'data.frame':	164 obs. of  2 variables:
     $ patient_id : int  203820 203584 71919 204435 71917 72091 203581 204115 204338 205766 ...
     $ variant_u_s: chr  "variant_u_s" "variant_u_s" "variant_u_s" "variant_u_s" ...



```R
summary(Var_Candidate_variants_interpretation_variant_u_s)
```


       patient_id     variant_u_s       
     Min.   : 71774   Length:164        
     1st Qu.:203782   Class :character  
     Median :204340   Mode  :character  
     Mean   :188472                     
     3rd Qu.:205017                     
     Max.   :205901                     


### b. Pathogenic


```R
Candidate_variants_interpretation_pathogenic <-"12_Candidate variants/03 Interpretation/pathogenic"
```


```R
system.time(Var_Candidate_variants_interpretation_pathogenic <- picsure(env, token, Candidate_variants_interpretation_pathogenic, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /12_Candidate variants/03 Interpretation/pathogenic
    /i2b2-udn/Demo/12_Candidate variants/12_Candidate variants/03 Interpretation/pathogenic/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #439
    
    Waiting for PIC-SURE to return the query
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 76 observations of 2 variables. Its size is 1.7 Kb



       user  system elapsed 
      0.352   0.013   4.987 



```R
str(Var_Candidate_variants_interpretation_pathogenic)
```

    'data.frame':	76 obs. of  2 variables:
     $ patient_id: int  205080 203587 203584 203665 204253 205008 203581 204007 204339 204338 ...
     $ pathogenic: chr  "pathogenic" "pathogenic" "pathogenic" "pathogenic" ...



```R
summary(Var_Candidate_variants_interpretation_pathogenic)
```


       patient_id      pathogenic       
     Min.   : 71774   Length:76         
     1st Qu.:203914   Class :character  
     Median :204384   Mode  :character  
     Mean   :198135                     
     3rd Qu.:205026                     
     Max.   :246196                     


### c. Likely Pathogenic


```R
Candidate_variants_interpretation_likely_pathogenic <-"12_Candidate variants/03 Interpretation/likely_pathogenic"
```


```R
system.time(Var_Candidate_variants_interpretation_likely_pathogenic <- picsure(env, token, Candidate_variants_interpretation_likely_pathogenic, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /12_Candidate variants/03 Interpretation/likely_pathogenic
    /i2b2-udn/Demo/12_Candidate variants/12_Candidate variants/03 Interpretation/likely_pathogenic/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #441
    
    Waiting for PIC-SURE to return the query
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 33 observations of 2 variables. Its size is 1.2 Kb



       user  system elapsed 
      0.327   0.014   4.447 



```R
str(Var_Candidate_variants_interpretation_likely_pathogenic)
```

    'data.frame':	33 obs. of  2 variables:
     $ patient_id       : int  203587 205262 205788 203774 203849 203603 205154 204156 204014 205771 ...
     $ likely_pathogenic: chr  "likely_pathogenic" "likely_pathogenic" "likely_pathogenic" "likely_pathogenic" ...



```R
summary(Var_Candidate_variants_interpretation_likely_pathogenic)
```


       patient_id     likely_pathogenic 
     Min.   : 71683   Length:33         
     1st Qu.:203876   Class :character  
     Median :204167   Mode  :character  
     Mean   :197666                     
     3rd Qu.:205041                     
     Max.   :246202                     


### d. Likely benign


```R
Candidate_variants_interpretation_likely_benign <-"12_Candidate variants/03 Interpretation/likely_benign"
```


```R
system.time(Var_Candidate_variants_interpretation_likely_benign <- picsure(env, token, Candidate_variants_interpretation_likely_benign, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /12_Candidate variants/03 Interpretation/likely_benign
    /i2b2-udn/Demo/12_Candidate variants/12_Candidate variants/03 Interpretation/likely_benign/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #443
    
    Waiting for PIC-SURE to return the query
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 2 observations of 2 variables. Its size is 0.9 Kb



       user  system elapsed 
      0.375   0.015   5.056 



```R
str(Var_Candidate_variants_interpretation_likely_benign)
```

    'data.frame':	2 obs. of  2 variables:
     $ patient_id   : int  205800 204567
     $ likely_benign: chr  "likely_benign" "likely_benign"



```R
summary(Var_Candidate_variants_interpretation_likely_benign)
```


       patient_id     likely_benign     
     Min.   :204567   Length:2          
     1st Qu.:204875   Class :character  
     Median :205184   Mode  :character  
     Mean   :205184                     
     3rd Qu.:205492                     
     Max.   :205800                     


### d. Benign


```R
Candidate_variants_interpretation_benign <-"12_Candidate variants/03 Interpretation/benign"
```


```R
system.time(Var_Candidate_variants_interpretation_benign <- picsure(env, token, Candidate_variants_interpretation_benign, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /12_Candidate variants/03 Interpretation/benign
    /i2b2-udn/Demo/12_Candidate variants/12_Candidate variants/03 Interpretation/benign/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #445
    
    Waiting for PIC-SURE to return the query
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 1 observations of 2 variables. Its size is 0.8 Kb



       user  system elapsed 
      0.322   0.018   4.384 



```R
str(Var_Candidate_variants_interpretation_benign)
```

    'data.frame':	1 obs. of  2 variables:
     $ patient_id: int 203849
     $ benign    : chr "benign"



```R
summary(Var_Candidate_variants_interpretation_benign)
```


       patient_id        benign         
     Min.   :203849   Length:1          
     1st Qu.:203849   Class :character  
     Median :203849   Mode  :character  
     Mean   :203849                     
     3rd Qu.:203849                     
     Max.   :203849                     


### e. N/A


```R
Candidate_variants_interpretation_NA <-"12_Candidate variants/03 Interpretation/NA"
```


```R
system.time(Var_Candidate_variants_interpretation_NA <- picsure(env, token, Candidate_variants_interpretation_NA, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /12_Candidate variants/03 Interpretation/NA
    /i2b2-udn/Demo/12_Candidate variants/12_Candidate variants/03 Interpretation/NA/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #447
    
    Waiting for PIC-SURE to return the query
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 36 observations of 2 variables. Its size is 1.5 Kb



       user  system elapsed 
      0.325   0.019   4.442 



```R
str(Var_Candidate_variants_interpretation_NA)
```

    'data.frame':	36 obs. of  2 variables:
     $ patient_id        : int  203525 204132 203804 203545 205769 71894 204250 204316 203745 205774 ...
     $ x03_Interpretation: Factor w/ 1 level "": 1 1 1 1 1 1 1 1 1 1 ...



```R
summary(Var_Candidate_variants_interpretation_NA)
```


       patient_id     x03_Interpretation
     Min.   : 71894   :36               
     1st Qu.:203904                     
     Median :204580                     
     Mean   :200961                     
     3rd Qu.:205114                     
     Max.   :205880                     


# Status


```R
system.time(Var_Status <- picsure(env, token, Status, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /13_Status
    /i2b2-udn/Demo/13_Status/13_Status/solved/
    /i2b2-udn/Demo/13_Status/13_Status/unsolved/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #449
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 806 observations of 2 variables. Its size is 7.5 Kb



       user  system elapsed 
      0.499   0.017   6.280 



```R
str(Var_Status)
```

    'data.frame':	806 obs. of  2 variables:
     $ patient_id: int  71897 71893 71894 205059 204192 204190 205055 205056 204859 205050 ...
     $ x13_Status: Factor w/ 2 levels "solved","unsolved": 2 2 2 1 2 2 2 2 2 2 ...



```R
summary(Var_Status)
```


       patient_id        x13_Status 
     Min.   : 71622   solved  :117  
     1st Qu.:203611   unsolved:689  
     Median :204415                 
     Mean   :177492                 
     3rd Qu.:205174                 
     Max.   :246216                 


# Diagnostic


```R
system.time(Var_Diagnostic <- picsure(env, token, Diagnostic, gabe = TRUE, verbose = TRUE))
```

    Hi Guillaume_Mellon thank you for using picsuRe!
    
    Retrieving the selected pathways:
      Using the "find" function of PICSURE
    
    Retrieving all variables associated with: /14_Disorders (in OMIM, from PhenoTips)
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/46,XX SEX REVERSAL 4/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/5,10-METHENYLTETRAHYDROFOLATE SYNTHETASE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/ACHONDROGENESIS, TYPE IA/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/ADRENOLEUKODYSTROPHY/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/ALAGILLE SYNDROME 1/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/ALEXANDER DISEASE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/ALZHEIMER DISEASE 3/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/ANGIOLIPOMATOSIS, FAMILIAL/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/ARTHROGRYPOSIS, DISTAL, WITH IMPAIRED PROPRIOCEPTION AND TOUCH/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/ATTENTION DEFICIT-HYPERACTIVITY DISORDER/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/AU-KLINE SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/BAINBRIDGE-ROPERS SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/BROWN-VIALETTO-VAN LAERE SYNDROME 1/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/CHARCOT-MARIE-TOOTH DISEASE, AXONAL, TYPE 2S/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/CILIARY DYSKINESIA, PRIMARY, 7/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/CINCA SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/COFFIN-LOWRY SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/COFFIN-SIRIS SYNDROME 1/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/COMBINED OXIDATIVE PHOSPHORYLATION DEFICIENCY 20/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/COMBINED OXIDATIVE PHOSPHORYLATION DEFICIENCY 31/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/CONGENITAL DISORDER OF GLYCOSYLATION, TYPE IIj/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/CONGENITAL DISORDER OF GLYCOSYLATION, TYPE IIm/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/CONGENITAL DISORDER OF GLYCOSYLATION, TYPE Ik/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/CONGENITAL HEART DEFECTS, DYSMORPHIC FACIAL FEATURES, AND INTELLECTUAL DEVELOPMENTAL DISORDER/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/CORNELIA DE LANGE SYNDROME 5/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/CYSTIC ANGIOMATOSIS OF BONE, DIFFUSE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/DEAFNESS-INFERTILITY SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/DYSTONIA 28, CHILDHOOD-ONSET/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/DYSTONIA, DOPA-RESPONSIVE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/EHLERS-DANLOS SYNDROME, HYPERMOBILITY TYPE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/EPILEPSY, FOCAL, WITH SPEECH DISORDER AND WITH OR WITHOUT MENTAL RETARDATION/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 17/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 33/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 36/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 4/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 44/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 50/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/FANCONI ANEMIA, COMPLEMENTATION GROUP R/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/FLOATING-HARBOR SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/FRONTOTEMPORAL DEMENTIA AND|OR AMYOTROPHIC LATERAL SCLEROSIS 1/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/GENERALIZED EPILEPSY WITH FEBRILE SEIZURES PLUS, TYPE 2/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/HELSMOORTEL-VAN DER AA SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/HYALINE FIBROMATOSIS SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/HYPOTONIA, INFANTILE, WITH PSYCHOMOTOR RETARDATION AND CHARACTERISTIC FACIES 2/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/HYPOTONIA, INFANTILE, WITH PSYCHOMOTOR RETARDATION AND CHARACTERISTIC FACIES 3/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/LETHAL CONGENITAL CONTRACTURE SYNDROME 7/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/LEUKODYSTROPHY, HYPOMYELINATING, 6/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MANDIBULOFACIAL DYSOSTOSIS, GUION-ALMEIDA TYPE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MARFAN SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MEGALENCEPHALIC LEUKOENCEPHALOPATHY WITH SUBCORTICAL CYSTS 2B, REMITTING, WITH OR WITHOUT MENTAL RETARDATION/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MENTAL RETARDATION, AUTOSOMAL DOMINANT 13/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MENTAL RETARDATION, AUTOSOMAL DOMINANT 5/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MENTAL RETARDATION, X-LINKED 102/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/METABOLIC ENCEPHALOMYOPATHIC CRISES, RECURRENT, WITH RHABDOMYOLYSIS, CARDIAC ARRHYTHMIAS, AND NEURODEGENERATION/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MITOCHONDRIAL DNA DEPLETION SYNDROME 6 (HEPATOCEREBRAL TYPE)/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MUCOPOLYSACCHARIDOSIS, TYPE IIIB/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MUCOPOLYSACCHARIDOSIS, TYPE IIIC/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MUSCULAR DYSTROPHY-DYSTROGLYCANOPATHY (LIMB-GIRDLE), TYPE C, 5/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MYOPATHY, MYOFIBRILLAR, 1/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/MYOPATHY, MYOFIBRILLAR, 8/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/NEURODEGENERATION WITH BRAIN IRON ACCUMULATION 2A/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/NEURODEGENERATION WITH BRAIN IRON ACCUMULATION 3/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/NEURODEVELOPMENTAL DISORDER WITH EPILEPSY, CATARACTS, FEEDING DIFFICULTIES, AND DELAYED BRAIN MYELINATION/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/NEURODEVELOPMENTAL DISORDER WITH HYPOTONIA, SEIZURES, AND ABSENT LANGUAGE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/NEURODEVELOPMENTAL DISORDER WITH OR WITHOUT ANOMALIES OF THE BRAIN, EYE, OR HEART/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/NEUTROPHILIC DERMATOSIS, ACUTE FEBRILE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/PARAGANGLIOMAS 1/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/PONTOCEREBELLAR HYPOPLASIA, TYPE 2D/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/PONTOCEREBELLAR HYPOPLASIA, TYPE 6/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/PSEUDOHYPOPARATHYROIDISM, TYPE IB/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/PSEUDOPSEUDOHYPOPARATHYROIDISM/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/RETT SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/RETT SYNDROME, CONGENITAL VARIANT/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SCHAAF-YANG SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SENIOR-LOKEN SYNDROME 3/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SENIOR-LOKEN SYNDROME 5/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SHASHI-PENA SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SHWACHMAN-DIAMOND SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SJOGREN SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SPASTIC PARAPLEGIA 11, AUTOSOMAL RECESSIVE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SPASTIC PARAPLEGIA 7, AUTOSOMAL RECESSIVE/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SPINAL MUSCULAR ATROPHY, LOWER EXTREMITY-PREDOMINANT, 1, AUTOSOMAL DOMINANT/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SPINOCEREBELLAR ATAXIA 28/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SPINOCEREBELLAR ATAXIA, AUTOSOMAL RECESSIVE 24/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SPINOCEREBELLAR ATAXIA, AUTOSOMAL RECESSIVE 8/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/STORMORKEN SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/SYSTEMIC LUPUS ERYTHEMATOSUS/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/TRICHORHINOPHALANGEAL SYNDROME, TYPE I/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/VAN MALDERGEM SYNDROME 2/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/WIEACKER-WOLFF SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/WIEDEMANN-STEINER SYNDROME/
    /i2b2-udn/Demo/14_Disorders (in OMIM, from PhenoTips)/14_Disorders (in OMIM, from PhenoTips)/WILLIAMS-BEUREN SYNDROME/
    
    Building the "where" part of the query
      default subset = "ALL"
      -> will look for all the patients that have a value for at list one of the variable selected
    
    Building the "select" part of the query
    
    Combining the "select" and "where" part of the query to build the json body
    Exporting the json query to /home/jovyan/work/Guillaume
    
    Getting a result ID
      -> Query #451
    
    Waiting for PIC-SURE to return the query
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      ...still waiting
      Result available \o/
    
    Downloading the data frame
    
    Making the dataframe pretty
      ordering the columns according to the order of the variables you selected
      combining the categorical variables
      making the columns' name pretty
    
    The data.frame downloaded contains 96 observations of 2 variables. Its size is 10.8 Kb



       user  system elapsed 
      8.147   0.181  96.903 



```R
str(Var_Diagnostic)
```

    'data.frame':	96 obs. of  2 variables:
     $ patient_id                             : int  204916 205080 203584 203525 203728 204007 204339 204115 203627 203566 ...
     $ x14_Disorders_.in_OMIM._from_PhenoTips.: Factor w/ 92 levels "46,XX SEX REVERSAL 4",..: 16 39 15 58 31 68 43 4 72 57 ...


# Clinical symptoms and physical findings


```R
system.time(Var_Phenotype <- picsure(env, token, Phenotype, gabe = TRUE, verbose = TRUE))
```

# Medications


```R
system.time(Var_Medications <- picsure(env, token, Medications, gabe = TRUE, verbose = TRUE))
```

# Metabolite


```R
system.time(Var_Metabolites <- picsure(env, token, Metabolites, gabe = TRUE, verbose = TRUE))
```
