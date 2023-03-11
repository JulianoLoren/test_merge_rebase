# KiotPro CRM

KiotPro CRM là dự án CRM của KiotVietConnect

## Table of Contents

- [Getting Started](#getting-started)
- [Usage](#usage)
- [Contributing](#contributing)

## Getting Started

Hướng dẫn cài đặt để chạy local

### Prerequisites

Các phần mềm, công cụ cần để chạy project.

- MySql >= 10.6
- PHP 7.4
- docker-compose (môi trường dev)

### Installation

Hướng dẫn cài đặt môi trường dev của dự án và các dependencies

* Clone repo: `git clone https://gitlab.citigo.com.vn/citigo/kiotviet-pro-crm.git`
- unzip file `` vào thư mục gốc dự án
- Chạy các lệnh sau

    ```
    cd kiotviet-pro-crm
    unzip user_privileges.zip
    mkdir -p storage/vtlib
    mkdir -p tests
    chmod -R 777 *
    cp config.inc.local.example.php config.inc.php
    ```

- Sửa file config.inc.php ở các dòng sau:

    ```
    $dbconfig['db_server'] = 'mysql';
    $dbconfig['db_port'] = ':3306';
    $dbconfig['db_username'] = 'default';
    $dbconfig['db_password'] = 'secret';
    $dbconfig['db_name'] = 'default';
    $dbconfig['db_type'] = 'mysqli';
    $dbconfig['db_status'] = 'true';
    $site_URL = 'http://kpcrm.local';
    $root_directory = '/var/www/kiotviet-pro-crm';
    ```

- Sửa file `/etc/hosts` máy bạn thêm dòng sau `127.0.0.1 kpcrm.local`
- Cài `docker-compose`:
    ```
    apt-get update -y
    apt-get install sudo -y
    curl -sL https://get.docker.com/ | sudo -E bash -
    sudo curl -L https://github.com/docker/compose/releases/download/1.29.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    docker-compose --version
    ```


* Cài laradock:

    ```
    git clone https://github.com/Laradock/laradock.git
    cd laradock
    cp .env.example .env
    cp ./nginx/sites/default.conf ./nginx/sites/kpcrm.conf
    ```
- Kiểm tra file `.env` vừa copy. Nếu có dòng `PHP_VERSION=7.4` là đúng. Sửa file đó như sau:
    ```
    PHP_FPM_INSTALL_MONGO=true
    WORKSPACE_INSTALL_MONGO=true
    MYSQL_PORT=3307
    ```

- Sửa file `kpcrm.conf` vừa copy ở 4 dòng sau

    ```
    listen 80;# default_server;
    #listen [::]:80 default_server ipv6only=on;
    server_name kpcrm.local; # site trong file host
    root /var/www/kiotviet-pro-crm; # lấy tên thư mục là thư mục code clone ở bước đầu tiên, giữ nguyên /var/www/...
    ```

## Usage

Hướng dẫn sử dụng dự án và các thông tin liên quan

- Vào thư mục laradock chạy lệnh sau `docker-compose up -d nginx mysql workspace`

- Nếu là lần đầu tiên khởi chạy:
    - Cài các dependencies 
        ```
        docker-compose exec workspace bash
        cd /var/www/kiotviet-pro-crm
        composer i
        ```
    - Đưa db vào mysql của laradock:
        ```
        mysql -hlocalhost -P3307 -u default -psecret

        use default;

        ALTER DATABASE `default`
        DEFAULT CHARACTER SET eucjpms
        DEFAULT COLLATE utf8mb4_general_ci;

        source /path/to/sql/file.sql;
        ```
    
- Truy cập qua đường dẫn `kpcrm.local` với username/password `admin/Citigo@123`

## Contributing

Hướng dẫn cách thức đóng góp cho dự án.

Ví dụ

* Cách submit pull requests
* Coding conventions.
* etc
