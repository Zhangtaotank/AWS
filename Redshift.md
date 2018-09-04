# AWS

## Data Ingestion: Depulication/Upsert

Begin;
CREATE TEMP TABLE staging(LIKE deep_dive);
COPY staging FROM 'S3://bucket/dd.csv'
:'creds' COMPUPDATE OFF;
DELETE FROM deep_dive d
USING staging s WHERE d.aid = s.aid;
INSERT INTO deep_dive SELECT * FROM staging;
DROP TABLE staging;
COMMIT;


