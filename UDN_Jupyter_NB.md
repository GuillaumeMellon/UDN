
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

```R
#install_git('https://github.com/hms-dbmi/picsuRe')
library(picsuRe)
```

In order to built table, the user can install 'pander' package typing the following in an R session:


```R
install.packages('pander', repos='http://cran.us.r-project.org')
library(pander)
```

# Step 2 - Define your token

To authenticate with PIC-SURE put your token in an text file in your JupyterNotebook's folder. The token will be read from there so the token does not get seen by anyone except you. You can find the token under the user profile (https://udn.hms.harvard.edu/transmart/user).


```R
token <- as.character(read.table("token.txt", sep=",")[1,1])
```

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
