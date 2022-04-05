run-pg-compose:
	docker-compose -f docker-compose.yml down && \
	docker-compose -f docker-compose.yml up --build -d && \
	sleep 5 && \
	echo "Postgres is running!"

run-pg-container:
	docker run --rm \
	-p 5432:5432 \
	-v myvol:/var/lib/postgresql/data \
	--env POSTGRES_USER=postgres \
	--env POSTGRES_PASSWORD=postgres \
	--env POSTGRES_DB=dbName \
	postgres:11