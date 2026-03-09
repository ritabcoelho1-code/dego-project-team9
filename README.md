# DEGO Credit Governance Project — Team 9

### DEGO 2606 Group Project | NovaCred Credit Decision Governance Assessment

## Team Members
- **Tom Osterwald** — Lead  
- **Lara Corina Martins** — Governance  
- **Lucrezia Salerno** — Engineer
- **Rita Borges Coelho** — Scientist

---

## Executive Summary

This project conducts a governance audit of NovaCred’s automated credit decision dataset as part of the **Data Ecosystems and Governance in Organizations (DEGO 2606)** course project. The objective is to identify risks related to **data quality, algorithmic fairness, and privacy compliance**, and to propose governance improvements for responsible AI deployment.

The analysis is based on NovaCred’s historical credit application dataset containing **502 records in nested JSON format**. The dataset includes applicant demographic attributes, financial indicators, behavioural spending data, and the final automated loan decision.

The governance audit focuses on three analytical dimensions:

### Data Quality

Several structural issues were identified in the raw dataset that could undermine reliability and auditability of automated decisions. Key findings include:

- **Duplicate application IDs**, affecting traceability of decisions  
- **Schema inconsistencies** between `annual_income` and `annual_salary` fields  
- **Inconsistent categorical encoding** in demographic attributes such as gender  
- **Mixed date formats** across temporal fields  
- **Invalid negative financial values**, including savings balances and credit history duration  

These issues highlight weaknesses in NovaCred’s data ingestion and validation processes and demonstrate the need for stronger **data governance controls** before automated decision systems are deployed in production environments.

### Bias and Fairness

The fairness analysis evaluates potential disparities in loan approval outcomes across demographic groups.

Key findings are:

Female applicants are approved at materially lower rates than male applicants. The Disparate Impact (DI) ratio falls below the four-fifths threshold of 0.80, indicating potential disparate impact. This suggests that gender-based differences in approval outcomes represent a significant fairness concern that requires further scrutiny.

Approval outcomes vary significantly across age groups. Statistical testing confirms that approval rates are not independent of age category. While some variation may reflect structural life-cycle differences (such as credit history accumulation), the magnitude of the observed differences warrants continued monitoring from a governance perspective.

The interaction analysis reveals that gender disparities are not uniformly distributed across age groups. Segment-level DI ratios indicate that certain age bands exhibit more pronounced gender gaps than others. This highlights the importance of intersectional analysis, as aggregate fairness metrics may mask concentrated subgroup disparities.

ZIP code (state derived from ZIP) is strongly associated with gender, indicating that geographic variables encode protected-attribute information. Although geography does not independently drive approval outcomes in the controlled model, its strong demographic concentration presents a proxy risk. The inclusion of ZIP-level geographic features in predictive models may therefore introduce indirect fairness and compliance concerns.

These results suggest that fairness risks may exist in the automated credit approval process and that additional monitoring mechanisms are required to ensure equitable outcomes.

### Privacy and Governance

The dataset contains several forms of **personally identifiable information (PII)** including names, email addresses, social security numbers, IP addresses, and birth dates. These elements raise compliance concerns under the **General Data Protection Regulation (GDPR)**.

To demonstrate privacy-preserving practices, a transformed dataset was created using:

- hashing of email addresses  
- masking of social security numbers  
- removal of direct identifiers  
- generalization of dates of birth into age groups  

These steps illustrate how **privacy-by-design principles** can reduce re-identification risks while preserving analytical value.

### Governance Implications

The findings indicate that NovaCred’s credit decision pipeline would benefit from stronger governance controls in three areas:

- improved **data validation and schema management**  
- continuous **fairness monitoring and bias detection**  
- stronger **privacy protections and regulatory compliance practices**

Implementing these measures would enhance the **trustworthiness, transparency, and regulatory readiness** of NovaCred’s automated credit decision system.

# Table of Contents

