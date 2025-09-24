import requests, os, time
from datetime import datetime

# যার রিপো track করবে তার username বসাও
username = "TARGET_USERNAME"

# লোকালে ক্লোন হওয়ার ফোল্ডার
save_path = "/data/data/com.termux/files/home/repos/"
log_file = os.path.join(save_path, "repo_log.txt")

# ফোল্ডার না থাকলে বানিয়ে নাও
if not os.path.exists(save_path):
    os.makedirs(save_path)

# আগে থেকে দেখা repo ট্র্যাক করার জন্য
seen = set(os.listdir(save_path))

def log_repo(name, url):
    with open(log_file, "a") as f:
        now = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        f.write(f"[{now}] Cloned: {name} | {url}\n")

while True:
    try:
        url = f"https://api.github.com/users/{username}/repos"
        repos = requests.get(url).json()

        for repo in repos:
            name = repo["name"]
            clone_url = repo["clone_url"]

            if name not in seen:   # শুধু নতুন repo হলে clone করবে
                target = os.path.join(save_path, name)
                os.system(f"git clone {clone_url} {target}")
                print(f"[+] New repo cloned & saved permanently: {name}")
                log_repo(name, clone_url)  # লগে লিখবে
                seen.add(name)

    except Exception as e:
        print("Error:", e)

    time.sleep(60)  # প্রতি ১ মিনিট পর পর চেক করবে
