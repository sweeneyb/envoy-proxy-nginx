To run:
```
mkdir log
touch log/ingress_listener.log
sudo chmod 101 log/ingress_listener.log
```

Envoy uses the 101 user, so I needed a spot for it to write on my system

Running the processes:

```
docker run --rm -d -v `pwd`/green:/usr/share/nginx/html --name nginx-green -p 8081:80 nginx:mainline-alpine
docker run --rm -d -v `pwd`/blue:/usr/share/nginx/html --name nginx-blue -p 8082:80 nginx:mainline-alpine

docker run --rm -it \
      -v $(pwd)/front-envoy.yaml:/front-envoy.yaml \
      -v $(pwd)/log/:/log/ \
      -p 8080:8080 \
      -p 8443:8443 \
      -p 8001:8001 \
      envoyproxy/envoy-dev:93cd7c7835ac4c0be64acdc90d3a55129fdd39e8 \
          -c /front-envoy.yaml
```
