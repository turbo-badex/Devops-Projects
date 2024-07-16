# Bootstrap EKS Cluster with ArgoCD using Terraform, Helm


### Introduction
In this article, we will learn how to bootstrap our Kubernetes cluster with ArgoCD. To achieve this, we need a running cluster which can either be EKS, GKS, or AKS, our manifest files which are inside our GitHub repository, and an ArgoCD account.


### Clone the repository

```sh
git clone https://github.com/Victoria-OA/manifests.git
```

cd into the eks folder which contains the Terraform files to provision your cluster on AWS:

```sh
terraform init
terraform plan
terraform apply
```
The terraform configuration provisions the following listed below:

1. EKS cluster on AWS
2. Node and Node group using SPOT instances
3. Update Kube config on the machine you’re running terraform from.
4. Install ArgoCD helm chart on the provisioned cluster

### Apply Manifest Files

After updating your cluster to receive manifests configuration, apply the files in the manifest folder with:

```sh
kubectl apply -f a.yaml
```

### Bootstrapping with ArgoCD

Once your manifest files have been applied, the next step is to bootstrap them to ArgoCD to enhance continuous delivery. Follow these steps:

** Check that argocd is installed and the pods are running

<img width="622" alt="image" src="https://github.com/user-attachments/assets/088e3363-7acd-46d0-9fcb-734af885dfec">


1. **Map Port for ArgoCD Access:**

    When the pods are ready, map your port to access ArgoCD in the browser:

    ```sh
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```

2. **Retrieve ArgoCD Admin Password:**

    Upon access, you will be required to log in with a username and a password. The username is `admin`, and the password can be retrieved using the following command:

    ```sh
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```

    <img width="1236" alt="image" src="https://github.com/user-attachments/assets/939c62ae-fb74-4c5e-a97a-3939e85503b2">


3. **Log in to ArgoCD:**

    When the password has been retrieved, log in to ArgoCD with:

    ```sh
    argocd login localhost:8080
    ```

4. **Connect GitHub Repository to ArgoCD:**

    Once logged in successfully, connect the GitHub repo that contains the manifest with the following command:

    ```sh
    argocd repo add https://github.com/username/repourl --username <your-github-username> --password <your-personal-access-token>
    ```

<img width="1826" alt="image" src="https://github.com/user-attachments/assets/538d37fa-fec9-4bac-9a60-ecd30f4d6667">

    Note: To get your GitHub password, use your GitHub token, which can be generated in developer’s settings.

5. **Add Your Cluster to ArgoCD:**

    Once your repo has been connected successfully, add your cluster to the ArgoCD server using the following command:

    ```sh
    kubectl config get-contexts
    argocd cluster add <context-name>
    ```

    <img width="1707" alt="image" src="https://github.com/user-attachments/assets/a743da4f-aab5-455a-999f-6d19033fb6a7">


6. **Create and Sync Your Application:**

    Once the cluster has been added successfully, proceed to create your app and configure your ArgoCD using:

    ```sh
    argocd app create appname \
       --repo https://github.com/username/repourl \
       --path manifests/ \
       --dest-server https://kubernetes.default.svc \
       --dest-namespace argocd
    ```

    <img width="690" alt="image" src="https://github.com/user-attachments/assets/d0e1c121-2aa0-47c0-9b88-f8e4f04d61cb">


7. **Sync Your Application:**

    Finally, sync your app using the following command:

    ```sh
    argocd app sync newapp
    ```

<img width="1266" alt="image" src="https://github.com/user-attachments/assets/29eef22b-133e-4c4e-8749-0822ee2db6e6">

Once the sync is successful, you can log in to your ArgoCD via the browser to check it out.

<img width="471" alt="image" src="https://github.com/user-attachments/assets/2cef722b-20de-4359-b004-8661b9c72f7f">


## Conclusion

By following these steps, you have set up a Kubernetes cluster, applied your manifest files, and integrated ArgoCD for continuous delivery. This setup not only streamlines your deployment process but also enhances the manageability and scalability of your applications.
