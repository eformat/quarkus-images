## quarkus-native-s2i-ubi

```
docker build -t quarkus-native-s2i-ubi .
docker tag quarkus-native-s2i-ubi:latest quay.io/eformat/quarkus-native-s2i-ubi:graalvm-19.1.1
docker tag quarkus-native-s2i-ubi:latest quay.io/eformat/quarkus-native-s2i-ubi:latest
docker push quay.io/eformat/quarkus-native-s2i-ubi:graalvm-19.1.1
docker push quay.io/eformat/quarkus-native-s2i-ubi:latest
```