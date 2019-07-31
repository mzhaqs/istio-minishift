Get `minishift` >= v1.24.0. ** https://github.com/minishift/minishift/releases
Get docker ** https://www.docker.com/docker-mac
`brew install openshift-cli`
`oc (eval $(minishift oc-env))`
minishift profile set istio
minishift config set memory 5GB
minishift config set cpus 2
minishift config set image-caching true
minishift config set openshift-version v3.11.0
minishift addon enable admin-user
minishift config set skip-startup-checks true
Get and install virtual box
minishift start --vm-driver virtualbox
minishift ssh -- sudo setenforce 0 (also after every restart)
minishift addon apply anyuid
eval $(minishift oc-env)
oc login -u system:admin
oc adm policy add-cluster-role-to-user cluster-admin admin
oc login $(minishift ip):8443 -u admin -p admin
oc adm policy add-scc-to-user anyuid -z istio-ingress-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z default -n istio-system
oc adm policy add-scc-to-user anyuid -z prometheus -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-egressgateway-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-citadel-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-ingressgateway-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-cleanup-old-ca-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-mixer-post-install-account -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-mixer-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-pilot-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-sidecar-injector-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-galley-service-account -n istio-system
oc adm policy add-scc-to-user anyuid -z istio-security-post-install-account -n istio-system 
oc adm policy add-scc-to-user privileged -z default -n <target-namespace>
curl -L https://github.com/istio/istio/releases/download/1.1.9/istio-1.1.9-osx.tar.gz | tar xz
cd istio-1.1.9
export ISTIO_HOME=`pwd`
export PATH=$ISTIO_HOME/bin:$PATH
oc apply -f install/kubernetes/helm/istio-init/files/crd-11.yaml
oc apply -f install/kubernetes/istio-demo.yaml
oc project istio-system
oc expose svc istio-ingressgateway --port=80
oc expose svc grafana
oc expose svc prometheus
oc expose svc tracing
oc expose service kiali --path=/kiali 
oc adm policy add-cluster-role-to-user admin system:serviceaccount:istio-system:kiali-service-account -z default
oc get pods -w
minishift console
