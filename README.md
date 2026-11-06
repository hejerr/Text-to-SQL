# Text-to-SQL with Encoder–Decoder Models

Course project (NLP): text generation with encoder–decoder architectures.

The goal is to translate a question about a table into a SQL query. I build the whole pipeline from scratch in PyTorch: preprocessing, vocabulary, encoder, decoder, attention, training, generation and evaluation. The dataset is [WikiSQL](https://github.com/salesforce/wikisql).

```
Question: How many players are from Tunisia?
Columns : Player | Country | Club
SQL     : SELECT COUNT(Player) FROM table WHERE Country = 'Tunisia'
```

## Structure of the project

```
text2sql/
├── README.md       this file
├── notebooks/      one notebook per step, 01 ... 07, each with the full code of its step
├── report/         the written report
└── results/        the numbers and figures produced by the notebooks
```

**Everything is in the notebooks.** Each notebook is one step of the project: it explains the step, contains the **full code** of that step, runs it on the real data, and discusses what the results mean. Read them in order.

| Notebook | Question it answers |
| :---- | :---- |
| `01_data` | What is in the data? How do I load it and run a query on a table? |
| `02_text_preprocessing` | How are text and queries turned into numbers? What does it cost? |
| `03_lstm_baselines` | What is the baseline? How is a model built, trained and judged? |
| `04_attention` | Does attention help? How does the model decode (greedy, beam search)? |
| `05_copy_and_bpe` | Can the model write words it has never seen? |
| `06_experiments` | Does a bigger model help? What are the final results on the test set? |
| `07_error_analysis` | Which mistakes remain, and why? |

**About the code.** A code cell that starts with `%%writefile {CODE}/path/to/file.py` saves its content as a code file of the project. So the code written in the notebook is exactly the code that the project uses; nothing is hidden in another file. When a notebook runs, these files are written to a folder called `code` (see the first cell of each notebook). A later notebook rebuilds the code of the earlier ones from their cells, so every notebook can also be run alone.

## Models

I start from a simple baseline and add one component at a time, so that the effect of each component can be measured.

| Model | Description |
| :---- | :---- |
| M0 | Most frequent query shape (no learning) |
| M1 | LSTM encoder–decoder, no attention (the baseline) |
| M1b | Bidirectional LSTM encoder, no attention |
| M2 | Bidirectional LSTM encoder \+ attention |
| M3 | M2 \+ copy mechanism |

Models are compared with exact match, execution accuracy, token accuracy and perplexity, followed by an analysis of the remaining errors.

&nbsp;

## References

- Zhong et al., *Seq2SQL: Generating Structured Queries from Natural Language using Reinforcement Learning*, 2017\. The paper that introduced WikiSQL.  
- WikiSQL dataset and evaluation script, BSD-3 license: [https\://github.com/salesforce/wikisql](https://github.com/salesforce/wikisql)