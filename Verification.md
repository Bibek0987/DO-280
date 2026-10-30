Step 1: Verify the Secret's Existence (Core Command)
Execute the following command as a user with cluster-admin privileges. If the output shows a Secret named kubeadmin with type Opaque, it means the account exists and has not been deleted .

bash
# Check if the kubeadmin secret exists in the kube-system namespace
$$ oc get secrets kubeadmin -n kube-system

Expected output example:

text
NAME        TYPE     DATA   AGE
kubeadmin   Opaque   1      6h3m
Note: If it returns Error from server (NotFound), it means this account has been removed or disabled .

Step 2: (Optional) Retrieve the Password
If you need to confirm the password (for example, to verify it matches the one provided during initial installation), you can decode the data in the Secret. The password is stored in base64 encoding .

bash
# Decode and view the plaintext password of kubeadmin

$$ oc get secret kubeadmin -n kube-system -o jsonpath='{.data.kubeadmin}' | base64 --decode && echo
Step 3: (Optional) Confirm Its Privileges
Although this user is not in the user list, you can verify its bound permissions. kubeadmin is typically directly bound to the cluster-admin role.

bash
# Check cluster role bindings and filter for kubeadmin-related items
$$ oc get clusterrolebindings -o wide | grep kubeadmin

⚠️ Key Reminder: Do Not Delete
The requirement you mentioned to "not delete" is very important. Official OpenShift documentation explicitly warns: Before you have created another user with cluster-admin privileges and verified that they can log in normally through an identity provider (such as LDAP/AD), absolutely do not execute oc delete secrets kubeadmin -n kube-system. Otherwise, the cluster will be permanently locked, and the only solution is to reinstall the cluster .

