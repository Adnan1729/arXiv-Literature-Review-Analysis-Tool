# arXiv Literature Review Analysis Tool

A comprehensive Python-based tool for programmatic analysis of academic literature from arXiv, designed to support systematic literature reviews through temporal trend analysis, thematic categorization, and domain clustering visualization.

## Overview

This tool was developed to support the methodology section of the paper **"A Critical Review of Modular Bayesian Frameworks for Real-Time Uncertainty Quantification in Autonomous Systems"** by Adnan Mahmud (University of Edinburgh, SPADs CDT). It enables researchers to conduct large-scale corpus analysis of academic papers, identifying temporal patterns, thematic evolution, and disciplinary fragmentation across research domains.

The analysis in the paper examined 1,247 papers from arXiv (2010-2025) and revealed critical insights into the research landscape, including the explosive growth of surrogate modeling approaches, delayed emergence of digital twins literature, and disciplinary fragmentation between statistics and machine learning communities.

## Features

### Core Capabilities

1. **Automated Literature Retrieval**
   - Fetches papers from arXiv using structured Boolean queries
   - Configurable search parameters with support for complex query operators
   - Automatic extraction of metadata (title, abstract, year, categories, authors)
   - Robust error handling for API interactions

2. **Temporal Analysis**
   - Publication trend visualization over time
   - Year-by-year paper count statistics
   - Bar chart visualization of temporal patterns

3. **Bibliometric Analysis**
   - Identification of top research domains (arXiv categories)
   - Frequency analysis of CS and Statistics categories
   - Domain distribution statistics

4. **Thematic & Methodological Analysis**
   - Keyword-based categorization across six predefined themes:
     - **T_Digital_Twins**: Digital twin and virtual representation concepts
     - **T_Real_Time_Systems**: Real-time execution and dynamic modeling
     - **M_Modular_Frameworks**: Modular and component-based architectures
     - **M_Multi_Source_Data**: Multi-fidelity and heterogeneous data integration
     - **M_Uncertainty_Quantification**: Bayesian inference and UQ methods
     - **M_Surrogate_Modeling**: Emulators and reduced-order models
   - Horizontal bar chart showing relative dominance of themes
   - Identification of top papers within dominant themes

5. **Temporal Thematic Evolution**
   - Multi-line chart tracking theme emergence over time
   - Analysis of how research focus shifts across years
   - Filtering for years with sufficient data volume

6. **Domain-Theme Co-occurrence Analysis**
   - Heatmap visualization of theme distribution across research domains
   - Clustering analysis revealing disciplinary patterns
   - Identification of interdisciplinary connections

## Installation

### Prerequisites

```bash
pip install arxiv
pip install pandas
pip install matplotlib
```

### Python Version
- Python 3.7 or higher recommended
- Tested on Python 3.8+

## Usage

### Basic Execution

1. Clone the repository:
```bash
git clone https://github.com/yourusername/arxiv-literature-review.git
cd arxiv-literature-review
```

2. Open the Jupyter notebook:
```bash
jupyter notebook arxiv_analysis.ipynb
```

3. Run all cells or execute `python arxiv_analysis.py` from command line

### Configuration

Modify the configuration parameters at the top of the script:

```python
# Search query configuration
SEARCH_QUERY = '((ti:Bayesian OR abs:Bayesian OR ti:"Uncertainty Quantification" 
                  OR abs:"Uncertainty Quantification" OR ti:UQ OR abs:UQ) 
                 AND (all:Modular OR all:"Multi-Fidelity" OR all:"Real-Time" 
                  OR all:"Digital Twin"))'

# Maximum results to fetch
MAX_RESULTS = 10000
```

### Custom Search Queries

The tool supports arXiv's search syntax:
- `ti:keyword` - Search in title
- `abs:keyword` - Search in abstract
- `all:keyword` - Search everywhere
- Boolean operators: `AND`, `OR`
- Phrase search: Use quotes `"exact phrase"`

Example custom query:
```python
SEARCH_QUERY = '(ti:"machine learning" OR ti:"deep learning") AND (all:robotics OR all:autonomous)'
```

### Customizing Themes

To analyze different thematic categories, modify the `themes` dictionary in `thematic_analysis()`:

```python
themes = {
    'T_Your_Theme': ['keyword1', 'keyword2', 'keyword3'],
    'M_Your_Method': ['method1', 'approach', 'technique'],
    # Add more themes as needed
}
```

## Function Reference

### Utility Functions

#### `clean_theme_name(theme_key)`
Converts internal theme keys (e.g., `'T_Digital_Twins'`) to display-friendly names (e.g., `'Digital Twins'`).

**Parameters:**
- `theme_key` (str): Theme identifier with prefix (T_ or M_)

**Returns:**
- str: Cleaned theme name for visualization

### Data Retrieval Functions

#### `fetch_arxiv_papers(query, max_results)`
Retrieves papers from arXiv based on search query.

