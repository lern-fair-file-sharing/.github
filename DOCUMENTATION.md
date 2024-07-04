# 🎓 Lern-Fair File Sharing Documentation 🚀

The following is the documentation for setting up this project, which consists of a NextCloud backend and React Native frontend.

## 💡 What can you expect from this application?
The app will show a mocked [Lern-fair](https://www.lern-fair.de/) mobile app. The goal is to demonstrate how a file system can be integrated into the app to store, sort and filter files based on in which course chat they were uploaded.
For that we introduce a new tab: "Dateien", which contains the file system with the following structure: On the root level there is a "Persönliche Ablage" folder and
folders with the names of courses in which the testuser is enrolled (these are dummy courses and created later in the documentation). Under the "Kurs" tab you will see a list of the actual courses.
When you click on a course you will see a chat which can be used to upload files. These uploaded files will land in the corresponding folder inside the "Dateien" tab. You can also upload files independently of courses
by using the "Persönliche Ablage" folder.

Since our goal wasn't to integrate authentication but just to demonstrate the file sharing capability, everything runs over a single test-user which authentication is hardcoded via a ``.env`` in the frontend.

## 1️⃣ Setup backend
Our backend consists of a Nextcloud instance, a Postgres database and Redis caching database.

## ⚠️ Warning: This container does not run on Windows. Windows Users should [install WSL2](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers)

### 🛠️ Setup Instructions
#### 1. Clone the Repository
First, clone the repository to your local machine using the following command:
```bash
git clone https://github.com/lern-fair-file-sharing/backend
cd backend
```

#### 2. Configure envrionment variables
Create a .env file with the following variables:
```
NCUSR=<nextcloud-user>
NCPW=<nextcloud-password>
```
An examplary .env file is included, but pleae don't use this in a production environment.

#### 3. Set NextCloud secrets
Inside the ``./secrets/`` folder create the following files:
- ``nextcloud_admin_password.txt``: In here put the admin password
- ``nextcloud_admin_user.txt``: In here put the admin user name
- ``postgres_db.txt``: In here put the database name "postgresdb"
- ``postgres_password.txt``: In here put the postgres user password
- ``postgres_user.txt``: In here simply the postgres user name

Examplary secrets are included, but pleae don't use them in a production environment.

### 🚀 Run backend
#### 1. Start containers:
```bash
docker compose up
```
After this the NextCloud instance will be starting and the admin user automatically created.
You can visit NextCloud at [http://localhost:8080](http://localhost:8080) (or whatever port you specified) and register as the admin.
   
#### 2. Initialize examplary folder structure:
Our application requires a specific folder structure which contains dummy files (see [./initial-setup/example-folder-structure/](https://github.com/lern-fair-file-sharing/backend/tree/master/inital-setup/example-folder-structure)). To create it execute the script [initialization.py](https://github.com/lern-fair-file-sharing/backend/blob/master/initialization.py). Make sure you have the [requests](https://pypi.org/project/requests/) package installed.

As long as you don't delete the container and volumes, you don't have to do these steps everytime you want to use this. Just start docker and the backend will start automatically. But if you need to reset the NextCloud instance use the following command:
```bash
docker compose down -v
```

---

## 2️⃣ Setup frontend

### 🛠️ Setup Instructions
#### 1. Clone the Repository
First, clone the repository to your local machine using the following command:
```bash
git clone https://github.com/lern-fair-file-sharing/app
cd app
```
#### 2. Install Dependencies
Make sure you have Node.js installed. Then, install the required dependencies:
```
npm install
```
#### 3. Configure Environment Variables
Create a .env file in the root of the project and add the following environment variables (these are our development variables, for production please replace):
```env
EXPO_PUBLIC_HOST_PORT=8080
EXPO_PUBLIC_USER=testuser
EXPO_PUBLIC_TOKEN="dGVzdHVzZXI6MTIzNA=="
```

### 🚀 Run frontend
Now, you can start the Expo development server:

#### Option 1: Using the Expo Go App
```
npm start --tunnel
```
Scan the QR code.

#### Option 2: Using the Anrdoid Studio Emulator
Use Android Studio or XCode for installing a mobile emulator. If installed run the following command:
```
npm run android
# or
npm run ios
```
