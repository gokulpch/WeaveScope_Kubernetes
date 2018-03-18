# WeaveScope_Kubernetes
Weave Scope on Kubernetes for Visualization and Monitoring

Provision Weave Scope
a. Follow the instructions in the official Weave Scope guide https://www.weave.works/docs/scope/latest/installing/#k8s

b. This will deploy required Service, Daemonset and agent-pods in a namespace "weave" ad adds required ClusterRole bindings.


By default Weave Scope creates a Service with Type: ClusterIP, if user has a ingress controller in the cluster then the route can be added to expose the service to internet.  This requires a ingress controller like traefik, Voyager or Linkerd. User can edit the service to enable NodePort to access the UI using NodePort.

kubectl edit svc weave-scope-app -n weave

-------------

Change "type" and add a "nodeport" in the configuration

spec:
  clusterIP: 10.105.189.160
  externalTrafficPolicy: Cluster
  ports:
  - name: app
    nodePort: 32185
    port: 80
    protocol: TCP
    targetPort: 4040
  selector:
    app: weave-scope
    name: weave-scope-app
    weave-cloud-component: scope
    weave-scope-component: app
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}

--------------

After editing the configuration above the svc should have type: NodePort

root@ip-172-25-1-34:~# kubectl get svc -n weave
NAME              TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
weave-scope-app   NodePort   10.105.189.160   <none>        80:32185/TCP   12d

c. Now you should be able to connect to the ui using http://<host_ip>:32185

![alt text](https://github.com/gokulpch/Weavescope_kubernetes/blob/master/images/ws-1.png)

![alt text](https://github.com/gokulpch/Weavescope_kubernetes/blob/master/images/ws-2.png)

![alt text](https://github.com/gokulpch/Weavescope_kubernetes/blob/master/images/ws-3.png)


d. The UI is totally self-explanatory. There are multiple filters that filters the components based on the scope required.

![alt text](https://github.com/gokulpch/Weavescope_kubernetes/blob/master/images/ws-ui-1.png)

e. Hosts topology view

![alt text](https://github.com/gokulpch/Weavescope_kubernetes/blob/master/images/ws-ui-2.png)

f. Pods view

![alt text](https://github.com/gokulpch/Weavescope_kubernetes/blob/master/images/ws-ui-3.png)

g. Containers view

![alt text](https://github.com/gokulpch/Weavescope_kubernetes/blob/master/images/ws-ui-4.png)

h. Services view

![alt text](https://github.com/gokulpch/Weavescope_kubernetes/blob/master/images/ws-ui-5.png)

i. Service detail view and Pod detailed view

![alt text](https://github.com/gokulpch/Weavescope_kubernetes/blob/master/images/ws-ui-6.png)








