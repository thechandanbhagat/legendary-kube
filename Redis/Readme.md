# Set the password
Line number 82 and 83 in redis.yaml file   
`masterauth <password>`   
`requirepass <password>`

# Apply the yaml file   
`kubectl apply -f redis.yaml`   

# Check the log of master pod
`kubectl -n redis logs redis-0`   

# Check the log of slave pod
`kubectl -n redis logs redis-1`

# Get more details about the master pod
`kubectl -n redis describe pod redis-0`

# Authenticate to the master pod
`kubectl -n redis exec -it redis-0 -- sh`   
`redis-cli`   
`auth <password>`   

## Get repplication status
`info replication`

## Get the master pod IP
`kubectl -n redis get pod redis-0 -o wide`

## Create key value pair list
`set key1 value1`
`set key2 value2`

## Get the key value pair list
`get key1`
`get key2`