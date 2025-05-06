# CI/CD Pipeline for Auto-Deploying Static Website using GitHub, Python, Bash, and Nginx
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
## 1️⃣ Clone your GitHub repo
```bash
git clone https://github.com/shakti468/CI-CD.git
```


