# NetflixMovieCatalog
### Create a EC2 instance
![image](https://github.com/user-attachments/assets/72a45473-fb49-4656-8fa7-a888b6c6bcf7)

### Edit inbound rules and open 8080 port
![image](https://github.com/user-attachments/assets/eb74b576-636c-47b0-a88d-3635cb3b7679)

### Create subdomain from # Route 53 service
![image](https://github.com/user-attachments/assets/b706090a-1345-40c2-a6ee-7cc0f03df93a)
 

NetflixMovieCatalog is a simple Flask-based API that provides information about TV shows and movies.

## Endpoints

- `GET /` - Welcome message
- `GET /discover` - Discover movies or TV shows
- `POST /updatePopularity` - Update the popularity of a specific movie
- `GET /status` - Check the API status

## Setup

1. Clone the repository:
    ```sh
    git clone https://github.com/yourusername/NetflixMovieCatalog.git
    cd NetflixMovieCatalog
    ```
   
    While changing the `yourusername` to your GitHub account name.

2. Create a Python virtual environment and activate it:
    ```sh
    python3 -m venv venv
    source venv/bin/activate
    ```

3. Install the required dependencies:
    ```sh
    pip install -r requirements.txt
    ```

4. Run the application:
    ```sh
    python app.py
    ```

## Data Files

- `data/data_tv.json` - Data for TV shows
- `data/data_movies.json` - Data for movies

## Generate the .github/workflows/service-deploy.yaml file
name: Netflix Movie Catalog Service Deployment

on:
  push:
    branches:
      - main

env:
  EC2_PUBLIC_IP: 51.20.68.173 # TODO replace to your EC2 instance public IP
  SSH_PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}  # TODO define this secret in your GitHub repo settings

jobs:
  Deploy:
    name: Deploy in EC2
    runs-on: ubuntu-latest
    steps:
      - name: Checkout the app code
        uses: actions/checkout@v2

      - name: SSH to EC2 instance
        run: |
          echo "$SSH_PRIVATE_KEY" > key.pem
          chmod 600 key.pem
          
          
          # Copy the files from the current work dir into the EC2 instance, under `~/NetflixMovieCatalog`.
          scp -o StrictHostKeyChecking=no -i key.pem -r * ubuntu@$EC2_PUBLIC_IP:~/NetflixMovieCatalog
          
          # Connect to your EC2 instance and execute the `deploy.sh` script (this script is part of the repo files). 
          # TODO You need to implement the `deploy.sh` script yourself.
          #
          # Upon completion, the NetflixMovieCatalog app should be running with its newer version. 
          # To keep the app running in the background independently on the terminal session you are logging to, configure it as a Linux service.
          ssh -i key.pem ubuntu@$EC2_PUBLIC_IP "bash ~/NetflixMovieCatalog/deploy.sh"


## Generate the ssh keys from settings




