  <li>deploy.yaml을 실행하여 ingress-nginx 설치 및 실행</li>
  <p>kubectl create -f deploy.yaml</p>

  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/bf908e52-4e43-488f-aff1-0f4629e08bdb)
  <br>
  <li>webhook 구성을 삭제 -> ingress가 정상적으로 구동되지 않는 현상이 있음</li>
  <p>kubectl delete validatingwebhookconfigurations.admissionregistration.k8s.io ingress-nginx-admission</p>

  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/89a3dde9-5b72-40a4-9e74-ac79f8316ffe)
  <br>
  <li>TLS 생성</li>
  <p>openssl req -x509 -nodes -days 365 -newkey rsa:2048 \<br>
      -out ingress-tls.crt \<br>
      -keyout ingress-tls.key \<br>
      -subj "/CN=ingress-tls"<br></p>

  <p>kubectl create secret tls ingress-tls \<br>
      --namespace ingress-nginx \<br>
      --key ingress-tls.key \<br>
      --cert ingress-tls.crt</p>

  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/cd5ad862-c229-46a9-b856-668586bb097a)
  <br>
  <li>kustomization.yaml을 적용하여 mysql 암호를 secret으로 생성하고, mysql, frontend, backend deployment 실행</li>
  <p>kubectl apply -k ./</p>

  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/b3f23891-b67a-4c0a-a412-8e6bfab3e84b)
  <br>
  <li>kubectl get pod -n ingress-nginx 를 실행하여 mysql pod에 접속</li>
  <p>kubectl get pod -n ingress-nginx</p>
  
  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/c2e5fe53-edbe-4216-bc58-dee5b7951650)
  <p>kubectl exec -it webserver-mysql-6dd485864f-96gs2 -n ingress-nginx -- /bin/bash</p>
  
  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/5e55156c-bd93-4057-a5d6-cd90b33a63d7)
  <p>mysql -u root -p</p>
  <p>choijihyeok</p>
  
  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/b1421db8-397e-412d-87cc-5b78b449fd7c)
  <br>
  <li>명령어를 사용하여 database의 character set을 변경하고 lists 테이블을 생성</li>
  <p>ALTER DATABASE myapp DEFAULT CHARACTER SET utf8;</p>
  <p>use myapp;</p>
  <p>CREATE TABLE lists (id INTEGER AUTO_INCREMENT, value TEXT, PRIMARY KEY (id)) DEFAULT CHARSET=utf8;</p>

  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/32394b77-500a-403e-916f-d80667456a65)
  <br>
  <li>nginx-ingress.yaml을 실행하여 ingress 실행</li>
  <p>kubectl create -f nginx-ingress.yaml</p>

  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/29af5fc3-adff-4ab5-a537-a7aebf7976fd)
  <br>
  <li>web-ingress ADDRESS에 IP가 나타난 후 접속</li>
  <p>kubectl get ing -n ingress-nginx</p>
  
  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/bc230da3-e3ab-4f8b-b5ce-e6dd3f32433f)
  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/e1760d7d-f304-4540-93ea-a2a53a33ba5b)
  <br>
  <li>작동 확인</li>
  
  ![image](https://github.com/Crete666/goorm_kubernetes_practical_task/assets/121783191/81f88412-08c0-42c2-8464-ef2ffd51e4f8)
