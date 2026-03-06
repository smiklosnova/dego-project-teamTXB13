# dego-project-teamTXB13

DeGo Assignment TXB13 Team

# Team Members & Roles


| Name | Role | Responsibilities |
|------|------|------------------|
| Amal Boulila | Product Lead | Project coordination, README documentation, executive summary, presentation structure, governance synthesis |
| Milena | Data Scientist | Bias analysis, fairness metrics (Disparate Impact), statistical testing, visualizations |
| Schieszler Miki | Data Engineer | Data loading, JSON normalization, data cleaning pipeline, data quality checks |
| Gabriel Novais Vieira | Governance Officer | GDPR mapping, privacy assessment, AI Act classification, governance recommendations |

## Project Description


As the Data Governance Task Force for NovaCred, we conducted a comprehensive audit of 502 historical credit applications to assess:

1. Data quality issues  
2. Algorithmic bias in lending decisions  
3. Privacy and governance compliance  

Our analysis identified significant data quality weaknesses, measurable disparate impact by gender, and critical GDPR compliance gaps. We propose concrete governance interventions to mitigate regulatory and operational risk.

---

## Structure

- `data/` - Dataset files
- `notebooks/` - Jupyter analysis notebooks
- `src/` - Python source code
- `reports/` - Final deliverables


# 1. Data Quality Assessment

## Dataset Overview
- 502 applications
- 21 flattened columns
- Nested JSON structure normalized into tabular format

## Key Findings

### 1. Inconsistent Data Types
- `annual_income` stored as object instead of numeric.
- Required coercion using `pd.to_numeric()`.

Dimension: **Consistency**

---

### 2. Missing Values
- Missing financial fields in multiple records.
- Missing rejection reasons for some denied applications.

Dimension: **Completeness**

---

### 3. Duplicate Records
- Identified duplicate application IDs.
- Risk of double-counting in model training.

Dimension: **Accuracy**

---

### 4. Invalid or Suspicious Values
- Potential outliers in income and debt-to-income ratio.

Dimension: **Validity**

---

## Remediation Demonstrated
- Type conversion and validation
- Duplicate detection and removal
- Missing value reporting
- Numeric validation checks

---

# 2. Bias Detection & Fairness

## Gender Disparate Impact

We calculated the Disparate Impact (DI) ratio:

DI = Approval Rate (Unprivileged Group) / Approval Rate (Privileged Group)

Result:
- DI < 0.8 → indicates potential disparate impact under the four-fifths rule.

This suggests statistically significant gender-based disparities in approval decisions.

---

## Additional Bias Patterns

- Approval rate varies across age groups.
- ZIP code shows correlation with approval outcomes, indicating possible proxy discrimination.

ZIP code may act as a proxy for protected characteristics.


# 3. Privacy & Governance Assessment


## GDPR Risks

### 1. Personally Identifiable Information (PII) identified: 
- Full Name 
- Email Address 
- Social Security Number (SSN) 
- IP Address 
- Date of Birth
- ZIP Code

### 2. Missing GDPR Compliance Fields:
- Consent: The raw data lacks a "consent_timestamp", which violates the *GDPR Article 6* (Lawfulness of processing), since NovaCred is not able to show that applicants consented to the data processing.
- Data Retention: There is no "retention_until" date. And this violates the *GDPR Article 5* (Storage Limitation), which leads to unlimited storage of sensitive applicant data.

### 3. EU AI Act Classification:
- Under the EU AI Act, algorithms used for credit scoring are classified as *High-Risk AI Systems*. This requires mandatory fairness testing and strict human oversight.

## Governance Recommendations

1. **Implement Consent Mechanisms:** 
   - Add mandatory tracking for when and how users consent to data processing before their application is scored.
2. **Enforce Data Retention Policies:** Automate the deletion of PII for rejected applications after a legally justified period.
3. **Establish Audit Trails:** Log every automated decision with the specific model version and inputs used to ensure explainability.
4. **Require Human Oversight:** Implement a "Human-in-the-loop" policy where any algorithmic rejection is reviewed by a human agent to prevent automated discrimination.

# Conclusion

Our audit demonstrates that NovaCred faces:

- Data quality risks impacting model reliability
- Measurable disparate impact in lending decisions
- Significant GDPR compliance gaps
