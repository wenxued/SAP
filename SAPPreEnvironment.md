EC

MDW 

s/4

https://github.wdf.sap.corp/pages/cap/cds/

cds 建表  vs code

​     view

1.git 

pull request





**配置信息**

1. 配置连接信息 在下列文件中`srv\src\main\resources\connection.properties`

   ```
   #DB Connection Properties
   schema=
   username=
   password=
   connectionURL=
   encrypt=true&validateCertificate=true&currentschema=
   
   ```

2. 替换db文件下面的 `package.json`

   ```json
   {
     "name": "deploy",
     "dependencies": {
       "@sap/hdi-deploy": "3.7.0",
       "@sap/reusemodel-countrysubdivision": "1.3.1-20181220040523"
     },
     "scripts": {
       "postinstall": "node .build.js",
       "start": "node node_modules/@sap/hdi-deploy/deploy.js --auto-undeploy"
     }
   }
   
   ```

   

3. db 文件下要执行

```java
   npm install
   npm start
```

4.mta.yaml 替换

```yml
ID: WorkforcePersonServiceOData_1
_schema-version: '2.1'
version: 0.0.1
modules:
 
  - name: workforce-person-db
    type: hdb
    path: db
    parameters:
      memory: 256M
      disk-quota: 256M
    requires:
      - name: workforce-person-hdi
      
  - name: workforce-person-srv
    type: java
    path: srv
    parameters:
      memory: 1G
      disk-quota: 1G
    provides:
      - name: srv_api
        public: true 
        properties:
          url: '${default-url}'
    requires:
      - name: workforce-person-hdi
        properties:
           JBP_CONFIG_RESOURCE_CONFIGURATION: '[tomcat/webapps/ROOT/META-INF/context.xml:
            {"service_name_for_DefaultDB" : "~{hdi-container-name}"}]'
      - name: workforce-person-uaa

resources:
  - name: workforce-person-hdi
    type: com.sap.xs.hdi-container
    properties:
      hdi-container-name: ${service-name}
      
  - name: workforce-person-uaa
    type: com.sap.xs.uaa
    parameters:
      config:
        xsappname: WorkforcePersonServiceOData_1
        tenant-mode: dedicated
        scopes:
         - name: $XSAPPNAME.Show
           description: display
         - name: $XSAPPNAME.Add
           description: create
         - name: $XSAPPNAME.Remove
           description: delete

```



**运行两种方案**

- 第一种

  生成的 war包 拷到`tomcat\webapps`中,启动`tomcat\bin\startup.bat` 文件即可

  

- 第二种

  切到 根目录 打包mtar文件，打包之前替换一下mta.yml 产生我们需要的（由于名字原因 替换一下ID 名字）

  ```java
  java -jar D:\buparepository\bp-reuse-services\prerequisites\mta.jar --build-target=CF build
  ```

  ```java
java -jar c:\A_erercise\other\mta.jar --build-target=CF build
  ```

  
  
  执行之后生成出一个`xxx.mtar`文件
  
  最后
  
  ```
   cf deploy xxx.mtar
  ```

 用 postman 进行查询



最后

https://github.wdf.sap.corp/pages/cap/get-started/local-dev/#using-cds

**Error:**

1. Can't find com.sap.db.jdbc 修改pom.xml 文件 

```xml
<dependency>
	<groupId>com.sap.cloud.db.jdbc</groupId>
	<artifactId>ngdbc</artifactId>
	<version>2.4.59</version>
</dependency> 
```





Down



https://tools.hana.ondemand.com/#cloud









