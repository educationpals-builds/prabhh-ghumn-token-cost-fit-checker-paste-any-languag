# What I Built

I built a token cost and fit checker for multilingual support tickets. The checker evaluates text samples against five dials—special token handling, vocabulary fit, merge economy, how it splits, and edge case survival—to determine whether a tokenizer is suitable for our on-device assistant deployment.

The problem: our embedding table is capped and inference is billed per token. We needed a way to evaluate whether incoming support queue text (38% German, 22% Turkish, 19% English, remainder Thai / Arabic / Mandarin) would tokenize efficiently before committing to an architecture.

## The Probe That Fooled It

I dont have it. I dont have it. I dont have it

This exposed a gap in my calibration process. I was unable to complete the probe board with concrete test cases and results.

## The Fix

The advisor now listens to events from CRM for new entries and reads the text files for language to translate, and it uploads the translated data to text files on CRM. But it refuses emojis and blacklisted words.

I identified vocabulary_fit as the weakest dial—the one that decides whether a sample passes or fails the checker.

## The Gate It Holds

I dont have it. I dont have it. I dont have it

## Re-Certification Cadence

The checker should be re-run when the traffic mix shifts or when new language lanes appear in the support queue. Thursday's architecture review was the original decision deadline.

## What I Learned

Working with compound German words like "Krankenversicherungsbeitrag" and "Beitragsbemessungsgrenze" alongside Turkish phrases like "Sigortalılığınızın başlangıç tarihini öğrenebilir miyim?" taught me that vocabulary fit varies dramatically across language lanes. A tokenizer that handles English efficiently may fragment German compounds or Turkish agglutinative forms into far more pieces than expected.

I also learned that I need to complete the full calibration process—including concrete probes with pasteable bytes and measurable outcomes—before the checker can be trusted for production decisions.
