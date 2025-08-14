1. Створити файл конфігурації для збирання кластера
```bash
nano kind-config.yaml
```

  Усередині вставте такий текст:

```bash
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes: 
- role: control-plane 
- role: worker 
- role: worker 
- role: worker
```

2. Створити кластер для ArgoCD
```bash
kind create cluster --name argocd --config kind-config.yaml
```

  Результат:

Creating cluster "argocd" ... 
✓ Ensuring node image (kindest/node:v1.33.1) 🖼 
✓ Preparing nodes 📦 📦 📦 📦 
✓ Writing configuration 📜 
✓ Starting control-plane 🕹️ 
✓ Installing CNI 🔌 
✓ Installing StorageClass 💾 
✓ Joining worker nodes 🚜
Set kubectl context to "kind-argocd"
Ви можете скористатися вашим cluster with:

kubectl cluster-info --context kind-argocd

Have a nice day! 👋

3. Встановимо ArgoCD
```bash
kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

```
4. Отримаємо доступ до графічної оболонки ArgoCD

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
  Зайдемо в ArgoCD через https://localhost:8080

  Ми можемо помітити, що браузер відображає домен localhost ненадійним. Щоб продовжити, потрібно вибрати нижче увійти (небезпечно)
<img width="1004" height="586" alt="Image" src="https://github.com/user-attachments/assets/777cdd44-a0f2-488f-9d1b-350ae5417b0d" />

<img width="884" height="339" alt="Image" src="https://github.com/user-attachments/assets/3624c2f4-0849-4d41-88f6-bd6c9a616d02" />

<img width="1004" height="660" alt="Image" src="https://github.com/user-attachments/assets/54e22d6e-0520-4d45-bafe-ae82cccf6f8d" />
  
  Далі отримаємо пароль до нашого ArgoCD
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
```
  У рядок Username вводимо admin, у password - пароль, який ми отримали

  Після цього ми отримали доступ до нашого облікового запису у ArgoCD

<img width="1004" height="494" alt="Image" src="https://github.com/user-attachments/assets/333e9798-d5f4-4309-b45c-65dea446204b" />
