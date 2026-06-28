
Capstone Project Pre-Submission Document
Proactive Learner Success & Dropout Risk Intelligence for LearnSphere Education Technologies
IITM – JARO Batch 02  |  Capstone Project  |  Domain: EdTech Learner Analytics
1. Business Problem Understanding & Objective Formulation
LearnSphere is an online learning platform offering certifications, university partnerships, and corporate training. Despite rising enrollments, leadership faces declining course completion and rising dropout rates, with interventions currently reactive rather than predictive. The objective is to shift to a proactive student-success strategy using behavioral and engagement data already collected across the LMS, assessments, forums, attendance, and support systems.
Business Objectives
•	Reduce dropout by identifying at-risk learners early enough for meaningful intervention.
•	Increase completion and certification rates across self-paced, instructor-led, and bootcamp formats.
•	Strengthen enterprise trust by improving completion metrics reported to corporate training clients.
Analytical Objectives
•	Diagnose: Quantify how attendance, study time, video/assignment completion, and engagement relate to learner outcomes.
•	Predict: Build a classification model that estimates dropout / non-completion risk per learner, early in the course lifecycle.
•	Prescribe: Translate risk scores into a prioritized, actionable early-warning dashboard for student-success teams.
Preliminary Evidence from the Dataset (n = 50,000 after de-duplication)
•	Behavioral gap between completers and non-completers: completers show 20–32% higher values across every core behavioral metric — quiz scores (+32.3%), video completion (+32.1%), assignment completion (+26.6%), study hours (+20.6%), attendance (+20.2%), and engagement score (+20.4%).
•	Non-linear, threshold-driven risk: success rate is flat at ~45–50% across the bottom 7 attendance/engagement deciles, then jumps to ~70–78% in the top 3 deciles — risk is not gradual, it concentrates at a clear behavioral cliff.
•	Compounding risk: learners in the bottom 3 deciles of both attendance AND engagement (9.1% of learners) succeed at only 37.8%, versus 88.8% for learners in the top 3 deciles of both — a >50-point swing, suggesting combined low-engagement learners are the highest-value intervention target.
•	Independent, additive signals: pairwise correlations among the six core behavioral features are all ≈ 0.00 — each metric carries distinct information, supporting a multi-feature model over any single proxy metric.
•	No meaningful signal in demographics or course design: success rates are flat (54.5–55.6%) across course category, difficulty, and enrollment type, and show no pattern with support-ticket volume — ruling these out as primary drivers and re-confirming that behavior, not background, predicts outcome.
2. Challenges and Constraints
Data Quality Issues Identified in Initial Review
•	Target leakage: The field certificate_issued_flag is mathematically identical to the target student_success_flag (Pearson r = 1.00, 0 mismatches across 50,000 rows) and must be excluded from modeling — it is an outcome, not a predictor.
•	Inconsistent categorical encoding: the employment_status field encodes “Professional” as three case variants — “Professional” (12,506), “professional” (650), “PROFESSIONAL” (659) — fragmenting roughly 2.6% of records into duplicate categories that must be merged before use.
•	Duplicate records: 500 of 50,500 rows (0.99%) are exact duplicates and were removed prior to all analysis above.
•	Legacy / low-signal identifiers: fields such as legacy_student_reference, administrative_code, batch_reference, and learning_cluster_id show correlation with the target below |r| = 0.006 — statistically indistinguishable from noise — and appear to be inherited administrative artifacts.
•	Missing values: mentor_interaction_count is missing in 9.93% of rows and feedback_score in 10.02%, both requiring an explicit imputation strategy (e.g., median or model-based) rather than row deletion, given the volume affected.
Modeling Constraints & Assumptions
•	Demographics carry no usable signal (flat correlation, |r| < 0.01), so the model must rely entirely on behavioral/engagement features — raising the bar on feature engineering.
•	Engagement metrics are static snapshots, not week-by-week trajectories, making true early-course (vs. end-of-course hindsight) prediction harder to validate without timestamps.
•	Risk scores must remain interpretable for non-technical staff and free of bias against demographic or course-category cohorts.
•	student_success_flag is assumed a reliable proxy for completion/certification; behavioral logs are assumed accurately captured by source systems.
3. Background Study / Literature Review
Student dropout prediction is a well-studied problem in Educational Data Mining (EDM) and Learning Analytics. Prior work consistently shows that behavioral engagement signals — LMS login frequency, video/assignment completion, forum participation, and assessment performance — outperform static demographic attributes as predictors of dropout, which is consistent with the correlation patterns observed in this dataset.
Key Techniques Commonly Used in Industry/Research
•	Classification models: logistic regression for interpretable baselines; ensemble methods (Random Forest, Gradient Boosting/XGBoost) for higher accuracy on tabular behavioral data.
•	Early-warning systems (EWS): MOOC platforms (Coursera, edX) and university tools (e.g., Purdue’s Course Signals) use weekly engagement deltas, not cumulative totals, to flag at-risk learners early.
•	Survival analysis & clustering: survival/time-to-event models predict when a learner is likely to drop, while unsupervised clustering (k-means) separates personas such as “engaged-but-struggling” vs. “disengaged-early” for targeted outreach.
•	Explainable AI (SHAP/LIME): widely adopted in EdTech to make risk scores interpretable for advisors and instructors, supporting adoption and trust.
Best practice converges on a proactive, interpretable, intervention-linked pipeline — directly aligned with LearnSphere’s goal of moving from reactive to proactive student success management.
4. Proposed Methodology / Solution Approach
The proposed approach follows a standard CRISP-DM-aligned analytics pipeline, augmented with an explicit data-leakage audit (given the certificate-flag finding) and a closed intervention feedback loop:
 
