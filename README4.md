https://github.com/leo-dds/practica-4.git# practica-4

Docker network

Crear unha rede en docker
 **docker network create mired**
31f1232969a7efddf6ed476099ddea865d6dba3830e0188fcc18e11e05e3560d

Crear dous contenedores unidos a esa rede
**docker run -d --name contenedor1 --network mired nginx**
**docker run -d --name contenedor2 --network mired nginx**

Comprobar que os contenedores están na rede
**docker network inspect mired**
"Containers": {
            "7d08f7d36e542606388d80ab414d4b01e528e7875f786fed706c4349ff0e54f8": {
                "Name": "contenedor2",
                "EndpointID": "5af680153fb0e4f4281842296bcead7966c7c3801c8e24afff62468b894ae5e3",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "933635f17cf060e08b9dd19f2e2d2288eecef04b80835a11cb3d2db0f416fbf1": {
                "Name": "contenedor1",
                "EndpointID": "517f1c3e124db1dde9104bc5a4f27ddf67cc5db9ebeecdf97aa40420fbe6175b",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]


Comprobar que os contenedores poden verse entre eles
**docker exec -it contenedor1 sh** entramos en el contenedor mediante sh 
Escribimos bash para tener una terminal, intalamos el ping con **apt-get install -y iputils-ping**
Una vez este intalado ping hacemos **ping contenedor2** y si todo esta bien nos contestara al ping.

Listar os contenedores conectados á rede
**docker ps**
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                     NAMES
7d08f7d36e54   nginx     "/docker-entrypoint.…"   55 minutes ago   Up 55 minutes   80/tcp                                    contenedor2
933635f17cf0   nginx     "/docker-entrypoint.…"   55 minutes ago   Up 55 minutes   80/tcp                                    contenedor1
6e42c7793778   httpd     "httpd-foreground"       10 days ago      Up 10 days      0.0.0.0:8090->80/tcp, [::]:8090->80/tcp   asir_web2
b83fdd91778b   httpd     "httpd-foreground"       13 days ago      Up 13 days      0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   asir_httpd
f9c69f5db224   httpd     "httpd-foreground"       2 weeks ago      Up 2 weeks      0.0.0.0:8000->80/tcp, [::]:8000->80/tcp   asir_web1

Listar as propiedades da rede
 **docker network inspect mired**
[
    {
        "Name": "mired",
        "Id": "31f1232969a7efddf6ed476099ddea865d6dba3830e0188fcc18e11e05e3560d",
        "Created": "2024-10-18T19:20:36.828276908+02:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "7d08f7d36e542606388d80ab414d4b01e528e7875f786fed706c4349ff0e54f8": {
                "Name": "contenedor2",
                "EndpointID": "5af680153fb0e4f4281842296bcead7966c7c3801c8e24afff62468b894ae5e3",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            },
            "933635f17cf060e08b9dd19f2e2d2288eecef04b80835a11cb3d2db0f416fbf1": {
                "Name": "contenedor1",
                "EndpointID": "517f1c3e124db1dde9104bc5a4f27ddf67cc5db9ebeecdf97aa40420fbe6175b",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

Crea outra rede
**docker network create red2**
1bbc9860c8c6f484776eb508e42cd880f7f5e2c66ab6b46dc7fb0fed986f5b00
Lanza dous contenedores novos conectados a esa nova rede

Comproba as posibles conexións entre os 4 contenedores
**docker exec -it contenedor1 sh**
# bash
root@933635f17cf0:/# **ping 172.19.0.1**
PING 172.19.0.1 (172.19.0.1) 56(84) bytes of data.
64 bytes from 172.19.0.1: icmp_seq=1 ttl=64 time=1.49 ms
64 bytes from 172.19.0.1: icmp_seq=2 ttl=64 time=0.120 ms
64 bytes from 172.19.0.1: icmp_seq=3 ttl=64 time=0.113 ms

Cogemos como ejemplo la ip del contenedor3 para hacer la prueba, efectivamente si se hacen ping entre ellos. 

