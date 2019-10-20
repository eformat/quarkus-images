## quarkus-native-s2i-ubi

S2I image for building native images for quarkus based on maven and graalvm

```
docker build -t quarkus-native-s2i-ubi .
docker tag quarkus-native-s2i-ubi:latest quay.io/eformat/quarkus-native-s2i-ubi:graalvm-19.2.1
docker tag quarkus-native-s2i-ubi:latest quay.io/eformat/quarkus-native-s2i-ubi:latest
docker push quay.io/eformat/quarkus-native-s2i-ubi:graalvm-19.2.1
docker push quay.io/eformat/quarkus-native-s2i-ubi:latest
```