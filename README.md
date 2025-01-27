# Automatic News Scraping and Analysis

## Overview
This project is a Python-based solution for scraping, analyzing, and extracting insights from news articles. It fetches articles from RSS feeds, processes their content, and performs sentiment analysis and keyword extraction. The processed data is saved into Excel files for further use.

## Features
- **News Scraping**: Fetch articles from an RSS feed and extract details such as title, author, publish date, and content.
- **Data Cleaning**: Remove duplicates and empty entries to ensure clean data.
- **Sentiment Analysis**: Analyze the sentiment polarity of articles and label them as `positive`, `negative`, or `neutral`.
- **Keyword Extraction**: Identify top noun phrases (keywords) from articles and rank them by their sentiment relevance.
- **Data Export**: Save cleaned and analyzed data to Excel files.

## Installation
To get started, clone this repository and install the required dependencies:

```bash
# Clone the repository
git clone https://github.com/yourusername/your-repository.git

# Navigate to the project directory
cd your-repository

# Install dependencies
pip install -r requirements.txt
```

## Dependencies
The project uses the following Python libraries:
- `newspaper`: For article parsing and extraction
- `feedparser`: For parsing RSS feeds
- `pandas`: For data manipulation
- `textblob`: For sentiment analysis and natural language processing

## Usage
1. **Set the RSS Feed URL**:
   Update the `feed_url` variable in the script to specify the desired RSS feed.

   ```python
   feed_url = 'http://feeds.bbci.co.uk/news/rss.xml'
   ```

2. **Run the Script**:
   Execute the script to scrape articles, analyze sentiment, and export results:

   ```bash
   python news_scraper.py
   ```

3. **View Results**:
   - **`news_articles.xlsx`**: Contains the raw articles with metadata.
   - **`results.xlsx`**: Includes sentiment analysis, labels, and top keywords for each article.

## Code Explanation
### 1. Scraping Articles
The function `scrape_news_from_feed` fetches articles from the specified RSS feed and extracts relevant information:
```python
def scrape_news_from_feed(feed_url):
    articles = []
    feed = feedparser.parse(feed_url)
    for entry in feed.entries:
        article = newspaper.Article(entry.link)
        article.download()
        article.parse()
        articles.append({
            'title': article.title,
            'author': article.authors,
            'publish_date': article.publish_date,
            'content': article.text
        })
    return articles
```

### 2. Sentiment Analysis
The `get_sentiment` function calculates the sentiment polarity of each article:
```python
def get_sentiment(text):
    analysis = TextBlob(text)
    return analysis.sentiment.polarity  # Score between -1 (negative) and 1 (positive)
```

### 3. Keyword Extraction
The `get_top_keywords` function extracts noun phrases from the text:
```python
def get_top_keywords(text, top_n=3):
    analysis = TextBlob(text)
    keywords = [(phrase, TextBlob(phrase).sentiment.polarity) for phrase in analysis.noun_phrases]
    top_keywords = sorted(keywords, key=lambda x: abs(x[1]), reverse=True)[:top_n]
    return [kw[0] for kw in top_keywords]
```

## Output Files
- **`news_articles.xlsx`**: Includes all scraped articles with metadata.
- **`results.xlsx`**: Contains the cleaned dataset with additional sentiment scores, sentiment labels, and top keywords.

## Example
Here is an example of the output:

### Sample Output in `results.xlsx`:
| Title                 | Content                    | Sentiment Score | Sentiment Label | Top Keywords       |
|-----------------------|----------------------------|-----------------|-----------------|--------------------|
| Example Title         | Example article content...| 0.1             | Positive        | ['keyword1', '...']|

## Contributing
Contributions are welcome! If you have suggestions for improvement, feel free to submit an issue or a pull request.

## License
This project is licensed under the MIT License. See the LICENSE file for details.

---
**Author**: Your Name  
**Contact**: [your.email@example.com](mailto:your.email@example.com)

