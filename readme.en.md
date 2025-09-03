# 🐱 Kittygram — a social network for sharing photos of your favorite pets
![example workflow](https://github.com/VilmenAbramian/kittygram_final/actions/workflows/main.yml/badge.svg)

## 📖 Project Description

Kittygram allows users to register accounts, upload photos of their pets with descriptions, and specify the pet’s color and "achievements".

## ⚙️ Installation
<i>Note: All examples are shown for Linux</i><br>
1. Clone the repository to your computer:
    ```
    git clone git@github.com:VilmenAbramian/kittygram_final.git
    ```
2. Create a `.env` file and fill it with your own data. All required variables are listed in the `.env.example` file located in the root directory of the project.

## 🐳 Building Docker Images

1. Replace `YOUR_USERNAME` with your DockerHub login:
    ```
    cd frontend
    docker build -t YOUR_USERNAME/kittygram_frontend .
    cd ../backend
    docker build -t YOUR_USERNAME/kittygram_backend .
    cd ../nginx
    docker build -t YOUR_USERNAME/kittygram_gateway . 
    ```
2. Push the images to DockerHub:
    ```
    docker push YOUR_USERNAME/kittygram_frontend
    docker push YOUR_USERNAME/kittygram_backend
    docker push YOUR_USERNAME/kittygram_gateway
    ```

## 🚀 Deploy to Server

1. Connect to the remote server:
    ```
    ssh -i PATH_TO_SSH_KEY/SSH_KEY_NAME YOUR_USERNAME@SERVER_IP_ADDRESS 
    ```
2. Create the `kittygram` directory on the server:
    ```
    mkdir kittygram
    ```
3. Install Docker Compose on the server:
    ```
    sudo apt update
    sudo apt install curl
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    sudo apt install docker-compose
    ```
4. Copy the `docker-compose.production.yml` and `.env` files to the `kittygram/` directory on the server:
    ```
    scp -i PATH_TO_SSH_KEY/SSH_KEY_NAME docker-compose.production.yml YOUR_USERNAME@SERVER_IP_ADDRESS:/home/YOUR_USERNAME/kittygram/docker-compose.production.yml
    ```
    Where:
    - `PATH_TO_SSH_KEY` - path to your SSH key file
    - `SSH_KEY_NAME` - name of your SSH key file
    - `YOUR_USERNAME` - your server username
    - `SERVER_IP_ADDRESS` - your server IP address

5. Run Docker Compose in detached mode:
    ```
    sudo docker-compose -f /home/YOUR_USERNAME/kittygram/docker-compose.production.yml up -d
    ```
6. Apply migrations, collect static files from the backend, and copy them to `/backend_static/static/`:
    ```
    sudo docker-compose -f /home/YOUR_USERNAME/kittygram/docker-compose.production.yml exec backend python manage.py migrate
    sudo docker-compose -f /home/YOUR_USERNAME/kittygram/docker-compose.production.yml exec backend python manage.py collectstatic
    sudo docker-compose -f /home/YOUR_USERNAME/kittygram/docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
    ```
7. Open the Nginx configuration file in the nano editor:
    ```
    sudo nano /etc/nginx/sites-enabled/default
    ```
8. Modify the `location` settings in the `server` section:
    ```
    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:9000;
    }
    ```
9. Test the Nginx configuration:
    ```
    sudo nginx -t
    ```
    If you see the following response, there are no errors:
    ```
    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful
    ```
10. Reload Nginx:
    ```
    sudo service nginx reload
    ```

## 🛠 Technologies
![Python](https://img.shields.io/badge/Python-14354C?style=for-the-badge&logo=python&logoColor=white)![Django](https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/DRF-Django%20REST%20framework-red?style=for-the-badge)![Postgres](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)

## 👤 Author
[Vilmen Abramian](https://github.com/VilmenAbramian), vilmen.abramian@gmail.com  

Sprint 17
