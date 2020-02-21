# Exercise 2 Install k8s monitor to the managed cluster

[Go back to the Table of Contents](../../README.md)

In this exercise, you will be instrumenting your managed cluster with the kubernetes data collector (k8monitor).  The kubernetes data collector will provide detailed monitoring about the performance and health of the entire kubernetes cluster including the nodes, pods, containers, ingress, and more.

### 1. Verify the user id you are assigned

```
cat /home/localuser/README
```

### 2. In the terminal window where you are connected to managed cluster run the following commands, adjusting the **cluster_name** value
   
<span style="color:red">**ATTENTION: Remember to change the cluster_name in the command!**</span>

*HINT: If you are using Windows, first copy the lines to Notepad, edit the username and then copy/paste to Putty window*

<pre>
cd /home/localuser/install/app_mgmt_k8sdc
ansible-playbook helm-main.yaml --extra-vars="release_name=icam-kubernetes-resources namespace=default  \
docker_registry=edge-server.demo:8500 docker_group=default tls_enabled=true <span style="color:red"><b>cluster_name=user1</b></span>"
</pre>

Ansible playbook should finish with the following line
```
PLAY RECAP ****************************************************************************************************************************
127.0.0.1                  : ok=15   changed=8    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0   
```

You have now completed the installation of the kubernetes data collector

If the Ansible playbook failed, read the instructions for step 8 again. Run the script ```/home/localuser/scripts/uninstall-k8smonitor.sh```. Then, correct the command and run it again.

### 3. Verify that the K8Monitor is running
```
localuser@edge-server:~/install/app_mgmt_k8sdc$ kubectl get pod -n default
NAME                                                   READY   STATUS    RESTARTS   AGE
icam-kubernetes-resources-k8monitor-54876594c5-9xvzs   2/2     Running   0          48s
```

This concludes the exercise. Your managed cluster has now the Kubernetes data collector deployed and reports data back to the IBM Cloud App Management server. In the next exercise you will explore what data is monitored for different resource types. 

[Go back to the Table of Contents](../../README.md)

<table>
  <tr>
    <td>Version</td>
    <td>1.0</td>
  </tr>
  <tr>
    <td>Author</td>
    <td>Wlodek Dymaczewski, IBM</td>
  </tr>
  <tr>
    <td>email</td>
    <td>dymaczewski@pl.ibm.com</td>
  </tr>
</table>