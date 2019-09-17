### Day 2

SAP Cloud Platform cockpit :

https://account.int.sap.hana.ondemand.com

### SAP development Tools

https://tools.hana.ondemand.com/#

```yaml
ID: bookshop
_schema-version: "2.1"
version: 0.0.1
# Space: wenxue - Applications
# 
#  --bookshop-db
#  --bookshop-srv 
#  -app
modules:
  - name: bookshop-db
    type: hdb
    path: db
    parameters:
      memory: 256M
      disk-quota: 256M
    requires:
      - name: bookshop-db-hdi-container
  - name: bookshop-srv
    type: java
    path: srv
    parameters:
      memory: 1024M
    provides:
      - name: srv_api
        properties:
          url: ${default-url}
    requires:
      - name: bookshop-db-hdi-container
        properties:
          JBP_CONFIG_RESOURCE_CONFIGURATION: '[tomcat/webapps/ROOT/META-INF/context.xml:
            {"service_name_for_DefaultDB" : "~{hdi-container-name}"}]'  



  - name: app
    type: html5
    path: app
    parameters:
       disk-quota: 256M
       memory: 256M
    build-parameters:
       builder: grunt
    requires:
     - name: srv_api
       group: destinations
       properties:
          forwardAuthToken: true
          strictSSL: false
          name: srv_api
          url: ~{url}
     - name: uaa_bookshop

resources:
  - name: bookshop-db-hdi-container
    type: com.sap.xs.hdi-container
    properties:
      hdi-container-name: ${service-name}

  - name: uaa_bookshop
    parameters:
       path: ./xs-security.json
       service-plan: application
       service: xsuaa
    type: org.cloudfoundry.managed-service


```

















#### Problem

1. Service 无法启动 

   原因：SAP HANA Service Dashboard 中未添加到自己的space中。

2. unable to create a destination

   

3. About User authentication and authorization over SCP

   https://jam4.sapjam.com/wiki/show/cmpz8iZs3OEco8MKUITLbU 

4. SF Engineering的文档

https://confluence.successfactors.com/display/ENG/Home 

