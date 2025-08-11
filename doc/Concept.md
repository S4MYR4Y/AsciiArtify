# Concept: Локальні Kubernetes інструменти для PoC стартапу "AsciiArtify"

## Команда "AsciiArtify" розглянула три інструменти для локального розгортання Kubernetes:
- **Minikube** — кластер на одній машині, підтримка багатьох драйверів.
- **kind** — Kubernetes у Docker контейнерах.
- **k3d** — Kubernetes (k3s) у Docker контейнерах з Rancher Kubernetes Engine.

## Характеристики

| Характеристика              | Minikube                         | kind                                  | k3d                                      |
|------------------------------|-----------------------------------|----------------------------------------|-------------------------------------------|
| Підтримувані ОС              | Linux, macOS, Windows            | Linux, macOS, Windows                  | Linux, macOS, Windows                     |
| Архітектури                  | amd64, arm64                     | amd64, arm64                           | amd64, arm64                              |
| Контейнерний рушій           | Docker, Podman, VirtualBox, ін.  | Docker (можливий Podman)               | Docker (можливий Podman)                  |
| Автоматизація                | CLI, API, addons                 | CLI, YAML конфіги                      | CLI, YAML конфіги                         |
| Додаткові функції            | Dashboard, Ingress, metrics      | Простота інтеграції з CI/CD            | Легкий, швидкий, сумісний з k3s            |
| Моніторинг                   | metrics-server                   | Через додаткові інструменти            | metrics-server, Rancher dashboard         |
| Керування                    | kubectl, dashboard               | kubectl                                | kubectl, Rancher UI                       |

## Переваги
### Minikube
**+** Легкий старт, офіційна підтримка Kubernetes, багатий набір аддонів  

### kind
**+** Мінімальні ресурси, ідеально для CI/CD, швидке створення кластерів  

### k3d
**+** Дуже швидкий старт, легкий, сумісний з k3s (Rancher), зручний для edge PoC  

## Недоліки
### Minikube
**–** Повільніший на слабких ПК, важче емуляція мультивузлів

### kind
**–** Орієнтований на тестування, менше додаткових функцій

### k3d
**–** Не всі фічі «великих» кластерів доступні

## Демонстрація
### Minikube

1. Встановити kubectl

```bash
curl -LO https://dl.k8s.io/release/`curl -LS https://dl.k8s.io/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

Преевірте, що встановлена остання версія

```bash
kubectl version --client

  Client Version: v1.33.3
  Kustomize Version: v5.6.0
```

2. Встановіть minikube

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
&& chmod +x minikube
```

3. Запустіть кластер
```bash
minikube start
```
Пройти повинен час щоб кластер був ствоений та готовий до роботи.

```bash
  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

minikube status

minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

4. Створіть под із контейнером, у якому буде повідомлення  «Hello World».

```bash
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
```
Можна подивитися про нього інеформацію 
```bash
kubectl get deployments
  NAME         READY   UP-TO-DATE   AVAILABLE   AGE
  hello-node   1/1     1            1           29s
```

5. Подивіться на повідомлення

```bash
minikube service hello-node

| NAMESPACE |    NAME    | TARGET PORT |            URL            |
|-----------|------------|-------------|---------------------------|
| default   | hello-node |        8080 | http://192.168.49.2:30352 |
|-----------|------------|-------------|---------------------------|
🏃  Starting tunnel for service hello-node.
|-----------|------------|-------------|------------------------|
| NAMESPACE |    NAME    | TARGET PORT |          URL           |
|-----------|------------|-------------|------------------------|
| default   | hello-node |             | http://127.0.0.1:36807 |
|-----------|------------|-------------|------------------------|
🎉  Opening service default/hello-node in default browser...
👉  http://127.0.0.1:36807
```
### kind

1. Встановити kubectl

```bash
curl -LO https://dl.k8s.io/release/`curl -LS https://dl.k8s.io/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

Преевірте, що встановлена остання версія

```bash
kubectl version --client

  Client Version: v1.33.3
  Kustomize Version: v5.6.0
```

2. Встановіть kind

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.29.0/kind-linux-arm64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

kind version
  kind v0.29.0 go1.24.2 linux/arm64
```

3. Запустіть кластер
```bash
kind create cluster --name hello-world
```
Пройти повинен час щоб кластер був ствоений та готовий до роботи.

```bash
 Set kubectl context to "kind-hello-world"
  You can now use your cluster with:

kubectl cluster-info --context kind-hello-world

Thanks for using kind! 😊

kubectl cluster-info --context kind-hello-world

  Kubernetes control plane is running at https://127.0.0.1:33667
  CoreDNS is running at https://127.0.0.1:33667/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

  To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
4. Створіть под із контейнером, у якому буде повідомлення  «Hello World».
   
```bash
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
kubectl port-forward service/hello-node 8080:8080 
```
5. Подивитися на результат виведення через браузер
   http://localhost:8080

У консолі буде такий результат:
```bash
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
```

### k3d

1. Встановити kubectl

```bash
curl -LO https://dl.k8s.io/release/`curl -LS https://dl.k8s.io/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

Преевірте, що встановлена остання версія

```bash
kubectl version --client

  Client Version: v1.33.3
  Kustomize Version: v5.6.0
```

2. Встановіть kind

```bash
wget -q -O - https://raw.githubusercontent.com/ran... | bash
export KUBECONFIG=$(k3d kubeconfig write mycluster)
```
3. Запустіть кластер
```bash
k3d cluster create hello-world

  Cluster 'hello-world' created successfully!  
   You can now use it like this:                
kubectl cluster-info


kubectl cluster-info
  Kubernetes control plane is running at https://0.0.0.0:44141
  CoreDNS is running at https://0.0.0.0:44141/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
  Metrics-server is running at https://0.0.0.0:44141/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy
```
   
4. Створіть под із контейнером, у якому буде повідомлення  «Hello World».
   
```bash
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
kubectl port-forward service/hello-node 8080:8080

kubectl get svc hello-node

  NAME         TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
  hello-node   NodePort   10.43.52.169   <none>        8080:31891/TCP   11s

kubectl port-forward service/hello-node 8080:8080


```
5. Подивитися на результат виведення через браузер
   http://localhost:8080

У консолі буде такий результат:
```bash
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
```

# Висновки

## Minikube — найкращий вибір для невеликих завдань на одному ПК. Простий у запуску, має багато вбудованих аддонів (Ingress, dashboard, metrics-server) і підходить для швидких локальних тестів або навчання. Чудово підійде, коли не потрібно масштабування чи складних мережевих налаштувань.

## kind — оптимальний варіант для проєктів з перетворення зображень у ASCII-art за допомогою Machine Learning, де важлива швидка інтеграція з CI/CD, можливість автоматизованого створення/знищення кластерів у Docker і відсутність зайвих компонентів, що споживають ресурси.

## k3d має потужні можливості для великих проєктів завдяки легкості та швидкодії k3s у Docker, підтримці мультивузлових конфігурацій і сумісності з Rancher. Добре підходить для розробки PoC, які планується масштабувати або переносити на edge/cloud середовища.
