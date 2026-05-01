---
title: "Activity 5 Reflection - Constituent Services Hub"
type: reflection
version: "1.0.0"
---

# Activity 5 Reflection

Answer each question in 3-5 sentences. Thoughtful, specific responses earn full credit.

## 1. PII Redaction in Practice

What PII categories did the Azure AI Language service detect in the Memphis 311 complaints? Did you notice any false positives (non-PII flagged as sensitive) or false negatives (PII that was missed)? How might PII detection requirements differ between a government agency processing citizen complaints and a commercial customer service system?

AI Language was able to detect PII across the 311 complaints with no obvious false positives. A government agency definitely has stricter requirements than a commercial system when it comes to PII (Which is understandable but also I won't get into my personal beliefs on that especially with how common data breaches are), and redacting PII before storage is a l;egal requirement. Government usage is much more broadly required than private/commercial.

## 2. Sentiment as a Routing Signal

How could sentiment analysis be used to prioritize Memphis 311 complaints — for example, routing highly negative complaints to senior staff? What are the risks of relying solely on sentiment for prioritization (consider complaints that are neutral in tone but urgent in nature)? How would you combine sentiment with other signals for a more robust routing system?

Sentiment analysis could flag highly negative complaints for priority review, which prioritizes complaints possibly more likely to be escalated to representatives or loical media. The main reisk is that neutral complaints can describe genuinely urgent situations, and also that some people refuse to seem aggressive at all when making a legitimate complaint out of politeness, which can skew priorities. A more robust system would combine sentiment analysis with key phrase matching, possibly?

## 3. Multilingual Challenges

How accurately did the Language service detect the language of short versus long text samples? What challenges might arise with code-switching (mixing languages in a single message), which is common in multilingual communities? How would you handle a complaint where language detection confidence is low?

Language detection is more accurate for longer text but confidence dropped for shorter messages where there are fewer words to work with. Mixing languages in a single message, common in many of the multicultural communities in Memphis, could possibly be detected as one language, possibly missing nuance. For lower confidence detections, a human reviewer is most definitely a necessity.

## 4. CLU vs. Keyword Matching

Compare the CLU model's intent classification with the keyword-based fallback. In what scenarios did CLU perform better, and where did keyword matching suffice? What are the trade-offs of training and maintaining a custom CLU model versus using a simpler rule-based approach for a city's 311 system? *(If CLU was not configured in your environment, discuss how you would expect it to differ based on the training data in `data/intent_examples.json`.)*

Based on the training data, CLU would definitely outperform keyword matching on ambiguous phrasing, with many phrases would score as check status via CLU but also be missed by keyword rules. Keytword matching works more reliably on direct complaints, but CLU would be the better choice for production with keyword matching as a degradation fallback in the case of an outage or other issues.

## 5. Pipeline Design

Which step in your NLP pipeline was most critical to get right first, and why? If you were deploying this pipeline for production use handling thousands of complaints daily, what monitoring, fallback mechanisms, or error handling would you add? How would you measure pipeline health over time?

PII redaction was the most important part because it gates every step after. A buig in this part of the pipeline could potentially expose sensitive data. In production handling thousands of complaints daily, per step latency monitoring alerting when error rates exceed a certain threshold, and dead letter queuing to ensure that no complaints are dropped in error would be essential. Pipeline health over time would most efficiently be measured by a labeled holdout set monthly, alerting distribution of anything drifgting significantly over baseline.
