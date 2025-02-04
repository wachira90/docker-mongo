Here’s a `docker-compose.yml` file to set up a **MongoDB 8.0.4 replica set** with three nodes using the `public.ecr.aws/docker/library/mongo:8.0.4` image:

### **docker-compose.yml**
```yaml
version: '3.8'

services:
  mongo1:
    image: public.ecr.aws/docker/library/mongo:8.0.4
    container_name: mongo1
    restart: always
    command: ["mongod", "--replSet", "rs0", "--bind_ip_all"]
    ports:
      - "27017:27017"
    networks:
      - mongo_network
    volumes:
      - mongo1_data:/data/db

  mongo2:
    image: public.ecr.aws/docker/library/mongo:8.0.4
    container_name: mongo2
    restart: always
    command: ["mongod", "--replSet", "rs0", "--bind_ip_all"]
    ports:
      - "27018:27017"
    networks:
      - mongo_network
    volumes:
      - mongo2_data:/data/db

  mongo3:
    image: public.ecr.aws/docker/library/mongo:8.0.4
    container_name: mongo3
    restart: always
    command: ["mongod", "--replSet", "rs0", "--bind_ip_all"]
    ports:
      - "27019:27017"
    networks:
      - mongo_network
    volumes:
      - mongo3_data:/data/db

  mongo-setup:
    image: public.ecr.aws/docker/library/mongo:8.0.4
    container_name: mongo-setup
    depends_on:
      - mongo1
      - mongo2
      - mongo3
    networks:
      - mongo_network
    entrypoint: [ "bash", "-c", "sleep 5 && mongosh --host mongo1:27017 --eval 'rs.initiate({_id: \"rs0\", members: [{ _id: 0, host: \"mongo1:27017\" }, { _id: 1, host: \"mongo2:27017\" }, { _id: 2, host: \"mongo3:27017\" }]})'" ]

networks:
  mongo_network:
    driver: bridge

volumes:
  mongo1_data:
  mongo2_data:
  mongo3_data:
```

---

### **How It Works**
1. **Three MongoDB containers** (`mongo1`, `mongo2`, `mongo3`) are created.
2. Each MongoDB container runs with the `--replSet rs0` flag to enable replication.
3. A **mongo-setup** container runs a script to initiate the replica set and add all three nodes.

### **How to Use**
1. **Start the containers**:
   ```bash
   docker-compose up -d
   ```
2. **Check the replica set status**:
   ```bash
   docker exec -it mongo1 mongosh --eval "rs.status()"
   ```

Let me know if you need any modifications! 🚀
