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


