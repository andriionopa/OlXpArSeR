# OlXpArSeR

OlXpArSeR is an asynchronous OLX.ua price parser that collects product listings, normalizes core product fields, and exports results to JSON and Excel.

The project is useful for price monitoring, catalogue research, and market analysis where the source website terms allow automated collection.

## Features

- Async HTTP collection with configurable request limits and delays
- OLX category discovery from a supplied base URL
- Single-category or all-category parsing flow
- Pagination support with a default safety limit
- Product extraction for name, price, URL, availability, SKU, category, image, and metadata
- JSON export for full and essential datasets
- Excel export with formatted rows and clickable product links
- Reusable parser base class for adding another target site

## Tech Stack

- Python 3.8+
- aiohttp
- BeautifulSoup4
- lxml
- openpyxl
- asyncio

## Repository Layout

```text
.
├── main.py
├── base_parser.py
├── olx_parser.py
├── example_parser.py
├── models.py
├── config.py
├── excel_exporter.py
├── test_excel_exporter.py
└── requirements.txt
```

## Installation

```bash
git clone git@github.com:andriionopa/OlXpArSeR.git
cd OlXpArSeR
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

## Usage

Run the interactive CLI:

```bash
python main.py
```

Then provide an OLX.ua URL when prompted. The tool displays discovered categories and lets you choose one category or parse all detected categories.

Generated files are written to the configured output directory, `parsed_data` by default:

- `*_full.json`
- `*_essential.json`
- Excel workbook with formatted product data

## Configuration

Parser defaults live in `config.py`.

Important settings:

- `base_url`: target OLX URL
- `request_timeout`: request timeout in seconds
- `max_concurrent_requests`: concurrent request limit
- `delay_between_requests`: delay between requests
- `categories_to_parse`: optional category filter
- `parse_entire_catalog`: full catalogue mode
- `output_directory`: export directory
- `log_level` and `log_file`: logging controls

Example:

```python
from config import ParserConfig

config = ParserConfig(
    base_url="https://www.olx.ua/uk/",
    max_concurrent_requests=5,
    delay_between_requests=1.5,
    output_directory="parsed_data"
)
```

## Creating Another Parser

Use `BasePriceParser` for shared HTTP, parsing, and utility behavior, then implement site-specific category and product extraction in a subclass.

Required methods:

- `get_categories`
- `get_products_from_category`

Use `models.Product`, `models.Category`, and `models.ParsingResult` for consistent output.

## Responsible Use

- Check the target site's robots.txt and terms of service.
- Keep delays and concurrency conservative.
- Do not collect personal data unless you have a lawful basis and a clear retention policy.
- Avoid running high-volume jobs from shared or production networks without monitoring.

## Validation

```bash
python3 -m py_compile *.py
python test_excel_exporter.py
gitleaks detect --source . --no-banner
```

## License

No license file is currently included. Add a license before distributing or accepting external contributions.
