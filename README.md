## project: k8s-client-server
- [github project](https://github.com/yhhsieh82/k8s-client-server)
- [project topology](https://app.diagrams.net/#G17xYS5dA2ITGEWMrUpZdsvr20kbWCc-go)
### Experiment
啟動serivce-a0(client), service-b(server)。去實驗k8s Deployment、Service、設定networkpolicy來擋request。最後使用helm chart來管理一個服務的部署。
#### Deployment
- k8s rolling-update會對deployment做不停機的container更新。
    - kubectl:
    `$ kubectl set image deployment/{my-deployment} {container_name}={new_image_name}`
    - helm:
    `$  helm upgrade service-a0 ./helm/chart-service-a0/ --set image.tag=1.3`
    `$ helm upgrade {chart-name} {path to your chart} --set key=value`
#### Service
- 從cluster外部打service:
    - NodePort: 
    `$ minikube service --url <service-name>`, 他會給你一個url，打該位置就會tunnel到目標service。**有其他做法嗎**
    - LoadBalancer
- client(service-a0) call server(service-b):
    - pods之間的溝通是利用service。以被呼叫者的service name作為hostname
- note: docker expose port < k8s service port  
    - Dockerfile: docker expose僅作為一個文件紀錄。告知建立container的人要把哪個port開放。
#### NetworkPolicy
- network policy允許誰進來。**允許他出去外網嗎**
    - start minikube with calico for networking experiment 
    - config ingress using pod selector
    - config egress(在minikube上測試networkpolicy的outbound規則。沒有試成功): 
        - 外網
        - pod of other service: 不正確運作。
#### Ingress
- **設定k8s Ingress(router)。**
#### Helm Chart
- 利用helm chart定義一個服務(i.e. 所有k8s所需的所有resource object)。
- 利用helm將服務部署上k8s。
- 利用helm更新當前k8s上的服務。
