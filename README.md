# Spotify Data Pipeline (AWS ETL)

An end-to-end serverless data engineering pipeline that extracts data from the
Spotify API, transforms it, and makes it queryable with SQL — built entirely on AWS.

## Architecture
![Architecture](architecture.png)

**Flow:** Spotify API → Lambda (Extract) → S3 (Raw) → Lambda (Transform) →
S3 (Transformed) → Glue Crawler → Glue Data Catalog → Athena

## Tech Stack
- **Language:** Python (spotipy, pandas, boto3)
- **Extract:** AWS Lambda + Amazon CloudWatch (daily trigger)
- **Store:** Amazon S3 (raw & transformed buckets)
- **Transform:** AWS Lambda (triggered on S3 object-put)
- **Catalog:** AWS Glue Crawler + Data Catalog
- **Query:** Amazon Athena (SQL analytics)

## How It Works
1. **Extract** — CloudWatch triggers a Lambda daily. It calls the Spotify API
   and saves raw JSON to an S3 bucket.
2. **Transform** — A new file in the raw bucket triggers a second Lambda, which
   cleans the data and splits it into `albums`, `artists`, and `songs` tables (CSV).
3. **Load & Query** — A Glue Crawler infers the schema into the Data Catalog,
   and Athena runs SQL queries directly on the S3 data.

## Setup
1. Create a Spotify Developer app to get your `CLIENT_ID` and `CLIENT_SECRET`.
2. Store credentials as Lambda environment variables (never hard-code them).
3. Deploy both Lambda functions and set up the two triggers.
4. Configure the Glue Crawler to point at the transformed S3 bucket.

## Key Learnings
- Serverless, event-driven design (time-based + event-based triggers)
- Separating raw vs. transformed data layers
- Schema-on-read querying with Glue + Athena
