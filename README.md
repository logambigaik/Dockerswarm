# docker-swarm

# Pre-Requisites
    Minimum Two EC2-Instance
    Install Docker
# Initilize Docker Swarm Master using below command
    docker swarm init
![image](https://user-images.githubusercontent.com/54719289/105714923-f7837580-5f42-11eb-80ad-b037e6f9d650.png)

# Open port: 2377 in security group of master server
# Provide token number in Slave server
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

With nginx:
===========
    docker service create --name nginx --replicas=5 nginx:latest

![image](https://user-images.githubusercontent.com/54719289/105748274-e9941b80-5f67-11eb-9f8c-0ce8468fe51b.png)
    
    # Checking service in master:
        docker service ls 
        docker ps
![image](https://user-images.githubusercontent.com/54719289/105748466-311aa780-5f68-11eb-8cb4-23f8dad040d7.png)
    
    # Checking service entry in node 1:
        docker ps
    
![image](https://user-images.githubusercontent.com/54719289/105748663-75a64300-5f68-11eb-9ede-a9e0539e2f98.png)


    # Checking service entry in node 2:
        docker ps

![image](https://user-images.githubusercontent.com/54719289/105748749-91114e00-5f68-11eb-93f9-f1581d341622.png)
    
    # docker update
         docker service update --publish-add 80 nginx

![image](https://user-images.githubusercontent.com/54719289/105749073-f2392180-5f68-11eb-983c-aa6850360a4b.png)


    If service is not started, use below command after removing existing service.
    
        docker service nginx
        docker service create --name my_web --replicas=3 --publish published=80,target=80 nginx
        
![image](https://user-images.githubusercontent.com/54719289/105750462-cae35400-5f6a-11eb-8075-03b021563660.png)

     # Docker master:
   
![image](https://user-images.githubusercontent.com/54719289/105750688-1269e000-5f6b-11eb-8f99-2c61f9555be2.png)

    # Docker node1:
    
![image](https://user-images.githubusercontent.com/54719289/105750810-2f9eae80-5f6b-11eb-9098-b6cd19f81204.png)

    #  Docker node2:
    
![image](https://user-images.githubusercontent.com/54719289/105750922-5361f480-5f6b-11eb-8c87-07e7673989d5.png)


# Docker node drain:

    docker node update --availability drain node-hostname
    
    docker node update --availability drain nodde1
    
    docker ps in master
    
![image](https://user-images.githubusercontent.com/54719289/105751684-60331800-5f6c-11eb-8540-6402c3e47cd5.png)

    docker ps in nodde1
    
![image](https://user-images.githubusercontent.com/54719289/105752017-cae45380-5f6c-11eb-8135-da0928f68f29.png)

    
    Second node drain:
    
    docker node update --availability drain node2
           
![image](https://user-images.githubusercontent.com/54719289/105751849-92447a00-5f6c-11eb-98c9-18665628b77b.png)
    
    docker ps in master
    
![image](https://user-images.githubusercontent.com/54719289/105751915-ab4d2b00-5f6c-11eb-89a8-22cae884c3e3.png)

    docker ps in node2:
    
![image](https://user-images.githubusercontent.com/54719289/105752133-e8b1b880-5f6c-11eb-9798-b6523a0d6454.png)





