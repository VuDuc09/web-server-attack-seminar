# web-server-attack-seminar
@timoxoszt x @FlyingPeg x @VuDuc09 x @19520623
---

This repository provides instructions and resources for setting up Nginx and Apache web servers using Docker for educational purposes in a web server attack seminar. It includes configurations for both Linux and Windows environments.

## Table of Contents
- [Structures](#structures)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Running Apache on Linux](#running-apache-on-linux)
- [Running Nginx on Linux](#running-nginx-on-linux)
- [Notes](#notes)
- [License](#license)
- [Disclaimer](#disclaimer)

## Structures
```bash
.
└── web-server-attack-seminar/
    ├── Linux/
    │   ├── Apache/
    │   │   ├── vul-apache/
    │   │   └── docker-compose.yml
    │   └── Nginx/
    │       ├── config/
    │       ├── web/
    │       ├── Dockerfile
    │       ├── docker-compose.yml
    │       └── start.sh
    └── Windows/
        └── ...
```

## Prerequisites
Before you begin, make sure you have the following prerequisites installed on your system:

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/) (for running the provided `docker-compose.yml` files)

## Getting Started
1. Clone this repository to your local machine:
```bash
git clone https://github.com/F1301/web-server-attack-seminar.git
cd web-server-attack-seminar
```
Navigate to the web server you want to set up (Apache or Nginx).

## Running Apache on Linux
Navigate to the `Linux/Apache` directory.
Modify the `docker-compose.yml` file and any other configurations as needed.
Run Apache:
```bash
docker-compose up -d
```
The Apache web server will be accessible at `http://localhost:8082` in your web browser.

## Running Nginx on Linux
Navigate to the `Linux/Nginx` directory.
Modify the `docker-compose.yml` file and any other configurations as needed.
Run Nginx:
```bash
docker-compose up -d
```
Nginx web server will be accessible at `http://localhost:8083` in your web browser.

## Notes
Ensure that no other services are running on the same port as your web server (e.g., port 80) to avoid conflicts.

## License
This repository is provided for educational purposes and should be used responsibly and in compliance with applicable laws and regulations.

## Disclaimer
This repository is intended for educational purposes only. Use responsibly and only in a controlled environment. The authors and contributors are not responsible for any misuse or illegal activities carried out using the information and resources provided here.

Happy learning and stay safe!
