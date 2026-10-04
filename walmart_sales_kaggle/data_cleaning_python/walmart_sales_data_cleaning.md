# Walmart Sales Data Cleaning Workflow

## Problem
The raw `sales.csv` file could not be loaded directly into PostgreSQL/pgAdmin due to file formatting and illegal byte encoding errors.

## Solution
Used Python and `pandas` to handle encoding issues, clean column names, skip corrupted lines, and export a clean UTF-8 CSV ready for database ingestion.

### Python Cleaning Script

```python
import csv
import pandas as pd

input_file = "/Users/ashokthapa/Desktop/sales.csv"
output_file = "/Users/ashokthapa/Desktop/salesclean.csv"

# Read CSV handling illegal bytes and auto-detecting delimiters
df = pd.read_csv(
    input_file,
    sep=None,
    engine="python",
    encoding="latin1",
    on_bad_lines="skip",
)

# Clean leading/trailing whitespaces in column names
df.columns = df.columns.str.strip()

# Save clean UTF-8 CSV for pgAdmin import
df.to_csv(output_file, index=False, encoding="utf-8", quoting=csv.QUOTE_MINIMAL)

print(f"Success! Cleaned CSV saved to: {output_file}")
