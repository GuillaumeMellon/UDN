
# The Undiagnosed Disease Network: description of the database

# Introduction

Identifying the exact diagnosis of atypical clinical presentation is sometimes a medical challenge. Such clinical presentation can be simply the fact of common disease with atypical presentations or several diseases with overlapping symptoms. However, these presentations can also lead to undiagnosed disease. Diagnostic wavering is often the cause of a misdiagnosis causing suffering of the patient and behavior often inappropriate of those around him. Thus, more than half of the patients report a degraded quality of life associating physical or psychological prejudices.

A network of rare disease professionals was therefore needed. That's why, the Undiagnosed Disease Network (UDN) had been set up in 2015 to bring together a network of expert centers specializing in complex diagnostics.

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
      '/tmp/RtmpcpyrZD/devtools3b7e4bcce66d/hms-dbmi-picsuRe-cfdda08'  \
      --library='/opt/conda/lib/R/library' --install-tests 
    



```R
#install_git('https://github.com/hms-dbmi/picsuRe')
library(picsuRe)
```

In order to built table, the user can install 'tableone' package typing the following in an R session:


```R
if (!file.exists(Sys.getenv("TAR")))  Sys.setenv(TAR = "/bin/tar")
install_github("kaz-yos/tableone", force = TRUE)
library(tableone)
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
Ethnicity <- "00_Demographics/Ethnicity"
Gender<-"00_Demographics/Gender"
Race<-"00_Demographics/Race"
Age<-"00_Demographics/Current age in years"
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

Here we use the picsure function, to do it we inform the arguments: environment, token and variable's name.

# Ethnicity


```R
system.time(Var_Ethnicity <- picsure(env, token, Ethnicity, gabe = TRUE, verbose = TRUE))
```     

# Gender

```R
system.time(Var_Gender <- picsure(env, token, Gender, gabe = TRUE, verbose = TRUE))
```

# Race


```R
system.time(Var_Race <- picsure(env, token, Race, gabe = TRUE, verbose = TRUE))
```

# Age


```R
system.time(Var_Age <- picsure(env, token, Age, gabe = TRUE, verbose = TRUE))
```

Now, we merge the whole demographics variables to built a dataframe


```R
Tab_000<-(merge(Var_Gender,Var_Ethnicity,all = TRUE))
```


```R
Tab_001<-(merge(Tab_000,Var_Race,all = TRUE))
```

# Primary symptoms


```R
system.time(Var_Primary_symptom <- picsure(env, token, Primary_symptom, gabe = TRUE, verbose = TRUE))
```

```R
Var_Primary_symptom$Primary_symptom<-Var_Primary_symptom$x01_Primary_symptom_category_reported_by_patient_or_caregiver
```

```R
Tab_002<-(merge(Tab_001,Var_Primary_symptom,all = TRUE))
```

# Sequence submitted


```R
system.time(Var_Sequence_Submitted <- picsure(env, token, Sequence_Submitted, gabe = TRUE, verbose = TRUE))
```

```R
Tab_003<-(merge(Tab_002,Var_Sequence_Submitted,all = TRUE))
```

# Type of Sequencing


```R
system.time(Var_Type_sequencing <- picsure(env, token, Type_sequencing, gabe = TRUE, verbose = TRUE))
```

```R
Tab_004<-(merge(Tab_003,Var_Type_sequencing,all = TRUE))
```

# UDN Clinical Site


```R
system.time(Var_Clinical_Site <- picsure(env, token, Clinical_Site, gabe = TRUE, verbose = TRUE))
```

  

```R
Tab_005<-(merge(Tab_004,Var_Clinical_Site,all = TRUE))
```

# Maternal Ethnicity


```R
system.time(Var_Maternal_ethnicity <- picsure(env, token, Maternal_ethnicity, gabe = TRUE, verbose = TRUE))
``` 

```R
Tab_<-(merge(Tab_,Var_Maternal_ethnicity,all = TRUE))
```

# Paternal Ethnicity


```R
system.time(Var_Paternal_ethnicity <- picsure(env, token, Paternal_ethnicity, gabe = TRUE, verbose = TRUE))
```  

```R
Tab_006<-(merge(Tab_005,Var_Paternal_ethnicity,all = TRUE))
```

# Family History


```R
system.time(Var_Family_history <- picsure(env, token, Family_history, gabe = TRUE, verbose = TRUE))
```

```R
Tab_007<-(merge(Tab_006,Var_Family_history,all = TRUE))
```

# Prenatal and perinatal history


```R
system.time(Var_Prenatal_history <- picsure(env, token, Prenatal_history, gabe = TRUE, verbose = TRUE))
```

```R
Tab_<-(merge(Tab_,Var_Prenatal_history,all = TRUE))
```

# Candidate genes

```R
system.time(Var_Candidate_genes <- picsure(env, token, Candidate_genes, gabe = TRUE, verbose = TRUE))
```

 - Candidate genes status

```R
Candidate_genes_status <-"11_Candidate genes/Status"
```

```R
system.time(Var_Candidate_genes_status <- picsure(env, token, Candidate_genes_status, gabe = TRUE, verbose = TRUE))
```

 - Candidate genes strategy

```R
Candidate_genes_strategy <-"11_Candidate genes/Strategy"
```

```R
system.time(Var_Candidate_genes_strategy <- picsure(env, token, Candidate_genes_strategy, gabe = TRUE, verbose = TRUE))
```

# Candidate variants


```R
system.time(Var_Candidate_variants <- picsure(env, token, Candidate_variants, gabe = TRUE, verbose = TRUE))
```

 - Candidate variants interpretation


```R
system.time(Var_Candidate_variants_interpretation <- picsure(env, token, Candidate_variants_interpretation, gabe = TRUE, verbose = TRUE))
```

 a. Variant US


```R
Candidate_variants_interpretation_variant_u_s <-"12_Candidate variants/03 Interpretation/variant_u_s"
```


```R
system.time(Var_Candidate_variants_interpretation_variant_u_s <- picsure(env, token, Candidate_variants_interpretation_variant_u_s, gabe = TRUE, verbose = TRUE))
```

 b. Pathogenic


```R
Candidate_variants_interpretation_pathogenic <-"12_Candidate variants/03 Interpretation/pathogenic"
```

```R
system.time(Var_Candidate_variants_interpretation_pathogenic <- picsure(env, token, Candidate_variants_interpretation_pathogenic, gabe = TRUE, verbose = TRUE))
```

 c. Likely Pathogenic


```R
Candidate_variants_interpretation_likely_pathogenic <-"12_Candidate variants/03 Interpretation/likely_pathogenic"
```


```R
system.time(Var_Candidate_variants_interpretation_likely_pathogenic <- picsure(env, token, Candidate_variants_interpretation_likely_pathogenic, gabe = TRUE, verbose = TRUE))
```

 d. Likely benign


```R
Candidate_variants_interpretation_likely_benign <-"12_Candidate variants/03 Interpretation/likely_benign"
```


```R
system.time(Var_Candidate_variants_interpretation_likely_benign <- picsure(env, token, Candidate_variants_interpretation_likely_benign, gabe = TRUE, verbose = TRUE))
```

 d. Benign


```R
Candidate_variants_interpretation_benign <-"12_Candidate variants/03 Interpretation/benign"
```


```R
system.time(Var_Candidate_variants_interpretation_benign <- picsure(env, token, Candidate_variants_interpretation_benign, gabe = TRUE, verbose = TRUE))
```

 e. N/A


```R
Candidate_variants_interpretation_NA <-"12_Candidate variants/03 Interpretation/NA"
```


```R
system.time(Var_Candidate_variants_interpretation_NA <- picsure(env, token, Candidate_variants_interpretation_NA, gabe = TRUE, verbose = TRUE))
```

# Status


```R
system.time(Var_Status <- picsure(env, token, Status, gabe = TRUE, verbose = TRUE))
```

```R
Tab_008<-(merge(Tab_007,Var_Status,all = TRUE))
```

# Diagnostic


```R
system.time(Var_Diagnostic <- picsure(env, token, Diagnostic, gabe = TRUE, verbose = TRUE))
```

```R
Tab_009<-(merge(Tab_008,Var_Diagnostic,all = TRUE))
```

 Clinical symptoms and physical findings


```R
#system.time(Var_Phenotype <- picsure(env, token, Phenotype, gabe = TRUE, verbose = TRUE))
```

# Medications


```R
#system.time(Var_Medications <- picsure(env, token, Medications, gabe = TRUE, verbose = TRUE))
```

# Metabolite


```R
#system.time(Var_Metabolites <- picsure(env, token, Metabolites, gabe = TRUE, verbose = TRUE))
```

# Step 6 - Analyse your data

Here with the "Tableone" package, we summarize the dataset using.


```R
CreateTableOne(data = Tab_009)
```
                                                                                                                        
                                                                                                                         Overall        
      n                                                                                                                    1055         
      patient_id (mean (sd))                                                                                             528.00 (304.70)
      Gender (%)                                                                                                                        
         Female                                                                                                             539 (51.1)  
         Male                                                                                                               515 (48.8)  
         Other                                                                                                                1 ( 0.1)  
      Ethnicity (%)                                                                                                                     
                                                                                                                              4 ( 0.4)  
         Hispanic or Latino                                                                                                 157 (14.9)  
         Not Hispanic or Latino                                                                                             776 (73.6)  
         UnknownNot Reported Ethnicity                                                                                      118 (11.2)  
      Race (%)                                                                                                                          
         American Indian or Alaska Native                                                                                     2 ( 0.2)  
         American Indian or Alaska NativeAsian                                                                                1 ( 0.1)  
         American Indian or Alaska NativeAsianBlack or African AmericanNative Hawaiian or Other Pacific Islander              1 ( 0.1)  
         American Indian or Alaska NativeNative Hawaiian or Other Pacific IslanderWhite                                       1 ( 0.1)  
         American Indian or Alaska NativeOther                                                                                1 ( 0.1)  
         American Indian or Alaska NativeWhite                                                                                9 ( 0.9)  
         Asian                                                                                                               59 ( 5.7)  
         AsianBlack or African American                                                                                       1 ( 0.1)  
         AsianNative Hawaiian or Other Pacific IslanderOtherWhite                                                             1 ( 0.1)  
         AsianNative Hawaiian or Other Pacific IslanderWhite                                                                  3 ( 0.3)  
         AsianOther                                                                                                           1 ( 0.1)  
         AsianWhite                                                                                                          12 ( 1.2)  
         Black or African American                                                                                           54 ( 5.2)  
         Black or African AmericanOther                                                                                       1 ( 0.1)  
         Black or African AmericanOtherWhite                                                                                  3 ( 0.3)  
         Black or African AmericanWhite                                                                                       4 ( 0.4)  
         Native Hawaiian or Other Pacific Islander                                                                            1 ( 0.1)  
         Native Hawaiian or Other Pacific IslanderOther                                                                       1 ( 0.1)  
         Native Hawaiian or Other Pacific IslanderWhite                                                                       1 ( 0.1)  
         Other                                                                                                               72 ( 7.0)  
         OtherWhite                                                                                                           6 ( 0.6)  
         White                                                                                                              800 (77.3)  
      x01_Primary_symptom_category_reported_by_patient_or_caregiver (%)                                                                 
                                                                                                                             28 ( 2.7)  
         Allergies and Disorders of The Immune System                                                                        53 ( 5.0)  
         Cardiology and vascular conditions (heart, artery, vein, and lymph disorders)                                       48 ( 4.6)  
         Dentistry and craniofacial (bones of head and face)                                                                  7 ( 0.7)  
         Dermatology (skin diseases and disorders)                                                                           10 ( 0.9)  
         Endocrinology (disorder of the endocrine glands and hormones)                                                       27 ( 2.6)  
         Gastroenterology (disorder of the stomach and intestines)                                                           41 ( 3.9)  
         Gynecology and reproductive medicine                                                                                 2 ( 0.2)  
         Hematology (blood diseases and disorders)                                                                           16 ( 1.5)  
         Infectious diseases                                                                                                  2 ( 0.2)  
         Musculoskeletal and orthopedics (structural and functional disorders of muscles, bones, and joints)                124 (11.8)  
         Nephrology (kidney diseases and disorders)                                                                          17 ( 1.6)  
         Neurology (disorders of the nervous system, including brain and spinal cord)                                       495 (47.0)  
         Oncology (Tumors and cancer)                                                                                         3 ( 0.3)  
         Ophthalmology (Eye disorders and diseases)                                                                          11 ( 1.0)  
         Other                                                                                                              107 (10.2)  
         Psychiatry                                                                                                           6 ( 0.6)  
         Pulmonology (Lung disorders and diseases)                                                                           20 ( 1.9)  
         Rheumatology (immune disorders of the joints, muscles, and ligaments)                                               35 ( 3.3)  
         Urology                                                                                                              1 ( 0.1)  
      Primary_symptom (%)                                                                                                               
                                                                                                                             28 ( 2.7)  
         Allergies and Disord                                                                                                53 ( 5.0)  
         Cardiology and vascu                                                                                                48 ( 4.6)  
         Dentistry and cranio                                                                                                 7 ( 0.7)  
         Dermatology (skin di                                                                                                10 ( 0.9)  
         Endocrinology (disor                                                                                                27 ( 2.6)  
         Gastroenterology (di                                                                                                41 ( 3.9)  
         Gynecology and repro                                                                                                 2 ( 0.2)  
         Hematology (blood di                                                                                                16 ( 1.5)  
         Infectious diseases                                                                                                  2 ( 0.2)  
         Musculoskeletal and                                                                                                124 (11.8)  
         Nephrology (kidney d                                                                                                17 ( 1.6)  
         Neurology (disorders                                                                                               495 (47.0)  
         Oncology (Tumors and                                                                                                 3 ( 0.3)  
         Ophthalmology (Eye d                                                                                                11 ( 1.0)  
         Other                                                                                                              107 (10.2)  
         Psychiatry                                                                                                           6 ( 0.6)  
         Pulmonology (Lung di                                                                                                20 ( 1.9)  
         Rheumatology (immune                                                                                                35 ( 3.3)  
         Urology                                                                                                              1 ( 0.1)  
      x02_Sequence_Submitted = true (%)                                                                                     685 (98.7)  
      x02_Type_of_sequencing (%)                                                                                                        
         Targeted Variant                                                                                                     3 ( 0.4)  
         Transcriptome                                                                                                        1 ( 0.1)  
         Whole Exome                                                                                                        346 (49.9)  
         Whole Genome                                                                                                       344 (49.6)  
      x03_UDN_Clinical_Site (%)                                                                                                         
         baylor                                                                                                             160 (15.2)  
         duke                                                                                                               144 (13.6)  
         harvard-affiliate                                                                                                  117 (11.1)  
         nih                                                                                                                266 (25.2)  
         stanford                                                                                                           137 (13.0)  
         ucla                                                                                                               109 (10.3)  
         vanderbilt                                                                                                         122 (11.6)  
      x06_Paternal_ethnicity_.from_PhenoTips. (%)                                                                                       
         Afghanistan                                                                                                          1 ( 0.2)  
         African                                                                                                              1 ( 0.2)  
         African American                                                                                                    14 ( 2.5)  
         African AmericanLumbee                                                                                               1 ( 0.2)  
         African Americans                                                                                                    5 ( 0.9)  
         African Americanscaucasian                                                                                           1 ( 0.2)  
         African AmericansCaucasianNative American                                                                            1 ( 0.2)  
         African AmericansNative American                                                                                     1 ( 0.2)  
         AfricanGermanItalian                                                                                                 1 ( 0.2)  
         Aleutian IndianGerman                                                                                                1 ( 0.2)  
         Arabs                                                                                                                1 ( 0.2)  
         ArgentinaAshkenazi JewishPolishRussian                                                                               1 ( 0.2)  
         Armenian                                                                                                             1 ( 0.2)  
         ArmenianRussian                                                                                                      1 ( 0.2)  
         Ashkenazi Jewish                                                                                                     7 ( 1.3)  
         Ashkenazi Jewish (25%)EnglishFrench (not French Canadian)Italian (25%)Native AmericanNorwegian                       1 ( 0.2)  
         Ashkenazi JewishDutch                                                                                                1 ( 0.2)  
         Ashkenazi JewishEastern European                                                                                     1 ( 0.2)  
         Ashkenazi JewishEuropean Americans                                                                                   1 ( 0.2)  
         Ashkenazi JewishPolish                                                                                               1 ( 0.2)  
         Ashkenazi JewishRussian                                                                                              1 ( 0.2)  
         Ashkenazi JewsRussian                                                                                                1 ( 0.2)  
         AshkenaziJewishRussian                                                                                               1 ( 0.2)  
         Asian Indian                                                                                                         8 ( 1.4)  
         Asian IndianItalian                                                                                                  1 ( 0.2)  
         AustrianItalian                                                                                                      1 ( 0.2)  
         AustriansHungariansPolish                                                                                            1 ( 0.2)  
         BolivianEuropean Caucasian                                                                                           1 ( 0.2)  
         British                                                                                                              1 ( 0.2)  
         BritishGerman                                                                                                        1 ( 0.2)  
         BritishNorthern Scottish                                                                                             1 ( 0.2)  
         BritishWelshEnglish                                                                                                  1 ( 0.2)  
         Canary islandCzech                                                                                                   1 ( 0.2)  
         Caucasian                                                                                                           19 ( 3.4)  
         CaucasianHispanic                                                                                                    1 ( 0.2)  
         Cherokee IndianIrish                                                                                                 1 ( 0.2)  
         CherokeeDutch                                                                                                        1 ( 0.2)  
         CherokeeEnglishIrish                                                                                                 1 ( 0.2)  
         CherokeeNon JewishWestern European                                                                                   1 ( 0.2)  
         CherokeePolish                                                                                                       1 ( 0.2)  
         Chinese                                                                                                              5 ( 0.9)  
         Columbia                                                                                                             1 ( 0.2)  
         CroatsHispanic-Mexico                                                                                                1 ( 0.2)  
         CroatsItalianNorwegian                                                                                               1 ( 0.2)  
         Czech                                                                                                                1 ( 0.2)  
         CzechEnglishIrishScottish                                                                                            1 ( 0.2)  
         CzechGermanPolish                                                                                                    1 ( 0.2)  
         CzechoslovakianEnglish                                                                                               1 ( 0.2)  
         DanishEnglishIrish                                                                                                   1 ( 0.2)  
         DanishNorthern European                                                                                              1 ( 0.2)  
         DenmarkIrishScottishSwedes                                                                                           1 ( 0.2)  
         Dutch                                                                                                                1 ( 0.2)  
         DutchEnglish                                                                                                         1 ( 0.2)  
         DutchEnglishItalian                                                                                                  1 ( 0.2)  
         DutchFrench Canadian                                                                                                 1 ( 0.2)  
         DutchGerman                                                                                                          1 ( 0.2)  
         DutchGermanMexicanNative AmericanSpanish                                                                             1 ( 0.2)  
         DutchHungarian                                                                                                       1 ( 0.2)  
         DutchIrish                                                                                                           1 ( 0.2)  
         Eastern European                                                                                                     1 ( 0.2)  
         Ecuador                                                                                                              1 ( 0.2)  
         Ecuadorian                                                                                                           1 ( 0.2)  
         Egyptian                                                                                                             2 ( 0.4)  
         El Salvador                                                                                                          3 ( 0.5)  
         El Salvador (South)                                                                                                  1 ( 0.2)  
         El SalvadorNative American                                                                                           1 ( 0.2)  
         English                                                                                                              9 ( 1.6)  
         EnglishCzechoslovakian                                                                                               1 ( 0.2)  
         EnglishEuropean Caucasian                                                                                            1 ( 0.2)  
         EnglishFrench Canadian                                                                                               1 ( 0.2)  
         EnglishGerman                                                                                                        3 ( 0.5)  
         EnglishGermanHungarian                                                                                               1 ( 0.2)  
         EnglishGermanIrish                                                                                                   5 ( 0.9)  
         EnglishGermanIrishNative AmericanSwedish                                                                             1 ( 0.2)  
         EnglishGermanIrishNo known Jewish ancestry                                                                           1 ( 0.2)  
         EnglishGermanIrishScottish                                                                                           1 ( 0.2)  
         EnglishGermanIrishSwiss                                                                                              1 ( 0.2)  
         EnglishGermanNative American                                                                                         1 ( 0.2)  
         EnglishGermanNo known Ashkenazi Jewish ancestry                                                                      1 ( 0.2)  
         EnglishGermanNon Jewish                                                                                              1 ( 0.2)  
         EnglishGermanNon JewishSwiss                                                                                         1 ( 0.2)  
         EnglishGermanPossible distant Ashkenazi Jewish ancestry                                                              1 ( 0.2)  
         EnglishGermans                                                                                                       1 ( 0.2)  
         EnglishGermansIrish                                                                                                  1 ( 0.2)  
         EnglishHungarianIrishPolish                                                                                          1 ( 0.2)  
         EnglishIrish                                                                                                         6 ( 1.1)  
         EnglishIrishItalian                                                                                                  1 ( 0.2)  
         EnglishIrishNative American                                                                                          1 ( 0.2)  
         EnglishIrishNon Jewish                                                                                               1 ( 0.2)  
         EnglishIrishScottish                                                                                                 4 ( 0.7)  
         EnglishNative American                                                                                               1 ( 0.2)  
         EnglishNon Jewish                                                                                                    2 ( 0.4)  
         EnglishNon JewishSicilian                                                                                            1 ( 0.2)  
         EnglishNorwegian                                                                                                     1 ( 0.2)  
         EnglishOther unknownSicilian                                                                                         1 ( 0.2)  
         EnglishPolish                                                                                                        1 ( 0.2)  
         EnglishScandanavian                                                                                                  1 ( 0.2)  
         EnglishScottish                                                                                                      2 ( 0.4)  
         EnglishSeminole Native American                                                                                      1 ( 0.2)  
         EnglishSwedish                                                                                                       1 ( 0.2)  
         Ethiopian                                                                                                            2 ( 0.4)  
         European Americans                                                                                                   5 ( 0.9)  
         European AmericansIrish                                                                                              1 ( 0.2)  
         European Caucasian                                                                                                  17 ( 3.1)  
         European CaucasianFilipino                                                                                           1 ( 0.2)  
         Filipino                                                                                                             1 ( 0.2)  
         FilipinoIrish                                                                                                        1 ( 0.2)  
         FinnishFrench Canadian                                                                                               1 ( 0.2)  
         FinnishItalianMexicanNative AmericanSpanish                                                                          1 ( 0.2)  
         FinnishNon Jewish                                                                                                    1 ( 0.2)  
         Flemish                                                                                                              1 ( 0.2)  
         FrancePortugalUnited Kingdom                                                                                         1 ( 0.2)  
         French Canadian                                                                                                      1 ( 0.2)  
         French CanadianIrishNative AmericanScottish                                                                          1 ( 0.2)  
         French CanadianIrishScottish                                                                                         1 ( 0.2)  
         French CanadianIrishUkrainian                                                                                        1 ( 0.2)  
         French CanadianNative American                                                                                       1 ( 0.2)  
         French CanadianPolish                                                                                                1 ( 0.2)  
         French-Canadian                                                                                                      1 ( 0.2)  
         FrenchGermanWelshEnglish                                                                                             1 ( 0.2)  
         german                                                                                                               1 ( 0.2)  
         German                                                                                                              18 ( 3.2)  
         GermanHispanic                                                                                                       1 ( 0.2)  
         GermanHungarian                                                                                                      1 ( 0.2)  
         GermanHungarianNon Jewish                                                                                            1 ( 0.2)  
         germanIrish                                                                                                          1 ( 0.2)  
         Germanirish                                                                                                          1 ( 0.2)  
         GermanIrish                                                                                                          5 ( 0.9)  
         GermanIrishItalianNative American - Cherokee                                                                         1 ( 0.2)  
         GermanIrishItalianNo known Jewish ancestry                                                                           1 ( 0.2)  
         GermanIrishNative AmericanNo known Ashkenazi Jewish ancestry                                                         1 ( 0.2)  
         GermanIrishScottish                                                                                                  2 ( 0.4)  
         GermanIrishSwedish                                                                                                   1 ( 0.2)  
         GermanItalian                                                                                                        1 ( 0.2)  
         GermanItalianNon Jewish                                                                                              1 ( 0.2)  
         GermanItalianPolish                                                                                                  1 ( 0.2)  
         GermanItalianScottish                                                                                                1 ( 0.2)  
         GermanMexican                                                                                                        1 ( 0.2)  
         GermanNative American                                                                                                1 ( 0.2)  
         Germannative americanScottish                                                                                        1 ( 0.2)  
         GermanNo known Jewish ancestryPolish                                                                                 1 ( 0.2)  
         GermanNon Jewish                                                                                                     2 ( 0.4)  
         GermanNon JewishNorwegianSwedish                                                                                     1 ( 0.2)  
         GermanNorwegianRussian                                                                                               1 ( 0.2)  
         GermanPolish                                                                                                         2 ( 0.4)  
         GermanPortuguese                                                                                                     2 ( 0.4)  
         GermanRussian                                                                                                        1 ( 0.2)  
         Germans                                                                                                              6 ( 1.1)  
         GermanScottish                                                                                                       2 ( 0.4)  
         GermansIrish                                                                                                         5 ( 0.9)  
         GermansIrishNative American                                                                                          1 ( 0.2)  
         GermansItalian                                                                                                       1 ( 0.2)  
         GermansNative American                                                                                               2 ( 0.4)  
         GermansNorwegian                                                                                                     1 ( 0.2)  
         GermanSwedish                                                                                                        1 ( 0.2)  
         GermanSwiss                                                                                                          1 ( 0.2)  
         GermanWelsh                                                                                                          1 ( 0.2)  
         Guatemalan                                                                                                           2 ( 0.4)  
         hispanic                                                                                                             1 ( 0.2)  
         Hispanic                                                                                                             7 ( 1.3)  
         Hispanic - Mexico                                                                                                    2 ( 0.4)  
         Hispanic-Mexico                                                                                                     27 ( 4.8)  
         HungarianCaucasianEnglishIrish                                                                                       1 ( 0.2)  
         India                                                                                                                1 ( 0.2)  
         indian                                                                                                               1 ( 0.2)  
         Irani                                                                                                                1 ( 0.2)  
         Iranian                                                                                                              3 ( 0.5)  
         IranianSephardic Jewish                                                                                              1 ( 0.2)  
         IraniJewish                                                                                                          1 ( 0.2)  
         Iraqi                                                                                                                1 ( 0.2)  
         Irish                                                                                                               16 ( 2.9)  
         IrishCherokee IndianChoctaw IndianGermanScottish                                                                     1 ( 0.2)  
         IrishGerman                                                                                                          1 ( 0.2)  
         IrishItalian                                                                                                         2 ( 0.4)  
         IrishItalianNative American                                                                                          1 ( 0.2)  
         IrishMexicanWelsh                                                                                                    1 ( 0.2)  
         IrishNative American                                                                                                 2 ( 0.4)  
         IrishNon Jewish                                                                                                      1 ( 0.2)  
         IrishNorwegian                                                                                                       1 ( 0.2)  
         IrishPuerto Rican                                                                                                    1 ( 0.2)  
         IrishScottish                                                                                                        6 ( 1.1)  
         Italian                                                                                                              6 ( 1.1)  
         ItalianMexican                                                                                                       1 ( 0.2)  
         ItalianNative American                                                                                               2 ( 0.4)  
         ItalianNo known Ashkenazi Jewish ancestryPolish                                                                      1 ( 0.2)  
         ItalianPolish                                                                                                        1 ( 0.2)  
         ItalianPortuguese                                                                                                    1 ( 0.2)  
         Italians                                                                                                             1 ( 0.2)  
         ItalianSicilian                                                                                                      1 ( 0.2)  
         ItaliansScottish                                                                                                     1 ( 0.2)  
         Italy                                                                                                                1 ( 0.2)  
         Japanese                                                                                                             4 ( 0.7)  
         Japenese                                                                                                             1 ( 0.2)  
         Jewish                                                                                                               1 ( 0.2)  
         JewishPolishRussian                                                                                                  2 ( 0.4)  
         JewishTurkish                                                                                                        1 ( 0.2)  
         Kazakhstani                                                                                                          1 ( 0.2)  
         Korean                                                                                                               4 ( 0.7)  
         Lebanese                                                                                                             1 ( 0.2)  
         LebaneseMixed European                                                                                               1 ( 0.2)  
         Mexican                                                                                                             26 ( 4.7)  
         MexicanMixed EuropeanPuerto Rican                                                                                    1 ( 0.2)  
         MexicanPuerto RicanSpanish                                                                                           1 ( 0.2)  
         MexicanSpanish                                                                                                       1 ( 0.2)  
         Mexico                                                                                                               3 ( 0.5)  
         michoacan                                                                                                            1 ( 0.2)  
         Mixed European                                                                                                       1 ( 0.2)  
         Muhajir (Pakistan)                                                                                                   1 ( 0.2)  
         MXGuerrero                                                                                                           1 ( 0.2)  
         Native American                                                                                                      1 ( 0.2)  
         Native American - CherokeePolish                                                                                     1 ( 0.2)  
         Netherlands                                                                                                          2 ( 0.4)  
         Nigerian                                                                                                             1 ( 0.2)  
         Non Jewish                                                                                                           1 ( 0.2)  
         Non JewishNorthern European                                                                                          2 ( 0.4)  
         Non JewishScottish                                                                                                   1 ( 0.2)  
         Non JewishTurkish                                                                                                    2 ( 0.4)  
         Non-Hispanicunknown                                                                                                  1 ( 0.2)  
         Northern European                                                                                                   11 ( 2.0)  
         Northern EuropeanRussian                                                                                             1 ( 0.2)  
         Northern EuropeanScottish                                                                                            1 ( 0.2)  
         Northern India                                                                                                       2 ( 0.4)  
         Norwegian                                                                                                            1 ( 0.2)  
         Norwegians                                                                                                           1 ( 0.2)  
         NorwegiansSwiss                                                                                                      1 ( 0.2)  
         NorwegianSwedish                                                                                                     1 ( 0.2)  
         pakistan                                                                                                             1 ( 0.2)  
         Pakistan                                                                                                             3 ( 0.5)  
         Palestinian                                                                                                          1 ( 0.2)  
         Panamanian                                                                                                           1 ( 0.2)  
         Pennsylvania Dutch                                                                                                   1 ( 0.2)  
         Pennsylvania German                                                                                                  1 ( 0.2)  
         Phillipino                                                                                                           2 ( 0.4)  
         PhillipinoPolynesiansWhite                                                                                           1 ( 0.2)  
         Polish                                                                                                               5 ( 0.9)  
         PolishRussian                                                                                                        1 ( 0.2)  
         PolishScottish                                                                                                       1 ( 0.2)  
         Portuguese                                                                                                           2 ( 0.4)  
         Puerto Rican                                                                                                         1 ( 0.2)  
         Puerto Rico                                                                                                          1 ( 0.2)  
         Puerto-Rican                                                                                                         1 ( 0.2)  
         Punjabis                                                                                                             1 ( 0.2)  
         Romanian                                                                                                             2 ( 0.4)  
         Russian                                                                                                              1 ( 0.2)  
         Russian Jewish                                                                                                       1 ( 0.2)  
         ScandanavianScottishSwedish                                                                                          1 ( 0.2)  
         Scandinavian                                                                                                         1 ( 0.2)  
         Scottish                                                                                                             2 ( 0.4)  
         ScottishAfrican AmericanIndo-Trinidadian                                                                             1 ( 0.2)  
         Sephardic Jewish                                                                                                     1 ( 0.2)  
         Serbian                                                                                                              1 ( 0.2)  
         Serbs                                                                                                                1 ( 0.2)  
         Sicilian                                                                                                             1 ( 0.2)  
         SicilianIrishItalian                                                                                                 1 ( 0.2)  
         South African                                                                                                        1 ( 0.2)  
         South AfricanSwedish                                                                                                 1 ( 0.2)  
         South American (Colombia)                                                                                            1 ( 0.2)  
         South India                                                                                                          1 ( 0.2)  
         South Korean                                                                                                         1 ( 0.2)  
         Spain                                                                                                                1 ( 0.2)  
         SpainCalatayud                                                                                                       1 ( 0.2)  
         Spaniards                                                                                                            1 ( 0.2)  
         Spanish                                                                                                              1 ( 0.2)  
         Sri Lanka                                                                                                            1 ( 0.2)  
         SwissEnglishScottish                                                                                                 1 ( 0.2)  
         Taiwanese                                                                                                            1 ( 0.2)  
         unknown                                                                                                              6 ( 1.1)  
         Unknown                                                                                                              3 ( 0.5)  
         UNKNOWN                                                                                                              1 ( 0.2)  
         Urkraine                                                                                                             1 ( 0.2)  
         Vietnamese                                                                                                           2 ( 0.4)  
         Welsh                                                                                                                1 ( 0.2)  
         Western European                                                                                                     4 ( 0.7)  
         White                                                                                                               13 ( 2.3)  
         yemen                                                                                                                2 ( 0.4)  
      Affected_Relatives (%)                                                                                                            
                                                                                                                            124 (20.6)  
         false                                                                                                              374 (62.2)  
         true                                                                                                               103 (17.1)  
      Consanguinity (%)                                                                                                                 
                                                                                                                             24 ( 4.0)  
         false                                                                                                              558 (92.8)  
         true                                                                                                                19 ( 3.2)  
      Miscarriages (%)                                                                                                                  
                                                                                                                             75 (12.5)  
         false                                                                                                              491 (81.7)  
         true                                                                                                                35 ( 5.8)  
      x13_Status = unsolved (%)                                                                                             896 (87.7)  
      x14_Disorders_.in_OMIM._from_PhenoTips. (%)                                                                                       
         46,XX SEX REVERSAL 4                                                                                                 1 ( 0.9)  
         5,10-METHENYLTETRAHYDROFOLATE SYNTHETASE                                                                             1 ( 0.9)  
         ACHONDROGENESIS, TYPE IA                                                                                             1 ( 0.9)  
         ADRENOLEUKODYSTROPHY                                                                                                 1 ( 0.9)  
         ALAGILLE SYNDROME 1                                                                                                  1 ( 0.9)  
         ALEXANDER DISEASE                                                                                                    1 ( 0.9)  
         ALZHEIMER DISEASE 3                                                                                                  1 ( 0.9)  
         ARTHROGRYPOSIS, DISTAL, WITH IMPAIRED PROPRIOCEPTION AND TOUCH                                                       1 ( 0.9)  
         ATTENTION DEFICIT-HYPERACTIVITY DISORDER                                                                             1 ( 0.9)  
         AU-KLINE SYNDROME                                                                                                    1 ( 0.9)  
         BAINBRIDGE-ROPERS SYNDROME                                                                                           1 ( 0.9)  
         BETHLEM MYOPATHY 1                                                                                                   2 ( 1.8)  
         BROWN-VIALETTO-VAN LAERE SYNDROME 1                                                                                  1 ( 0.9)  
         CEREBELLAR ATAXIA, AREFLEXIA, PES CAVUS, OPTIC ATROPHY, AND SENSORINEURAL HEARING LOSS                               1 ( 0.9)  
         CHARCOT-MARIE-TOOTH DISEASE, AXONAL, TYPE 2S                                                                         1 ( 0.9)  
         CHROMOSOME 16p12.1 DELETION SYNDROME, 520-KB                                                                         1 ( 0.9)  
         CHROMOSOME 1q21.1 DUPLICATION SYNDROME                                                                               1 ( 0.9)  
         CILIARY DYSKINESIA, PRIMARY, 7                                                                                       1 ( 0.9)  
         CINCA SYNDROME                                                                                                       1 ( 0.9)  
         COFFIN-LOWRY SYNDROME                                                                                                1 ( 0.9)  
         COFFIN-SIRIS SYNDROME 1                                                                                              1 ( 0.9)  
         COMBINED OXIDATIVE PHOSPHORYLATION DEFICIENCY 20                                                                     1 ( 0.9)  
         COMBINED OXIDATIVE PHOSPHORYLATION DEFICIENCY 31                                                                     2 ( 1.8)  
         CONGENITAL DISORDER OF GLYCOSYLATION, TYPE IIj                                                                       1 ( 0.9)  
         CONGENITAL DISORDER OF GLYCOSYLATION, TYPE IIm                                                                       1 ( 0.9)  
         CONGENITAL DISORDER OF GLYCOSYLATION, TYPE Ik                                                                        1 ( 0.9)  
         CONGENITAL HEART DEFECTS, DYSMORPHIC FACIAL FEATURES, AND INTELLECTUAL DEVELOPMENTAL DISORDER                        1 ( 0.9)  
         CORNELIA DE LANGE SYNDROME 5                                                                                         1 ( 0.9)  
         CORTICAL DYSPLASIA, COMPLEX, WITH OTHER BRAIN MALFORMATIONS 3                                                        1 ( 0.9)  
         CYSTIC ANGIOMATOSIS OF BONE, DIFFUSE                                                                                 1 ( 0.9)  
         DEAFNESS-INFERTILITY SYNDROME                                                                                        1 ( 0.9)  
         DYSTONIA 28, CHILDHOOD-ONSET                                                                                         1 ( 0.9)  
         DYSTONIA, DOPA-RESPONSIVE                                                                                            1 ( 0.9)  
         EHLERS-DANLOS SYNDROME, HYPERMOBILITY TYPE                                                                           1 ( 0.9)  
         EPILEPSY, FOCAL, WITH SPEECH DISORDER AND WITH OR WITHOUT MENTAL RETARDATION                                         1 ( 0.9)  
         EPILEPSY, HEARING LOSS, AND MENTAL RETARDATION SYNDROME                                                              1 ( 0.9)  
         EPILEPSY, PROGRESSIVE MYOCLONIC 7                                                                                    1 ( 0.9)  
         EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 17                                                                        1 ( 0.9)  
         EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 33                                                                        1 ( 0.9)  
         EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 35                                                                        1 ( 0.9)  
         EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 36                                                                        1 ( 0.9)  
         EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 4                                                                         1 ( 0.9)  
         EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 44                                                                        1 ( 0.9)  
         EPILEPTIC ENCEPHALOPATHY, EARLY INFANTILE, 50                                                                        1 ( 0.9)  
         EPILEPTIC ENCEPHALOPATHY, INFANTILE OR EARLY CHILDHOOD, 2                                                            1 ( 0.9)  
         FANCONI ANEMIA, COMPLEMENTATION GROUP R                                                                              1 ( 0.9)  
         FLOATING-HARBOR SYNDROME                                                                                             1 ( 0.9)  
         FRONTOTEMPORAL DEMENTIA ANDOR AMYOTROPHIC LATERAL SCLEROSIS 1                                                        1 ( 0.9)  
         GASTRIC CANCER, HEREDITARY DIFFUSE                                                                                   1 ( 0.9)  
         GENERALIZED EPILEPSY WITH FEBRILE SEIZURES PLUS, TYPE 2                                                              1 ( 0.9)  
         HELSMOORTEL-VAN DER AA SYNDROME                                                                                      1 ( 0.9)  
         HYALINE FIBROMATOSIS SYNDROME                                                                                        1 ( 0.9)  
         HYPOTONIA, INFANTILE, WITH PSYCHOMOTOR RETARDATION AND CHARACTERISTIC FACIES 2                                       1 ( 0.9)  
         HYPOTONIA, INFANTILE, WITH PSYCHOMOTOR RETARDATION AND CHARACTERISTIC FACIES 3                                       1 ( 0.9)  
         LETHAL CONGENITAL CONTRACTURE SYNDROME 7                                                                             1 ( 0.9)  
         LEUKODYSTROPHY, HYPOMYELINATING, 6                                                                                   1 ( 0.9)  
         MANDIBULOFACIAL DYSOSTOSIS, GUION-ALMEIDA TYPE                                                                       1 ( 0.9)  
         MARFAN SYNDROME                                                                                                      1 ( 0.9)  
         MEGALENCEPHALIC LEUKOENCEPHALOPATHY WITH SUBCORTICAL CYSTS 2B, REMITTING, WITH OR WITHOUT MENTAL RETARDATION         1 ( 0.9)  
         MENTAL RETARDATION, AUTOSOMAL DOMINANT 13                                                                            1 ( 0.9)  
         MENTAL RETARDATION, AUTOSOMAL DOMINANT 26                                                                            1 ( 0.9)  
         MENTAL RETARDATION, AUTOSOMAL DOMINANT 5                                                                             1 ( 0.9)  
         MENTAL RETARDATION, X-LINKED 102                                                                                     1 ( 0.9)  
         METABOLIC ENCEPHALOMYOPATHIC CRISES, RECURRENT, WITH RHABDOMYOLYSIS, CARDIAC ARRHYTHMIAS, AND NEURODEGENERATION      1 ( 0.9)  
         MITOCHONDRIAL COMPLEX II DEFICIENCY                                                                                  1 ( 0.9)  
         MITOCHONDRIAL DNA DEPLETION SYNDROME 6 (HEPATOCEREBRAL TYPE)                                                         1 ( 0.9)  
         MUCOPOLYSACCHARIDOSIS, TYPE IIIB                                                                                     1 ( 0.9)  
         MUCOPOLYSACCHARIDOSIS, TYPE IIIC                                                                                     1 ( 0.9)  
         MUSCULAR DYSTROPHY-DYSTROGLYCANOPATHY (LIMB-GIRDLE), TYPE C, 5                                                       1 ( 0.9)  
         MYOPATHY, MYOFIBRILLAR, 1                                                                                            1 ( 0.9)  
         MYOPATHY, MYOFIBRILLAR, 8                                                                                            1 ( 0.9)  
         NEURODEGENERATION WITH BRAIN IRON ACCUMULATION 2A                                                                    2 ( 1.8)  
         NEURODEGENERATION WITH BRAIN IRON ACCUMULATION 3                                                                     1 ( 0.9)  
         NEURODEVELOPMENTAL DISORDER WITH EPILEPSY, CATARACTS, FEEDING DIFFICULTIES, AND DELAYED BRAIN MYELINATION            1 ( 0.9)  
         NEURODEVELOPMENTAL DISORDER WITH HYPOTONIA, SEIZURES, AND ABSENT LANGUAGE                                            1 ( 0.9)  
         NEURODEVELOPMENTAL DISORDER WITH OR WITHOUT ANOMALIES OF THE BRAIN, EYE, OR HEART                                    1 ( 0.9)  
         NEUTROPHILIC DERMATOSIS, ACUTE FEBRILE                                                                               1 ( 0.9)  
         PARAGANGLIOMAS 1                                                                                                     1 ( 0.9)  
         PONTOCEREBELLAR HYPOPLASIA, TYPE 2D                                                                                  1 ( 0.9)  
         PONTOCEREBELLAR HYPOPLASIA, TYPE 6                                                                                   1 ( 0.9)  
         PORETTI-BOLTSHAUSER SYNDROME                                                                                         1 ( 0.9)  
         PSEUDOHYPOPARATHYROIDISM, TYPE IB                                                                                    1 ( 0.9)  
         PSEUDOPSEUDOHYPOPARATHYROIDISM                                                                                       1 ( 0.9)  
         RETT SYNDROME                                                                                                        3 ( 2.7)  
         RETT SYNDROME, CONGENITAL VARIANT                                                                                    1 ( 0.9)  
         SCHAAF-YANG SYNDROME                                                                                                 2 ( 1.8)  
         SENIOR-LOKEN SYNDROME 5                                                                                              1 ( 0.9)  
         SHASHI-PENA SYNDROME                                                                                                 1 ( 0.9)  
         SHWACHMAN-DIAMOND SYNDROME 1                                                                                         1 ( 0.9)  
         SJOGREN SYNDROME                                                                                                     1 ( 0.9)  
         SPASTIC PARAPLEGIA 11, AUTOSOMAL RECESSIVE                                                                           1 ( 0.9)  
         SPASTIC PARAPLEGIA 7, AUTOSOMAL RECESSIVE                                                                            1 ( 0.9)  
         SPASTIC PARAPLEGIA 9A, AUTOSOMAL DOMINANT                                                                            1 ( 0.9)  
         SPINAL MUSCULAR ATROPHY, LOWER EXTREMITY-PREDOMINANT, 1, AUTOSOMAL DOMINANT                                          1 ( 0.9)  
         SPINOCEREBELLAR ATAXIA 28                                                                                            1 ( 0.9)  
         SPINOCEREBELLAR ATAXIA 8                                                                                             1 ( 0.9)  
         SPINOCEREBELLAR ATAXIA, AUTOSOMAL RECESSIVE 8                                                                        1 ( 0.9)  
         STICKLER SYNDROME, TYPE I                                                                                            1 ( 0.9)  
         STIFF-PERSON SYNDROME                                                                                                1 ( 0.9)  
         STORMORKEN SYNDROME                                                                                                  1 ( 0.9)  
         VAN MALDERGEM SYNDROME 2                                                                                             1 ( 0.9)  
         WIEACKER-WOLFF SYNDROME                                                                                              1 ( 0.9)  
         WIEDEMANN-STEINER SYNDROME                                                                                           1 ( 0.9)  
         WILLIAMS-BEUREN SYNDROME                                                                                             1 ( 0.9)  



```R
names(Tab_009)
```


<ol class=list-inline>
	<li>'patient_id'</li>
	<li>'Gender'</li>
	<li>'Ethnicity'</li>
	<li>'Race'</li>
	<li>'x01_Primary_symptom_category_reported_by_patient_or_caregiver'</li>
	<li>'Primary_symptom'</li>
	<li>'x02_Sequence_Submitted'</li>
	<li>'x02_Type_of_sequencing'</li>
	<li>'x03_UDN_Clinical_Site'</li>
	<li>'x06_Paternal_ethnicity_.from_PhenoTips.'</li>
	<li>'Affected_Relatives'</li>
	<li>'Consanguinity'</li>
	<li>'Miscarriages'</li>
	<li>'x13_Status'</li>
	<li>'x14_Disorders_.in_OMIM._from_PhenoTips.'</li>
</ol>




```R
class(Tab_009)
```


'data.frame'



```R
dput(names(Tab_009))
```

    c("patient_id", "Gender", "Ethnicity", "Race", "x01_Primary_symptom_category_reported_by_patient_or_caregiver", 
    "Primary_symptom", "x02_Sequence_Submitted", "x02_Type_of_sequencing", 
    "x03_UDN_Clinical_Site", "x06_Paternal_ethnicity_.from_PhenoTips.", 
    "Affected_Relatives", "Consanguinity", "Miscarriages", "x13_Status", 
    "x14_Disorders_.in_OMIM._from_PhenoTips.")



```R
if( nchar("Tab_009$Primary_symptom") > 20 ){
  Tab_009$Primary_symptom <- substr(Tab_009$Primary_symptom, 1, 20)
}
```


```R
if( nchar("Tab_009$Race") > 20 ){
  Tab_009$Race <- substr(Tab_009$Race, 1, 20)
}
```
Here, we create a vector of variables to summarize

```R
myVars <- c("Gender", "Ethnicity", 
"Primary_symptom", "x02_Sequence_Submitted", "x02_Type_of_sequencing", 
"x03_UDN_Clinical_Site", 
"Affected_Relatives", "Consanguinity", "Miscarriages", "x13_Status")
```
Vector of categorical variables that need transformation

```R
catVars <- c("Gender", "Ethnicity","x02_Sequence_Submitted","x13_Status","Affected_Relatives","Consanguinity","Miscarriages")
```

```R
tab2 <- CreateTableOne(vars = myVars, data = Tab_009, factorVars = catVars)
```

```R
tab2
```

                                       
                                        Overall     
      n                                 1055        
      Gender (%)                                    
         Female                          539 (51.1) 
         Male                            515 (48.8) 
         Other                             1 ( 0.1) 
      Ethnicity (%)                                 
                                           4 ( 0.4) 
         Hispanic or Latino              157 (14.9) 
         Not Hispanic or Latino          776 (73.6) 
         UnknownNot Reported Ethnicity   118 (11.2) 
      Primary_symptom (%)                           
                                          28 ( 2.7) 
         Allergies and Disord             53 ( 5.0) 
         Cardiology and vascu             48 ( 4.6) 
         Dentistry and cranio              7 ( 0.7) 
         Dermatology (skin di             10 ( 0.9) 
         Endocrinology (disor             27 ( 2.6) 
         Gastroenterology (di             41 ( 3.9) 
         Gynecology and repro              2 ( 0.2) 
         Hematology (blood di             16 ( 1.5) 
         Infectious diseases               2 ( 0.2) 
         Musculoskeletal and             124 (11.8) 
         Nephrology (kidney d             17 ( 1.6) 
         Neurology (disorders            495 (47.0) 
         Oncology (Tumors and              3 ( 0.3) 
         Ophthalmology (Eye d             11 ( 1.0) 
         Other                           107 (10.2) 
         Psychiatry                        6 ( 0.6) 
         Pulmonology (Lung di             20 ( 1.9) 
         Rheumatology (immune             35 ( 3.3) 
         Urology                           1 ( 0.1) 
      x02_Sequence_Submitted = true (%)  685 (98.7) 
      x02_Type_of_sequencing (%)                    
         Targeted Variant                  3 ( 0.4) 
         Transcriptome                     1 ( 0.1) 
         Whole Exome                     346 (49.9) 
         Whole Genome                    344 (49.6) 
      x03_UDN_Clinical_Site (%)                     
         baylor                          160 (15.2) 
         duke                            144 (13.6) 
         harvard-affiliate               117 (11.1) 
         nih                             266 (25.2) 
         stanford                        137 (13.0) 
         ucla                            109 (10.3) 
         vanderbilt                      122 (11.6) 
      Affected_Relatives (%)                        
                                         124 (20.6) 
         false                           374 (62.2) 
         true                            103 (17.1) 
      Consanguinity (%)                             
                                          24 ( 4.0) 
         false                           558 (92.8) 
         true                             19 ( 3.2) 
      Miscarriages (%)                              
                                          75 (12.5) 
         false                           491 (81.7) 
         true                             35 ( 5.8) 
      x13_Status = unsolved (%)          896 (87.7) 


Her we grouping by exposure categories (1) Status and then (2) Gender

```R
tab3 <- CreateTableOne(vars = myVars, strata = "x13_Status" , data = Tab_009, factorVars = catVars)
```

```R
tab3
```


                                       Stratified by x13_Status
                                        solved      unsolved     p      test
      n                                 126         896                     
      Gender (%)                                                  0.650     
         Female                          69 (54.8)  454 ( 50.7)             
         Male                            57 (45.2)  441 ( 49.2)             
         Other                            0 ( 0.0)    1 (  0.1)             
      Ethnicity (%)                                               0.105     
                                          0 ( 0.0)    4 (  0.4)             
         Hispanic or Latino              27 (21.4)  122 ( 13.6)             
         Not Hispanic or Latino          84 (66.7)  668 ( 74.6)             
         UnknownNot Reported Ethnicity   15 (11.9)  102 ( 11.4)             
      Primary_symptom (%)                                         0.121     
                                          1 ( 0.8)   27 (  3.0)             
         Allergies and Disord             3 ( 2.4)   50 (  5.6)             
         Cardiology and vascu             5 ( 4.0)   40 (  4.5)             
         Dentistry and cranio             0 ( 0.0)    6 (  0.7)             
         Dermatology (skin di             0 ( 0.0)    8 (  0.9)             
         Endocrinology (disor             5 ( 4.0)   22 (  2.5)             
         Gastroenterology (di             1 ( 0.8)   36 (  4.0)             
         Gynecology and repro             0 ( 0.0)    2 (  0.2)             
         Hematology (blood di             0 ( 0.0)   15 (  1.7)             
         Infectious diseases              0 ( 0.0)    2 (  0.2)             
         Musculoskeletal and             17 (13.6)  106 ( 11.8)             
         Nephrology (kidney d             5 ( 4.0)   12 (  1.3)             
         Neurology (disorders            68 (54.4)  411 ( 45.9)             
         Oncology (Tumors and             0 ( 0.0)    3 (  0.3)             
         Ophthalmology (Eye d             0 ( 0.0)   10 (  1.1)             
         Other                           16 (12.8)   88 (  9.8)             
         Psychiatry                       0 ( 0.0)    6 (  0.7)             
         Pulmonology (Lung di             3 ( 2.4)   16 (  1.8)             
         Rheumatology (immune             1 ( 0.8)   34 (  3.8)             
         Urology                          0 ( 0.0)    1 (  0.1)             
      x02_Sequence_Submitted = true (%) 103 (99.0)  582 ( 98.6)   1.000     
      x02_Type_of_sequencing (%)                                  0.233     
         Targeted Variant                 0 ( 0.0)    3 (  0.5)             
         Transcriptome                    0 ( 0.0)    1 (  0.2)             
         Whole Exome                     61 (58.7)  285 ( 48.3)             
         Whole Genome                    43 (41.3)  301 ( 51.0)             
      x03_UDN_Clinical_Site (%)                                  <0.001     
         baylor                          25 (19.8)  126 ( 14.1)             
         duke                            21 (16.7)  118 ( 13.2)             
         harvard-affiliate               10 ( 7.9)   97 ( 10.8)             
         nih                              1 ( 0.8)  258 ( 28.8)             
         stanford                        15 (11.9)  122 ( 13.6)             
         ucla                            29 (23.0)   78 (  8.7)             
         vanderbilt                      25 (19.8)   97 ( 10.8)             
      Affected_Relatives (%)                                      0.090     
                                         16 (13.6)  108 ( 22.4)             
         false                           82 (69.5)  292 ( 60.5)             
         true                            20 (16.9)   83 ( 17.2)             
      Consanguinity (%)                                           0.001     
                                          4 ( 3.4)   20 (  4.1)             
         false                          104 (88.1)  454 ( 94.0)             
         true                            10 ( 8.5)    9 (  1.9)             
      Miscarriages (%)                                            0.062     
                                         10 ( 8.5)   65 ( 13.5)             
         false                          105 (89.0)  386 ( 79.9)             
         true                             3 ( 2.5)   32 (  6.6)             
      x13_Status = unsolved (%)           0 ( 0.0)  896 (100.0)  <0.001     



```R
tab4 <- CreateTableOne(vars = myVars, strata = "Gender" , data = Tab_009, factorVars = catVars)
```

```R
tab4
```

                                       Stratified by Gender
                                        Female       Male         Other      p     
      n                                 539          515          1                
      Gender (%)                                                             <0.001
         Female                         539 (100.0)    0 (  0.0)  0 (  0.0)        
         Male                             0 (  0.0)  515 (100.0)  0 (  0.0)        
         Other                            0 (  0.0)    0 (  0.0)  1 (100.0)        
      Ethnicity (%)                                                           0.927
                                          3 (  0.6)    1 (  0.2)  0 (  0.0)        
         Hispanic or Latino              76 ( 14.1)   81 ( 15.7)  0 (  0.0)        
         Not Hispanic or Latino         401 ( 74.4)  374 ( 72.6)  1 (100.0)        
         UnknownNot Reported Ethnicity   59 ( 10.9)   59 ( 11.5)  0 (  0.0)        
      Primary_symptom (%)                                                     0.045
                                         13 (  2.4)   14 (  2.7)  1 (100.0)        
         Allergies and Disord            25 (  4.6)   28 (  5.4)  0 (  0.0)        
         Cardiology and vascu            24 (  4.5)   24 (  4.7)  0 (  0.0)        
         Dentistry and cranio             4 (  0.7)    3 (  0.6)  0 (  0.0)        
         Dermatology (skin di             7 (  1.3)    3 (  0.6)  0 (  0.0)        
         Endocrinology (disor            15 (  2.8)   12 (  2.3)  0 (  0.0)        
         Gastroenterology (di            22 (  4.1)   19 (  3.7)  0 (  0.0)        
         Gynecology and repro             1 (  0.2)    1 (  0.2)  0 (  0.0)        
         Hematology (blood di             8 (  1.5)    8 (  1.6)  0 (  0.0)        
         Infectious diseases              1 (  0.2)    1 (  0.2)  0 (  0.0)        
         Musculoskeletal and             64 ( 11.9)   60 ( 11.7)  0 (  0.0)        
         Nephrology (kidney d             3 (  0.6)   14 (  2.7)  0 (  0.0)        
         Neurology (disorders           262 ( 48.7)  233 ( 45.3)  0 (  0.0)        
         Oncology (Tumors and             0 (  0.0)    3 (  0.6)  0 (  0.0)        
         Ophthalmology (Eye d             5 (  0.9)    6 (  1.2)  0 (  0.0)        
         Other                           52 (  9.7)   55 ( 10.7)  0 (  0.0)        
         Psychiatry                       3 (  0.6)    3 (  0.6)  0 (  0.0)        
         Pulmonology (Lung di             8 (  1.5)   12 (  2.3)  0 (  0.0)        
         Rheumatology (immune            21 (  3.9)   14 (  2.7)  0 (  0.0)        
         Urology                          0 (  0.0)    1 (  0.2)  0 (  0.0)        
      x02_Sequence_Submitted = true (%) 349 ( 98.3)  335 ( 99.1)  1 (100.0)   0.643
      x02_Type_of_sequencing (%)                                              0.406
         Targeted Variant                 3 (  0.8)    0 (  0.0)  0 (  0.0)        
         Transcriptome                    0 (  0.0)    1 (  0.3)  0 (  0.0)        
         Whole Exome                    184 ( 51.8)  162 ( 47.9)  0 (  0.0)        
         Whole Genome                   168 ( 47.3)  175 ( 51.8)  1 (100.0)        
      x03_UDN_Clinical_Site (%)                                               0.352
         baylor                          85 ( 15.8)   75 ( 14.6)  0 (  0.0)        
         duke                            72 ( 13.4)   72 ( 14.0)  0 (  0.0)        
         harvard-affiliate               57 ( 10.6)   59 ( 11.5)  1 (100.0)        
         nih                            144 ( 26.7)  122 ( 23.7)  0 (  0.0)        
         stanford                        73 ( 13.5)   64 ( 12.4)  0 (  0.0)        
         ucla                            46 (  8.5)   63 ( 12.2)  0 (  0.0)        
         vanderbilt                      62 ( 11.5)   60 ( 11.7)  0 (  0.0)        
      Affected_Relatives (%)                                                  0.785
                                         69 ( 22.1)   55 ( 19.1)  0 (  0.0)        
         false                          188 ( 60.3)  185 ( 64.2)  1 (100.0)        
         true                            55 ( 17.6)   48 ( 16.7)  0 (  0.0)        
      Consanguinity (%)                                                       0.891
                                         13 (  4.2)   11 (  3.8)  0 (  0.0)        
         false                          287 ( 92.0)  270 ( 93.8)  1 (100.0)        
         true                            12 (  3.8)    7 (  2.4)  0 (  0.0)        
      Miscarriages (%)                                                        0.626
                                         45 ( 14.4)   30 ( 10.4)  0 (  0.0)        
         false                          248 ( 79.5)  242 ( 84.0)  1 (100.0)        
         true                            19 (  6.1)   16 (  5.6)  0 (  0.0)        
      x13_Status = unsolved (%)         454 ( 86.8)  441 ( 88.6)  1 (100.0)   0.650
                                       Stratified by Gender
                                        test
 
```R

```
