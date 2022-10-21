# k8s-rbac-setup
- openssl genrsa -out my_user.key 2048
- openssl req -new -key my_user.key -out my_user.csr 
- openssl req -new -key my_user.key -out my_user.csr -subj "/CN=my_user/O=dev"
- openssl x509 -req -in my_user.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out my_user.crt -days 365
- kubectl --kubeconfig my_user.kubeconfig config set-cluster kubernetes --server https://<ip-adddress:6443> --certificate-authority=ca.crt
# To get server url, use syntax "kubectl config view"
# Register the credntial with the syntax
- kubectl --kubeconfig enomor.kubeconfig config set-credentials enomor --client-certificate =/home/ubuntu/my_user.crt --client-key=/home/ubuntu/my_user.key
# set the context
- kubectl --kubeconfig my_user.kubeconfig config set-context my_user-kubernetes --cluster kubernetes --namespace preferred_namespace --user my_user
# As the admin, copy the existing cluster kubeconfig file in the path /home/user/.kube/config file to user.kubeconfig 
# edit the newly created user.kubeconfig file by changing the user, namespace, context, current-context, client-certificate-data (using the generated user.crt) and client-key-data (using the user.key file)
# edit the kubeconfig file and change current context to enomor-kubernetes, change the user keys and certificate in base 64 using the generated keys/certificate and set the preferred namespace.

NEXT STEP:
# create a role either using declarative or config file
# finally create a rolebinding to use the role just created

