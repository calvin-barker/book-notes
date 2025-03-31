# Data Matching: Concepts and Techniques for Record Linkage, Entity Resolution, and Duplicate Detection
Author: Peter Christen

*Disclaimer*: This is not a summary of the book. These are simply my notes and questions I have as I read. This probably won't be particularly useful for you if you don't read the book.

## Forward

### How would I describe naive Bayes to a five-year-old?

Imagine you have a big bucket of LEGO pieces. Some are red, some are blue, and some are yellow.

Let's say you know that:
- Red LEGOs are usually cars
- Blue LEGOs are often boats
- Yellow LEGOs are usually planes

Now, you pick up a LEGO piece you've never seen before. It's red. You don't know what it is for certain, but based on what you've seen before, what would you guess? A *car* because most red LEGOs are cars.

This is what Naive Bayes does. It looks at things it's seen before, remembers patterns, and when it sees something new, it makes a smart guess based on the colors (or words, or features) it knows.

It's called "naive" because it assumes all the clues it sees are independent -- kind of like assuming color is the only thing that matters, not shape or size. It's not perfect, but it's fast and usually pretty good at guessing.

### How would I explain the Fellegi-Sunter model to a five-year-old?

Imagine you have two boxes of cards, and each card has information about a person -- like their name, birthday, or where they live.

Now, your job is to match cards from one box to cards in the other box if they're the *same person*.

But sometimes, the names are spelled a little differently (like "Jon" instead of "John"), or maybe one card is missing a birthday.

So, how do you know if two cards are really about the same person?

The *Fellegi-Sunter* model is like a smart robot helper. For each pair of cards, it checks:
- Are the names similar?
- Are the birthdays similar?
- Do the addresses match?

Then the robot say:
- If everything looks very close: "Yes! Probably the same person."
- If nothing matches: "Nope, different people."
- If it's in-between: "Hmm, I'm not sure. Maybe you should look."

So, it's like a game of matching with fuzzy rules, and the robot uses match to help you decide if two things are the same, different, or need a closer look.

### Why does data matching (entity resolution) matter?

Without it, my data is lying to me.

Organizations collect information from many systems (CRM, transactions, web behavior, support logs, etc.), but none of it matters if we can't say, *"These two records actuall refer to the same real-world entity."*


Entity resolution is the foundation for a unified, accurate, and actionable view of people, businesses, and events.

### What can you do with it?

Some examples:

1. *Customer 360 & Personalization*: build complete profiles across fragmented systems; power targeted marketing, product recommendations, and retention strategies; improve support and customer satisfaction with context-rich interactions.
2. *Audience building and segmentation*: enrich targeting for ad platforms; group users by behavior, demographics, or lifecycle stage; avoid over/under-targeting the same individual due to duplicate IDs.
3. *Fraud detection & compliance*: identify when someone is attempting to open multiple accounts; detect collusion, duplicate vendors, or false identites; ensure accurate KYC/AML processes.
4. *Data deduplication & cost savings*: reduce storage, processing, and licensing costs by avoiding duplicate records, clean up messing warehouses.
5. *Measurement & attribution*: resolve ad impressions, clicks, and purchases to the same person for accurate ROAS; unify sessions, devices, or offline events across a customer journey.

#### Economic implications
- Better targeting means more revenue. Better resolution means that every targeting impression or campaign becomes more efficient and relevent.
- Less duplication means lower costs of data storage.
- Better activation of data means better outcomes for any industry where identity is valuable (ad tech, marketplaces, finance, retail, healthcare, etc.)

## Preface

*Data matching* is the task of identifying, matching, and merging records that correspond to the same entities from sveral databases.

The task of *duplicate detection* involves finding records that refer to the same entity within a single database.

Commonly used names include *record* or *data linkage*, *entity resolution*, *object identification*, or *field matching*.

What exactly is "knowledge engineering?" It's the processes of *building systems that can think or make decisions like a human expert* by giving them the right knowledge in the right way. How does this relate to AI? It's a foundational pillar to AI. AI is making machines act intelligently; classic AI built expert systems that followed rules and logic; knowledge engineering works with human experts to better define those rules and logic.

How quickly does the quadratic complexity become a problem? Very quickly. Over 1 million records, naive approches can become unusable.

| Number of Records (n) | Number of Comparisons | Time (at 100µs per comparison) |
|------------------------|------------------------|---------------------------------|
| 100                   | 4,950                  | 0.5 seconds                     |
| 1,000                 | 499,500                | ~50 seconds                     |
| 10,000                | 49,995,000             | ~1.4 hours                      |
| 100,000               | 4,999,950,000          | ~5.8 days                       |
| 1,000,000             | 499,999,500,000        | ~1.6 years                      |

## Chapter 1: Introduction

### 1.1 Aims and Challenges of Data Matching

*How would I explain data warehousing to a five-year-old?*

