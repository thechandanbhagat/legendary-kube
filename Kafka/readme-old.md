 # intstall kafka in kubernetes

# 1. create namespace
kubectl create namespace kafka   

# 2. create service account
kubectl create serviceaccount -n kafka kafka

# 3. create cluster role
kubectl create clusterrolebinding kafka-cluster-role --clusterrole=cluster-admin --serviceaccount=kafka:kafka

# 4. create configmap
kubectl create configmap kafka-config --from-file=./kafka-config --namespace=kafka

# 5. create statefulset 
kubectl create -f kafka-statefulset.yaml

# 6. create service
kubectl create -f kafka-service.yaml 

# 7. create ingress
kubectl create -f kafka-ingress.yaml

# 8. create topic
kubectl exec -it kafka-0 --namespace=kafka -- /opt/kafka/bin/kafka-topics.sh --create --zookeeper zookeeper:2181 --replication-factor 1 --partitions 1 --topic test

# 9. list topic
kubectl exec -it kafka-0 --namespace=kafka -- /opt/kafka/bin/kafka-topics.sh --list --zookeeper zookeeper:2181

# 10. send message
kubectl exec -it kafka-0 --namespace=kafka -- /opt/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

# 11. receive message
kubectl exec -it kafka-0 --namespace=kafka -- /opt/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning

# 12. delete topic
kubectl exec -it kafka-0 --namespace=kafka -- /opt/kafka/bin/kafka-topics.sh --delete --zookeeper zookeeper:2181 --topic test

# 13. delete kafka
kubectl delete -f kafka-ingress.yaml
kubectl delete -f kafka-service.yaml
kubectl delete -f kafka-statefulset.yaml
kubectl delete configmap kafka-config --namespace=kafka
kubectl delete clusterrolebinding kafka-cluster-role
kubectl delete serviceaccount -n kafka kafka
kubectl delete namespace kafka

# 14. delete zookeeper
kubectl delete -f zookeeper-statefulset.yaml
kubectl delete -f zookeeper-service.yaml
kubectl delete configmap zookeeper-config --namespace=kafka
kubectl delete namespace kafka

# 15. delete pv
kubectl delete pv data-kafka-0
kubectl delete pv data-kafka-1
kubectl delete pv data-kafka-2
kubectl delete pv data-zookeeper-0
kubectl delete pv data-zookeeper-1
kubectl delete pv data-zookeeper-2

# 16. delete pvc
kubectl delete pvc data-kafka-0
kubectl delete pvc data-kafka-1
kubectl delete pvc data-kafka-2
kubectl delete pvc data-zookeeper-0
kubectl delete pvc data-zookeeper-1
kubectl delete pvc data-zookeeper-2