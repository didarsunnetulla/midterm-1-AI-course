# Midterm 1: answers

GitHub username: didarsunnetulla
Session code: ZEBRA-28
Fingerprint: c4c34c79

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used ChatGPT to understand the assignment, organize my answers, explain the formulas, and check my calculations. Python was used to generate the data and calculate the metrics. I will review the results to prepare for the oral check.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| **config** | **recall@1** | **recall@3** | **recall@5** | **MRR@10** |
| ---------- | ------------ | ------------ | ------------ | ---------- |
| A          | 0.4500       | 0.5500       | 0.6667       | 0.5344     |
| B          | 0.4000       | 0.5833       | 0.8167       | 0.5448     |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| **candidate** | **recall@k** | **lower** | **upper** |
| ------------- | ------------ | --------- | --------- |
| A, k=3        | 0.5500       | 0.4249    | 0.6691    |
| A, k=5        | 0.6667       | 0.5406    | 0.7727    |
| B, k=3        | 0.5833       | 0.4573    | 0.6994    |
| B, k=5        | 0.8167       | 0.7008    | 0.8944    |

### 1c. Annual model cost in USD

| **candidate** | **annual cost** |
| ------------- | --------------- |
| A, k=3        | 16891.69        |
| A, k=5        | 22872.14        |
| B, k=3        | 10637.71        |
| B, k=5        | 12448.84        |

### 1d. Decision (at most 150 words)

Chosen candidate:B, k=5

I recommend B, k=5. Its annual cost is 12448.84, below the 19900 budget, and its recall is 0.8167 (49/60), with a 95 % Wilson interval of 0.7008–0.8944. A, k=5 is over budget. The affordable alternatives A, k=3 and B, k=3 have intervals ending at 0.6691 and 0.6994, respectively, both below B, k=5's lower bound of 0.7008; under the course rule, their intervals do not overlap. For B, k=5, recall is 0.85 for EN (17/20), 0.75 for RU (15/20), and 0.85 for KK (17/20). Russian questions are served worst in this sample. The sample has only 20 questions per language, so this difference should be checked with more data.

### 1e. Next measurement (at most 60 words)
Evaluate at least 100 newly sampled questions per language with the same retrieval settings. Report recall@5 and 95 % Wilson intervals separately for EN, RU, and KK, and categorize the misses. This would test whether Russian remains the weakest language and whether B, k=5 keeps its advantage on an independent sample.
## Part 2: A document pipeline you can defend

### 2a. Numbers

| **pipeline** | **overall accuracy** | **weakest cell (language, condition)** | **accuracy there** | **lower** | **upper** | **annual cost** |
| ------------ | -------------------- | -------------------------------------- | ------------------ | --------- | --------- | --------------- |
| H            | 0.8889 (160/180)     | KK, degraded                           | 0.7000 (21/30)     | 0.5212    | 0.8334    | 16972.50        |
| L            | 0.8333 (150/180)     | KK, degraded                           | 0.7000 (21/30)     | 0.5212    | 0.8334    | 7200.00         |

Personal-data pages per year sent abroad under H:182500

### 2b. Memo (at most 300 words)
To: Head of Operations — Subject: Invoice extraction pipeline choice
Pipeline H has higher overall exact-field accuracy than L (160/180 = 88.9%, versus 150/180 = 83.3%), but costs 16972.50/year versus 7200.00/year for L. Both pipelines’ weakest cell is Kazakh degraded scans: 21/30 = 70.0%, with a 95% Wilson interval of 52.1%–83.3%. Each cell contains only 30 invoices, so the intervals are wide; the overall figures hide weaker performance on degraded Kazakh scans. L also has lower accuracy on clean Kazakh scans (25/30 = 83.3%) than on English and Russian clean scans (27/30 = 90.0%).
Under H, 10% of 5,000 daily pages contain a name and ИИН, so 182,500 personal-data pages per year would be sent to a data centre abroad. The supplied information tells us where the scan is processed, but not who can access it, where logs and backups are stored, or how long copies are retained. These points need independent verification with the vendor. L processes data on a server in Kazakhstan, but its access controls, logging, backup location, and deletion practices also need checking.
I recommend L because it costs 9772.50 less per year and keeps processing in Kazakhstan, although its measured overall accuracy is 5.6 percentage points lower. Verify role-based access, encryption in transit and at rest, redaction of names/ИИН from logs where feasible, retention and deletion schedules including backups, and auditable access logs. The residual risk is incorrect extraction, especially on degraded Kazakh scans; use a larger evaluation set and human review for low-confidence or high-impact fields.

## Part 3: Explain your own numbers

### Q1

Number: 2.13
Why (at most 40 words): The total-token ratio is 2.18, but input tokens cost $2 per million and output tokens cost $10 per million. The English and Kazakh requests have different input/output mixes, so the bill ratio differs from the total-token ratio.

### Q2

Number: 44.4%
Why (at most 40 words): An English chunk maps to about 920.5 e5 tokens, so 408.5 tokens (44.4%) exceed the 512-token limit. Russian maps to 448 and Kazakh to 266, so neither is cut. The truncated tail cannot affect the embedding.

### Q3

Number: 51.6%
Why (at most 40 words): Why (at most 40 words): Dividing scores by a positive temperature preserves their ranking. Temperature 0.7 sharpens the distribution and raises the top token's probability; softmax still assigns nonzero probability to every listed token.

### Q4

Number: 31.8%
Why (at most 40 words): Weighting account numbers (177 questions at 37%) and birthdays (327 at 29%) gives 31.8%. I exclude multiplication and arithmetic sequences because their answers are rule-solvable. A system prompt cannot recover arbitrary facts absent from learned knowledge.

### Q5

Number: 9135 input tokens
Why (at most 40 words): The first turn's 87 tokens are billed on all 14 turns. Total input is 87 × (1 + … + 14) = 9135. Prompt caching may reduce repeated-prefix processing or charges, depending on the service's caching rules.
