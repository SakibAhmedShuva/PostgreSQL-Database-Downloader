# PostgreSQL Database Downloader Toolkit

A Python-based toolkit utilizing a Jupyter Notebook (`database_downloader.ipynb`) for creating various types of exports from a PostgreSQL database. This tool supports selective column exports, single table exports, exporting predefined sets of tables, and comprehensive database dumps, primarily to CSV format or PostgreSQL's native backup formats. It's designed for data archiving, migration, analysis, or backup purposes.

## Features

- **Multiple Export Modes via Jupyter Notebook Cells:**
    -   **Selective Column Export**: Export specified columns from the `user` table.
    -   **Full `user` Table Export**: Export all columns and rows from the `user` table.
    -   **Predefined Set of Tables Export**: Export a specified list of tables to individual CSV files within a timestamped directory.
    -   **Interactive Full Database/Multiple Table Export Menu**:
        -   **Full Database Backup via `pg_dump`**: Supports `custom` (.dump), `plain` (.sql), and `directory` output formats. (Requires `pg_dump` accessible in PATH).
        -   **All Tables to CSVs**: Export all tables from the 'public' schema to individual CSV files in a timestamped directory.
        -   **Single Arbitrary Table to CSV**: Export any single table (specified by name at runtime) to a CSV file.
- **Timestamped Outputs**: Automatic timestamping for generated files and directories to prevent overwrites and organize exports.
- **Configuration**: Securely manage database credentials using a `.env` file.
- **Encoding**: UTF-8 encoding for all CSV exports.
- **Logging**: Clear console logs for tracking export progress and errors.
- **Modularity**: Organized into functions for clarity and potential reuse.
- **Type Hinting**: Enhances code readability and maintainability.

## Prerequisites

- Python 3.6+
- PostgreSQL database access
- `pg_dump` utility accessible in your system's PATH (for the full database backup feature).
- Required Python packages:
  - `psycopg2-binary`
  - `python-dotenv`

## Installation

1.  Clone the repository (if applicable, or simply download the `.ipynb` file):
    ```bash
    git clone https://github.com/SakibAhmedShuva/PostgreSQL-Database-Downloader-Toolkit.git
    cd PostgreSQL-Database-Downloader-Toolkit
    ```

2.  Install required Python packages (preferably in a virtual environment):
    ```bash
    pip install psycopg2-binary python-dotenv
    ```

3.  Create a `.env` file in the same directory as the `database_downloader.ipynb` notebook with your database credentials:
    ```env
    DB_NAME=your_database_name
    DB_USERNAME=your_username
    DB_PASS=your_password
    DB_HOST=your_host
    DB_PORT=your_port
    ```

## Usage

The primary way to use this toolkit is by opening the `database_downloader.ipynb` file in a Jupyter Notebook environment (e.g., Jupyter Lab, Jupyter Notebook, VS Code with Python extension). The notebook is divided into several executable code cells, each performing a specific export task.

Navigate to the desired cell in the notebook and run it.

1.  **Initial Setup Cell**:
    -   Installs `psycopg2-binary` and `python-dotenv` if not already present. Run this once if needed.
    -   Imports necessary libraries.

2.  **Selective Column export (from `user` table)**:
    -   Located under the markdown header "### Selective Column export".
    -   Edit the `columns_to_export` list in the `if __name__ == "__main__":` block within this cell to specify which columns from the `public.user` table you want to export.
    -   Run the cell. Output will be `user_export.csv` or a custom name if modified.

3.  **Single table export (full `user` table)**:
    -   Located under the markdown header "# Single table export".
    -   This cell is currently hardcoded to export the full `public.user` table.
    -   Run the cell. Output will be `user_table_export_YYYYMMDD_HHMMSS.csv`.

4.  **Multiple Tables Export (Predefined Set)**:
    -   Located under the markdown header "# Multiple Tables Export".
    -   This cell exports a predefined list of tables (defined in `TABLES_TO_EXPORT_PREDEFINED`).
    -   Ensure the `TABLES_TO_EXPORT_PREDEFINED` list contains the tables you need.
    -   Run the execution cell (`export_all_predefined_tables()`). Tables will be exported to individual CSVs in a new timestamped directory like `your_db_name_predefined_set_export_YYYYMMDD_HHMMSS/`.

