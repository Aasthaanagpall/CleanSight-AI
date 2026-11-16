# CleanSight: messy data to clear insights

Upload a messy CSV or Excel file. CleanSight checks the data quality, cleans it, and writes a short insights report for a non-technical manager who does not know SQL or Excel.



## The problem

A manager has a data file, but the file has duplicate rows, missing values and inconsistent spellings. Cleaning it by hand takes time, and a plain chatbot that "cleans data" can quietly invent numbers.

## How it works

```
Upload file  ->  Gemini decides which tools to run  ->  Pandas tools do the work  ->  Report + cleaned file
```

1. The manager uploads a file in a Gradio app and clicks **Clean and Report**.
2. Gemini is given three tools (Python functions) and decides when to call them: `quality_tool`, `clean_tool`, `kpi_tool`.
3. The tools are plain Pandas code. **Every number in the report comes from these tools, not from the AI.**
4. Every change the cleaner makes is written to a cleaning log, so nothing happens silently.
5. Gemini turns the tool results into a four-part report and the user can download the cleaned CSV.

### What the tools do

| Tool | What it does |
|---|---|
| `check_quality` | Counts rows, missing values per column, duplicate rows, and outliers |
| `clean_data` | Drops fully empty columns, removes duplicates, fills missing values (median for numbers, "Unknown" for text), makes different spellings of the same value consistent, and logs each change |
| `get_kpis` | Totals, averages, medians, min and max for numeric columns, plus group-wise averages and medians for text columns with 10 or fewer distinct values |

### The report

The report has four sections: data quality problems found, what was cleaned, key numbers with insights, and things to be careful about. The prompt tells Gemini to:

- take every number from the tools and never calculate or invent one
- compare groups with the median when a column has outliers
- treat the average of a 0/1 column as a rate
- describe differences as observations, not causes, and mention small group sizes

## Tech stack

Python, Pandas, NumPy, Google Gemini API (`google-genai`, function calling), Gradio, Google Colab.

## How to run

1. Get a free API key from [Google AI Studio](https://aistudio.google.com).
2. Open `CleanSight.ipynb` in Google Colab (File, Upload notebook).
3. In Colab, open the key icon (Secrets) on the left, add a secret named `GEMINI_API_KEY`, paste your key, and turn on Notebook access.
4. Run the cells in order, from top to bottom.
   - Cells 1 to 5 build and test the tools. They do **not** use Gemini.
   - Cells 6 to 8 connect to Gemini and start the app. Each click on **Clean and Report** sends several requests to the Gemini API, so the free daily limit can run out quickly.
5. Cell 8 prints a temporary public link. Anyone with the link can use the app with **your** key and quota, so do not share it, and stop the cell when you are done.

Never commit an API key to this repository.

<img width="1646" height="742" alt="image" src="https://github.com/user-attachments/assets/12b7e103-0098-42b7-be3e-7a9d1bc541c1" />

<img width="1690" height="732" alt="image" src="https://github.com/user-attachments/assets/95c7a29a-d92f-4093-a008-16664231736a" />
