 # Spring-Boot-Docker (Linux-Version)


這個[Docker-Compose](https://docs.docker.com/compose/)腳本幫助大家怎麼把Spring Boot Project, mariadb 專案打包成Docker
### `docker-compose up`

* **Step1.**  先自行設定Spring Boot專案,使專案能產生war檔,網路有許多教學

* **Step2.** mvn package (把專案打包成war檔案) 

* **Step3.** Dockerfile設定
修改Dockerfile腳本內的 /target/spring-boot-0.0.1-SNAPSHOT.war 的war檔為與專案產生的檔案名稱一致
* **Step4.** 編輯 docker-compose.yml  (解說)
  
&emsp;&emsp;&emsp;&emsp; spinrg_boo_mariadb:
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;     image: 資料庫版本: MariaDB 10.1(MySQL分支) <br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;     container_name: **spring_boot_mariadb**
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;     volumes:
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; ps.資料夾共用,目的是移除container後,資料庫的資料依然存在本機端
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                           **本機資料夾: ./mysql-data**
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                          **映射Docker資料夾: /var/lib/mysql**
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;              ps.資料庫初始化,目的是資料庫新增後, 同時給予資料庫資料
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                            **本機資料夾./mysql-init**
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                            **映射Docker資料夾/docker-entrypoint-initdb.d**
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                             ps.本機資料夾內可放要出初始化資料庫的sql檔案,記得插入資料前要先建立SCHEMA,因此插入資料的第一行為
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;     **CREATE SCHEMA `schemaName` DEFAULT CHARACTER SET utf8mb4 ;**
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;     environment:  設定資料庫密碼
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;                   **MYSQL_ROOT_PASSWORD: 123456**
                  
&emsp;&emsp;&emsp;&emsp;   spring_boot_project:
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;    image: DockerFile產生自製Spring Boot image
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;    container_name: spring_boot_project container名稱
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;    build:  (./)執行DockerFile，產生image
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;    volumes: 資料夾共用 (./:/var/lib/docker)
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;   **本機資料夾: .**
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;  **映射Docker資料夾: /var/lib/docker**
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;    depends_on:
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;         ps.目的是把spring boot container與 database contaner放在同一個網路下, depends_on名稱為資料庫名稱
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;     -   spinrg_boo_mariadb
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;     environment:
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;         資料庫於本機環境為localhost or 對外的ip 可以改成資料庫的container(spinrg_boo_mariadb)<br>
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;       - SPRING.DATASOURCE.DRUID.FIRST.URL=jdbc:mysql://spinrg_boo_mariadb:3306/schemaName?useUnicode=true&characterEncoding=utf8&useSSL=false
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;          - SERVER_PORT=8890<br>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;    expose:
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;      - "8890"
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;    ports:
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;        - 8890:8890


