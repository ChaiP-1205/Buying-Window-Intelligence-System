Buying-Window Intelligence System

A working prototype that finds sales-ready accounts by reading long unstructured documents that standard GTM tools cannot process.

Built as an interview artifact for a GTM Engineering role at Pinnacle Reliability. Everything in this repo is real: real source documents as input, real AI processing, real scored output.

View the interactive walkthrough

Watch the 90-second demo video

The 60-second version

If you have worked in GTM or RevOps, you already know the standard signal stack: Clay, Apollo, ZoomInfo, LinkedIn Sales Navigator. These tools work because SaaS buying signals (funding rounds, hiring bursts, tech stack changes, SOC-2 renewals) live in structured, packaged databases. A marketer can build a scored ICP list in an afternoon.

Now imagine you sell into an industry where none of those signals exist in those tools. Your buyers are large industrial facilities like oil refineries and chemical plants. The signals that tell you an account is entering a buying window are things like:

A recent safety incident at their facility, so the buyer is now under pressure to fix things
An announced upcoming shutdown for major maintenance, meaning a long planning cycle just opened
Mentions of equipment reliability spending in earnings calls
A new senior leader joining the safety or reliability function

Every one of those signals lives inside long text documents: government reports, transcripts, filings, trade press. None of it is neatly packaged. Clay cannot read a 60-page PDF and extract the specific facts an SDR needs to run a relevant outbound sequence.

This prototype reads those long documents automatically, extracts the key facts using AI, and produces a scored account list of who is likely in a buying window right now. In effect, it does for this industry what Clay does for SaaS GTM, but for signal sources no vendor packages.

Walk the prototype yourself, no code required

You can inspect every input, every output, and the code without running anything.

1. Inputs: real government safety reports

The input/ folder contains 8 actual PDFs the pipeline processed. Each is an official investigation report from the US Chemical Safety Board, a federal agency that investigates major accidents at industrial facilities, similar to how the NTSB investigates plane crashes.

File	What it covers
incident_1_bp_husky_oregon.pdf	Refinery fire, 2022
incident_2_husky_superior.pdf	Refinery explosion, 2018
incident_3_exxon_baton_rouge.pdf	Refinery unit fire, 2016
incident_4_pes_philadelphia.pdf	Refinery explosion, 2019
incident_5_dow_louisiana.pdf	Chemical plant explosion, 2023
incident_6_pemex_deer_park.pdf	Refinery chemical release, 2024
incident_7_kmco_crosby.pdf	Chemical plant fire, 2019
incident_8_wacker_polysilicon.pdf	Chemical plant release, 2020

These documents are 40 to 100 pages of technical prose. Standard enrichment tools cannot process them.

2. Processing: see the code

The full pipeline lives in pipeline/pdf_scraper_script.py. Five stages:

Load each PDF as raw bytes
Send to Google Gemini AI with a strict prompt tuned to Pinnacle's product
Extract structured information as clean JSON: which company, which facility, what went wrong, and whether it is the kind of problem Pinnacle's software (QRO / Newton) is built to prevent
Score each incident on a 0 to 100 urgency scale, a proxy for how open the buying window is
Output a sorted CSV ready to import into a CRM
3. Outputs: real scored account list

Click either file to view directly in the browser:

outputs/pinnacle_csb_signals.csv, the scored account list ready for HubSpot import
outputs/pinnacle_csb_signals.json, the full extraction with AI reasoning per row
4. Proof it ran

The screenshots/ folder has captures of the actual pipeline run and the CSV opened in Google Sheets. Nothing was hand-edited.

What the output tells us

Top 3 accounts for outbound this week:

Rank	Company	Facility	Score	Why they are a fit
1	Husky Energy	Superior Refinery, WI	70	Equipment wore out over time in a way Pinnacle's software would have flagged in advance
2	KMCO	Crosby, TX	70	Component failed from cumulative stress, exactly the failure type Pinnacle predicts
3	Dow Chemical	Plaquemine, LA	57	Sensor data had been showing warning signs for over a year

Here is the interesting result. ExxonMobil is one of the largest and most desirable logos in this ICP. One ExxonMobil incident was in the input set. It correctly ranked last. Why? The AI read the report and figured out it was caused by a worker unbolting the wrong piece of equipment during maintenance, not the kind of gradual equipment failure Pinnacle's software addresses. A basic keyword filter would have flagged it as an MQL. This engine was smart enough not to, which is exactly the difference between fit scoring and firmographic scoring.

That is the whole value: sending SDRs after the right accounts, not just the big-name accounts.

Why this matters for a GTM team

Before this engine, an SDR opens the CRM Monday morning and sees a target list of a few hundred accounts. No account-level intent, no idea which ones are actually in a buying window. So the sequence goes out to everyone the same way and reply rates stay low.

After this engine, the same SDR opens the CRM and sees a handful of accounts flagged with a live trigger event, the specifics of what happened, why it matters, and a suggested talk track. MQL to SQL conversion improves because the leads reaching sales already have a real reason to be relevant. Deals move faster through the pipeline because the AE walks into discovery already knowing the account's pain.

The end goal: predictable outbound at the account level, closed-won deals traced back to specific triggers instead of guesswork.

What is not built yet (the honest roadmap)

This prototype answers "which accounts to target." A production version also needs:

Contact discovery. Knowing the facility is not the same as knowing the buyer to email. Standard tools like Apollo have patchy coverage for site-level engineering titles. Next build: a custom enrichment layer combining Apollo with industry directories, trade publications, and LinkedIn Sales Navigator.
More signal sources. One source is a demo. Real defensibility comes from fusing many: shutdown announcements, earnings call mentions, leadership hire tracking, insurance renewal windows, each one widening the top of funnel with better-fit accounts.
AI-drafted first-touch outreach. Once we know the account, the trigger, and the contact, drafting a personalized first email that references the specific event is a natural extension of the same AI pattern.
Closed-loop attribution. The scoring weights right now are educated guesses. Once the engine runs alongside real pipeline for a few months, we can measure which signals actually predicted closed-won and recalibrate. Scoring gets sharper every quarter with no new engineering.

Together these create a signal engine competitors cannot buy from any vendor, which is the real moat in a market this concentrated.

Stack
Python 3 for the pipeline logic
Google Gemini 3.6 Flash for reading PDFs (native multimodal)
Google Colab for reproducible execution
Output format: CSV ready for HubSpot import



Built by Chaitrali Patne, GTM Engineer.
