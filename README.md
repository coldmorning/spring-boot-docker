 Spring-Boot-Docker (Linux-Version)

這個腳本幫助大家怎麼把Spring Boot Project, database專案打包成Docker

### Step1. 先自行設定Spring Boot專案,使專案能產生war檔,網路有許多教學

### Step2. mvn package (把專案打包成war檔案) 

#### Step3. Dockerfile設定
修改Dockerfile腳本內的 /target/spring-boot-0.0.1-SNAPSHOT.war 的war檔為與專案產生的檔案名稱一致

#### Step5. Docker-compose設定
 docker-compose.yml   
 
   spinrg_boo_mariadb:
     image: 資料庫版本: MariaDB 10.1(MySQL分支)
     container_name: 1.資料庫container名稱(spinrg_boo_mariadb)
     volumes: 2.資料夾共用,目的是移除container後,資料庫的資料依然存在本機端
                           本機資料夾: ./mysql-data 
                           映射Docker資料夾: /var/lib/mysql
              3.資料庫初始化,目的是資料庫新增後, 同時給予資料庫資料
                            本機資料夾./mysql-init:
                            映射Docker資料夾/docker-entrypoint-initdb.d
                            本機資料夾內可放要出初始化資料庫的sql檔案,記得插入資料前要先建立SCHEMA,因此插入資料的第一行為 
                            CREATE SCHEMA `schemaName` DEFAULT CHARACTER SET utf8mb4 ;
    environment:  4.設定資料庫密碼
                  MYSQL_ROOT_PASSWORD: 123456
                  
   spring_boot_project:
    image: DockerFile產生自製Spring Boot image
    container_name: spring_boot_project container名稱
    build:  (./)執行DockerFile，產生image 
    volumes: 5.資料夾共用 (./:/var/lib/docker)
                         本機資料夾: .
                         映射Docker資料夾: /var/lib/docker
    depends_on:
        6.目的是把spring boot container與 database contaner放在同一個網路下, depends_on名稱為資料庫名稱
    -   spinrg_boo_mariadb
    environment:
         7.資料庫於本機環境為localhost or 對外的ip 可以改成資料庫的container(spinrg_boo_mariadb)
        - SPRING.DATASOURCE.DRUID.FIRST.URL=jdbc:mysql://spinrg_boo_mariadb:3306/schemaName?useUnicode=true&characterEncoding=utf8&useSSL=false
        - SERVER_PORT=8890
    expose:
      - "8890"
    ports:
        - 8890:8890
 
#### Step5. 啟動
### `docker-compose up`
