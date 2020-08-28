# Monitoring with Grafana and Prometheus

## Install Prometheus

1. [Deploy configMap with targets and rules](prometheus/prometheus-configmap.yaml)  
2. [Deploy ClusterRole, ServiceAccount, ClusterRoleBinding](prometheus/prometheus-rbac.yaml)  
You need to change:
    - *ClusterRole* name
    - *ServiceAccount* name
    - *ServiceAccount* namespace
    - *ClusterRoleBinding*:
       1. *ClusterRole* name
       2. *ServiceAccount* name
       3. *ServiceAccount* namespace  
3. [Deploy Prometheus deployment](prometheus/prometheus-deployment.yaml)  
    - Create *prometheus-ebs* storage 
    - Fix *name* and *labels* in the deployment
4. [Deploy Service](prometheus/prometheus-service.yaml)
    Fix selector app
    ```yaml
    selector:
      app: prometheus-az
    ```
5. [Deploy Route](prometheus/prometheus-route.yaml)  
You need to change:
    - Host name
    - Labels
    - Service name


## Install Grafana

1. [Deploy datasource configMap](grafana/datasource.yaml)  
You need to change url: 
    ```yaml
    datasource:
        url: <your service hostname>
    ```
2. [Deploy dashboard configMap](grafana/dashboard.yaml)  
Please change this name:
    ```yaml
    providers:
       name: <your any unique name>
    ```
3. [Deploy grafana-config configMap](grafana/grafana-config.yaml)  
Please change these lines:
    ```yaml
    ...
    namespace: "<your namespace>"
    ...
    root_url = "<Grafana URL>"
    ...
    The whole block code with "Generic OAuth"
    ...
    ```
4. [Deploy all-dashboards configMap](grafana/all-dashboards.yaml)  
Or you can use your dashboards

5. [Deploy grafana-deployment configMap](grafana/grafana-deployment.yaml)  
Please change:
    ```yaml
    ...
    parameters:
        name: IMAGE_GRAFANA
        name: ROUTE_URL
        name: NAMESPACE
    ...
    kind: ClusterRoleBinding
    metadata:
      name: grafana-cluster-reader-<your name>
    ...
    ```

