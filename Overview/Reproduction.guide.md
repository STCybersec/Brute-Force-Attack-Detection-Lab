# Reproduction Guide (step-by-step)

Follow these steps in order to reproduce the lab exactly as shown in the screenshots.

---

# Step-by-Step Reproduction Guide — SSH Brute-Force Detection Lab

## Step 1 — Upload Dataset to Splunk (Add Data → Upload)

- Splunk Web → **Settings → Add Data → Upload**
- Select `Dataset/Auth_log_labeled.csv`
- Set **sourcetype**: `csv` (or create `brute_csv`)
- Set **index**: `brute-force-logs` (create if necessary) or use `main`
- Finish and **Start Searching** to validate data ingestion.

 **Verify:**
 ```spl
 index="brute-force-logs" | head 10
 Confirm fields: `message`, `src_ip`, `user`, `label`.
```

## Step 2 — Create Saved Searches / Reports (Queries)

For each file in `Queries/`:

1. Open **Search & Reporting**
2. Paste the SPL from `Queries/Attacker_IPs_Used.spl` (or other `.spl` files)
3. Click **Save As → Report** → Name clearly (e.g., `BF - Attacker IPs`)
4. Repeat for:
   - `Top_5_Failed_Attempts_Over_Time.spl`
   - `Most_Targeted_Users.spl`
   - `Failed_To_Success_Logins.spl`

**Tip:** Replace `index=brute-force-logs` if you used a different index.

## Step 3 — Create Alerts (Optional)

1. Open a saved report (e.g., `Top_5_Failed_Attempts_Over_Time`)
2. Choose **Save As → Alert**
3. Configure schedule (every 1–5 minutes for lab)
4. Trigger condition: **If number of results > 0**
5. Configure actions:
   - Email
   - Webhook
   - Run script (lab demo: dummy script writes a local file)

## Step 4 — Import Dashboard JSON (Optional but Recommended)

1. Splunk Web → **Dashboards → Create New**
2. Click **Edit → Source** (Dashboard Studio)
3. Paste contents of `Dashboard/Brute_force_login_attempts_Splunk.json`
4. Save
5. Edit panels if needed to point to your index (`brute-force-logs`)

## Step 5 — Validate and Capture Evidence

1. Run saved searches and confirm results match screenshots in `Screenshots/`
2. Run timeline query: `Failed_to_success_logins.spl`
3. Export CSV evidence:
   - Search → Export → CSV
   - Save under `evidence/`

## Step 6 — Playbook Walkthrough (IR)

- Follow `Icident_Response/IR_SSH_Brute_Force_Detection.md` for triage, containment, evidence capture, and remediation.
