# HAP
## 初始化数据库
    mvn process-resources -D skipLiquibaseRun=false -D db.driver=com.mysql.jdbc.Driver -D db.url=jdbc:mysql://127.0.0.1:3306/hap_dev -Ddb.user=hap_dev -Ddb.password=hap_dev

## jar包安装到maven 仓库
    mvn install:install-file -Dfile=D:\Professional\JavaEnvironment\Maven\repository\com\hand\hap-test\core-classes.jar -DgroupId=com.hand -DartifactId=hap-test -Dversion=1.0.0 -Dpackaging=jar


## 新拉开发项目
    mvn org.apache.maven.plugins:maven-archetype-plugin:2.4:generate  -D archetypeGroupId=hap -D archetypeArtifactId=hap-webapp-archetype -D archetypeVersion=3.1-SNAPSHOT -D groupId=hbi -D artifactId=HbiParent -D package=hbi.core -D archetypeRepository=http://nexus.saas.hand-china.com/content/repositories/rdcsnapshot

## 创建数据库，用户
    create schema hap_dev default character set utf8;
    create schema hap_prod default character set utf8;
    CREATE USER hap_dev@'%' IDENTIFIED BY 'hap_dev';
    CREATE USER hap_dev@'localhost' IDENTIFIED BY 'hap_dev';
    GRANT ALL PRIVILEGES ON hap_dev.* TO hap_dev@'%';
    GRANT ALL PRIVILEGES ON hap_dev.* TO hap_dev@'localhost';
    GRANT ALL PRIVILEGES ON hap_prod.* TO hap_dev@'%';
    GRANT ALL PRIVILEGES ON hap_prod.* TO hap_dev@'localhost';

	flush privileges;


## docker 数据库容器初始化

    docker run -d -e SA_PASSWORD=Jp@sqlserver -e SQLSERVER_DATABASE=hap_dev -e SQLSERVER_USER=hap_dev -e SQLSERVER_PASSWORD=hap_dev -p 1433:1433 exoplatform/sqlserver:latest

