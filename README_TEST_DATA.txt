Sample Dataset / Testing Data — TalentMatch AI (Task AI-2)
============================================================

This folder contains a realistic test dataset to verify the resume screening
and ranking system end-to-end.

FILES
-----
sample_job_description.txt   - A sample Python/ML developer job posting
resume_1_alice_johnson.txt   - Strong match: 4 yrs Python/ML experience
resume_2_brian_lee.txt       - Moderate match: backend dev, light ML exposure
resume_3_carla_mendes.txt    - Weak match: UI/UX designer (different field)
resume_4_david_okafor.txt    - Strongest match: senior ML engineer, 5+ yrs
resume_5_emma_wilson.txt     - Junior match: recent grad, internship-level

expected_output_rankings.csv  - Verified output from an actual run of
                                 resume_screener.py against the files above
expected_output_report.json   - Same run, full JSON report with score breakdowns

HOW THIS WAS GENERATED
-----------------------
These expected_output files were NOT hand-written. They were produced by
actually executing the Python backend:

    python resume_screener.py --jd sample_job_description.txt \
        --resumes . --min-exp 3 --priority balanced \
        --out-prefix expected_output

This serves two purposes:
1. You can immediately test the web app or CLI tool using real files
   instead of needing to find/create your own resumes first.
2. The expected_output files act as a regression check — if you modify
   the scoring logic, you can re-run this same command and compare against
   these files to see exactly what changed and why.

EXPECTED RESULT (for sanity-checking your own test runs)
----------------------------------------------------------
Rank 1: David Okafor   (~74.7% match) - senior ML engineer, strongest fit
Rank 2: Alice Johnson  (~68.5% match) - solid Python/ML background
Rank 3: Brian Lee      (~39.9% match) - backend dev, partial skill overlap
Rank 4: Emma Wilson    (~30.6% match) - junior/recent grad, some overlap
Rank 5: Carla Mendes   (~23.8% match) - different field (UI/UX), weakest fit

Note: exact percentages may vary slightly by +/-1-2% across environments
due to floating point rounding, but the RANKING ORDER should remain stable.

CROSS-ENGINE VERIFICATION
---------------------------
The web app (browser JavaScript) and the Python CLI use two independently
implemented scoring engines (same methodology, different languages). Both
were run against this exact dataset and produced the SAME ranking order:

    Python CLI : David(74.7%) > Alice(68.5%) > Brian(39.9%) > Emma(30.6%) > Carla(23.8%)
    Browser JS : David(79%)   > Alice(72%)   > Brian(47%)   > Emma(36%)   > Carla(28%)

The exact percentages differ slightly because the two are separate
implementations (not shared code), but the RELATIVE RANKING is identical,
which is what matters for a recruiter using the tool. This was verified by
actually executing both engines against this dataset, not just inspecting
the code.

HOW TO USE THIS DATA IN THE WEB APP
-------------------------------------
1. Open index.html in your browser
2. Step 1: paste the contents of sample_job_description.txt
3. Click "Analyze Job Description & Continue"
4. Step 2: upload all 5 resume_*.txt files at once (multi-select supported)
5. Click "Rank Candidates"
6. Compare your results against the EXPECTED RESULT section above
