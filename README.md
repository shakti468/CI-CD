# CI/CD Pipeline for Auto-Deploying a Static Website using GitHub, Python, Bash, and Nginx
This project sets up a simple CI/CD pipeline that automatically deploys a static website hosted on a GitHub repository to an Nginx server using Python and Bash scripts. The deployment process checks for new commits and updates the live website accordingly.
CI/CD Pipeline with GitHub, Python, Bash, and Nginx
This project demonstrates a basic CI/CD pipeline that automatically pulls changes from a GitHub repository and deploys them to a web server served by Nginx. The setup includes:

- A Python script to check for new commits on GitHub

- A Bash script to update the website content

- A wrapper script for automation

- A cron job to run the pipeline every 5 minutes



## 🔧 Technologies Used

- **Python** – For GitHub commit tracking
- **Bash** – For pulling updates and syncing files
- **Crontab** – For scheduled checking and deployment
- **GitHub** – Version control and code hosting
- **Nginx** – Web server to serve HTML content
- **Rsync** – For efficient file transfer


## 🌐 Project Structure
```bash
CI-CD/
├── check_github.py         # Checks for new GitHub commits
├── update_website.sh       # Pulls latest changes and updates web files
├── ci_cd_wrapper.sh        # Wraps Python & Bash scripts
├── last_commit.txt         # Stores latest commit SHA
└── index.html              # Web content
```

### 🚀 Step-by-Step Setup
##  Clone your GitHub repo
```bash
git clone https://github.com/shakti468/CI-CD.git
```

📸 Screenshot of terminal cloning the repo

![image](https://github.com/user-attachments/assets/a2b10743-6f97-475f-b2df-72801698d707)


----

## Install Nginx
```bash
sudo apt update
sudo apt install nginx
sudo systemctl start nginx
```

## Create a simple HTML project and push it to a GitHub repository. 
📸 Screenshot


![image](https://github.com/user-attachments/assets/743894d3-b1f1-4e84-a86f-6bf2ebb64511)

![image](https://github.com/user-attachments/assets/6b7f3de1-e723-4057-a1cd-e34f29640e0e)


---


## Install Python and Required Packages
```bash
sudo apt install python3 python3-pip
pip3 install requests
 ```

### Python Script: check_github.py
```bash
nano check_github.py
 ```
## Code
```bash
# check_github.py
import requests, os, sys

REPO_OWNER = 'shakti468'
REPO_NAME = 'CI-CD'
GITHUB_API_URL = f'https://api.github.com/repos/{REPO_OWNER}/{REPO_NAME}/commits'
LAST_COMMIT_FILE = '/home/shakt/b10/CI-CD/last_commit.txt'

def get_latest_commit():
    response = requests.get(GITHUB_API_URL)
    if response.status_code == 200:
        return response.json()[0]['sha']
    else:
        print(f"Failed to fetch commits: {response.status_code}")
        return None

def get_stored_commit():
    if os.path.exists(LAST_COMMIT_FILE):
        with open(LAST_COMMIT_FILE, 'r') as f:
            return f.read().strip()
    return None

def update_stored_commit(sha):
    with open(LAST_COMMIT_FILE, 'w') as f:
        f.write(sha)

def main():
    latest = get_latest_commit()
    if not latest:
        sys.exit(1)
    stored = get_stored_commit()
    if latest != stored:
        print("New changes detected")
        update_stored_commit(latest)
        sys.exit(0)
    else:
        print("No new changes")
        sys.exit(1)

if __name__ == "__main__":
    main()

 ```

## Grant Execute Permission to Python Script (chmod +x)
```bash
chmod +x check_github.py
```

📸 Screenshot of script output: "New changes detected" or "No new changes"

![image](https://github.com/user-attachments/assets/037fadc5-a83d-4e4c-83aa-f237709b5057)


---

### Bash Script: update_website.sh

```bash
nano update_website.sh

```

## Code
```bash
#!/bin/bash

REPO_DIR="/home/shakt/b10/CI-CD"
WEBSITE_DIR="/var/www/html"

cd $REPO_DIR || exit
git pull origin main
rsync -av --delete $REPO_DIR/ $WEBSITE_DIR/
sudo systemctl restart nginx
echo "Website updated successfully"


```

## Grant Execute Permission to Bash Script
```bash
chmod +x update_website.sh


```

📸 Screenshot showing "Website updated successfully"

![image](https://github.com/user-attachments/assets/9d17d879-ec45-4525-94e8-589479968dc2)

---

### Wrapper Script: ci_cd_wrapper.sh

```bash
nano ci_cd_wrapper.sh

```
## Code 
```bash
#!/bin/bash
python3 /home/shakt/b10/CI-CD/check_github.py
if [ $? -eq 0 ]; then
    /home/shakt/b10/CI-CD/update_website.sh
fi


```

## Grant Execute Permission to Wrapper Script
```bash
chmod +x ci_cd_wrapper.sh

```

📸 Screenshot of wrapper execution

![image](https://github.com/user-attachments/assets/615b8246-9cda-42e3-8fc9-72872c4f63b9)

---

### Cron Job 

## Edit crontab:
```bash
crontab -e

```

## Add:
```bash
*/5 * * * * /home/shakt/b10/CI-CD/ci_cd_wrapper.sh >> /home/shakt/b10/CI-CD/ci_cd.log 2>&1

```

📸 Screenshot of crontab entry

![image](https://github.com/user-attachments/assets/c1d65f67-8f59-40eb-b9e3-1c0521845e27)

---




  




