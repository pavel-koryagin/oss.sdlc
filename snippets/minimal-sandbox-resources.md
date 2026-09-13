## GCP

### DB

```shell
gcloud sql instances create postgres-db \
    --database-version=POSTGRES_15 \
    --tier=db-f1-micro \
    --storage-type=SSD \
    --storage-size=10 \
    --no-storage-auto-increase \
    --availability-type=zonal \
    --no-backup \
    --region=REGION
```
