Example playbook

command line:

安裝完成，/root資料夾有 account 密碼 ，admin_root_password ，replication_password.success

    # master execute
    ansible-playbook -i inventories/server/dev  playbooks/mysql.yml -e myhosts=local-master --tags master

    #slave exuecte
    ansible-playbook -i inventories/server/dev  playbooks/mysql.yml -e myhosts=local-slave -e service_id=2 -e account_password='Q12QWAS31wsx' -e replication_password='!qaZ1234QWE' -e replication_master_log_file='binlog.000002' -e replication_master_log_pos='2494' --tags slave 

    
-----------------------------------------------  master   ---------------------------------------
Master server:


    - name: Automation Install master , default create user admin_root
      become: true
      hosts: master
      vars:
          server_id: 1

      roles:
        - role: removeMysql
          tags: always
        - mysql

-----------------------------------------------  slave   ---------------------------------------

Slave server:

    - name: Automation Install master , default create user admin_root
      become: true
      hosts: slave
      vars:
          server_id: 2                                            # default 1
          account_password: "{{ account password }}"              # default Random
          master_address: "{{ master IP }}"                       # default 10.110.2.100
          mysql_password: "{{ master password }}"                 # default Random
          slave_address: "{{  slave IP }}"                        # default 10.110.2.101
          replication_password: "{{ replication Password }}"      # default Random
          #replication_master_log_file: "mysql-bin.000002"        # default Automation get master replication_master_log_file
          #replication_master_log_pos: "621"                      # default Automation get master replication_master_log_
          #account_user: admin_root                                     # default admin_root
      roles:
        - role: removeMysql
          tags: always
        - mysql
