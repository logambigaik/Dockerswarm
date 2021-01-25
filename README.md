# docker-swarm

# Pre-Requisites
    Minimum Two EC2-Instance
    Install Docker
# Initilize Docker Swarm Master using below command
    docker swarm init
![image](https://user-images.githubusercontent.com/54719289/105714923-f7837580-5f42-11eb-80ad-b037e6f9d650.png)

# Open port: 2377 in security group of master server
# Provide tocken number in Slave server
    docker swarm join --token SWMTKN-1-2az745icu3lmh3lf29b3yq5d8icoe9tlopag7o8j7ookegunrd-2qkycucyh0j4y7rx74w781zm7 172.31.55.29:2377

  ![image](https://user-images.githubusercontent.com/58024415/105353991-f3d7b200-5c15-11eb-9561-cf5151dbd605.png)
# Check nodes in master
    docker node ls
![image](https://user-images.githubusercontent.com/54719289/105715818-11718800-5f44-11eb-84c1-f3dd46e9a7c2.png)

# Create docker service in master
    docker service create --name redis --replicas=3 redis:3.0.6
![image](https://user-images.githubusercontent.com/54719289/105716385-bdb36e80-5f44-11eb-81e1-16ea1a09669f.png)

    If we run this command in node, will get error like that node is not swarm manager.

![image](https://user-images.githubusercontent.com/54719289/105716588-0f5bf900-5f45-11eb-9b46-746c93721654.png)

# Checking service in master:
    docker service ls 
    docker ps
 ![image](https://user-images.githubusercontent.com/54719289/105716867-6e217280-5f45-11eb-9dee-937f93084643.png)

# Checking service entry in node 1:
    docker ps
  ![image](https://user-images.githubusercontent.com/54719289/105717078-b2147780-5f45-11eb-8fa7-dd4937dc095c.png)


# Checking service entry in node 2:
    docker ps
  ![image](https://user-images.githubusercontent.com/54719289/105717231-e2f4ac80-5f45-11eb-869a-1447cf039750.png)


