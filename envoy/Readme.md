### [Envoy front proxy](https://dramasamy.medium.com/life-of-a-packet-in-istio-envoy-proxy-part-2-27950e820b04)

- A listener to accept incoming traffic on port 8000 and forward it to a cluster named “service” using RR LB.



![img](https://miro.medium.com/v2/resize:fit:875/1*8ShoNXirF6uSXB5vDIIsnw.png)



1. Create network

```bash
$ docker network create --subnet=192.168.0.0/16 local
```

2. Docker Compose up

```bash
$ docker compose up -d
```

3. Verify

- envoy: https://localhost:18000/service
- service_blue:  http://localhost:13000/
- service_green: http://localhost:23000/
- service:red:   http://localhost:33000/

4. Clean up

```bash
$ docker compose down
```

