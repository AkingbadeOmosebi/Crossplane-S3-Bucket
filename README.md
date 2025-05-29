# ☁️ Crossplane AWS S3 Bucket Demo

Up at 3AM, decided to play around with some interesting tools. 

This little fun project demonstrates how to provision and manage an **Amazon S3 bucket** using [Crossplane](https://crossplane.io) and the [Upbound AWS Provider](https://marketplace.upbound.io/providers/upbound/provider-aws) — all declaratively via Kubernetes YAML manifests.

---

## 📁 What's Inside

### `aws-provider.yaml`
- Installs:
  - `provider-aws` (full AWS support)
  - `provider-aws-s3` (modular support for just S3)
- Defines a `ProviderConfig` named `default`, which points to a Kubernetes Secret containing your AWS credentials.

### `s3bucket.yaml`
- Provisions an S3 bucket named `akings-crossplane-bucket` in the `eu-central-1` region.
- Applies a sample tag: `foo: test`
- Configures `BucketOwnershipControls` with the `BucketOwnerPreferred` setting, referencing the bucket by label.

---

## 🔐 AWS Credentials Setup

Before applying any manifests, make sure your AWS credentials are stored in a Kubernetes Secret:

```bash
kubectl create secret generic aws-secret \
  -n crossplane-system \
  --from-literal=creds='[default]
aws_access_key_id=YOUR_ACCESS_KEY_ID
aws_secret_access_key=YOUR_SECRET_ACCESS_KEY'

>> I mean you should Replace YOUR_ACCESS_KEY_ID and YOUR_SECRET_ACCESS_KEY with your real credentials.

🛠️ Tools I used:

>VS Code 

>Kubernetes + kubectl

>Crossplane as the control plane

>Upbound AWS Provider v1.5.1

>Upbound AWS S3 Provider v0.42.0

>Bash


🚀 How It Works
Once applied, Crossplane uses the AWS Provider to reconcile the S3 bucket based on the YAML specification. This enables GitOps workflows and infrastructure version control using Kubernetes-native tools.

📌 Status
✅ Clean and working S3 setup
🧹 Want to take it down? Just delete the Kubernetes resources and Crossplane will clean up the corresponding AWS infrastructure.

Run these commands:

>> kubectl delete -f s3bucket.yaml
>> kubectl delete -f aws-provider.yaml


Finally, whatever you do... DO NOT FORGET TO REMOVE YOUR AWS CREDENTIALS FROM THE KUBERNETES SECRET!, and if you want to, delete your ACCESS KEY ID and SECRET ACCESS KEY from your AWS account.

>> kubectl delete secret aws-secret -n crossplane-system

Made with ☕ by Akingbade Omosebi.
