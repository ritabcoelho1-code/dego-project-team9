# dego-project-team9
DEGO Course Project — Team 9

## Team Members
- Tom Osterwald, Lead
- Lara Corina Martins, Governance 
- Lucrezia Salerno
- Rita Borges Coelho 

## Project Description
Credit scoring bias analysis for DEGO course.

## Repository Structure
- ‘data/‘ - Dataset files
- ‘notebooks/‘ - Jupyter analysis notebooks
    1. Data Quality
    2. Bias Analysis
    3. Privacy & Governance 
- ‘presentation/‘ - Final deliverables


## 01 - Data Quality
The objective of this notebook was to conduct a structured data quality audit of NovaCred’s credit application dataset prior to performing bias detection and governance analysis. Because automated credit decision systems operate in a regulated environment, data integrity is not merely a technical requirement but a governance obligation. 

The audit revealed structural weaknesses across multiple dimensions of data governance; each issue was quantified, investigated, and remediated in a manner that preserved analytical transparency and avoided artificial correction of critical governance variables ensuring quality dimensions: consistency, validity, and structural integrity.

### Weaknesses Overview
1. The dataset exhibited violations of primary key uniqueness, with duplicated application identifiers detected across multiple records. In regulated financial systems, the primary key serves as the anchor of traceability, ensuring that each decision can be uniquely attributed, reconstructed, and audited. Duplicate identifiers compromise data integrity by introducing ambiguity in record lineage, distorting aggregations, and weakening the reliability of downstream analytical joins. Such violations typically indicate deficiencies in identity management controls, ingestion deduplication mechanisms, or transactional integrity enforcement within upstream systems. The duplicates were resolved through controlled record selection, preserving the most complete entries while restoring uniqueness constraints. From a governance perspective, this issue underscores the necessity of strict primary key enforcement and automated duplication checks as foundational safeguards in high-risk AI decision pipelines.

2. The dataset revealed a schema inconsistency in which the same financial attribute was recorded under two distinct column names: `financials.annual_income` and `financials.annual_salary`. Although the overlap affected only a small subset of records, the duplication reflects a breakdown in semantic standardization and data dictionary governance. In high-risk financial systems, a single business concept must correspond to a single authoritative field; parallel representations introduce ambiguity, increase the risk of silent aggregation errors, and weaken model transparency. Such inconsistencies typically arise from upstream pipeline modifications or incomplete schema version control. To restore structural coherence, both attributes were consolidated into a unified income variable, ensuring consistent feature interpretation and eliminating semantic fragmentation across the dataset.

3. The gender variable exhibited inconsistent encoding patterns, including full labels ("Male", "Female"), shorthand representations ("M", "F"), and placeholder strings. Without standardization, these inconsistencies would have corrupted group-level aggregations and fairness metrics such as disparate impact calculations. The field was normalized through controlled recoding, and placeholder values were converted to proper missing values to ensure accurate demographic analysis.


4. The assessment identified two distinct forms of temporal inconsistency within the dataset. The `date_of_birth` attribute appeared in multiple valid but structurally inconsistent formats, and the `processing_timestamp` field that exhibited extreme incompleteness (with 87.6% of records missing values).
    - Given that this field represents part of the audit trail for credit decision processing, imputing synthetic timestamps would have artificially reconstructed decision history and undermined governance transparency. Rather than fabricating values, we treated this as a structural control failure in the data collection pipeline. The column was therefore excluded from downstream analytical modeling while being retained in the data quality assessment as evidence of weakened audit traceability.
    This approach preserves analytical integrity and accurately reflects the governance risk associated with inadequate temporal logging in a regulated credit scoring environment.

5. We found out of the presence of logically impossible financial values, including negative credit history duration and negative savings balances. These entries violate fundamental business rules and indicate weaknesses in validation controls at the point of data ingestion. The existence of these values suggests either inadequate input validation, transformation errors within the data pipeline, or insufficient rule enforcement mechanisms.