5.  **Full Database Export / Interactive Menu**:
    -   Located under the markdown header "# Full Database Export" (or similar, it's the last main code cell for exports).
    -   When you run this cell, it will present an interactive menu in the output area:
        1.  **Full database backup using `pg_dump`**: Prompts for output format (custom, plain, directory).
        2.  **Export all tables to individual CSV files**: Exports all tables in the 'public' schema.
        3.  **Export a single table to CSV**: Prompts for the table name.
        4.  **Export selected tables to CSV files from a predefined list**: (This option was in a previous version of the cell, if it's still present, it allows choosing from a list).
    -   Follow the on-screen prompts to make your selection.

## Output Files

-   **Selective Column Export**: e.g., `user_export.csv` or custom filename.
-   **Full `user` Table Export**: e.g., `user_table_export_YYYYMMDD_HHMMSS.csv`.
-   **Predefined/All/Selected Tables to CSVs**: Files like `tablename_export_YYYYMMDD_HHMMSS.csv` within a timestamped parent directory (e.g., `dbname_predefined_set_export_YYYYMMDD_HHMMSS/` or `dbname_all_tables_csv_YYYYMMDD_HHMMSS/`).
-   **`pg_dump` Export**: e.g., `dbname_fulldb_export_YYYYMMDD_HHMMSS.dump` (custom format), `.sql` (plain format), or a directory.

## Key Functions

The notebook utilizes several key functions:

-   `load_db_config() -> dict`: Loads database connection parameters from the `.env` file.
-   `export_to_csv(columns: List[str], output_file: str)`: Exports specified columns from the `public.user` table.
-   `export_full_table_to_csv()`: Exports the entire `public.user` table.
-   `export_single_table_to_csv(table_name: str, output_dir: str, db_config: dict) -> bool`: A generic function to export any single specified table to a CSV file within the given output directory.
-   `export_all_predefined_tables()`: Iterates through `TABLES_TO_EXPORT_PREDEFINED` and uses `export_single_table_to_csv` for each.
-   `export_entire_database_pg_dump(output_format: str)`: Uses the `pg_dump` command-line tool to back up the entire database.
-   `export_all_tables_to_csvs()`: Fetches all table names from the 'public' schema and exports each to a CSV file using `export_table_to_csv` (which might be a helper function similar to `export_single_table_to_csv` within that cell).

## Error Handling

The scripts within the notebook include error handling for:
- Database connection issues.
- SQL query execution errors.
- File I/O problems.
- Missing `pg_dump` command.
Errors and progress are logged to the console/cell output.

## Security Notes

-   **Never commit the `.env` file** or hardcode credentials directly in the notebook if sharing.
-   Ensure the PostgreSQL user configured has the necessary read permissions for the tables/database you intend to export.
-   Be cautious when allowing arbitrary table name inputs if not properly sanitized (though `psycopg2`'s parameterization helps prevent SQL injection for data queries).

## Contributing

1.  Fork the repository.
2.  Create your feature branch (`git checkout -b feature/AmazingFeature`).
3.  Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4.  Push to the branch (`git push origin feature/AmazingFeature`).
5.  Open a Pull Request.

## Author

Sakib Ahmed Shuva - [GitHub Profile](https://github.com/SakibAhmedShuva)

## License

This project is licensed under the MIT License - see the `LICENSE` file for details (if one is provided in the repository).

## Acknowledgments

-   The Python and PostgreSQL communities.
-   Developers of `psycopg2` and `python-dotenv`.

## Example Usage Scenarios

1.  **Export Specific User Details**:
    -   Modify the `columns_to_export` list in the "Selective Column export" cell.
    -   Run the cell to get a CSV like `user_details_for_analysis.csv`.

2.  **Get a Quick Backup of the `user` Table**:
    -   Run the "Single table export" cell.

3.  **Archive a Standard Set of Operational Tables**:
    -   Ensure `TABLES_TO_EXPORT_PREDEFINED` in the "Multiple Tables Export" cell is up-to-date.
    -   Run the cell to get a directory of CSVs.

4.  **Create a Full Database Backup for Restoration**:
    -   Run the "Full Database Export" cell and choose option 1 for `pg_dump`. Select the 'custom' format for a versatile backup.

5.  **Export All Tables for Data Warehousing Staging**:
    -   Run the "Full Database Export" cell and choose option 2 to export all tables to CSVs.
