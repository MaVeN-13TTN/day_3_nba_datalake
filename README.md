# NBA Sports Analytics Data Lake Setup on AWS

This project sets up a data lake for NBA sports analytics using AWS services and the sportsdata.io API.

The data lake infrastructure is created on AWS, including S3 buckets for data storage, Glue databases and tables for data cataloging, and Athena for querying the data. The project fetches NBA player data from the sportsdata.io API, processes it, and stores it in the data lake.

## Repository Structure

- `delete_aws_resources.py`: Script to delete AWS resources created by this project.
- `src/setup_nba_data_lake.py`: Main script to set up the NBA data lake on AWS.

## Usage Instructions

### Prerequisites

- Python 3.6+
- AWS account with appropriate permissions
- sportsdata.io API key
- AWS CLI configured with your credentials

### Installation

1. Clone the repository:

   ```
   git clone https://github.com/MaVeN-13TTN/day_3_nba_datalake
   cd day_3_nba_datalake
   ```

2. Install required Python packages:

   ```
   pip install boto3 requests python-dotenv
   ```

3. Create a `.env` file in the project root with the following content:
   ```
   SPORTS_DATA_API_KEY=your_api_key_here
   NBA_ENDPOINT=https://api.sportsdata.io/v3/nba/stats/json/Players
   ```

### Setting up the Data Lake

To set up the NBA data lake, run:

```
python src/setup_nba_data_lake.py
```

This script performs the following actions:

1. Creates an S3 bucket for data storage
2. Creates a Glue database
3. Fetches NBA player data from sportsdata.io API
4. Converts the data to line-delimited JSON format
5. Uploads the data to the S3 bucket
6. Creates a Glue table for the data
7. Configures the Athena output location

### Deleting AWS Resources

To delete all AWS resources created by this project, run:

```
python delete_aws_resources.py
```

This script will delete:

- S3 buckets and their contents
- EC2 instances
- RDS instances
- Glue databases and tables
- Athena query results stored in the S3 bucket

### Troubleshooting

#### S3 Bucket Creation Fails

- **Problem**: The script fails to create the S3 bucket.
- **Solution**:
  1. Check if the bucket name is unique globally.
  2. Ensure your AWS credentials have the necessary permissions.
  3. If using a region other than `us-east-1`, modify the `region` variable in `setup_nba_data_lake.py`.

#### API Data Fetching Fails

- **Problem**: The script fails to fetch data from the sportsdata.io API.
- **Solution**:
  1. Verify your API key in the `.env` file.
  2. Check your internet connection.
  3. Ensure the API endpoint is correct and the service is available.

#### Glue Resource Creation Fails

- **Problem**: The script fails to create Glue databases or tables.
- **Solution**:
  1. Ensure your AWS account has the necessary permissions for Glue operations.
  2. Check if the Glue service is available in your chosen AWS region.

### Debugging

To enable debug mode:

1. Add the following at the beginning of both Python scripts:
   ```python
   import logging
   logging.basicConfig(level=logging.DEBUG)
   ```
2. Re-run the scripts to see detailed debug output.

Log files can be found in the CloudWatch Logs if you're running these scripts on an EC2 instance or as a Lambda function.

## Data Flow

The data flow in this project follows these steps:

1. NBA player data is fetched from the sportsdata.io API.
2. The data is converted to line-delimited JSON format.
3. The formatted data is uploaded to an S3 bucket.
4. A Glue table is created to catalog the data in the S3 bucket.
5. Athena is configured to query the data using the Glue table.

```
[sportsdata.io API] -> [Python Script] -> [S3 Bucket] -> [Glue Table] -> [Athena]
```

Note: Ensure that your AWS IAM permissions allow for creating and managing S3 buckets, Glue databases and tables, and Athena queries.

## Infrastructure

The project uses the following AWS resources:

- S3:

  - Bucket: `sports-analytics-data-lake-1324` (for storing NBA data and Athena query results)

- Glue:

  - Database: `glue_nba_data_lake`
  - Table: `nba_players` (in the `glue_nba_data_lake` database)

- Athena:
  - Output location: `s3://sports-analytics-data-lake-1324/athena-results/`
  - Database: `nba_analytics` (created if not exists)

Note: The S3 bucket name `sports-analytics-data-lake-1324` is used as an example. You should replace this with a unique bucket name when setting up the project.
