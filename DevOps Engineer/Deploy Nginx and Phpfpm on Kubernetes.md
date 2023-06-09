[Do run the below commands on jump_host server]
```
vi /tmp/nginx.yaml
```
Paste below script and just do some required changes like ports numbers as given in the task :-
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
  nginx.conf: |
    events {} 
    http {
      server {
        listen 8099;
        index index.html index.htm index.php;
        root  /var/www/html;
        location ~ \.php$ {
          include fastcgi_params;
          fastcgi_param REQUEST_METHOD $request_method;
          fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
          fastcgi_pass 127.0.0.1:9000;
        }
      }
    }
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx-phpfpm
  labels:
    app: nginx-phpfpm
spec:
  volumes:
    - name: shared-files
      emptyDir: {}
    - name: nginx-config-volume
      configMap:
        name: nginx-config
  containers:
    - name: nginx-container
      image: nginx:latest
      volumeMounts:
        - name: shared-files
          mountPath: /var/www/html
        - name: nginx-config-volume
          mountPath: /etc/nginx/nginx.conf
          subPath: nginx.conf
    - name: php-fpm-container
      image: php:7.3-fpm
      volumeMounts:
        - name: shared-files
          mountPath: /var/www/html
---                                                                                                           
apiVersion: v1                                                                                                
kind: Service                                                                                                 
metadata:                                                                                                     
  name: nginx-phpfpm                                                                                        
spec:                                                                                                         
   type: NodePort                                                                                             
   selector:                                                                                                  
     app: nginx-phpfpm                                                                                        
   ports:                                                                                                     
     - port: 8099                                                                                               
       targetPort: 8099                                                                                         
       nodePort: 30012     
```
```
kubectl create -f /tmp/nginx.yaml
```
```
kubectl get configmap
```
```
kubectl get pods
```
Wait for runnig status then run below commands
```
kubectl cp /opt/index.php nginx-phpfpm:/var/www/html/ --container=nginx-container
```
Validation
```
kubectl exec -it nginx-phpfpm -- /bin/bash
```
```
curl http://localhost:<paste given port no>
```
