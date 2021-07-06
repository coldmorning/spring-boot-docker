 # Spring-Boot-Docker (Linux-Version)


這個[Docker-Compose](https://docs.docker.com/compose/)腳本幫助大家怎麼把Spring Boot Project, mariadb 專案打包成Docker
### `docker-compose up`

* **Step1.**  先自行設定Spring Boot專案,使專案能產生war檔,網路有許多教學

* **Step2.** mvn package (把專案打包成war檔案) 

* **Step3.** Dockerfile設定
修改Dockerfile腳本內的 /target/spring-boot-0.0.1-SNAPSHOT.war 的war檔為與專案產生的檔案名稱一致
* **Step4.** 編輯 docker-compose.yml 