Rather than arbitrarily correcting these anomalies, the invalid entries were replaced with explicit missing values to preserve analytical transparency and prevent the introduction of fabricated data. This approach acknowledges the integrity breach while maintaining an accurate representation of upstream control failures. From a governance perspective, the issue underscores the necessity of embedding automated business rule validation within ingestion systems to ensure that decision models operate exclusively on logically coherent and policy-compliant inputs.

### Conclusion
The audit demonstrated that many data quality issues stem not from random noise but from upstream control gaps, including inconsistent schema enforcement, weak categorical validation, inadequate business rule constraints, and incomplete audit logging. Therefore, continuous data quality monitoring should be institutionalized, rather than relying on periodic audits, NovaCred should deploy automated quality dashboards that track missingness rates, schema deviations, categorical drift, and business rule violations over time. They must evolve from reactive correction to proactive monitoring.

Strengthening these controls will not only improve model reliability but also enhance regulatory defensibility, fairness auditing capability, and organizational accountability within high-risk AI systems.



## 02 - Privacy & Governance

### Overview of Personal Data in the Dataset
The dataset contains several types of personal data regulated under the GDPR. This includes identifiers such as full name, email address, SSN and IP address, as well as quasi-identifiers like birth date, zip code and timestamps. Because these elements can directly or indirectly identify an applicant, they fall under GDPR Article 4 and require strong protection and governance.

### Main Privacy and Governance Risks Identified
The analysis revealed that the dataset includes more personal information than is necessary for credit scoring, which conflicts with the GDPR principle of data minimisation. The presence of full SSNs, full dates of birth and IP addresses, all stored in plain text, indicates insufficient protection of sensitive data and inadequate application of GDPR Article 32.
A significant governance issue was found in the lack of timestamps, 440 out of 502 applications have no processing_timestamp. Without timestamps, NovaCred can’t prove when decisions were made, which model version was used or if there was change of policies. This undermines accountability under GDPR Article 5(2) and prevents the company from demonstrating lawful processing, retention compliance or responding properly to deletion and rectification requests. Also, missing timestamps violate the EU AI Act’s requirements for traceability in high-risk systems such as credit scoring.
The dataset also contains spending behaviour information that may indirectly reveal sensitive traits such as health status or religious practices, creating risks of unintended discrimination and unfair processing.

### Privacy Measures Implemented
To mitigate the identified issues, several privacy-enhancing transformations were applied in the 03 - privacy-demo.ipynb notebook. Email addresses were hashed using SHA-256, SSNs were masked to reduce exposure of sensitive identifiers and full dates of birth were converted into birth years. A privacy-friendly version of the dataset was created where direct identifiers were removed entirely. These steps apply the principles of Privacy by Design and Privacy by Default as required by GDPR Articles 25 and 32.
These transformations help preserve analytical utility while reducing identifiability and the risks associated with storing sensitive personal data.

### GDPR Compliance Alignment
The actions taken contribute to compliance with several GDPR principles. Data minimisation is supported through the removal or transformation of unnecessary personal data. Integrity and confidentiality are strengthened through pseudonymisation techniques. Storage limitation is reinforced by recommending clear retention rules. The accountability principle is addressed by identifying governance weaknesses and proposing corrective measures.

### AI Act Considerations for High-Risk Systems
Because credit scoring is classified as a high-risk AI system, NovaCred must comply with strict requirements related to transparency, human oversight, data governance and traceability. The lack of timestamps and the presence of unprotected identifiers highlight the importance of maintaining complete logs, documenting model versions and ensuring decisions are reviewable by humans. The privacy and data-quality improvements implemented in this project directly support these regulatory obligations.

### Governance Recommendations
To enhance compliance and reduce risk, NovaCred should enforce mandatory timestamps for all applications, standardised in UTC, and prevent the ingestion of records without one. The company should implement strong access controls for PII, apply pseudonymisation as early as possible in the data pipeline, document the lawful basis for all processing activities and define a clear retention period for personal data. Regular audits, ongoing monitoring of timestamp consistency and periodic DPIAs are recommended to maintain long-term compliance with both GDPR and the EU AI Act.

