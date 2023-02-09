

# Apply dashboard
`kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml`   
or   
`kubectl apply -f dashboard.yaml` # local file

# Create service account
`kubectl apply -f admin-user.yaml`

# Create service account
`kubectl apply -f cluster-role-binding.yaml`

# Get token
`kubectl -n kubernetes-dashboard create token admin-user`   
`kubectl -n kubernetes-dashboard create token admin-user --duration=24h` # 24 hours

# Proxy
`kubectl proxy` 

# Access
`http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/workloads?namespace=_all`
