**kubernetes is a popular container orchestrator**

**conatiner orchestrator** =  make many servers act like one -> takes all the containers, takes a series of servers or nodes and decides how to run, manage them.

kubernetes runs on top of docker as a set of API's in containers.
kubernetes provides API/CLI to manage containers across servers.



**Many app** run in containers -> wrapped together we call them as **pods**. \n
**Many pods** are managed by **Replica Set**. (****Replica Set**** can create many duplicate pods).
**Replica Set** is managed by **Deployment yaml context**.
**ALL COME TOGETHER IN ONE YAML FILE**.

