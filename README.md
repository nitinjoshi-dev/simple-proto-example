Simple proto file and auto generation feature. To Autgenerate the java code run the below command. 

`protoc --java_out=build/gen .\route_guide.proto`


### Intro
This repo uses a [bazel](https://bazel.build/) (Version = 6.0.0) to generate proto
definitions and service stub calls in different languages like Java etc.

### Setup
Install bazel using below command:
 ```shell script
brew install bazelisk
```
This will install bazelisk, which helps in managing different version of bazel. 
The version of bazel is defined in [.bazelversion](./.bazelversion) file.

### Generate and install the java code
To generate jar from Media folder proto file run the below command.
 ```shell script
bazel clean
bazel build //Media:all  --sandbox_debug --verbose_failures
```
This will generate the jar files in bazel-bin folder. 
The jar with *_proto-speed.jar is the jar with the proto classes and the jar with *_java_grpc.jar is the jar with grpc server and client classes

#### Install jar into local m2
Copy the jar from bazel-bin folder to Media/Gen folder and run the below command to install the jar into local m2.
 
```shell script
mv -R bazel-bin/Media/*.jar Media/Gen
mvn install:install-file -Dfile=Media/Gen/libmediia_service_java_grpc.jar -DgroupId=com.lomsmros.mediamanagement -DartifactId=media_service -Dversion=latest -Dpackaging=jar -DgeneratePom=true -Dsources=Media/Gen/libmediia_service_java_grpc-src.jar
mvn install:install-file -Dfile=Media/Gen/libmedia_service_proto-speed.jar -DgroupId=com.lomsmros.mediamanagement -DartifactId=media_proto -Dversion=latest -Dpackaging=jar -DgeneratePom=true -Dsources=Media/Gen/media_service_proto-speed-src.jar
```