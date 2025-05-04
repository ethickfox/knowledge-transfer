==**Docker**== is a technology that allows you to build, run, test, and deploy distributed applications. It uses operating-system-level virtualization to deliver software in packages called **containers**.

![Untitled 95.png](../Software_Architecture/_img/Untitled%2095.png)

---

## Docker Entities

### Image

It is a template for environment creation. It has everything your app needs: OS, Software, Code. And might be treated like Snapshot.

---

### Container

Container is a running instance of image

```Mermaid
graph LR
  A(IMAGE)-->|RUN| CONTAINER
```

These containers are isolated from each other and bundle their own tools, libraries, and configuration files. They can communicate with each other through well-defined channels. All containers are run by a single operating system kernel, and therefore use few resources.  
  
Images can be created by either using a   
`Dockerfile` (which contains all the necessary instructions for creating an image) or by using `Docker commit`, which takes an existing container and creates an image from it.

---

### Ports

Through the port we can communicate with container.

[![](https://lh5.googleusercontent.com/JvTYxIaUClIEsNlb3p6ftVLo1XaIwT-um_18pAxC3ulDWHwXnN_93NcxRn1fwXrFmkJjB1Kgab-sHRAAPU4DaQr1g13rJ63mqafHr8eKJy7C_TCMfhKcAkTi4FoNcnrBRgQ4Dpd8CCA0a6C3kDOAI_VePAU5UJ3KaWdhe9dJYuG8RD61CuuNUmBsRTtJPQ)](https://lh5.googleusercontent.com/JvTYxIaUClIEsNlb3p6ftVLo1XaIwT-um_18pAxC3ulDWHwXnN_93NcxRn1fwXrFmkJjB1Kgab-sHRAAPU4DaQr1g13rJ63mqafHr8eKJy7C_TCMfhKcAkTi4FoNcnrBRgQ4Dpd8CCA0a6C3kDOAI_VePAU5UJ3KaWdhe9dJYuG8RD61CuuNUmBsRTtJPQ)

---

### Volumes

Volume creating - it is the way, how we can share files between host and container / between containers.  
  
  
`docker volume create --name volume-data` - create our volume  
  
  
`docker run -d -v volume-data:/data --name nginx-test nginx:latest` - mount in the container /data folder.  
  

[![](https://lh4.googleusercontent.com/-VtLb3kW1eUz655ePXzIiscPaB8zQA5DMJ-EsvvLyb0z-T_Aqxn2nygVroVkWZoMVWMtJENeTxdaXSD0g3Wjg1RQ6QJ_VBtfdEYIFbyTFNOGnMe0WskMgzHfpzjt0UVNbaSXg0MI7pQ0KxXgh-kjlCz0dKTxvIb5sUiKNroDgGKzfuSjA25djP232ppKVw)](https://lh4.googleusercontent.com/-VtLb3kW1eUz655ePXzIiscPaB8zQA5DMJ-EsvvLyb0z-T_Aqxn2nygVroVkWZoMVWMtJENeTxdaXSD0g3Wjg1RQ6QJ_VBtfdEYIFbyTFNOGnMe0WskMgzHfpzjt0UVNbaSXg0MI7pQ0KxXgh-kjlCz0dKTxvIb5sUiKNroDgGKzfuSjA25djP232ppKVw)

[![](https://lh3.googleusercontent.com/wd0m2xNpVJhC_Dap3FbTv2-ITc22SoLOMMbGOVm6dc7XvNrfAe2mxTNCpOZ4Qvu87OobOal6rHoNJ_aoPrbnhWsA2mN7e5-IEwerFAEEqh7OzK_qatEPQHF8FCeg1U_UA3SmOb78mO3iIp2w9if-aXaBanVm6bEEzAUh9pXPPlS-TniZ2km9ToCDOYFfdw)](https://lh3.googleusercontent.com/wd0m2xNpVJhC_Dap3FbTv2-ITc22SoLOMMbGOVm6dc7XvNrfAe2mxTNCpOZ4Qvu87OobOal6rHoNJ_aoPrbnhWsA2mN7e5-IEwerFAEEqh7OzK_qatEPQHF8FCeg1U_UA3SmOb78mO3iIp2w9if-aXaBanVm6bEEzAUh9pXPPlS-TniZ2km9ToCDOYFfdw)

[![](https://lh4.googleusercontent.com/4MG8Rqlw-NF3r4xedaNdX8KciWhFvxwFzBsHmpLhPpCa-XnZkIv5AQSP86JNqgQJdifwA2O8tPhUq7Oxwm5SqChiOWzwgPlZNlvpIrWq4lnX_irGko3flAyCOJdy_Slnz1LCV9YTqlcesjh9n0BGlB1cNXPCOa6CPtbDX6_t0siJb4lpuILv9JQ5LfSQkQ)](https://lh4.googleusercontent.com/4MG8Rqlw-NF3r4xedaNdX8KciWhFvxwFzBsHmpLhPpCa-XnZkIv5AQSP86JNqgQJdifwA2O8tPhUq7Oxwm5SqChiOWzwgPlZNlvpIrWq4lnX_irGko3flAyCOJdy_Slnz1LCV9YTqlcesjh9n0BGlB1cNXPCOa6CPtbDX6_t0siJb4lpuILv9JQ5LfSQkQ)

---

### Dockerfile

Dockerfile it is an instruction how to build our container properly.

[![](https://lh5.googleusercontent.com/zF-01H8Qt2jd5jp9E2wATU5lZYivQMP9ymQ_QzRKUsithozjeeBrnk2OgnJDXYI8JQrrzeVYU-m19cDVEZa31vmBUmDlrlViczZjEDi5TJxa47BC5Cd1Jvdc8zStHl2EtSWzZTw3Q5L6Qt-JaqGP4HFFfW25iJVYKA3uMoZq1ip5nyka3_X2jG6eSTjLng)](https://lh5.googleusercontent.com/zF-01H8Qt2jd5jp9E2wATU5lZYivQMP9ymQ_QzRKUsithozjeeBrnk2OgnJDXYI8JQrrzeVYU-m19cDVEZa31vmBUmDlrlViczZjEDi5TJxa47BC5Cd1Jvdc8zStHl2EtSWzZTw3Q5L6Qt-JaqGP4HFFfW25iJVYKA3uMoZq1ip5nyka3_X2jG6eSTjLng)

[![](https://lh5.googleusercontent.com/13cRNBLKg_EnZQaUvr3hA9YMFzip-w4ixOPho5lv1AsKB5afrD6O9vca4Imu7o3hzZjWg1CT76k9xUH3WJYKI2_gr_8Sm0HWY47mjv7BN00L89RNQp8M5HToAno7zh6woKHDeHG8xzrr9PPVw_EVgqwhBJJDuZc-vZlhZtLYZ6oncKII2ybNFD2rFIIhkA)](https://lh5.googleusercontent.com/13cRNBLKg_EnZQaUvr3hA9YMFzip-w4ixOPho5lv1AsKB5afrD6O9vca4Imu7o3hzZjWg1CT76k9xUH3WJYKI2_gr_8Sm0HWY47mjv7BN00L89RNQp8M5HToAno7zh6woKHDeHG8xzrr9PPVw_EVgqwhBJJDuZc-vZlhZtLYZ6oncKII2ybNFD2rFIIhkA)

[![](https://lh4.googleusercontent.com/jQQlpn5e-ZFymJIhYayvdbCkQwKWxs6TRAoBogeZp_5VhUETVxg_-mna5InqUhw1XBYKbMD8wagfHkOh78p39iu_2Ct3Qdni6BG9t-BqP3fP9CJUhSbZJPmq_Na5jo7Gd7foMBadgRJQeGrmc2_-o-ZCwVVZ77g_cNpbPrsWxqpE2pKlYjvk3RNVGtLMhQ)](https://lh4.googleusercontent.com/jQQlpn5e-ZFymJIhYayvdbCkQwKWxs6TRAoBogeZp_5VhUETVxg_-mna5InqUhw1XBYKbMD8wagfHkOh78p39iu_2Ct3Qdni6BG9t-BqP3fP9CJUhSbZJPmq_Na5jo7Gd7foMBadgRJQeGrmc2_-o-ZCwVVZ77g_cNpbPrsWxqpE2pKlYjvk3RNVGtLMhQ)

[![](https://lh3.googleusercontent.com/_q-DP-oN6lftTSiAuSbneJXmRSGlb7-VCOG1D-mmUc-AuJlNLcIQ9CB_nzm86GMFP2_7uoRl2c0d5D-DDgnRjxuFW-AME0PJ_Sm6SyXgFpM5jhEAta0qYpwHQ2u2SujXk4nUtZdfoGt3zJBdneY8YjvjCtgbSwXSSiUfbfADY9ERtUTmFLzfFnBK9ahQdw)](https://lh3.googleusercontent.com/_q-DP-oN6lftTSiAuSbneJXmRSGlb7-VCOG1D-mmUc-AuJlNLcIQ9CB_nzm86GMFP2_7uoRl2c0d5D-DDgnRjxuFW-AME0PJ_Sm6SyXgFpM5jhEAta0qYpwHQ2u2SujXk4nUtZdfoGt3zJBdneY8YjvjCtgbSwXSSiUfbfADY9ERtUTmFLzfFnBK9ahQdw)

**ˇ**

[![](https://lh5.googleusercontent.com/G9v6q3R879azyEufgZ9YW3nbzjdykh2uOadE4g_3TC1SeD58HcbMWwXmvd_sic03ZiEE9F8qf79mDX074xhQYMEhhE7fxR6OgXNlp4bCbmE6rhkPyuEs7binpNtQJicuhM5gCL5l2XWsajHG462_35nKs9BrFqvIVZKX_qU-3bNYasgEcS5xAU-aB4dD7w)](https://lh5.googleusercontent.com/G9v6q3R879azyEufgZ9YW3nbzjdykh2uOadE4g_3TC1SeD58HcbMWwXmvd_sic03ZiEE9F8qf79mDX074xhQYMEhhE7fxR6OgXNlp4bCbmE6rhkPyuEs7binpNtQJicuhM5gCL5l2XWsajHG462_35nKs9BrFqvIVZKX_qU-3bNYasgEcS5xAU-aB4dD7w)

```Docker
\#each line creates new layer, each layer have their cache
FROM node:latest
# Open or create dir where you will work
WORKDIR /app
# All further steps will be rerun(cache not used) if there are any changes in this file
ADD package*.json ./
# so all dependencies will be cached and not installed again if package.json not changed
RUN npm install
ADD . .
CMD npm start
```

```Docker
FROM adoptopenjdk/openjdk11:jdk-11.0.11_9-alpine-slim
EXPOSE 8092
ARG JAR_FILE=build/libs/PostService-0.0.1-SNAPSHOT.jar
COPY ${JAR_FILE} /app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

### Tags

Tagging allows us to control image version and avoid breaking changes.  
  

**alpine images are very lightweight**

[![](https://lh5.googleusercontent.com/JGSAD7FaESCglpGkp_4fqIID00r2iin1mEjQ0KzDN7JOuQkOqd5wI6So5FkNlSv3IhCoX3_E3bEBcPQtEg2SgAL17uGizMZCJuJDQkyqhZiV_51Ys-5L9Monky_90Z-86CZxEgn7oVeFRbDu1zjvGPduBWa2t-qUJvlimzq59-ZjgKRXOPXW8OetUtWCgA)](https://lh5.googleusercontent.com/JGSAD7FaESCglpGkp_4fqIID00r2iin1mEjQ0KzDN7JOuQkOqd5wI6So5FkNlSv3IhCoX3_E3bEBcPQtEg2SgAL17uGizMZCJuJDQkyqhZiV_51Ys-5L9Monky_90Z-86CZxEgn7oVeFRbDu1zjvGPduBWa2t-qUJvlimzq59-ZjgKRXOPXW8OetUtWCgA)

[![](https://lh5.googleusercontent.com/fTQTcwXBBFJ-Opsiqqgkx-b00okwTn_Pk7hJAnzbdhH69qxijIp1paj6Q3PmrqS_JmQNF28TwEC7lIoFayhrEYJ3qcsGGEaXSqJvxcVo5X9dB5qQqrA5cVG3fudcnGW6L2P0VPIHUPcW06VAoNrPnLK9LUK0Qf6goZQTu6VAKNX_Ljno7-dbSaIj6cPx7Q)](https://lh5.googleusercontent.com/fTQTcwXBBFJ-Opsiqqgkx-b00okwTn_Pk7hJAnzbdhH69qxijIp1paj6Q3PmrqS_JmQNF28TwEC7lIoFayhrEYJ3qcsGGEaXSqJvxcVo5X9dB5qQqrA5cVG3fudcnGW6L2P0VPIHUPcW06VAoNrPnLK9LUK0Qf6goZQTu6VAKNX_Ljno7-dbSaIj6cPx7Q)

[![](https://lh3.googleusercontent.com/eCwq7_VvkGYO1bjvMwOd0_PW-7qlZr4z-4u-i8UXN_Pr8LPuuqaQ1i-22tSp6UZx0qTVE7rV4ktzcCSQSYoG7yA1f-yQoR7MMX5Bf5jlkTYk7U-YYvydufMNSTg8S87zgl7eaVpA8mVk0P9z0sR9wIL0PRIGSB8cxX2jxXxoMfoHHjLwvuzLShHmcOIUaw)](https://lh3.googleusercontent.com/eCwq7_VvkGYO1bjvMwOd0_PW-7qlZr4z-4u-i8UXN_Pr8LPuuqaQ1i-22tSp6UZx0qTVE7rV4ktzcCSQSYoG7yA1f-yQoR7MMX5Bf5jlkTYk7U-YYvydufMNSTg8S87zgl7eaVpA8mVk0P9z0sR9wIL0PRIGSB8cxX2jxXxoMfoHHjLwvuzLShHmcOIUaw)

---

### **Docker Registries**

- Highly scalable server side application that stores and lets you distribute Docker images.
- Used in your CI/CD Pipeline
- Run you applications

[![](https://lh6.googleusercontent.com/HooGbBjyX9nx6CAQFNR3Mm7JhhD3wXD9OSFpayYlxzNrZzDXLK9UE7M6aNLY6Dp2DqRXFTHyPIXRJrXHl8IBe9XYdxXzRiB3vgwFiFub-8498rR9RHHj0KorqF0ssUARX7e25rWIkouFo1HMR6CNQZXVjA4zNrcp-5WxagGNwqjrwv6WzC_-Mxjd6O-KlA)](https://lh6.googleusercontent.com/HooGbBjyX9nx6CAQFNR3Mm7JhhD3wXD9OSFpayYlxzNrZzDXLK9UE7M6aNLY6Dp2DqRXFTHyPIXRJrXHl8IBe9XYdxXzRiB3vgwFiFub-8498rR9RHHj0KorqF0ssUARX7e25rWIkouFo1HMR6CNQZXVjA4zNrcp-5WxagGNwqjrwv6WzC_-Mxjd6O-KlA)

[![](https://lh6.googleusercontent.com/cpReRW-HrXn28vbmyK7nvyyicJAibJsYfMxx4bQeKH9hXrziEs0A05utFRV3gGIL4gImGOyG_BtzD1VqB-0sf2mCdWP6D2s_kTcaXkS46xuEUam1pSm1lpnH1XtjKbHXlRzzClDs1BOM6MEafDFDJikEKyMkd16reJoGGYCUoQaR0r2h5Snoiwtni6Cjqg)](https://lh6.googleusercontent.com/cpReRW-HrXn28vbmyK7nvyyicJAibJsYfMxx4bQeKH9hXrziEs0A05utFRV3gGIL4gImGOyG_BtzD1VqB-0sf2mCdWP6D2s_kTcaXkS46xuEUam1pSm1lpnH1XtjKbHXlRzzClDs1BOM6MEafDFDJikEKyMkd16reJoGGYCUoQaR0r2h5Snoiwtni6Cjqg)

---

### Commands

|   |   |
|---|---|
|[docker attach](https://docs.docker.com/engine/reference/commandline/attach/)|Attach local standard input, output, and error streams to a running container|
|[**docker build**](https://docs.docker.com/engine/reference/commandline/build/)|Build an image from a Dockerfile|
|[docker builder](https://docs.docker.com/engine/reference/commandline/builder/)|Manage builds|
|[docker checkpoint](https://docs.docker.com/engine/reference/commandline/checkpoint/)|Manage checkpoints|
|[docker commit](https://docs.docker.com/engine/reference/commandline/commit/)|Create a new image from a container’s changes|
|[docker config](https://docs.docker.com/engine/reference/commandline/config/)|Manage Docker configs|
|[docker container](https://docs.docker.com/engine/reference/commandline/container/)|Manage containers|
|[docker context](https://docs.docker.com/engine/reference/commandline/context/)|Manage contexts|
|[**docker cp**](https://docs.docker.com/engine/reference/commandline/cp/)|Copy files/folders between a container and the local filesystem|
|[docker create](https://docs.docker.com/engine/reference/commandline/create/)|Create a new container|
|[docker diff](https://docs.docker.com/engine/reference/commandline/diff/)|Inspect changes to files or directories on a container’s filesystem|
|[docker events](https://docs.docker.com/engine/reference/commandline/events/)|Get real time events from the server|
|[docker exec](https://docs.docker.com/engine/reference/commandline/exec/)|Run a command in a running container(-it for interaction)|
|[docker export](https://docs.docker.com/engine/reference/commandline/export/)|Export a container’s filesystem as a tar archive|
|[docker history](https://docs.docker.com/engine/reference/commandline/history/)|Show the history of an image|
|[docker image](https://docs.docker.com/engine/reference/commandline/image/)|Manage images|
|[**docker images**](https://docs.docker.com/engine/reference/commandline/images/)|List images|
|[docker import](https://docs.docker.com/engine/reference/commandline/import/)|Import the contents from a tarball to create a filesystem image|
|[**docker info**](https://docs.docker.com/engine/reference/commandline/info/)|Display system-wide information|
|[**docker inspect**](https://docs.docker.com/engine/reference/commandline/inspect/)|Return low-level information on Docker objects|
|[**docker kill**](https://docs.docker.com/engine/reference/commandline/kill/)|Kill one or more running containers|
|[docker load](https://docs.docker.com/engine/reference/commandline/load/)|Load an image from a tar archive or STDIN|
|[**docker login**](https://docs.docker.com/engine/reference/commandline/login/)|Log in to a Docker registry|
|[**docker logout**](https://docs.docker.com/engine/reference/commandline/logout/)|Log out from a Docker registry|
|[**docker logs**](https://docs.docker.com/engine/reference/commandline/logs/)|Fetch the logs of a container|
|[docker manifest](https://docs.docker.com/engine/reference/commandline/manifest/)|Manage Docker image manifests and manifest lists|
|[docker network](https://docs.docker.com/engine/reference/commandline/network/)|Manage networks|
|[docker node](https://docs.docker.com/engine/reference/commandline/node/)|Manage Swarm nodes|
|[docker pause](https://docs.docker.com/engine/reference/commandline/pause/)|Pause all processes within one or more containers|
|[docker plugin](https://docs.docker.com/engine/reference/commandline/plugin/)|Manage plugins|
|[docker port](https://docs.docker.com/engine/reference/commandline/port/)|List port mappings or a specific mapping for the container|
|[docker ps](https://docs.docker.com/engine/reference/commandline/ps/)|List containers|
|[**docker pull**](https://docs.docker.com/engine/reference/commandline/pull/)|Pull an image or a repository from a registry|
|[**docker push**](https://docs.docker.com/engine/reference/commandline/push/)|Push an image or a repository to a registry|
|[docker rename](https://docs.docker.com/engine/reference/commandline/rename/)|Rename a container|
|[docker restart](https://docs.docker.com/engine/reference/commandline/restart/)|Restart one or more containers|
|[**docker rm**](https://docs.docker.com/engine/reference/commandline/rm/)|Remove one or more containers|
|[**docker rmi**](https://docs.docker.com/engine/reference/commandline/rmi/)|Remove one or more images|
|[**docker run**](https://docs.docker.com/engine/reference/commandline/run/)|Create and run a new container from an image|
|[docker save](https://docs.docker.com/engine/reference/commandline/save/)|Save one or more images to a tar archive (streamed to STDOUT by default)|
|[docker search](https://docs.docker.com/engine/reference/commandline/search/)|Search the Docker Hub for images|
|[docker secret](https://docs.docker.com/engine/reference/commandline/secret/)|Manage Docker secrets|
|[docker service](https://docs.docker.com/engine/reference/commandline/service/)|Manage services|
|[docker stack](https://docs.docker.com/engine/reference/commandline/stack/)|Manage Docker stacks|
|[**docker start**](https://docs.docker.com/engine/reference/commandline/start/)|Start one or more stopped containers|
|[docker stats](https://docs.docker.com/engine/reference/commandline/stats/)|Display a live stream of container(s) resource usage statistics|
|[**docker stop**](https://docs.docker.com/engine/reference/commandline/stop/)|Stop one or more running containers|
|[docker swarm](https://docs.docker.com/engine/reference/commandline/swarm/)|Manage Swarm|
|[docker system](https://docs.docker.com/engine/reference/commandline/system/)|Manage Docker|
|[**docker tag**](https://docs.docker.com/engine/reference/commandline/tag/)|Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE|
|[docker top](https://docs.docker.com/engine/reference/commandline/top/)|Display the running processes of a container|
|[docker trust](https://docs.docker.com/engine/reference/commandline/trust/)|Manage trust on Docker images|
|[docker unpause](https://docs.docker.com/engine/reference/commandline/unpause/)|Unpause all processes within one or more containers|
|[docker update](https://docs.docker.com/engine/reference/commandline/update/)|Update configuration of one or more containers|
|[docker volume](https://docs.docker.com/engine/reference/commandline/volume/)|Manage volumes|
|[docker wait](https://docs.docker.com/engine/reference/commandline/wait/)|Block until one or more containers stop, then print their exit codes|

## Docker Compose

```YAML
version: '3.8'
services:
  redis:
    image: redis:6.2-alpine
    restart: always
    ports:
      - '6379:6379'
      - '8001:8001'
    command: redis-server --save 20 1 --loglevel warning --requirepass eYVX7EwVmmxKPCDmwMtyKVge8oLd2t81
    volumes: 
      - redis:/data
volumes:
  redis:
    driver: './redis'
```