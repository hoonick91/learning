# Jenkins on K8S

1. kubectl create namespace ns-jenkins

2. ServiceAccount, ClusterRoleBinding, ClusterRoleBinding

   ```yaml
   ---
   apiVersion: v1
   kind: ServiceAccount
   metadata:
     namespace: ns-jenkins
     name: jenkins
   ---
   kind: ClusterRoleBinding
   apiVersion: rbac.authorization.k8s.io/v1beta1
   metadata:
     name: cluster-admin-clusterrolebinding
   subjects:
   - kind: ServiceAccount
     name: jenkins
     namespace: ns-jenkins
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: cluster-admin
   ---
   kind: ClusterRoleBinding
   apiVersion: rbac.authorization.k8s.io/v1beta1
   metadata:
     name: cluster-admin-clusterrolebinding-2
   subjects:
   - kind: ServiceAccount
     name: default
     namespace: ns-jenkins
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: cluster-admin
   ```

3. Deployment, Service

   ```yaml
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: jenkins-leader
     namespace: ns-jenkins
   spec:
     serviceName: local-service
     replicas: 1
     selector:
       matchLabels:
         app: jenkins-leader
     template:
       metadata:
         labels:
           app: jenkins-leader
       spec:
         serviceAccountName: jenkins
         securityContext:
           fsGroup: 1000
         containers:
           - name: jenkins-leader
             image: jenkins/jenkins:lts
             volumeMounts:
             - name: jenkins-home
               mountPath: /var/jenkins_home
             ports:
             - containerPort: 8080
             - containerPort: 50000
         volumes:
         - name: jenkins-home
     volumeClaimTemplates:
     - metadata:
         name: jenkins-home
       spec:
         accessModes:
         - ReadWriteOnce
         storageClassName: local-storage
         resources:
           requests:
             storage: 20Gi
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: jenkins-leader-svc
     namespace: ns-jenkins
     labels:
       app: jenkins-leader
   spec:
     type: NodePort
     ports:
     - port: 80
       targetPort: 8080
       name: http
       nodePort: 30500
       protocol: TCP
     - port: 50000
       name: agent
       nodePort: 30501
       protocol: TCP
     selector:
       app: jenkins-leader
   ```

   - volumeClaimTemplates의 storageClassName과 persistentVolme의 storageClassName을 맞춰주어야 한다.

4. persistentVolume

   ```yaml
   apiVersion: v1
   kind: PersistentVolume
   metadata:
     name: jenkins-local-pv
   spec:
     capacity:
       storage: 20Gi
     accessModes:
     - ReadWriteOnce
     persistentVolumeReclaimPolicy: Retain
     storageClassName: local-storage
     local:
       path: /e2e_log/jenkins
     nodeAffinity:
       required:
         nodeSelectorTerms:
         - matchExpressions:
           - key: kubernetes.io/hostname
             operator: In
             values:
             - dhiot-e2e01
   ```

5. 