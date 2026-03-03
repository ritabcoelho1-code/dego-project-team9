# dego-project-team9
DEGO Course Project — Team 9

## Team Members
- Tom Osterwald
- Lara Corina Martins

## Project Description
Credit scoring bias analysis for DEGO course.

## Structure
- ‘data/‘ - Dataset files
- ‘notebooks/‘ - Jupyter analysis notebooks
- ‘src/‘ - Python source code
- ‘reports/‘ - Final deliverables

## Privacy & Governance

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

