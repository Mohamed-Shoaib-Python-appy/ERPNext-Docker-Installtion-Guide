# ERPNext ON Docker Installtion Guide

Proven Step by Step Guide for ERPNext Installation on Docker

## Prequsites:
- wsl (windows subsystem for linux)
  ```shell
  wsl --install
  wsl.exe --install
  ```
- docker
  ```shell
  https://www.docker.com/products/docker-desktop/
  ```
- vscode
  ```shell
  https://code.visualstudio.com/download
  ```
- git
  ```shell
  https://git-scm.com/downloads
  ```
- Install Dev Containers extension in VSCode
- docker-compose (for Linux/mac)
- user added to docker group (for Linux/mac)

## Steps:

### Step 1: Clone and change directory to frappe_docker directory
open the directory you want to download files into and
Use commands: 
```shell
git clone https://github.com/frappe/frappe_docker.git zatca-dev
cd zatca-dev
```

### Step 2: Copy example devcontainer config from `devcontainer-example` to `.devcontainer`

```shell
cp -R devcontainer-example .devcontainer
```

### Step 3: Copy example vscode config for devcontainer from `development/vscode-example` to `development/.vscode`. This will setup basic configuration for debugging.

```shell
cp -R development/vscode-example development/.vscode
```

### Step 4: Open frappe_docker folder in VS Code.

```shell
code .
```

### Step 5:  `Dev Containers: Reopen in Container`. 

- Launch the command, from Command Palette (Ctrl + Shift + P) `Dev Containers: Reopen in Container`. You can also click in the bottom left corner to access the remote container menu.

- wait for its loading

#### expect to see the container in vscode like the following:
![image](imgs/first_container.png)

### step 6: Make sure files `installer.py` and `apps-example.json` are set to LF Not CRLF
![image](imgs/LF.png)
- 1 open the file
- 2 click on `CRLF`below 
- 3 Select LF from the upper window
- 4 save the file
- 5 repeat steps with `apps-example.json`

### Step 7: inside vscode open a new terminal in the container `Terminal Menu -> New Terminal`

### Step 8: run installer.py

- inside the terminal run:
 ```shell
python installer.py
```
- Don't close the terminal - wait for frappe and erpnext installation finish. -it may take a lot of time

### Step 8: Start Bench
- run commands:
 ```shell
cd frappe-bench
bench start
```
- Test frappe and erpnext
    - open `development.localhost:8000` in browser
    - use credintials: `Administrator` : `admin` to login
    - complete installation of frappe
        - Time Zone : `Asia/Riyadh`
        - Currency  : `SAR`
    - click next
    - company name : `Appy Innovate`
    - click `complete setup`
## Congratulations! Now You have your erpnext on Docker

- ----------------------------------------------------

# Steps To Install zatca Customization app:


## Confiigure Git with your company GitHub account via SSH

### Step 1: Make SSH Key:
- Use bech virtual env:
```shell
source frappe-bench/env/bin/activate
```
![image](https://github.com/Mohamed-Eid-Appy/ERPNext-Docker-Installtion-Guide/assets/170640563/56a7f21e-276b-4ee6-8032-713183b8e3f9)

- run this command to create the ssh key:
```shell
ssh-keygen -t rsa -b 4096 -C "MyEmail@appyinnovator.com" #change MyEmail@appyinnovator.com with your company github email
```

press enter, enter, enter
![image](https://github.com/Mohamed-Eid-Appy/ERPNext-Docker-Installtion-Guide/assets/170640563/36bd8684-8776-4885-a1e1-092e20f266d0)

- Add SSH Key to SSH Agent:
  - Start the SSH agent in the background:
  ```shell
  eval "$(ssh-agent -s)"
  ```
  - Add your SSH private key to the SSH agent:
  ```shell
  ssh-add ~/.ssh/id_rsa
  ```
  ![image](https://github.com/Mohamed-Eid-Appy/ERPNext-Docker-Installtion-Guide/assets/170640563/8864eca1-ce07-4e73-8b75-640a5e0d49e6)

- Add SSH Key to GitHub Account:
  - Copy the SSH key to your clipboard:
  ```shell
  cat ~/.ssh/id_rsa.pub
  ```
   copy  the result of previous command (Like the selected in the image below)
  ![image](https://github.com/Mohamed-Eid-Appy/ERPNext-Docker-Installtion-Guide/assets/170640563/34f9b3dc-a165-43b4-ae03-712acbce2a5e)

  - Go to GitHub > Settings > SSH and GPG keys > New SSH key, and paste the key and give it a name and click Add ssh Key.
    ![image](https://github.com/Mohamed-Eid-Appy/ERPNext-Docker-Installtion-Guide/assets/170640563/ce24daa7-54cf-4d5f-97b4-ffcf98607835)
  - Go back to terminal and make sure you're authenticated:
    run the command:
    ```shell
    ssh -T git@github.com
    ```
    you should see a welcome message like this:
    ![image](https://github.com/Mohamed-Eid-Appy/ERPNext-Docker-Installtion-Guide/assets/170640563/efb8016f-1e16-4300-8029-a0fc903bef6a)
    #### congraulations Your Git is now connected to tour company Github Account!

### Step 2: install zatca app on your bench:
- Install zatca app in your bench by running the command:
  ```shell
  bench get-app git@github.com:readerorg/ERPNext-Zatca.git
  ```
  ![image](https://github.com/Mohamed-Eid-Appy/ERPNext-Docker-Installtion-Guide/assets/170640563/a2155dd8-96c7-4355-8ffd-8732dd337dca)

- make sure your remote branch name is ```origin```:
  ```shell
  git remote -v
  ```
  -   if its name is ```upstream``` not ```origin`` run the command:
     ```shell
     git remote rename upstream origin
     ```
- If you don't have your branch yet, make your branch and switch to it:
  ```shell
  git checkout -b Mohamed-Eid # replace Mohamed-Eid with your branch name
  ```
  ![image](https://github.com/Mohamed-Eid-Appy/ERPNext-Docker-Installtion-Guide/assets/170640563/0e7370a4-4a64-4b45-a688-379fab983c5f)
  Push your new-branch to Github:
  ```shell
  git push -u origin Mohamed-Eid  # reolace Mohamed Eid with  your recently created branch name
  ```
  ![image](https://github.com/Mohamed-Eid-Appy/ERPNext-Docker-Installtion-Guide/assets/170640563/c973dac3-5767-4f93-8392-901b28ebc799)

- If You have a branch already, Just switch to it

### Step 3: Make Sure Developer Mode is Enabled:
- open the frappe-bench directory in trrminal
- run the command:
  ```shell
  bench set-config developer_mode 1
  ```

### If you want to create a new site 
```shell
bench new-site test.localhost
```
- if you faced 'dotenv' error run this command
  ```shell
  python -m pip install python-dotenv
  ```
### If you want to know what is apps installed on your site 
```shell
bench --site test.localhost list-apps
```
  
### To install erpnext and ztca on your site:
```shell
bench --site test.localhost install-app erpnext
bench --site test.localhost install-app zatca
```

### Then you need to make this site the default
```shell
bench use test.localhost
```
### Run your site
```shell
bench start
```

### Step 5: Make Your Edits and open ```frappe-bench/apps/zatca``` in terminal and push them to github



# Congratulations We're done!
