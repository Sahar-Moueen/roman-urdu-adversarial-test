[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Sahar-Moueen/roman-urdu-adversarial-test/blob/main/roman_urdu_adversarial_test.ipynb)

During my internship at Valyrian Systems, we built a system to flag harmful content. It worked on our test data, but I always wondered what would happen if someone tried to get past it on purpose. This is my small attempt to test that in Roman Urdu, where scams and abuse in Pakistan are often written and nobody spells words the same way.

## What I did

- Model: `textdetox/xlmr-large-toxicity-classifier` (score 0.5 or above = harmful)
- 100 Roman Urdu test messages made for this experiment: 50 harmful (scams, threats, insults) and 50 safe
- Changed the spelling of harmful messages in five ways: vowel swaps (bekaar → bekar), leetspeak (b3k@@r), spaced letters (b i l k u l), stretched vowels (ghatiiyaa), and a mix
- Measured evasion rate: how many messages the model caught before but missed after

## What I found

- Before any attack: precision 0.65, recall 0.66
- It caught 93% of insults and 80% of threats, but only 35% of scams
- It flagged 36% of safe messages, even "baarish ho rahi hai, chhatri le kar jana"
- Stretched vowels let 30% of caught harmful messages through, and half of the threats
- "tumhari dukaan jala dein ge agar bhatta nahi diya" dropped to a score of 0.002 after a few letters became symbols

| Attack | All | Insult | Scam | Threat |
|---|---|---|---|---|
| vowel_swap | 0.00 | 0.00 | 0.00 | 0.00 |
| leetspeak | 0.18 | 0.07 | 0.14 | 0.33 |
| spaced_letters | 0.21 | 0.00 | 0.14 | 0.50 |
| repeat_vowels | 0.30 | 0.21 | 0.14 | 0.50 |
| combined | 0.30 | 0.21 | 0.29 | 0.42 |

![Recall under attack](results_chart.png)

One surprise: some attacks made recall go up, because symbols made the model flag scams it had missed. So it seems to judge how a message looks, not what it says.

## Limitations

Only 100 messages, written for the test, one model and simple random attacks. Real messages could give different numbers.

## Next

A bigger, real-looking dataset, scam-specific models, and simple defences like spelling normalisation and adversarial training.