1. [Introduction](#1-introduction)  
2. [Dataset Description](#2-dataset-description)  
3. [Project Overview](#3-project-overview)  
4. [Data Quality Assessment](#4-data-quality-assessment)  
5. [Bias and Fairness Analysis](#5-bias-and-fairness-analysis)  
6. [Privacy and Governance Assessment](#6-privacy-and-governance-assessment)  
7. [Governance Recommendations](#7-governance-recommendations)

---

# 1. Introduction

This repository contains the analysis conducted for the **Data Ethics and Governance (DEGO) course project**, focused on evaluating governance risks associated with NovaCred’s automated credit decision system.

Automated credit decision systems operate in highly regulated environments where algorithmic outcomes may significantly impact individuals’ financial opportunities. Organizations deploying such systems must ensure that their data pipelines meet standards of **data quality, fairness, transparency, and regulatory compliance**.

This project conducts a governance audit of NovaCred’s historical credit application dataset in order to identify potential risks related to:

- data quality and reliability  
- algorithmic bias and fairness  
- privacy and regulatory compliance  

The objective is not to redesign the credit model itself, but to evaluate whether the **underlying data and governance processes support responsible AI deployment**.

---

# 2. Dataset Description

The analysis uses NovaCred’s historical **credit application dataset**, provided in nested JSON format.

The dataset contains approximately **500 credit application records (502 raw records before cleaning)**, each representing an automated credit decision generated by NovaCred’s internal scoring system.

Each record includes several categories of information:

| Category | Fields Included | Governance Relevance |
|---|---|---|
| Applicant Information | `full_name`, `email`, `ssn`, `ip_address`, `gender`, `date_of_birth`, `zip_code` | Contains direct and indirect identifiers relevant for privacy and fairness |
| Financial Information | `annual_income`, `credit_history_months`, `debt_to_income`, `savings_balance` | Core financial variables used for credit decisions |
| Spending Behaviour | categorized spending transactions | May reveal behavioural patterns and potential proxy variables |
| Decision Data | `loan_approved`, `interest_rate`, `approved_amount`, `rejection_reason` | Represents the outcome of the automated credit decision |
| System Metadata | `processing_timestamp` | Enables auditability and traceability of decisions |

The dataset intentionally contains **data quality weaknesses and governance risks** which must be identified and addressed before reliable analysis or responsible AI deployment.

---

# 3. Project Overview

## 3.1 Objective

The objective of this project is to conduct a structured governance assessment of NovaCred’s automated credit decision pipeline.

The analysis focuses on three core governance dimensions:

**Data Quality**  
Evaluate whether the dataset satisfies standards for completeness, consistency, validity, and structural integrity.

**Bias and Fairness**  
Assess whether loan approval outcomes exhibit disparities across demographic groups, particularly across gender and age.

**Privacy and Governance**  
Identify personal data within the dataset and evaluate regulatory risks under **GDPR** and governance requirements under the **EU AI Act**.

---

## 3.2 Scope

The project evaluates NovaCred’s historical credit application dataset and focuses on:

- assessing dataset completeness and consistency  
- identifying potential fairness disparities across demographic groups  
- detecting proxy variables correlated with protected attributes  
- evaluating privacy risks related to personally identifiable information (PII)  
- assessing governance requirements for high-risk AI systems  

The project focuses on **data governance and transparency**, rather than the internal architecture of the credit scoring algorithm.

---

## 3.3 Methodology

The governance audit follows a structured three-stage analytical framework.

### Data Quality Audit

The dataset was first evaluated to identify structural issues that could compromise analytical reliability. The assessment focused on:

- duplicate application identifiers  
- missing values across key variables  
- inconsistent schema definitions  
- inconsistent categorical encodings  
- logically impossible financial values  
- incomplete decision timestamps  

These checks correspond to core **data governance quality dimensions**, including completeness, validity, consistency, and structural integrity.

---

### Bias and Fairness Analysis

The fairness analysis evaluates whether the automated credit decision system exhibits unequal approval outcomes across demographic groups and whether non-protected variables may indirectly reproduce demographic disparities.

Approval rates were calculated across **gender groups** and compared to identify potential disparities. To formally assess fairness outcomes, the **Disparate Impact (DI) Ratio** was computed as the ratio of approval rates between female and male applicants:

- DI = Approval rate (female) / Approval rate (male)

The ratio was interpreted using the **four-fifths rule**, which indicates potential discriminatory impact when the value falls below **0.80**.

Approval rates were also analyzed across **age groups** derived from applicant birth dates. Age-based differences were evaluated descriptively and formally tested using a **chi-square test** of independence to determine whether approval outcomes vary significantly across age categories.

To assess whether disparities differ across combined demographic groups, we examined **interaction effects (Age × Gender)** by computing approval rates within each age–gender segment. In addition to descriptive approval comparisons, we calculated a **segment-level Disparate Impact (DI) ratio** within each age group (female approval rate relative to male approval rate).
This allows identification of subgroup-specific disparities that may not be visible in aggregate gender or age statistics, highlighting whether certain age segments experience amplified or mitigated gender gaps.

In addition to direct disparity testing, we conducted a **proxy discrimination analysis** to determine whether non-protected variables may act as indirect stand-ins for protected attributes. 

---

## 4.4 Temporal Data Inconsistencies

Temporal variables such as:

- `processing_timestamp`
- `date_of_birth`

contained **inconsistent date formats** across records.

Such inconsistencies make it difficult to parse, compare, and perform temporal analysis reliably.

All date fields were therefore standardized to a consistent datetime format during preprocessing.

---

## 4.5 Invalid Financial Values

Several financial variables contained **invalid negative values**, including:

- credit history length  
- savings balance

These values represent **data validity violations**, as credit history duration and savings balances cannot logically be negative.

Rather than imputing arbitrary values, these invalid entries were replaced with `NaN` in the cleaned dataset to prevent misleading downstream analysis.

---

## 4.6 Summary

The data quality audit revealed several structural weaknesses that could compromise analytical reliability and governance transparency.

The most critical issues identified were:

- duplicate primary keys affecting traceability  
- inconsistent schema definitions for income variables  
- inconsistent categorical encoding  
- inconsistent date formats  
- invalid financial values

These issues highlight the importance of **data governance controls and preprocessing procedures** before deploying automated credit decision systems.

---

# 5. Bias and Fairness Analysis

This audit evaluates whether NovaCred’s historical loan approval decisions exhibit systematic bias and whether any non-protected variables act as proxy indicators for protected attributes such as gender or age. The objective is to assess potential fairness risks as well as associated governance and regulatory exposure within the current decision framework.

The analysis focuses on four dimensions. First, whether approval outcomes differ systematically by gender. Second, whether approval rates vary across age groups. Third, whether disparities are amplified or concentrated within specific subgroups when considering the interaction between age and gender. Fourth, whether geographic variables, particularly state and ZIP code, may function as structural proxies by encoding demographic composition.

Overall, the audit is designed to distinguish between direct group disparities and indirect proxy risks, ensuring that both overt and structural sources of bias are evaluated within a coherent governance framework.

---

## 5.1 Gender Bias Analysis

The overall loan approval rate across the dataset is **58.4%**.

We begin by comparing approval outcomes across gender groups:

| Gender | Approved | Total | Approval Rate |
|---|---:|---:|---:|
| Male | 163 | 247 | **66.0%** |
| Female | 127 | 251 | **50.6%** |

To quantify fairness, we compute the Disparate Impact (DI) ratio, defined as the approval rate of the unprivileged group divided by that of the privileged group.

The **Disparate Impact Ratio** is therefore:

`0.506 / 0.660 = 0.767`

Under the commonly used four-fifths rule, a DI ratio below 0.80 signals potential disparate impact. The observed value of 0.768 therefore indicates that female applicants are approved at a substantially lower rate than male applicants.

Female applicants are roughly 23% less likely to be approved than male applicants when measured using approval rates alone. Because the DI ratio falls below the regulatory threshold, this pattern represents a potential fairness and compliance concern that warrants deeper investigation into the decision process.

---

## 5.2 Age Bias Analysis

Applicants were grouped into five age bands:

| Age Group | Approved | Total | Approval Rate |
|---|---:|---:|---:|
| 18–25 | 9 | 17 | **52.9%** |
| 26–35 | 46 | 105 | **43.8%** |
| 36–45 | 73 | 115 | **63.5%** |
| 46–55 | 37 | 57 | 64.9% |
| 56–65 | 26 | 39 | 67.7% |
| 66+ | 3 | 6 | 50.0% |

The lowest approval rate appears among applicants aged 26–35, while middle-aged groups (36–65) exhibit consistently higher approval rates. The youngest (18–25) and oldest (66+) categories show more moderate approval levels; however, both groups contain relatively few applicants, making their rates more sensitive to small changes in outcomes.

A chi-square test confirms that approval outcomes are statistically associated with age group (p = 0.0273<0.005) indicating that the variation across age segments is unlikely to be random.

Interpretation should account for both statistical significance and sample composition. The small size of the youngest and oldest groups limits the stability of conclusions for those segments. The more policy-relevant contrast lies between the 26–35 group and the middle-aged categories, where differences are observed within substantially larger samples.

From a structural perspective, younger applicants tend to have lower income levels, shorter credit histories, and smaller savings balances — patterns consistent with life-cycle financial effects. This suggests that part of the observed age disparity may reflect financial maturity differences rather than direct age-based discrimination. Nonetheless, because age is a protected characteristic, the magnitude and consistency of these differences warrant ongoing governance monitoring.

---

## 5.3 Interaction Effects: Age × Gender

To assess whether gender disparities vary across age segments, we compared approval rates for women and men within each age band and computed Disparate Impact (DI) ratios (female approval rate divided by male approval rate).

The results show clear variation in the magnitude of the gender gap across age groups:

- **18–25:** Female approval rate = 50.0% vs. male = 60.0% (DI = 0.833)
- **26–35:** Female = 36.8% vs. male = 52.1% (DI = 0.707)
- **36–45:** Female = 60.0% vs. male = 66.2% (DI = 0.907)
- **46–55:** Female = 58.6% vs. male = 70.4% (DI = 0.833)
- **56–65:** Female = 60.0% vs. male = 73.7% (DI = 0.814)
- **66+:** Female = 33.3% vs. male = 66.7% (DI = 0.500)

The most pronounced disparity appears in the **26–35 group**, where the DI ratio is **0.707**, meaning women in this age band are approved at roughly 71% of the rate of men. The **66+ group** shows an even lower DI of **0.500**; however, this segment contains only six applicants in total (three women and three men), making the estimate highly unstable and sensitive to small changes in outcomes.

Similarly, the **18–25 group** includes only 17 applicants overall, which limits the robustness of conclusions in that segment. In contrast, the 26–35 and 36–45 groups contain substantially larger samples, making disparities observed there more reliable from a statistical and governance standpoint.

Applying the 0.80 Disparate Impact benchmark as a reference point, adverse impact is not uniformly present across all age groups. Most segments remain above the screening threshold, indicating that gender disparities in those bands do not meet the conventional adverse impact trigger.

However, the 26–35 group falls below the 0.80 benchmark, signaling a meaningful adverse impact concern within that age band. The 66+ group also falls below the threshold, though conclusions there should be treated cautiously due to the very small number of applicants in that segment.

Overall, the results suggest that gender-based adverse impact is **concentrated rather than systemic across all ages.** The fairness risk emerges primarily within a specific age band, highlighting the importance of subgroup monitoring rather than relying solely on aggregate gender metrics.

---

## 5.4 Proxy discrimination

To assess whether non-protected variables function as indirect channels of bias, we evaluate whether geography (state derived from ZIP code) acts as a proxy for gender and whether it independently influences loan approval decisions. The analysis follows a two-step logic: first, testing whether geography strongly encodes gender (proxy potential), and second, assessing whether it independently affects approval outcomes after controlling for gender and financial risk factors (proxy in action).

Approval rates differ across geographic groups, with New York exhibiting a higher approval rate (64.5%) than Los Angeles (51.7%). 

A chi-square test examining the relationship between state and gender yields a large test statistic (χ² = 324.668) and an extremely small p-value (p < 0.001), indicating a strong statistical association between geography and gender. Gender composition varies substantially across states ( Los Angeles is predominantly female, while New York is predominantly male), confirming that geography encodes protected-attribute information.

To evaluate whether geography independently drives decisions, we estimate a logistic regression including state, gender, and core financial variables. The results show that the state indicator corresponds to an odds ratio of 1.066, implying a 6.6% increase in approval odds for New York relative to Los Angeles after controls are included. In contrast, the gender indicator (male) exhibits an odds ratio of 1.337 (33.7% higher odds), and financial variables such as income (OR = 1.335) and credit history (OR = 1.257) show materially stronger associations with approval outcomes.

The findings indicate that geography possesses clear proxy potential, as it strongly encodes gender composition (χ² = 324.668, p < 0.001). However, there is limited evidence of proxy in action, since the independent contribution of state to approval outcomes is comparatively small (OR = 1.066) once gender and financial risk factors are controlled for.

Accordingly, while state qualifies as a structural proxy variable, the results do not suggest that approval decisions are being meaningfully driven by geographic signal beyond legitimate financial considerations. From a governance standpoint, this represents a monitoring risk rather than confirmed proxy discrimination, warranting continued oversight of geographic features in future modeling or policy adjustments.

---

## 5.4 Summary

The bias and fairness assessment identifies a clear governance concern in the NovaCred decision pipeline.

The main findings are:

- a substantial and statistically significant approval gap between male and female applicants  
- a **Disparate Impact Ratio below 0.80**, indicating potential disparate impact under the four-fifths rule  
- age-related differences in approval outcomes, especially for younger applicants  
- evidence that ZIP code may operate as a structurally risky proxy variable  
- subgroup-level disparities that may be hidden by aggregate fairness metrics  

Overall, the results suggest that fairness risks are present in the automated approval process and should be addressed through stronger monitoring, model review, and governance controls.

---

# 6. Privacy and Governance Assessment

The final stage of the analysis evaluates the dataset from a **privacy and regulatory governance perspective**. Automated credit decision systems operate in a highly regulated environment where personal data must be handled according to strict legal and ethical standards.

The NovaCred dataset contains several categories of **personally identifiable information (PII)** that could expose individuals to privacy risks if improperly handled.

Examples of sensitive data present in the dataset include:

- full names  
- email addresses  
- social security numbers  
- IP addresses  
- birth dates  
- ZIP codes  

These variables make it possible to **directly or indirectly identify individuals**, which introduces potential compliance risks under the **General Data Protection Regulation (GDPR)**.

---

## 6.1 Identification of Personal Data

Several attributes in the dataset qualify as **direct identifiers**, meaning they can uniquely identify an individual without additional information. These include names, social security numbers, and email addresses.

Other attributes function as **quasi-identifiers**, which may not uniquely identify an individual on their own but can lead to identification when combined with other variables. Examples include date of birth, ZIP code, and IP address.

The presence of both direct and quasi-identifiers significantly increases the **re-identification risk** within the dataset.

---

## 6.2 GDPR Compliance Risks

From a governance perspective, the dataset raises several potential compliance issues under GDPR principles.

First, the dataset includes personal identifiers that may not be strictly necessary for the automated credit decision process. This raises concerns related to the **data minimisation principle**, which requires organizations to collect only the data that is necessary for a specific purpose.

Second, the dataset contains highly sensitive identifiers such as **social security numbers**, which require strong security safeguards. If such data were exposed or misused, it could lead to identity theft or financial fraud.

Finally, the dataset does not contain explicit indicators of user consent or documented processing purposes. This may create **accountability and transparency concerns** if the dataset were used in real-world automated decision systems.

These risks relate particularly to core GDPR requirements, including **Article 5** on data minimisation and storage limitation, **Article 17** on the right to erasure, and **Article 32** on the security of processing.

---

## 6.3 Privacy-Preserving Transformations

To demonstrate privacy-preserving governance practices, a **privacy-enhanced dataset** was created as part of the analysis.

Several transformations were applied to reduce the risk of personal identification:

- email addresses were hashed  
- social security numbers were masked  
- dates of birth were generalized into age groups  
- direct identifiers such as names were removed  

These transformations illustrate how **privacy-by-design principles** can be implemented in AI data pipelines while preserving analytical usefulness.

---

## 6.4 Governance Implications

Automated credit decision systems fall under the category of **high-risk AI systems** under the EU AI Act.

Such systems require strong governance controls, including:

- documented data governance procedures  
- fairness monitoring and risk assessment  
- traceability of automated decisions  
- protection of personal data throughout the data lifecycle

The presence of direct identifiers and quasi-identifiers in the raw dataset highlights the importance of **robust privacy governance practices** before deploying automated decision systems in production environments.

---

# 7. Governance Recommendations

Based on the findings from the data quality, fairness, and privacy assessments, several governance improvements are recommended for NovaCred’s credit decision pipeline.

These recommendations focus on reducing regulatory risk, improving data reliability, and strengthening accountability in automated decision-making.

---

## 7.1 Strengthen Data Validation at Ingestion

NovaCred should implement stronger validation checks before records enter the analytics or decision pipeline.

These checks should include:

- enforcing unique application IDs  
- validating numeric ranges for financial variables  
- rejecting logically impossible values  
- standardizing date and categorical formats at ingestion  

This would reduce the number of downstream cleaning interventions required and improve the reliability of both analysis and decision-making.

---

## 7.2 Standardize the Data Schema

The audit identified structural inconsistencies such as the use of both `annual_income` and `annual_salary` for the same concept.

NovaCred should establish a **single governed schema** for all core variables, with:

- standardized field names  
- fixed data types  
- clear documentation for each attribute  
- version control for schema updates  

A consistent schema is essential for auditability, reproducibility, and model governance.

---

## 7.3 Implement Fairness Monitoring

The fairness analysis identified a **Disparate Impact Ratio below 0.80** and statistically significant approval differences across gender groups.

NovaCred should therefore implement an ongoing fairness monitoring process that includes:

- approval-rate tracking across demographic groups  
- regular recalculation of disparate impact metrics  
- subgroup analysis by gender and age  
- review of proxy variables such as ZIP code  

These checks should be integrated into routine model governance rather than performed only after regulatory concerns arise.

---

## 7.4 Reduce the Use of Sensitive Personal Data

The raw dataset contains several direct identifiers and quasi-identifiers that create unnecessary privacy risk.

NovaCred should apply **data minimisation and privacy-by-design principles** by:

- removing identifiers that are not necessary for decisioning  
- pseudonymizing sensitive fields before analysis  
- limiting access to raw personal data  
- documenting retention and deletion policies  

This would improve compliance with GDPR and reduce the likelihood of misuse or re-identification.

---

## 7.5 Improve Auditability and Human Oversight

Because credit scoring is a high-risk decision context, NovaCred should improve its governance documentation and oversight controls.

This should include:

- maintaining an audit trail of automated decisions  
- documenting model versions and data transformations  
- defining human review procedures for rejected or borderline applications  
- clearly assigning accountability for fairness and privacy risks  

Better traceability and oversight would support compliance with both GDPR and the EU AI Act while making the credit decision process more transparent and defensible.

---

## 7.6 Summary

Overall, the governance assessment suggests that NovaCred’s current credit decision pipeline would benefit from stronger controls in three areas:

- **data quality governance**, to improve consistency and reliability  
- **fairness governance**, to detect and mitigate discriminatory outcomes  
- **privacy governance**, to reduce exposure of personal data and support compliance  

Together, these measures would improve the trustworthiness, transparency, and regulatory readiness of the system.