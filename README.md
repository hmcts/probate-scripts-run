# run_shell_cmd_servers

run shell command on dev, test and demo


Example comamnd :
----------------

Use for install and remove the RPM to the servers.

 ```
    For Web server
    --------------
    yum -y remove probate* && yum -y install probate-frontend-0.0.1_SNAPSHOT-422

    For App Server
    --------------
    yum -y remove probate* && yum -y install probate-submit-service-0.0.1_SNAPSHOT-74 &&
    yum -y install probate-sol-ccd-service-0.0.1_SNAPSHOT-100 && 
    yum -y install probate-business-service-0.0.1_SNAPSHOT-42  &&
    yum -y install probate-persistence-service-0.0.1_SNAPSHOT-50

 ```
