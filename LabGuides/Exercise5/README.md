# Exercise 5 Instrumenting a new microservice using Appsody

## Installing the Appsody 

Normally you install Appsody on the development environment - typically your local workstation. However for the course of this lab you will use the managed cluster to install and run Appsody (More dedicated Appsody and Kabanero labs are planned in other sessions)

The purpose of this exercise it to demonstrate that the Appsody stacks by default include the monitoring instrumentation for selected runtimes and how to connect the genereated microservices to the IBM Cloud App Management server.

### 1. Install the Appsody on the managed cluster

Appsody binaried has been downloaded for you and are located in /home/localuser/install. Run the following command to install Appsody:
```
cd /home/localuser/install
sudo apt-get install -y ./appsody_0.5.4_amd64.deb
```

### 2. Generating and deploying a new microservice

In order to generate a new Node.js microservice create a new directory and run the appsody init there.

```
cd
mkdir newapp
cd newapp
appsody init nodejs-express
```

### 3. Building and deploying the microservice to the managed cluster

You can review the sample application app.js. It is very simple - there is nothing special in the JavaScript code. The instrumentation is hidden in the ```package-lock.json``` file.

To build the application run the following command
```
appsody build
```
You can verify that container was build successfully running the following command
```
docker images |grep newapp
```

Output should show recently build docker image
```
dev.local/newapp                                                   latest              c98cbb4ee3bb        8 minutes ago       177MB
```

You will deploy the microservice to the default namespace. To instruct the monitoring data collector embedded in the microservice where to send the data, you need to create a secret with ICAM server configuration. Run the steps similar to the 
To deploy the image on the managed cluster use the Step 2 in [Exercise 3 Installing Bookinfo app to a managed cluster](../Exercise3/README.md). The only difference is that you will use *default* namespace instead on *bookinfo*

```
LOCALDIR=`pwd`
cd /home/localuser/install/app_mgmt_k8sdc/ibm-cloud-apm-dc-configpack
kubectl -n default create secret generic icam-server-secret \
--from-file=keyfiles/keyfile.jks \
--from-file=keyfiles/keyfile.p12 \
--from-file=keyfiles/keyfile.kdb \
--from-file=global.environment
cd $LOCALDIR
```

Create the deployment.yaml using the following content

<pre>
apiVersion: v1
kind: Service
metadata:
  name: newapp
  labels:
    app.kubernetes.io/name: newapp
spec:
  type: ClusterIP
  ports:
    - port: 3000
      targetPort: http
      protocol: TCP
      name: http
  selector:
    app.kubernetes.io/name: newapp

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: newapp
  labels:
    app.kubernetes.io/name: newapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/name: newapp
  template:
    metadata:
      labels:
        app.kubernetes.io/name: newapp
    spec:
      containers:
        <b>- env:
          - name: OPENTRACING_ENABLED
            value: "true"
          - name: JAEGER_SAMPLER_TYPE
            value: probabilistic
          - name: JAEGER_SAMPLER_PARAM
            value: "1"
          - name: LATENCY_SAMPLER_PARAM
            value: "1"</b>
          name: newapp
          <b>image: "dev.local/newapp:latest"</b>
          imagePullPolicy: IfNotPresent
          ports:
            - name: http
              containerPort: 3000
              protocol: TCP
          livenessProbe:
            httpGet:
              path: /health
              port: http
          readinessProbe:
            httpGet:
              path: /health
              port: http
          resources:
            {}
          <b>volumeMounts:
          - mountPath: /opt/ibm/apm/serverconfig
            name: global-environment
      volumes:
      - name: global-environment
        secret:
          defaultMode: 420
          optional: true
          secretName: icam-server-secret</b>
</pre>

Create the deployment using kubectl
```
kubectl create -f deployment.yaml 

service/newapp created
deployment.apps/newapp created
```

