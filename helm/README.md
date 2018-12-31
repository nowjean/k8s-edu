<h1> 패키지 매니져 Helm</h1>

<h2>1. GCP - Mac 연동</h2>

<https://cloud.google.com/sdk/docs/quickstart-macos> 를 참조

<h2>2. Helm Installation</h2>
<h3>2-1. Helm Installation 파일 받기 맟 설치</h3>

```
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```

설치 완료 후
```
helm init
```

<h3>2-2. Trouble Shooting </h3>
 * helm forbidden error - deprecated 와 같은 에러가 나올 경우
 
 ```
 kubectl create serviceaccount --namespace kube-system tiller
 kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
 kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'      
 helm init --service-account tiller --upgrade
 ```
 
 위 방법이 안되면,
 * Setup Tiller as cluster admin for cert manager
 
 ```
 Find password: gcloud container clusters describe <cluster_name> --zone us-west1-a
 kubectl apply --username=admin --password=FROMABOVE -f create-helm-service-account.yaml
 helm init --service-account helm
 gcloud projects add-iam-policy-binding $PROJECT --member=user:**example@gmail.com** --role=roles/container.admin
 ```
