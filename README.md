# docker-swarm
    Docker swarm:
    ==========
		If we want to Maintain or create cluster of  docker machine or engine we need to use docker swarm.
		Docker machine can be hosted on different nodes and these nodes will be form of cluster when connected with docker swarm.
		Docker swarm is a container orchestration(automation )tool which allow user to manage multiple containers deployed across multiple machines
		In system admin, orchestration is the automated configuration , coordination, and management of computer systems and software.
# Pre-Requisites
    Three EC2-Instance
    Install Docker and start the service 
    service docker start
    
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

    
    Second node drain: (staus is active but Availability is Drain)
    
    docker node update --availability drain node2
           
![image](https://user-images.githubusercontent.com/54719289/105751849-92447a00-5f6c-11eb-98c9-18665628b77b.png)
    
    docker ps in master
    
![image](https://user-images.githubusercontent.com/54719289/105751915-ab4d2b00-5f6c-11eb-89a8-22cae884c3e3.png)

    docker ps in node2:
    
![image](https://user-images.githubusercontent.com/54719289/105752133-e8b1b880-5f6c-11eb-9798-b6523a0d6454.png)


# Docker node active command:

         
        # Docker service update:

        docker service update --replicas=5 --publish-add=80 my_web

   ![image](https://user-images.githubusercontent.com/54719289/105757460-ffa7d900-5f73-11eb-9fe8-47a915fa7590.png)
   
        docker node update --availability active node-hostname
       
        docker node update --availability active nodde1   
   
   ![image](https://user-images.githubusercontent.com/54719289/105757683-4bf31900-5f74-11eb-84c4-6e818cf71ea3.png)

    After activate the node, the workload will not be shared to nodes and it will happen to new workload not for current one.
    
 ![image](https://user-images.githubusercontent.com/54719289/105758704-92954300-5f75-11eb-881e-b110adc56dbb.png)
    
    ![image](https://user-images.githubusercontent.com/54719289/105758403-38947d80-5f75-11eb-9a4c-d37e3f8f3139.png)
    
   Still the page is active  in both node but if you leave from the node it wont be.
   
 ![image](https://user-images.githubusercontent.com/54719289/105758904-cf613a00-5f75-11eb-84bc-607346faf719.png)
 ![image](https://user-images.githubusercontent.com/54719289/105758921-d720de80-5f75-11eb-9cbb-f4c92b38ebcb.png)

        In node 2:  (Status itself Downand not in ready)
        
            docker swarm leave 
            
![image](https://user-images.githubusercontent.com/54719289/105759167-22d38800-5f76-11eb-8a6f-69378db8fa0b.png)
![image](https://user-images.githubusercontent.com/54719289/105759209-2ff07700-5f76-11eb-9d01-caf20a5a5f6d.png)

        in master:
        
 ![image](https://user-images.githubusercontent.com/54719289/105759308-50203600-5f76-11eb-9bbc-99825180a1ad.png)

        
       # To reuse node2 again, we need to use join command
            docker swarm join --token SWMTKN-1-3zd56ii6r8hga67w3z9ukvkrmnxwycht5wm9fuo1acvguskjrd-2u2f1qzp505eayh8pov3fov7w 172.31.89.138:2377

![image](https://user-images.githubusercontent.com/54719289/105759825-ee140080-5f76-11eb-82a0-1418ecb9f40c.png)

![image](https://user-images.githubusercontent.com/54719289/105759850-f704d200-5f76-11eb-804d-a3a80b9690bb.png)

![image](https://user-images.githubusercontent.com/54719289/105759923-10a61980-5f77-11eb-894a-1dbce0129be7.png)


        # In case you need to add multi master
        
            docker swarm join-token manager

![image](https://user-images.githubusercontent.com/54719289/105764522-17378f80-5f7d-11eb-94a3-593f387f4ea9.png)

![image](https://user-images.githubusercontent.com/54719289/105773260-a3e84a80-5f89-11eb-8763-aa52f7b56270.png)
		
		docker node ls


![image](https://user-images.githubusercontent.com/54719289/105773398-d5f9ac80-5f89-11eb-8b48-f205bebc275d.png)

   


##########################################################################################################################################

###   Docker swarm with docker compose

#########################################################################################################################################

Sample:https://docs.docker.com/engine/swarm/stack-deploy/


    1. Create a directory for the project:

        $ mkdir stackdemo
        $ cd stackdemo
    2.  Create a file called app.py in the project directory and paste this in:

        from flask import Flask
        from redis import Redis

        app = Flask(__name__)
        redis = Redis(host='redis', port=6379)

        @app.route('/')
        def hello():
            count = redis.incr('hits')
            return 'Hello World! I have been seen {} times.\n'.format(count)

        if __name__ == "__main__":
            app.run(host="0.0.0.0", port=8000, debug=True)
        
     3.   Create a file called requirements.txt and paste these two lines in:

            flask
            redis

    4.      Create a file called Dockerfile and paste this in:

            FROM python:3.4-alpine
            ADD . /code
            WORKDIR /code
            RUN pip install -r requirements.txt
            CMD ["python", "app.py"]

    5.      Create a file called docker-compose.yml and paste this in:

            version: "3.9"

            services:
            web:
                image: 127.0.0.1:5000/stackdemo
                build: .
                ports:
                - "8000:8000"
            redis:
                image: redis:alpine

        The image for the web app is built using the Dockerfile defined above. It’s also tagged with 127.0.0.1:5000 - the address of the registry created earlier. This is important when distributing the app to the swarm.
        
        
    6. Install docker-compose:
    
    
         sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
         sudo chmod +x /usr/local/bin/docker-compose
         sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
         docker-compose --version
    
    
   7.   docker-compose up -d
        
        docker ps
        
        After creating image, check with docker ps
![image](https://user-images.githubusercontent.com/54719289/105767240-d3df2000-5f80-11eb-95cb-6a82924819b1.png)

  8.	docker-compose down   (checked the image with docker compose,so will try with with docker-stack)
  
  
![image](https://user-images.githubusercontent.com/54719289/105767864-99c24e00-5f81-11eb-9a33-19703567f235.png)


  	 Check the image with curl
   
        	curl localhost:8000/
        
   ![image](https://user-images.githubusercontent.com/54719289/105767573-30dad600-5f81-11eb-9238-bead4f13500a.png)
  

  9.	docker stack deploy --compose-file docker-compose.yml stackdemo
  	Changed compose-yml version as 3.8
	
![image](https://user-images.githubusercontent.com/54719289/105768254-23721b80-5f82-11eb-82da-3452ba528821.png)

	docker ps
	
![image](https://user-images.githubusercontent.com/54719289/105768542-8663b280-5f82-11eb-95e6-fcc9c8fc8008.png)

	docker-compose push
	
	need to change the composefile with our repo as klogambigai/stackdemo
	
	
![image](https://user-images.githubusercontent.com/54719289/105769098-5072fe00-5f83-11eb-8708-7bab1ee198e9.png)
	
	follow step 7, docker-compose build & docker-compose push
![image](https://user-images.githubusercontent.com/54719289/105769582-06d6e300-5f84-11eb-8b65-3b319cd226c9.png)

![image](https://user-images.githubusercontent.com/54719289/105769627-1b1ae000-5f84-11eb-971e-56c70a3ff8ff.png)

	docker stack deploy --compose-file docker-compose.yml stackdemo
	
![image](https://user-images.githubusercontent.com/54719289/105769800-6208d580-5f84-11eb-9763-2a2eff103076.png)
	
	Check in master slaves

![image](https://user-images.githubusercontent.com/54719289/105770216-ea877600-5f84-11eb-82ae-e2475cb1dac4.png)
![image](https://user-images.githubusercontent.com/54719289/105770271-f7a46500-5f84-11eb-88d5-fc7790c3fc78.png)
![image](https://user-images.githubusercontent.com/54719289/105770296-012dcd00-5f85-11eb-86b1-c83ab28a6fa3.png)
![image](https://user-images.githubusercontent.com/54719289/105770329-0be86200-5f85-11eb-8376-1de4ebb48f5d.png)
![image](https://user-images.githubusercontent.com/54719289/105770341-11de4300-5f85-11eb-94f3-82346bba4acc.png)
![image](https://user-images.githubusercontent.com/54719289/105770358-19055100-5f85-11eb-9148-50fff17b014e.png)



  10.	docker stack services stackdemo
  
  	docker stack ls
	
	docker stack services stackdemo
	
![image](https://user-images.githubusercontent.com/54719289/105770744-a21c8800-5f85-11eb-84ae-444e69dec265.png)

  
  11.	curl http://localhost:8000  to test
  
  ![image](https://user-images.githubusercontent.com/54719289/105770889-cbd5af00-5f85-11eb-8926-6bf6f464c5ce.png)
  
  12.	To update the stack
  
  	  docker service update --replicas=5 stackdemo_redis
	  docker service update --replicas=5 stackdemo_web
	  
![image](https://user-images.githubusercontent.com/54719289/105771593-ccbb1080-5f86-11eb-9d41-18f835cb2cd5.png)
	  
	Totally ten services are in master,node1 and node2
	
![image](https://user-images.githubusercontent.com/54719289/105772242-e3159c00-5f87-11eb-94f2-4a7a147b379e.png)
![image](https://user-images.githubusercontent.com/54719289/105772263-edd03100-5f87-11eb-862f-c4bd45287320.png)
![image](https://user-images.githubusercontent.com/54719289/105772281-f7f22f80-5f87-11eb-9399-6a12cf1a9a28.png)
	
	docker node ls in master and master2:

![image](https://user-images.githubusercontent.com/54719289/105774055-d3e41d80-5f8a-11eb-946c-2884cde68b93.png)

	but in master no images
	
![image](https://user-images.githubusercontent.com/54719289/105774267-17d72280-5f8b-11eb-9f89-e93dadabf9a2.png)


  
  12.	Bring the stack down with docker stack rm:

	$ docker stack rm stackdemo

	Removing service stackdemo_web
	Removing service stackdemo_redis
	Removing network stackdemo_default
	
![image](https://user-images.githubusercontent.com/54719289/105774385-35a48780-5f8b-11eb-96e5-b8bd852c20c0.png)

	
  13.	Bring the registry down with docker service rm:

	$ docker service rm registry
	
	If you’re just testing things out on a local machine and want to bring your Docker Engine out of swarm mode, use docker swarm leave:

![image](https://user-images.githubusercontent.com/54719289/105774479-4d7c0b80-5f8b-11eb-924f-ad6d22974075.png)

	$ docker swarm leave --force

![image](https://user-images.githubusercontent.com/54719289/105774565-64baf900-5f8b-11eb-89b5-019263d68db6.png)


  
  