**Parameters:**
- `query` (str): Structured arXiv search query
- `max_results` (int): Maximum number of papers to fetch

**Returns:**
- pandas.DataFrame: Paper metadata including title, abstract, year, categories, authors, URL

**Notes:**
- Sorts by submission date (newest first)
- Filters categories to CS and Statistics domains
- Includes error handling for API failures

### Analysis Functions

#### `temporal_analysis(df)`
Analyzes and visualizes publication trends over time.

**Parameters:**
- `df` (pandas.DataFrame): DataFrame containing paper metadata with 'year' column

**Output:**
- Console: Year-by-year paper counts
- Visualization: Bar chart of temporal trends

#### `bibliometric_analysis(df)`
Identifies top research categories and domains.

**Parameters:**
- `df` (pandas.DataFrame): DataFrame with 'categories' column

**Output:**
- Console: Top 10 most frequent arXiv categories

#### `thematic_analysis(df)`
Categorizes papers by thematic keywords and identifies dominant themes.

**Parameters:**
- `df` (pandas.DataFrame): Paper metadata DataFrame

**Returns:**
- pandas.DataFrame: Original DataFrame augmented with boolean theme flags

**Output:**
- Console: Theme occurrence counts, top papers for dominant theme
- Visualization: Horizontal bar chart of thematic distribution

#### `temporal_thematic_evolution(df_with_themes)`
Tracks how thematic focus evolves over time.

**Parameters:**
- `df_with_themes` (pandas.DataFrame): DataFrame with theme flag columns

**Output:**
- Console: Year-by-year theme counts
- Visualization: Multi-line chart showing theme evolution (filtered to years with >10 papers)

#### `category_thematic_co_occurrence(df_with_themes)`
Analyzes co-occurrence between research domains and themes.

**Parameters:**
- `df_with_themes` (pandas.DataFrame): DataFrame with categories and theme flags

**Output:**
- Console: Co-occurrence matrix
- Visualization: Heatmap showing domain-theme relationships (categories with ≥50 papers)

### Main Execution Function

#### `main()`
Orchestrates the complete analysis pipeline:
1. Fetches papers from arXiv
2. Performs temporal analysis
3. Conducts bibliometric analysis
4. Executes thematic categorization
5. Analyzes temporal thematic evolution
6. Generates domain-theme co-occurrence heatmap

## Output Examples

The tool generates three primary visualizations (as shown in Figure 1 of the paper):

1. **Temporal Evolution Chart**: Line graph showing yearly publication counts for each theme (2010-2025)
2. **Thematic Distribution**: Horizontal bar chart displaying relative dominance of keyword categories
3. **Domain-Theme Heatmap**: Viridis colormap showing co-occurrence intensity between arXiv categories and themes

## Limitations

### ArXiv API Constraints
- **No geographical data**: Cannot perform country-based analysis (author affiliations unavailable)
- **Rate limiting**: Excessive requests may trigger API throttling
- **Metadata completeness**: Some papers may have incomplete category information

### Methodological Considerations
- **Keyword dependency**: Theme detection relies on predefined keywords; may miss papers using alternative terminology
- **Category filtering**: Restricts to CS (`cs.*`) and Statistics (`stat.*`) categories; may exclude relevant work in other domains
- **Threshold sensitivity**: Co-occurrence heatmap filters categories with <50 papers; adjustment may reveal different patterns

## Future Extensions

Potential enhancements for researchers building on this tool:

1. **Citation Network Analysis**: Integrate with Semantic Scholar or CrossRef APIs for citation graphs
2. **Author Collaboration Networks**: Map co-authorship patterns across themes
3. **Semantic Analysis**: Use NLP techniques (topic modeling, embeddings) for deeper thematic discovery
4. **Comparative Corpus Analysis**: Compare multiple search queries or time periods
5. **Interactive Visualizations**: Implement Plotly or Bokeh for explorable charts
6. **Export Capabilities**: Generate LaTeX tables or publication-ready figures

## Citation

If you use this tool in your research, please cite the associated paper:

```bibtex
@article{mahmud2024modular,
  title={A Critical Review of Modular Bayesian Frameworks for Real-Time Uncertainty Quantification in Autonomous Systems},
  author={Mahmud, Adnan},
  institution={SPADs CDT, University of Edinburgh},
  year={2024}
}
```

## License

This codebase is licensed under the terms of the [MIT License](LICENSE).

## Contact

**Adnan Mahmud**  
University of Edinburgh  
Email: s2887048@ed.ac.uk

For questions, suggestions, or collaboration inquiries, please [open an issue](https://github.com/yourusername/arxiv-literature-review/issues) or contact via [your email/contact method].

## Acknowledgments

This work was supported by the SPADS Centre for Doctoral Training at the University of Edinburgh, under the supervision of Dr. Amy Wilson and Professor Chris Dent. The tool was developed as part of coursework for ERMGC (Enabling Research Methods and Generic Competencies).
