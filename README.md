# Monitoring


## felk : Filebeat / Elastic / Logstash / Kibana


## v 6.8
### Elasticsearch
* update elastisearch.yml file
* To activate security launch : ```$ELASTIC_HOME/bin/elasticsearch-setup-passwords auto```
* Keep in a safe place elastic user password (admin user)

### Logstash 
* needs jre 11
* add felk/logstash/nginx-conf in ```config```
* adapt if needed nginx.conf (input and output part) 
* command to launch : ```bin/logstash -f ../config/nginx-conf``` (better to configure as a service)
* create a user allowed to create and push on a specific index :
  * create new role :  
    ``` 
         POST _xpack/security/role/logstash_writer
             {
               "cluster": ["manage_index_templates", "monitor", "manage_ilm"], 
               "indices": [
                 {
                   "names": [ "filebeat-*" ], 
                   "privileges": ["write","delete","create_index","manage","manage_ilm"]  
                 }
               ]
             }
                         
  * create user with the previous role :  
    ``` 
        POST _xpack/security/user/logstash_push
        {
          "password" : "************",
          "roles" : [ "logstash_writer"],
          "full_name" : "Internal Logstash User"
        }       

### Filebeat
* install agent
* update conf in felk/filebeat/ in ```/etc/filebeat/filebeat.yml```
* launch agent : service filebeat start  
more info here : https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation.html

