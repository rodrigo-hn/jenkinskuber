# Crear secret  
 kubectl apply -f .\secret.yaml

# crear confguracion de variables 
 kubectl apply -f .\configmap.yaml

# Crear volumenes 
kubectl apply -f .\mysql-pv.yaml 
 kubectl apply -f .\mysql-pvc.yaml

 kubectl apply -f .\postgres-pv.yaml 
 kubectl apply -f .\postgres-pvc.yaml
# crear BBDD
kubectl apply -f .\deployment-mysql.yaml -f .\svc-mysql.yaml
kubectl apply -f .\deployment-postgres.yaml -f .\svc-postgres.yaml

