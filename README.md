# playStudios-test


## 1. Navigate to the `docker` folder1. 
Run the following command:
```bash
cd nuxtapp
```

## 2. Copy file .env.example to .env
Use the command:

bash
Sao chép mã

```
cp .env.example .env
```

## 3. Start the containers
```
docker compose up -d 

Install dependencies:
docker exec -it docker-web-1 yarn 
```

## 4. Start the development server
Use the command
```
docker exec -it docker-web-1 yarn dev --host
```

## 5. Access the application
Open your browser and go to:
http://playstudios.localhost:8000

Thank you and best regards,
