kubeconfig is used for accessing the cluster with different username. with the access level permission. 

default path for kubeconfig is /root/.kube/config
to use other kube-config without changing default config use below steps.
1. vi ~/.bashrc
2. export KUBECONFIG=/path/to/yourconfig
3. source ~/.bashrc

https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/


