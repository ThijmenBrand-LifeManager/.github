## Demo steps
### Step 1: Deploy github actions azure infrastructure
1. Navigate to `infrastructure/terraform/github-deployment`
2. Plan the deployment using: `$ terraform plan -var-file=tfvars/terraform.tfvars -out tfplan`
3. Apply the plan using `$ terraform apply tfplan`
4. Wait

### Step 2: Copy managed identity values into github secrets
1. Navigate to the azure portal > managed identities > [gh-lfm-identity](https://portal.azure.com/#@thijmenik.onmicrosoft.com/resource/subscriptions/fb2cae0e-2fab-42f5-96fe-c8a6c4a7559c/resourceGroups/lfm-rg-identity/providers/Microsoft.ManagedIdentity/userAssignedIdentities/gh-lfm-identity-development/overview)
2. Copy the following values and paste them in the respective [Github Environment secrets](https://github.com/ThijmenBrand-LifeManager/infrastructure/settings/environments/4856352609/edit)
   - `Client ID`
   - `Subscription ID`
3. Navigate to [tenant properties](https://portal.azure.com/#view/Microsoft_AAD_IAM/TenantProperties.ReactView)
4. Copy the following and paste them in the respective [Github Environment secrets](https://github.com/ThijmenBrand-LifeManager/infrastructure/settings/environments/4856352609/edit)
   - `Tenant ID`
  
### Step 3: Run the Azure deployment pipeline
1. Navigate to the Infrastructure repository > [github actions](https://github.com/ThijmenBrand-LifeManager/infrastructure/actions)
2. Run the [Deploy azure infrastructure](https://github.com/ThijmenBrand-LifeManager/infrastructure/actions/workflows/deploy_infra.yaml) pipeline.

### Step 4: Copy the secrets into the helm values
1. Navigate to the [service bus](https://portal.azure.com/#@thijmenik.onmicrosoft.com/resource/subscriptions/fb2cae0e-2fab-42f5-96fe-c8a6c4a7559c/resourceGroups/lfm-rg-dev/providers/Microsoft.ServiceBus/namespaces/lfm-dev-servicebus/saskey).
1.1. copy the SAS Policy connection string
1.2. Paste the value in the [values.yaml](https://github.com/ThijmenBrand-LifeManager/infrastructure/blob/master/kubernetes/values.yaml) file

2. Paste the database passwords into the respective fields in the [values.yaml](https://github.com/ThijmenBrand-LifeManager/infrastructure/blob/master/kubernetes/values.yaml) file.
3. Paste the `jwt secret` into its respective field in the [values.yaml](https://github.com/ThijmenBrand-LifeManager/infrastructure/blob/master/kubernetes/values.yaml) file.

### Step 5: Connect to the AKS cluster localy
1. Run the following command to get the credentials for the [AKS cluster](https://portal.azure.com/#@thijmenik.onmicrosoft.com/resource/subscriptions/fb2cae0e-2fab-42f5-96fe-c8a6c4a7559c/resourceGroups/lfm-rg-dev/providers/Microsoft.ContainerService/managedClusters/lfm-dev-k8s/overview)
```bash
$ az aks get-credentials --resource-group lfm-rg-dev --name lfm-dev-k8s --overwrite-existing
```
2. Create the image pull secret using the follwoing command:
```bash
$ kubectl create secret docker-registry ghcr-pullsec --docker-server=https://ghcr.io/ --docker-username=ThijmenBrand --docker-password=
```
3. Navigate to the `infrastructure/kubernetes` directory
4. Run `helm install`
```bash
$ helm install lifemanager .
```

## Hi there 👋

<!--

**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->