Imagine you have a giant toy box where you keep all your toys (LEGOs, stuffed animals, race cars, puzzles) all in one place, sorted, and organized, so you can find any toy really quickly when you want to play.

Now image that your toys come from different places (home, daycare, friends, grandma). You bring them all into this big toy box. You put labels on them and sort them into neat bins. Now, when you want to play with something, you know exactly where to look.

The big box is the data waarehouse.

*How would I explain data mining to a five-year-old?*

Now, imagine that your big toy box is full of toys that you've been collecting for a very long time.

You want to figure out what toys you play with the most, which ones are your favorites, if you have any of the same kind, if there's a new game you could make by combining them, etc.

Data mining is like being a detective in your toy box. You're looking for hidden patterns and cool surprises that you didn't notice before.

It's not about getting new toys -- it's about discovering cool stuff from the ones you already have.

Three tasks of integrating data:
1. Schema matching: identifying database tables, attributes, and conceptual structures from disparate databases that contain data corresponding to the same type of information 
2. Data matching: identifying and matching individual records from disparate databases referring to the same real-world entity
3. Data fusion: the process of merging pairs of groups of records that have been classified as matches into a clean and consistent record that represents an indentity.

*How would I explain an ontology to a five-year-old?*

Imagine you have a giant picture book that shows all the things in your world (animals, toys, people, food) and tells you how they're connected.

- It says a *dog* is an *animal*
- A *teddy bear* is a toy
- A *banana* is a *fruit*, and a *fruit* can be a *food*.
- Your *teacher* is a *person* who works at a *school*.

The picture book helps you understand:
- What things are
- What they do
- How they relate to each other

It's a way of organizing knowledge so everything has a place and so that we know how things are connected and different from one another.

#### 1.1.1 Lack of Unique Entity Identifiers and Data Quality

Examples of entity identifiers:
- Social Security Numbers (SSN)
- Taxpayer Identification Number (TIN)
- Passport Number
- Driver's License Number
- Unique Patient Identifier (UPI)
- Employee ID
- Student ID
- *Voter Registration Number*
- Insurance Policy Number
- Employer Identification Number (EIN)
- Universial Product Code (UPC)
- ISBN for books
- Vehicle Identification Number (VIN)
- Email address
- IP address
- Device ID
- Cookie ID
- Property Parcel Number (PPN)

#### 1.1.2 Computational Complexity

Make identity resolution more efficient with blocking, cheap similarity grouping, and parallelized scoring, backed by probabilistic or ML-drivien filtering.

- Blocking: group records by a shared feature (zip, email domain, first letter of name) and compare only within blocks
- Canopy clustering: use a cheap similarity metric (Jaccard) to form overlapping clusters/canopies, then only compare within clusters with the more expensive logic.
- Sorted neighborhood: sort the dataset on one or more keys (name + DOB) and then slide a fixed-size window over the records and compare only within the window.
- Pre-filtering with Heuristics: before fuzzy matching, filter reacord pairs that don't match on hgih-confidence features (ZIP + DOB) and are clearly disjoint on key fields (countries) to remove a big chunk of non-matches.
- Parallelization/distributed compute with something like Spark (Spark NLP/Dedupe workflows)
- Pairwise similarity pre-scoring: lightweight ML model that outputs a probability of a match: filter out low-confidence pairs, prioritize review queues, feed into Fellegi-Sunter-style scoring

#### 1.1.3 Lack of Training Data Containing the True Match Status

There are no real gold standards or ground truths for people. Some examples:
- Febrl (freely extensible biomedical record linkage): commonly used in ER research
- Restaurant/census datasets
- Walmart-Amazon product matching
- Mockaroo/Faker-generated datasets
- ERBench

#### 1.1.4 Privacy and Confidentiality

Zero-relationship encoding is used in privacy-preserving record linkage (PPRL) to prevent attackers from reverse-engineering or linking records based on structural clues. It ensures that encoded records don't expose clues about how they relate to one another.

### 1.2 Data Integration and Link Analysis

#### How would I explain "semantic and syntactic similarities" to a five-year-old?

Imagine you have two sentences or two words.

If they look or sound almost the same, that's called *syntactic similarity.* For example, if you see "dog" and "dogs," they look almost the same.

But, if they *mean* the same thing, even if they don't look alike, that's called *semantic similarity.* For example, "puppy" and "dog" look different, but they mean something very close.

Syntactic: about how things look.

Semantic: about what things mean.

#### How would I create a simple, accessible definition of "link analysis?"

Link analysis is the process of looking at how things are connected to each other, like dots and lines in a web, to find patters, relationships, or important pieces in a network.

It's like looking at a map of who talks to whom, or which websites link to each other, so you can find out who's most important, who's connected, or who's part of the same group.

#### How can LLMs accelerate schema matching?

