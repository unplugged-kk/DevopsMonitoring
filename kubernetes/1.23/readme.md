
git clone https://github.com/unplugged-kk/DevopsMonitoring.git

cd    DevopsMonitoring/kubernetes/1.23

Setup CRDs
Let's create the CRD's and prometheus operator

kubectl create -f ./manifests/setup/
Setup Manifests
Apply rest of manifests

kubectl create -f ./manifests/
Check Monitoring
kubectl -n monitoring get pods

NAME                                   READY   STATUS    RESTARTS   AGE
alertmanager-main-0                    2/2     Running   0          26m
alertmanager-main-1                    2/2     Running   0          26m
alertmanager-main-2                    2/2     Running   0          26m
blackbox-exporter-6b79c4588b-rvd2n     3/3     Running   0          27m
grafana-7fd69887fb-vzshr               1/1     Running   0          27m
kube-state-metrics-55f67795cd-gmwlk    3/3     Running   0          27m
node-exporter-77d29                    2/2     Running   0          27m
node-exporter-7ndbl                    2/2     Running   0          27m
node-exporter-pgzq7                    2/2     Running   0          27m
node-exporter-vbxrt                    2/2     Running   0          27m
prometheus-adapter-85664b6b74-nhjw8    1/1     Running   0          27m
prometheus-adapter-85664b6b74-t5zfj    1/1     Running   0          27m
prometheus-k8s-0                       2/2     Running   0          26m
prometheus-k8s-1                       2/2     Running   0          26m
prometheus-operator-6dc9f66cb7-8bg77   2/2     Running   0          27m
View Dashboards
You can access the dashboards by using port-forward to access Grafana. It does not have a public endpoint for security reasons

kubectl -n monitoring port-forward svc/grafana 3000
Then access Grafana on localhost:3000

Check Prometheus
Similar to checking Grafana, we can also check Prometheus:

kubectl -n monitoring port-forward svc/prometheus-operated 9090
Check Service Monitors
To see how Prometheus is configured on what to scrape , we list service monitors

kubectl -n monitoring get servicemonitors
NAME                      AGE
alertmanager-main         8m58s
blackbox-exporter         8m57s
coredns                   8m57s
grafana                   8m57s
kube-apiserver            8m57s
kube-controller-manager   8m57s
kube-scheduler            8m57s
kube-state-metrics        8m57s
kubelet                   8m57s
node-exporter             8m57s
prometheus-adapter        8m56s
prometheus-k8s            8m56s
prometheus-operator       8m56s

kubectl -n monitoring describe servicemonitor node-exporter
Label selectors are used to map service monitor to kubernetes services.

That is how Prometheus is configured on what to scrape.

