# Docker

### What is container ?

เป็นเทคนิคการจัดการ Package ของ Software รูปแบบหนึ่ง

### What is Docker ?

![docker_image](https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-appliction-blue-border_2.png)

Docker คือแพลตฟอร์มซอฟต์แวร์ที่ช่วยให้สร้าง ทดสอบ และติดตั้งแอปพลิเคชันใช้จริงได้อย่างรวดเร็ว

Docker จะบรรจุซอฟต์แวร์ลงไปในหน่วยที่เป็นมาตรฐานเรียกว่า Container ซึ่งจะมีทุกสิ่งที่ซอฟต์แวร์ต้องใช้ในการเรียกใช้งาน รวมทั้งไลบรารี เครื่องมือสำหรับระบบ โค้ด และรันไทม์ เมื่อใช้ Docker ใช้งานและปรับปรุงแอปพลิเคชันให้เหมาะกับทุกสภาพแวดล้อม

### Why should you use docker ?

1. Consistent and Isolate Environment

    Container แต่ละตัวมี Environment ที่แยกออกจากกัน ไม่ว่าจะทำงานกี่ครั้งจะได้ผลลัพธ์เหมือนกันเสมอ

2. Rapid Application Deployment

    ทำให้การ Deploy application ไวขึ้น เช่น ไม่เสียเวลาในส่วนของการ Boot OS, ไม่ต้อง Set up environment ใหม่ทุกการ Deploy เพราะมีการกำหนดไว้ใน `Dockerfile` หมด, ใช้งานร่วมกับ CI/CD workflow

3. Ensures Scalability & Flexibility

    `Dockerfile` เดียวสามารถสร้าง Container ใหม่ได้เรื่อย ๆ รวมถึงการทำ Update version ของ Application

4. Better Portability

    ทำงานบน Platform ไหนก็ได้ที่ Support container เช่น Amazon EC2, GCP

5. In-Built Version Control System

    สามารถทำ Versioning ของ Image ได้

6. Security

    มีการแยก Environment ออกไปจากการทำงานตามปกติ ทำให้ Container แต่ละอันแยกออกจากกันและหากมีปัญหาก็สามารถลบ Container นั้นทิ้งได้เลย


# Manipulate with Docker

## Docker command

- docker pull

    ```bash
    docker pull [OPTIONS] NAME[:TAG|@DIGEST]
    # Pull postgres image
    docker pull postgres
    ```

- docker run

    ```bash
    docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
    # Run database in 1 command
    docker run --name dev-db -e POSTGRES_PASSWORD=qwerty -d postgres
    ```

- docker stop

    ```bash
    docker stop [OPTIONS] CONTAINER [CONTAINER...]
    # Stop the dev-db conatiner
    docker stop dev-db
    ```

- docker start

    ```bash
    docker start [OPTIONS] CONTAINER [CONTAINER...]
    # Start the container
    docker start dev-db
    ```

- docker exec

    ```bash
    docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
    ls
    whoami
    # Run a commmand inside running conatiner
    docker exec -it dev-db sh
    ls
    whoami
    exit
    ```

- docker logs

    ```bash
    docker logs [OPTIONS] CONTAINER
    # Fetch logs of container
    docker logs -f dev-db
    ```

- docker ps

    ```bash
    docker ps [OPTIONS]
    # List container
    docker ps --all
    ```

- docker top

    ```bash
    docker top dev-db
    # Display the running processes of a container
    ```

- docker stats

    ```bash
    # Show stats for all containers
    docker stats
    ```

- docker images

    ```bash
    docker image ls
    ```

- docker rmi

    ```bash
    docker rmi IMAGE [IMAGE...]
    docker rmi postgres
    # You will get error ca, container dev-db using this image as ref
    ```

- docker rm

    ```bash
    docker rm [OPTIONS] CONTAINER [CONTAINER...]
    docker ps
    docker rm dev-db
    docker ps
    docker rmi postgres
    ```

- docker build

    ```bash
    docker build [OPTIONS] PATH | URL | -
    docker build github.com/creack/docker-firefox
    ```

- Cleaning up

    ```bash
    docker system prune
    ```


## Dockerfile

[Build your Go image](https://docs.docker.com/language/golang/build-images/)

Docker จะทำการ Build image จากคำสั่งที่ถูกกำหนดลงใน Dockerfile

วิธีการใช้งาน

```bash
cd '1. Dockerfile'
docker build -t web-server .
docker run -it -p 8080:8080 web-server
curl http://localhost:8080
```

## Docker compose

[Postgres - Official Image | Docker Hub](https://hub.docker.com/_/postgres)

```bash
cd '2. Dockercompose + docker hub'
docker-compose up
# Terminate
docker-compose down
```

## Docker hub

Docker เค้าจะมีระบบ registry ที่มีชื่อว่า *Docker Hub *****ที่จะเปิดให้คนทั่วไปสามารถเอา image ที่ตัวเองสร้างขึ้นมาเก็บไว้ที่ส่วนกลาง

เหมือนตอนที่เราดึง postgres image มาตอน `docker pull postgres`

สามารถ Push Image ที่เรามีขึ้นไปได้ด้วยจาก Dockerfile

```bash
cd '1. Dockerfile'
docker build --tag docker-gs-ping .
docker tag docker-gs-ping <username>/<repo>
docker push <username>/<repo>
```

## Docker env

สามารถ Mount ตัว Environment variables เข้าไปยัง Container ได้ด้วย `-e`

```bash
docker run --name dev-db -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=qwerty -d postgres
# List the use that belong to database
docker exec -it dev-db psql -U admin
\l
exit
```

# LAB

## Create a Web Server Using Docker

[Nginx - Official Image | Docker Hub](https://hub.docker.com/_/nginx) -> Hosting some simple static content

—> ubuntu/nginx

```bash
cd '3. web server'
docker system prune --all --force
echo "Docker more layer"
time docker build -t demo -f Dockerfile.more-layers .
docker system prune --all --force
echo "Docker less layer"
time docker build -t demo -f Dockerfile.less-layers .
```

```bash
docker run -p 8080:80 demo
```

## Create a Web application Using ......

[Sample application | Docker Documentation](https://docs.docker.com/get-started/02_our_app/)

1. ดาว์นโหลด [https://github.com/docker/getting-started/archive/refs/heads/master.zip](https://github.com/docker/getting-started/archive/refs/heads/master.zip) และเปิดโปรเดอร์ app ใน code editor ที่ชอบ
2. สร้างไฟล์ `Dockerfile` ในโฟล์เดอร์เดียวกับที่มีไฟล์ `package.json`
3. เปิด terminal และใช้คำสั่ง

```bash
cd '4. web application'
docker system prune --all --force
time docker build -t app -f Dockerfile.latest .
# 1:03.03 minute
docker system prune --all --force
time docker build -t app-alpine -f Dockerfile.alpine-version .
# 1:20.83 minutes
docker system prune --all --force
time docker build -t app-multi-stg -f Dockerfile .
# 24.683 total
docker image list
```
