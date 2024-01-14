ELK
=========

    
    
    - name: Automation Install elk
      become: true
      hosts: "{{ myhosts }}"
      gather_facts: yes
      vars:
        ES_IP: 10.110.2.41
        KIBANA_IP: 10.110.2.41
        LOGSTASH_IP: 10.110.2.41
        //for DEV
        KIBANA_REMOTE_IP: 
        TELEGRAM_TOKEN: 
    
      roles:
        - dashboard_ELK
    

## core調整
有升級到core 就需要重新設定一次 

要修改 /sys/fs/cgroup/cpuset/docker/cpuset.cpus 核心數

    0-3  default: 0-3 4核心

version:

    Elasticsearch 8.7.1
    Kibana 8.7.1
    Logstash 8.7.1

Build docker files
    
    - Dockerfile_es
    - Dockerfile_kibana
    - Dockerfile_logstash

Default yml 
    
    - elasticsearch.yml.j2
    - kibana.yml.j2
    - logstash.yml.j2

Default ENV

    ES_IMAGE: service-nexus.com:15000/elasticsearch
    ES_TAG: 1.0.0
    ES_PATH: /opt/elasticsearch
    ES_IP: 10.110.2.41
    ES_NODE_NAME: service
    ES_PORT: 9200
    
    KIBANA_IMAGE: service-nexus.com:15000/kibana
    KIBANA_TAG: 1.0.0
    KIBANA_PATH: /opt/kibana
    KIBANA_IP: 10.110.2.41
    KIBANA_SERVER_NAME: service-kibana
    KIBANA_REMOTE_IP: 'http://{{KIBANA_IP}}:8081/app/discover#/view/'
    
    LOGSTASH_IMAGE: service-nexus.com:15000/logstash
    LOGSTASH_TAG: 1.0.1
    LOGSTASH_PATH: /opt/logstash
    LOGSTASH_IP: 10.110.2.41
    
    TELEGRAM_TOKEN: ""
    TELEGRAM_CHAT_ID: "" #dev 服務器通知
    TELEGRAM_DICTIONARY_CHAT_ID: "" #dev 字典通知
    NAME_ID: service
    