Figure 1: End-to-end workflow — from raw multi-source data to dashboard-driven intervention, with a feedback loop for continuous retraining.
Workflow Summary
•	Data Engineering: merge multi-source data, deduplicate, standardize categorical encodings, impute missing values, and remove leakage fields.
•	EDA: correlation analysis, cohort/segment profiling (by course category, difficulty, enrollment type) to surface behavioral drivers of success.
•	Modeling: train and compare logistic regression (interpretable baseline) against tree-ensemble models (Random Forest / Gradient Boosting), using engagement, attendance, study-hour, and assessment features as primary predictors.
•	Evaluation: assess with AUC-ROC, precision/recall (prioritizing recall on the at-risk class, since missed at-risk learners are costlier than false alarms), and basic fairness checks across demographic groups.
•	Deployment Layer: convert model output into a per-learner risk score feeding a dashboard for student-success and instructor teams, paired with a recommended intervention (outreach, mentoring, workload adjustment).
•	Feedback Loop: track post-intervention completion outcomes to validate impact and periodically retrain the model.
5. Proposed Timeline
A 7-week execution schedule is proposed, covering data understanding through final reporting:
Week	Activity	Milestone
1	Data understanding, cleaning, leakage audit, standardization	Clean, leakage-free dataset
2	Exploratory Data Analysis & cohort/segment profiling	EDA report with key drivers
3–4	Feature engineering & predictive model development	Baseline + ensemble models trained
5	Model evaluation, tuning, and fairness checks	Validated final model
6	Dashboard design & risk-scoring integration	Working early-warning dashboard
7	Business recommendations, documentation & reporting	Final report & presentation

6. Expected Deliverables / Outputs
•	Data Analysis Report: EDA findings, key dropout drivers, and cohort-level insights.
•	Predictive Model: trained dropout/non-completion risk classifier with documented evaluation metrics.
•	Early-Warning Dashboard: visualization of learner risk scores and contributing factors for student-success teams.
•	Business Recommendations: prioritized, data-driven intervention strategies for at-risk learner segments.
•	Technical Documentation: data dictionary, cleaning steps, modeling methodology, and assumptions.
•	Final Presentation: summary of approach, findings, and business impact for stakeholders.
•	Deployment Proposal: outline for integrating the risk-scoring engine into LearnSphere’s existing LMS workflow.
