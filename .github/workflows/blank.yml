name: Deploy React + PHP App (react-app)

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Build React frontend
        run: |
          cd frontend
          npm install
          npm run build

      - name: Deploy to EC2
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ${{ secrets.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            cd /var/www/react-app
            git pull origin main

            # Deploy React build
            #rm -rf public/*
            #cp -r frontend/build/* public/

            sudo rm -rf /var/www/react-app/public/*
            sudo cp -r /var/www/react-app/frontend/dist/* /var/www/react-app/public/


            sudo cp -r /var/www/react-app/frontend/dist/* /var/www/react-app/public/
            sudo chown -R www-data:www-data /var/www/react-app/public
            sudo chmod -R 755 /var/www/react-app/public
            sudo systemctl reload nginx

            # PHP dependencies (optional)
            if [ -f backend/composer.json ]; then
              cd backend
              composer install --no-dev --optimize-autoloader
            fi

