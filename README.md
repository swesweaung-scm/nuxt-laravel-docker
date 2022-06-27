## Technology
- PHP 8.1.0
- Laravel 8.75
- NuxtJS 2.x
- BootstrapVue, ClosureTable
- Docker

## Environment setup

1. Change to project's root directory.  
    ``` cd <this project directory> ```  

2. Run Docker Containers.  
    ``` cp api/.env.example api/.env ```  
    ``` cp client/.env.example client/.env ```
   
3. Run Docker Containers.    
    ``` docker-compose up -d ```
    
4. Install php libraries.  
    ``` docker-compose exec app composer install --no-scripts ```

5. Create laravel-app-key.  
    ``` docker-compose exec app php artisan key:generate ```

6. Create tables and Insert data.  
    ``` docker-compose exec app php artisan migrate --seed ```
  
7. Connect `http://localhost:8090`.
