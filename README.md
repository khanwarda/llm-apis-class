# Model Comparison Report: Gemini vs Groq

## Topic
Python basics (lists, tuples, dictionaries, references). I know this topic well, so I could judge the answers myself.

## Setup
- Both models were called through the OpenAI SDK with a different `base_url` and key.
- Models: `gemini-3.8-flash` (Google Gemini) and `openai/gpt-oss-20b` (Groq).
- temperature = 0.2, max_tokens = 2000, timeout = 120s, up to 3 attempts with waiting on errors.
- Code: `04_assignment_model_comparison.ipynb`. Results: `results.csv`.

## Test questions
1. What is a list in Python?
2. What is the difference between a list and a tuple?
3. When should I use a dictionary instead of a list?
4. What does `list(range(3, 10, 2))` return?
5. Why does `a = [1,2]; b = a; b.append(3)` change `a` too?

## Results

| | Gemini | Groq |
|---|---|---|
| Correct | 5 / 5 | 3 / 5 (2 partly correct) |
| Wrong | 0 | 0 |
| Average latency | 154.5 s | 2.6 s |
| Average tokens in | 14.8 | 84.2 |
| Average tokens out | 610.6 | 1559.6 |

Ratings per answer:

| Question | Gemini | Groq |
|---|---|---|
| 1 | correct | partly correct (cut off) |
| 2 | correct | correct |
| 3 | correct | partly correct (cut off) |
| 4 | correct | correct |
| 5 | correct | correct |

## Observations
- No model gave a wrong answer. Groq's two "partly correct" ratings are because the answer was cut off when it reached 2000 output tokens. The part that was written was correct.
- `gpt-oss-20b` is a reasoning model. Its thinking tokens count towards `max_tokens`, so it can run out of tokens before finishing the answer. With `max_tokens=400` it even returned empty answers in my first run.
- Groq was about 60 times faster on average (2.6 s vs 154.5 s).
- Gemini's latency changed a lot (28 s to 353 s). Its shortest answer (87 tokens) took 232 s, so the delay came from the server and not from the answer length.
- Gemini also gave many errors on the free tier (429 rate limit, 500 server errors, timeouts), and some questions only worked after retries.

## My choice: Groq (`openai/gpt-oss-20b`)
For this topic I would choose Groq. Both models knew the material, so accuracy was almost equal. Groq answered in about 2 seconds, while Gemini needed minutes and often failed on the free tier. Its only weakness (cut-off answers) can be fixed by raising `max_tokens` or asking for shorter answers in the system prompt.
I would choose Gemini instead if I needed complete, well-structured answers every time and speed did not matter.

## Limitations
- Only 5 questions on one easy topic, so the result is not a strong benchmark.
- Latency depends on server load at that moment, and I ran the test only once at each time.
- I rated the answers myself, so ratings are partly subjective.
- Free-tier limits affected Gemini, which may not happen on a paid plan.

## How to run
```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```
Copy `.env.example` to `.env` and add your own keys, then run the notebook. Never commit `.env`.
