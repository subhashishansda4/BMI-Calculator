## Introduction
Built an app for **fitness enthusiasts** and their **tracking of Body Mass Index**. It would be overall beneficial for the user's health and fitness goals.

### Body Mass Index
Body mass index (BMI) is a **measure of body fat** based on **height** and **weight** that applies to adult men and women.

`BMI = weight (kg) / height (m)^2`

BMI is a screening tool, not a diagnostic tool. It is used to identify people who may be at risk for health problems associated with being overweight or obese.

BMI does not take into account body composition, which is the ratio of muscle to fat. People who are muscular may have a high BMI even though they have a healthy amount of body fat.

**World Health Organization (WHO)** defines the following BMI categories:
* Underweight: BMI <18.5
* Normal weight: BMI 18.5-24.9
* Overweight: BMI 25-29.9
* Obese: BMI 30-34.9
* Severely obese: BMI ≥35
 
## User Interface
### Splash
![freq_plot](https://github.com/subhashishansda4/BMI-Calculator/blob/main/assets/plots/plot_bioactivity_class.jpg)

### Acetylcholinesterase
Cholinergic enzyme primarily found at postsynaptic neuromuscular junctions, especially in muscles and nerves \
Lack of acetylcholinesterase contributes to neuromuscular junction dysfunction in Type 1 diabetic neuropathy \
\
Search **"acetylcholinesterase"** in all targets :  referring to target proteins and target organisms \
Selected Target: CHEMBL220 \
standard_type: IC50 : inhibitory concentration @50% of target protein \
standard_value: potency of the drug \
Biologically, these compounds will come into contact with the protein/organism and induce a modulotory activity towards it {either to activate or to inhibit}

### Activity Classification
* active compound : IC50 < 1000 nM
* inactive compound : IC50 > 10000 nM
* intermediate compound : 1000 < IC50 < 10000 

## EDA
### Lipinski Descriptors
**rdkit** allows us to compute molecular decriptors for the compounds in the dataset that we have compiled \
**Christopher Lipinski**, a scientist at Pfizer came up with a set of rule-of-five for evaluating the 'relative druglikeness of the compound' of compounds based on the key pharmaceutical kinetic properties:
* Absorption
* Distribution
* Metabolism
* Excretion

Lipinski analyzed all orally active FDA-approved drugs in the formulation whether it can be absorbed into the body, distributed to the proper tissue/organs and become metabolised. Following 4 descriptors that was used for his analysis has corresponding values in multiples of 5:
* Molecular Weight < 500 Dalton
* Octanol-Water partition coefficient (LogP) < 5
* Hydrogen bond donors < 5
* Hydrogen bond acceptors < 10

#### Calculating Descriptors
Converting standard_value from IC50 to pIC50 to allow IC50 to be more uniformly distributed. We will convert the IC50 values to the negative logarithmic scale which is essentially -log10. Applying value normalization because values greater than negative logarithmic of 100,000,000 will become negative

### Chemical Space Analysis
**Jose Medina Franco** (author) \
"Each chemical compound can be thought of as stars, i.e. active molecules would be compared to as constellations" \
\
He developed an approach termed as "constellation plot" whereby one can perform chemcial space analysis and create constellation plot where active molecule have correspondingly have larger size compared to less active molecule \
\
Frequency plot of 2 bioactivity classes comparing the inactive and active molecules
![freq_plot](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/plot_bioactivity_class.jpg) \
\
Scatter plot of molecular weight (MW) v/s molecular solubilityt (LogP) \
It can be seen that the 2 bioactivity classes are spanning similar chemical spaces as evident by this \
![scatter_plot](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/plot_MW_vs_logP.jpg)

### Mann Whitney Analysis
U-Test used to test whether two samples are likely to derive from the same population \
Box plots for pIC50, MW, LogP, NumHDonors, NumHAcceptors \
pIC50 \
![pIC50](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/plot_ic50.jpg) \
\
MW \
![MW](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/plot_MW.jpg) \
\
LogP \
![LogP](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/plot_LogP.jpg) \
\
NumHDonors \
![NumHDonors](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/plot_NumHDonors.jpg) \
\
NumHAcceptors \
![NumHAcceptors](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/NumHAcceptors.jpg) \
\
pIC50 values of actives and inactives displayed statistically significant differenece which is to be expected since threshold values are (pIC50 > 6 for active AND pIC50 < 5 for inactive) \
\
Out of the 4 Lipinski descriptors only NumHDonors exhibited no difference between the actives and inactives while the others showed significant difference between the two

## Dataset Preparation
### Lipinski Descriptors v/s PubChem Fingerprints
| Lipinski Descriptors | PubChem Fingerprints |
| :-------------------------- |:--------------------------- |
| provides set of simple molecular descriptors of molecule | describing local features of molecules |
| 4 descriptors responsible for drug-like properties | each molecule will be described by the unique building blocks of molecule |
| describes global features of molecules such as molecular size, solubility, number of H bond donors and acceptors | molecules provides most potency towards target protein that it wants to interact while also being safe and non-toxic |

Using **PaDEL Descriptors** to calculate molecular descriptors and fingerprints\
**padel.sh**:
- jar - for using PaDEL-Descriptor.jar
- removesalt - removing sodium & chloride which are in the chemical structure / small organic assets from chemical structure
- fingerprints - tells the program that we gonna compute the molecular fingerprints {fingerprint_type : PubChem}

## Machine Learning
Which funtional group or fingerprints are essential for designing a good drug so that the target variable that we are using for our prediction i.e. pIC50 can be minimum while also being active \
Removing **Low Variance Features** because lower variance features are constant and thus, it cannot be used for finding any interesting patterns and can be removed from the dataset

### Principal Component Analysis
Simplifying the complexity of our high-dimensional data while retaining trends and patterns using PCA \
Used only 40 features for machine learning models since it describes 80% of variance \
![PCA](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/PCA.jpg)

## Model Building
Multiple Linear Regression \
![MLR](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/Multiple%20Linear%20Regression.jpg) \
\
Decision Tree Regression \
![DTR](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/Decision%20Tree%20Regression.jpg) \
\
Random Forest Regression \
![RFR](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/Random%20Forest%20Regression.jpg) \
\
Ridge Regression \
![RR](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/Ridge%20Regression.jpg) \
\
Bayesian Regression \
![BR](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/Bayesian%20Regression.jpg) \
\
K-Nearest Neighbour \
![KNN](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/K-Nearest%20Neighbour.jpg) \
\
Support Vector Regression \
![SVR](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/Support%20Vector%20Regression.jpg) \
\
XGBoost Regression \
![XGB](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/XGBoost%20Regression.jpg) \

## Conclusion
\
![error](https://github.com/subhashishansda4/Bio-Informatics/blob/main/assets/plots/error_values.jpg)



    



