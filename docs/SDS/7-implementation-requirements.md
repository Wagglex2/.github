## H/W platform requirements


* **Application (Client PC)**
    * Processor: Intel i3 / AMD Ryzen 3 (또는 동급 이상)
    * RAM: 4GB+
    * Storage: N/A (웹 브라우저 기반)
    * Network: connected to WAN over TCP/IP

## S/W platform requirements


* **Client (Frontend) Development**
    * IDE: Visual Studio Code
    * Framework: React.js
    * Implementation Language: JavaScript, HTML, CSS
* **Server (Backend) Development**
    * IDE: IntelliJ
    * Language: Java 17
    * Build Tool: Gradle
* **Common**
    * App OS (Target Client): Windows 10 (및 주요 웹 브라우저)
    * Develop Environment OS: Windows 10/11, macOS

## Server platform requirements


* **H/W (App server)**
    * Microsoft Azure VM (권장)
    * Instance type: Standard_B1s (1 vCPU, 1GB RAM) (t3.micro와 유사)
    * Storage: 30GB
* **H/W (DB server - MySQL)**
    * Azure Database for MySQL
    * Instance type: Basic (1-2 vCore)
    * Storage: 20GB
* **H/W (Cache server - Redis)**
    * Azure Cache for Redis
    * Instance type: Basic C0 (250MB Cache)
* **S/W (Server)**
    * OS: Windows Server 또는 Ubuntu 22.04 LTS (권장)
    * Language: Java 17
    * Framework: Spring Boot 3.5.6, Spring Security, Spring Data JPA
    * Libraries: OpenFeign, QueryDSL, KOMORAN, JJWT
    * DBMS: MySQL, Redis
