
# PDF to SQL Transaction Processor

This project automates the extraction, transformation, and loading (ETL) of transactional data from a PDF document into a structured SQL database. The primary use case is to process bank statements and store the transactions in a MySQL database for further analysis.

## Features

- **PDF Parsing**: Extracts text data from PDF files using PyPDF2.
- **Data Transformation**:
  - Cleans and structures data into meaningful rows and columns.
  - Splits transactions into `date`, `particular`, `debit`, `credit`, and `balance`.
  - Generates unique identifiers for each transaction.
- **JSON Output**: Converts the processed data into JSON format.
- **Database Integration**:
  - Creates a MySQL database and a transactions table.
  - Inserts the structured data into the database.
- **Customizable Configurations**:
  - Reads column mappings and start row configurations from a JSON file.

## Prerequisites

- Python 3.x
- MySQL Server
- Required Python libraries:
  - `PyPDF2`
  - `mysql-connector-python`
  - `pandas`

Install the dependencies with:
```bash
pip install PyPDF2 mysql-connector-python pandas
```

## Workflow

1. **PDF Extraction**: Extract transaction details from the uploaded PDF file.
2. **Data Cleaning**:
   - Remove irrelevant rows.
   - Correct overlapping text issues.
3. **Data Structuring**: Split raw data into structured fields like `date`, `description`, `debit`, `credit`, etc.
4. **Database Storage**:
   - Create a database and table in MySQL.
   - Insert the processed transactions into the database.

## File Outputs

- `transaction_output.json`: A JSON file containing the structured data.
- MySQL table: Stores the transaction records in the database.

## Usage

### Step 1: Extract and Transform Data
Use the `extraction_of_pdf_to_text` function to extract data from a PDF file:
```python
to_text = extraction_of_pdf_to_text("sample_bank_statement.pdf")
```

Clean and transform the extracted data:
```python
pdf_lines_cleaned = remove_first_rows(to_text, 15)
df = pd.DataFrame(split_data, columns=['date', 'particular', 'debit', 'credit', 'total_balance'])
```

### Step 2: Save Data to JSON
Save the structured data into a JSON file:
```python
df.to_json('transaction_output.json', orient='records', lines=True)
```

### Step 3: Insert Data into MySQL
Ensure the database and table are created:
```python
create_database_and_table()
```

Insert data into MySQL:
```python
insert_dataframe(df)
```

## MySQL Configuration

Modify the `host`, `user`, and `password` parameters in the `connect_to_db` function to match your MySQL server settings.

## Example

A sample transaction entry in the MySQL database:
| unique_id                              | date       | description          | amount  | transaction_type |
|----------------------------------------|------------|----------------------|---------|------------------|
| 841253a4-dd36-4a6c-8117-f6068753dddf   | 2025-01-01 | Opening Balance      | 0.0     | Credit           |
