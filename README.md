# PostgreSQL Table Chronicler

A lightweight Python utility for creating CSV exports of PostgreSQL database tables. This tool supports both full table exports with timestamps and selective column exports, making it perfect for data archiving, migrations, or backup purposes.

## Features

- Two export modes:
  - Full table export with automatic timestamps
  - Selective column export with custom filename
- Environment variable based configuration
- Column name preservation in exports
- UTF-8 encoding support
- Detailed export logging
- Secure credential management through `.env` file
- Type hinting for better code maintainability

## Prerequisites

- Python 3.6+
- PostgreSQL database access
- Required Python packages:
  - psycopg2
  - python-dotenv

## Installation

1. Clone the repository:
```bash
git clone https://github.com/SakibAhmedShuva/PostgreSQL-Table-Chronicler.git
cd PostgreSQL-Table-Chronicler
```

2. Install required packages:
```bash
pip install psycopg2-binary python-dotenv
```

3. Create a `.env` file in the project root with your database credentials:
```env
DB_USERNAME=your_username
DB_PASS=your_password
DB_HOST=your_host
DB_PORT=your_port
DB_NAME=your_database_name
```

## Usage

### Full Table Export

To export the entire table with a timestamp:

```bash
python database_copy.ipynb
```

This will:
- Export all columns from the 'user' table
- Create a file named `user_table_export_YYYYMMDD_HHMMSS.csv`
- Display export statistics including column count and names

### Selective Column Export

To export specific columns:

```python
from database_copy import export_to_csv

# Specify the columns you want to export
columns_to_export = ["id", "email", "fullName", "phone", "dateOfBirth"]

# Optional: Specify a custom output filename (defaults to 'user_export.csv')
export_to_csv(columns_to_export, output_file='custom_export.csv')
```

## Output Files

### Full Table Export
```
user_table_export_YYYYMMDD_HHMMSS.csv
```
Example: `user_table_export_20241230_143022.csv`

### Selective Column Export
Default: `user_export.csv` or your specified filename

## Function Documentation

### `load_db_config() -> dict`
Loads database configuration from environment variables.

Returns:
- Dictionary containing database connection parameters

### `export_full_table_to_csv()`
Exports the entire user table with all columns and timestamp in filename.

### `export_to_csv(columns: List[str], output_file: str = 'user_export.csv') -> None`
Exports specified columns from the user table.

Parameters:
- `columns`: List of column names to export
- `output_file`: (Optional) Name of the output CSV file

## Error Handling

The script includes comprehensive error handling for:
- Database connection issues
- Query execution errors
- File writing problems
- Invalid column names

All errors are logged to the console with descriptive messages.

## Security Notes

- Never commit the `.env` file to version control
- Keep your database credentials secure
- Ensure appropriate database user permissions
- Validate column names to prevent SQL injection

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Author

Sakib Ahmed Shuva - [GitHub Profile](https://github.com/SakibAhmedShuva)

## License

This project is licensed under the MIT License - see the LICENSE file for details

## Acknowledgments

- Python PostgreSQL community
- Contributors to python-dotenv

## Example Usage Scenarios

### 1. Full Table Backup
```python
# For complete table backup with timestamp
python database_copy.ipynb
```

### 2. Export User Contact Information
```python
# For exporting just contact-related columns
columns = ["id", "email", "phone"]
export_to_csv(columns, "user_contacts.csv")
```

### 3. Export User Profiles
```python
# For exporting profile-related columns
columns = ["id", "fullName", "dateOfBirth"]
export_to_csv(columns, "user_profiles.csv")
```