They can automate processing messy field names, using context to piece together descriptions, table names, and sample data, reason over natural language (birth date, DOB, date-of-birth), learning from examples, automate manual mapping, and work flexibly across domains and languages.

### 1.3 A Short History of Data Matching

#### How is Dunn's dream of a book of life looking nowadays?

ChatGPT says it's becoming real, but it's distributed across tons and tons of databases including: genomics, wearable devices and smartphone tracking, medical records and claims, social graphs and transactional data (browsing, purchases, messages), etc. LLMs and identity graphs can help index the book of life with automated context gathering across all of them, but the data remains highly fragmented across different silos, privacy remains a serious concerns, and the comprehensiveness of much of life (emotions, relationships, one-off events) remains hard to digitize meaningfully.

### 1.4 Example Application Areas

#### 1.4.1 National Census

Given legal constraints in some countries around data retention, how can we overcome these challenges to create meaningful longitudinal data sets?

We can transform or summarize data instead of storing raw personal details. Techniques like pseudonymization, federated learning, or synthetic data also help retain value without holding onto sensitive records. The key would be to preserve insight and not identity.

What granularity of data can we get from the US Census Bureau?

Nation, region, state county; census track for neighborhood (1200-8000); block group (600-3000); census block (~40), but very limited variables and heavily privacy-protected -- population counts by total pop, sex/age, race/origin, household and housing units, group quarters population; public use microdata areas (PUMAs). Checkout SF1 by census.

#### 1.4.2 The Health Sector

What is the "best-practice protocol of separating the personal identifiers required for matching from the medical information needed for research studies" developed in Western Australia?

It's called the "separation principle" and is tied to WA's data linkage system (WADLS). Roles are clearly separated: data linkage unit (WA Data Linkage Branch handles only personal identifiers), researcher receive de-identified datasets with medical or research variables, the unit linking data never sees health content, and the researchers never see the direct identifiers.

Each individual is assigned a unique linkage key after records are matched. Linkage-keys are often project-specific and can't be reused across studies to re-identify people. There's strict governance and ethical oversight with auditing and logging.

#### 1.4.3 National Security

What's the current statuses of the total information awareness (TIA) and multistate anti-terrorism information exchange system (MATRIX) initiatives?

They were both discontinued in the mid-2000s. TIA was revealed in 2002, and Congress cut its funding in 2003. MATRIX drew concerns around privacy and was terminated in 2005.

There were about 500,000 people on US terror watchlists in 2008. Is this still the case? How effective is this?

As of 2023, it's up to about 2,000,000 (Terrorist Screening Database, TSDB). Effectiveness is a matter of debate. There haven't been major terror attacks on US soil, but the size of the data make it hard in magaing and utilizing data effectively.

#### 1.4.4 Crime and Fraud Detection and Prevention

*How do we define "synthetic identities?"*

These are fake identities created by combining real and fictious information, often to impersonate a plausible person who doesn't actually exist.

*If individuals can deliberately modify their details because they don't want to be identified, how can this impact targeted advertising, and how can advertisers mitigate this?*

This disrupts identity resolution and directly weakens targeted advertising, leading to fragmented user identities (one person looking like multiple different users), reduced ad personalization quality, reduces attribution models, wasted ad spend (i.e. targeting new instead of existing user) loss of meaasurement precision, etc.

Mitigations: adopt probabilisitc identity resolution (ML), leverage first-party data (loyalty programs, login systems), use privacy-compliant identity graphs (e.g. LiveRamp, Neustar) to link fragmented profiles into a coherent view, focus on contextual targeting instead of individual targeting (focus on what the individual is doing).

#### 1.4.5 Business Mailing Lists

*How effective are business mailing lists compared to online ads?*

Online ads seem to offer lower CPAs at scale, but mailing lists can outperform in niche, high-value B2B use cases.

#### 1.4.6 Bibliographic Databases

*How would I describe collective entity resolution to a five-year-old?*

Imagine you have a big box of LEGO people, and some of them look really similar, like they might be the same person wearing a different hat. You try to figure out who is who. But instead of looking at just one piece at a time, you look at all their friends, where they live, and what they're doing. That way, you can tell if two LEGO people are actually the same person in diguise, because they're always hanging out with the same friends or going to the same places.

*This seems like household resolution might be pretty useful, even if I'm not targeting the household. Is this correct?*

Yes, collective entity resolution helps by looking at relationships between people, like shared addresses or behaviors, to improve identity matching. Even if you're targeting individuals, it can reveal household connections, reduce duplicates, and uncover shared intent. This leads to smarter targeting, better attribution, and lower acquisition costs. (I wonder if household size matters.)

#### 1.4.7 Online Shopping

*How saturated is the Shopify app space for price tracking/comparison apps?*

It appears to be "moderately" saturated: see Prisync, Pricefy, Price Compare, PriceMole, GJ Price Tracker.

#### 1.4.8 Social Sciences and Geneaology

### 1.5 Further Reading
