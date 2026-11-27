Vector-Enhanced Review Sentiment Analysis

Analyzes diamond jewelry customer reviews with local LLMs (via Ollama) and a FAISS vector store of labelled examples used for few-shot prompting.

For each review it:

Identifies which categories are discussed: Diamond, Style, Manufacturing, Merchandising, Retailer-related (or Uncategorized).
Assigns a sentiment per category: Positive, Negative or Return (the customer returned the product because of that aspect).
Extracts the supporting phrases behind each sentiment.
Writes everything to a new Excel file, one row per review/category pair.
Requirements
Python 3.10+
Ollama running locally (http://localhost:11434) with these models pulled:
ollama pull qwen3:4b
ollama pull gemma3:12b
Python packages:
pip install -r requirements.txt
Input format

An Excel file (.xlsx) with these columns:

Product Style	Timeline	Date	Review
Usage
python sentiment_analysis.py --input path/to/reviews.xlsx --output-dir results

Options:

Flag	Default	Description
--input	required	Input Excel file
--output-dir	.	Folder for the results file
--sleep-every	10	Pause after this many reviews (0 disables)
--sleep-seconds	120	Length of each pause
--verbose	off	Print model responses and debug info

Output is saved as <input name>_vector_enhanced_<timestamp>.xlsx with columns: Product Style, Timeline, Date, Review, Review_Category, Sentiment Type, Analysis, Count.

How it works
Knowledge base: ~30 labelled example reviews are embedded with sentence-transformers/all-MiniLM-L6-v2 and stored in FAISS. The most similar examples are injected into each prompt.
Models: qwen3:4b for categories and sentiment, gemma3:12b for keyword extraction.
Return override: a rule-based check forces "Return" when a return mention appears in the same or previous sentence as a category-specific term.
Notes
Do not commit real customer data. Excel files are ignored by .gitignore.
Model names and categories can be changed at the top of sentiment_analysis.py.
