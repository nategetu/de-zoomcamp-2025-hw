# Question 1: Understanding docker first run
docker run -it  --entrypoint=bash python:3.12.8 

pip --version

Answer: pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)

# Question 2. Understanding Docker networking and docker-compose
docker compose up -d

login to http://127.0.0.1:8080/login?next=/

register server, login w/hostname postgres at port 5432

# Question 3. Trip Segmentation Count
compose upload_data.ipynb to upload trip and zone data

```sql
select case when trip_distance <= 1 then  'Up to 1 mile'
    when trip_distance > 1 and trip_distance <= 3 then'In between 1 (exclusive) and 3 miles (inclusive)'
    when trip_distance > 3 and trip_distance <= 7 then 'In between 3 (exclusive) and 7 miles (inclusive)'
    when trip_distance > 7 and trip_distance <= 10 then 'In between 7 (exclusive) and 10 miles (inclusive)'
    when trip_distance > 10 then 'Over 10 miles'
	else 'unknown' end as trip_length, count(*)
from green_taxi_trips
group by 1;
```

> "Up to 1 mile"	104838
> "In between 1 (exclusive) and 3 miles (inclusive)"	199013
> "In between 3 (exclusive) and 7 miles (inclusive)"	109645
> "In between 7 (exclusive) and 10 miles (inclusive)"	27688
> "Over 10 miles"	35202

# Question 4. Longest trip for each day
```sql
select cast(lpep_pickup_datetime as DATE) from green_taxi_trips order by trip_distance desc limit 1;
```

> 2019-10-31

# Question 5. Three biggest pickup zones
```sql
select b."Zone" 
from green_taxi_trips a
join zones b
on a."PULocationID" = b."LocationID"
where cast(lpep_pickup_datetime as DATE) = '2019-10-18'
group by b."Zone"
having sum(total_amount) > 13000;
```
> "East Harlem North"
> "East Harlem South"
> "Morningside Heights"

# Question 6. Largest tip
```sql
select dropoff."Zone" 
from green_taxi_trips a
join zones pickup
on a."PULocationID" = pickup."LocationID"
join zones dropoff
on a."DOLocationID" = dropoff."LocationID"
where pickup."Zone" = 'East Harlem North'
order by a.tip_amount desc
limit 1;
```

> "JFK Airport"

# Question 7. Terraform Workflow

We began terraform setup with terraform init, executed our .tf file with terraform apply (with the -auto-approve flag to automatically execute), and finally removed resources with terraform destroy.