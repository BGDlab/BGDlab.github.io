Defined Arcus Cohorts

## Summary

Our cohorts consist of patients who have radiology reports for brain/head MRIs and meet other criteria using data pulled from the patient's EHR. These criteria are specified in a SQL query. The patients included in a cohort may also be subject to filtering by diagnosis.

## Brain/Head MRIs

The reports included in our database are limited to those labelled as brain or head scans obtained on MR scanners. MR scanners, by their nature, are capable of obtaining many different types of information. MR scans can include structural scans, which are the type we are primarily interested in working with, but they can also include functional scans obtained over a series of minutes, spectroscopy and metabolic scans that record a range of frequency responses from a single area, and outside scans that may not actually be present in the PACS system.

For this reason, reports containing the following words in their descriptions are excluded: "functional", "spectroscopy", "metabolic", "outside", and "autopsy". Additionally, reports included must contain the strings "brain" and "mr" (which may ultimately be redundant).

## Cohort List

**SLIP Neonates**
- Age at scan: <= 1 month
- Exclusion diagnoses: standard
- Additionaly criteria:
    - Reports are added to the grading queue with those who have gestational age present in the EHR first
    - Reports are added to the grading queue in order of the scan date

**SLIP Toddlers**
- Age at scan: 1 month < age <= 2 years 
- Exclusion diagnoses: standard
- Additionaly criteria:
    - Reports are added to the grading queue with those who have gestational age present in the EHR first
    - Reports are added to the grading queue in order of the scan date


**SLIP PreK**
- Age at scan: 2 years < age <= 5 years
- Exclusion diagnoses: standard
- Additionaly criteria:
    - Reports are added to the grading queue with those who have gestational age present in the EHR first
    - Reports are added to the grading queue in order of the scan date


**SLIP Elementary**
- Age at scan: 5 years < age < 12 years
- Exclusion diagnoses: standard
- Additionaly criteria:
    - Reports are added to the grading queue with those who have gestational age present in the EHR first
    - Reports are added to the grading queue in order of the scan date


**SLIP Adolescents**
- Age at scan: 12 years <= age <= 20 years
- Exclusion diagnoses: standard
- Additionaly criteria:
    - Reports are added to the grading queue in order of the scan date


**Clinical Imaging Genetics**
- Age at scan: any
- Exclusion diagnoses: none 
- Additional criteria: genetic information available upon further request

## Help

For help, reach out to Jenna via Slack. 
