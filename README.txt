Reproducible exploratory analysis of compiled microplastic decision records

INPUT

Four_Reviewer_Analyst_Reference_Aligned.xlsx
SHA256: ce03124d396f53a6caa82c4e5f5b03d593ad2878c4de06ede9db63809f6b0640

The analysis reads but never writes the workbook. The archived questions,
eligibility labels, gate states, rationales and worksheet names are unchanged.
The workbook is a compiled record, not the original reviewer input forms.
The source file is supplied separately or in the accompanying input folder.

RUN

Use Python 3.12 (the verified run used 3.12.14) with the packages pinned in
requirements.txt. The numerical package versions are also recorded in
results/results.json under environment.

python -m pip install -r requirements.txt
python analysis.py --input input/Four_Reviewer_Analyst_Reference_Aligned.xlsx --output reproduced_results

The output directory is created if needed. Existing files with the same output
names are replaced. No input worksheet is recalculated, edited or exported.

To reproduce Figure 1 from the computed JSON, also install matplotlib and run:

python make_figure.py --results reproduced_results/results.json --output reproduced_figures

This produces PNG, PDF and SVG files. The script derives every statistical
count from results.json; labels and the 8.6 by 5.4 inch figure design are fixed.
The verified plotting version is matplotlib 3.10.8. The PNG reproduces the
manuscript figure exactly under the recorded runtime and fonts. Export metadata
in PDF/SVG can vary between executions.

ANALYTIC INPUTS

02_CASES!A1:F49: identifiers, question type, archived specified use and matrix.
02_CASES!H2:K49: literal eligibility states.
03_GATE_MATRIX!D2:AA49: literal gate states, joined by Question_ID and checked
against exact Paper_ID and DOI.
04_GATE_EVIDENCE: redundant gate states, archived rationale strings and locators.

Final classifications are independently reconstructed from eligibility and
gate states in Python. Formula texts and cached outcome values are inspected
as audit comparators. Cached kappa/concordance sheets are not statistical inputs.
This is not an Excel-engine recalculation or independent scientific adjudication.

RULE

Ineligible records retain Ineligible status. For eligible records with six
complete gates, any Fail yields Not supported; otherwise any Unresolved yields
Unresolved; otherwise the outcome is Supported. Not required and Pass remain
distinct gate states. An ineligible blank is never converted to Not required.

PRIMARY DESCRIPTIVE ANALYSIS

Eligibility: 48 questions and four compiled records.
Classification/gates: the 43 questions eligible in all four records.
Nominal agreement: exact four-record unanimity, mean of the six pairwise
agreements, Conger kappa with separate fixed-rater margins, and Fleiss kappa
with pooled margins. Kappa is undefined when expected agreement equals one.
Overall, Limited and Stronger results are reported separately.

Bootstrap: 20,000 draws, NumPy PCG64 seed 20261014. Exact retained DOIs are sorted
lexicographically. Each draw samples 22 DOI clusters with replacement and keeps
all questions in a selected cluster; A-D remain fixed. The same cluster draws
are used for Overall and Limited. Intervals are the 2.5th and 97.5th percentiles
of finite estimates using numpy.quantile(method='linear'). Exact cluster order
is stored in results.json. Undefined draws remain missing in the CSV; they are
counted and excluded from percentile calculation, never replaced with zero.

These bootstrap ranges describe the purposively selected compiled records.
DOI clusters are not independently verified dataset or sampling-campaign
families; resampling does not resolve selection or provenance limitations.

POST HOC DIAGNOSTICS

1. Question-gate disagreement: unanimity; disagreement involving only Pass
   and Not required; or other disagreement. The latter is not a label of error.
2. Full six-gate vectors: four-record unanimity and pairwise Hamming distance.
3. Type-only descriptive comparator: Stronger -> Not supported, Limited ->
   Supported. Match to derived labels is not scientific accuracy.
4. Leave-one-gate-out: temporarily omit each gate from the deterministic rule
   and recompute outcomes. This measures rule sensitivity in these records;
   it does not justify deleting a gate or show that the new outcomes are valid.
5. Leave-one-DOI-out: omit every question from each retained DOI, then recompute
   Overall and Limited agreement. This is a descriptive influence analysis.

The archival analyst reference is reconstructed under the recorded voting and
override rules and excluded from all agreement calculations. It is not an
independent fifth rater, adjudication panel, or truth standard.

OUTPUTS

results.json: complete machine-readable findings and methods metadata.
audit_summary.json: compact headline findings.
questions.csv: all 48 archived questions, identifiers and derived outcomes.
ratings.csv: all 1,152 question-gate-rater positions, including 42 retained
ineligible blanks, with source cell and archived rationale.
rater_question_outcomes.csv: 192 records linking six gates to each outcome.
agreement.csv: eligibility and stratified classification agreement.
gate_agreement.csv: agreement for each gate and question stratum.
question_gate_disagreement.csv: 258 common-eligible question-gate units.
question_diagnostics.csv: question-level unanimity and shared Fail states.
rater_pair_vectors.csv: 258 pairwise six-gate-vector comparisons.
bootstrap_draws.csv: each Overall and Limited bootstrap estimate and sample n.
leave_one_gate_out.csv: summaries after each temporary gate omission.
leave_one_gate_out_changes.csv: every outcome that changes under omission.
leave_one_doi_out.csv: all 44 source-omission comparisons.
formula_audit.csv: formula-pattern and independently derived cache checks.
reference_audit.csv: all 258 analyst-reference gate checks.
rationale_prefix_mismatches.csv: explicit state/rationale-prefix inconsistencies.
archived_codebook.json: the six original worksheet codebook rows, unmodified.

LIMITS

The analysis reproduces numerical results from a compiled workbook. It does
not authenticate the original raters, reconstruct unavailable original forms,
verify all scientific source interpretations, or assess decision accuracy.
No archived rating is overwritten using a rationale or a source check.


PACKAGE ORGANIZATION

Run the commands above from the extracted Microplastic_Exploratory_Analysis
directory. Supplied results/ and figures/ preserve the reviewed outputs;
reproduced_results/ and reproduced_figures/ are new output locations.

input/ contains the unchanged compiled workbook.
results/ contains the 18 numerical extraction and audit CSV/JSON files.
codebook/ contains the exact archived definitions, the proposed specification
in text and JSON, a blank decision-record form, and four constructed examples.
sources/source_checks.csv indexes three targeted source inspections; findings
and their limits are reported in the manuscript and Supplement S8.
figures/ contains the reviewed Figure 1 in PNG, PDF, and SVG formats.
checksums.sha256 records every supplied payload file except itself.

VERIFICATION

An independent execution reproduced all 18 CSV/JSON outputs byte-for-byte.
The unchanged input checksum was checked after execution. The standalone
figure script reproduced the supplied PNG byte-for-byte under the recorded
environment. These computational checks do not authenticate assessor identity,
rating independence, or the scientific validity of archived interpretations.

The expanded operational specification is proposed for future applications.
It was not used to generate or recode the archived assessment records. The
four constructed examples are illustrative calculations, not new environmental
observations. Primary article PDFs and unavailable original assessor forms are
not included. Publication and data-sharing permissions remain for the authors
to confirm before public repository deposit.
