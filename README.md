# pgsql-s3-restore

Restore a SQL backup from S3 to PostgresSQL. See [pgsql-s3-backup](https://github.com/chadweimer/pgsql-s3-backup) for taking the backups

Special thanks to @schickling for the initial basis of these images. See <https://github.com/schickling/dockerfiles>.

## Warning

This will potentially put your database in a very bad state or complete destroy your data, be very careful.

## Limitations

This is made to restore a backup made from pgsql-s3-backup, if you backup came from somewhere else please check your format.

- Your s3 bucket path (including any prefix) *must* only contain backups which you wish to restore - it will always grabs the 'latest' based on unix sort with no filtering
- They must be gzip encoded text sql files
- If your bucket has more than a 1000 files the latest may not be restore, only one s3 ls command is made

## Usage

Docker:
```sh
$ docker run -e S3_ACCESS_KEY_ID=key -e S3_SECRET_ACCESS_KEY=secret -e S3_BUCKET=my-bucket -e S3_PREFIX=backup -e POSTGRES_DATABASE=dbname -e POSTGRES_USER=user -e POSTGRES_PASSWORD=password -e POSTGRES_HOST=localhost ghcr.io/chadweimer/pgsql-s3-restore
```

## Dropping public

If you wish to drop the public schema (drop schema public cascade; create schema public) then set the environment variable DROP_PUBLIC=yes. This is useful for situations where you wish to restore a database which currently has data / schemas in it.
