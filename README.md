<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>
<!-- <p align="center">
  <a href="https://laravel.com" target="_blank">
    <img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="200" alt="Laravel Logo">
  </a>
  <a href="https://vuejs.org" target="_blank" style="margin-left: 20px;">
    <img src="https://vuejs.org/images/logo.png" width="200" alt="Vue.js Logo">
  </a>
  <a href="https://inertiajs.com" target="_blank" style="margin-left: 20px;">
    <img src="https://raw.githubusercontent.com/inertiajs/inertia/master/logo.svg" width="200" alt="Inertia.js Logo">
  </a>
</p> -->

# Project Name

## Overview
This project is built using Laravel and Vue.js 3 with the Composition API. Follow the instructions below to set up the project on your local machine.

## Prerequisites
Ensure you have the following installed before proceeding:
- PHP (>=8.0 recommended)
- Composer
- Node.js (>=16.x recommended)
- NPM or Yarn
- MySQL or any other supported database

## Installation

### 1. Clone the Repository
```sh
git clone https://github.com/your-username/your-repository.git
cd your-repository
```

### 2. Setup Laravel Backend

#### Install PHP dependencies
```sh
composer install
```

#### Copy Environment File
```sh
cp .env.example .env
```

#### Generate Application Key
```sh
php artisan key:generate
```

#### Configure Database
Edit the `.env` file and update the database settings:
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database_name
DB_USERNAME=your_database_user
DB_PASSWORD=your_database_password
```

#### Run Migrations & Seeders (if applicable)
```sh
php artisan migrate --seed
```

#### Serve the Application
```sh
php artisan serve
```

### 3. Setup Frontend (Vue.js)

#### Install Node.js Dependencies
```sh
npm install  # or yarn install
```

#### Build Assets
For development:
```sh
npm run dev
```
For production:
```sh
npm run build
```

#### Run Vite Development Server (if needed)
```sh
npm run dev
```

## Additional Commands

### Clear Cache
```sh
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear
```

### Storage Link (for file uploads)
```sh
php artisan storage:link
```

## Contributing
Feel free to contribute to this project. Please follow best coding practices and submit a pull request.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
