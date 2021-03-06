# tensorboardprotobuf
Write result files (protobuffers) for Tensorboard with minimal dependencies (protobuf libs). Downside is we shamelessly duplicate the
protobuf files from the tensorflow repo. Basically [tensorboardX](https://github.com/lanpa/tensorboardX) for C++.

Platform | Build status
---------|-------------:
Linux (gcc-8 -std=c++17)| [![Build Status](https://travis-ci.org/plang85/tensorboardprotobuf.svg?branch=master)](https://travis-ci.org/plang85/tensorboardprotobuf)

## Interface
The interface is intentionally kept to a minmum: A `Run` instance is tied to one tensorboard input file and contains an arbitrary number of `Record` instances. A `Record` is tied to a step and contains values (scalars, tensors, etc...) listed in `types.h`.

```
tbproto::Run run;

// first iteration (e.g.)
{
  tbproto::Record rec;
  rec.set_step(0);
  rec.add("my/value", tbproto::Scalar(0.0));
  rec.add("my/othervalue", tbproto::Scalar(1.0));
  run.write(rec);
}
// second iteration (e.g.)
{
  tbproto::Record rec;
  rec.set_step(1);
  rec.add("my/value", tbproto::Scalar(1.0));
  rec.add("my/othervalue", tbproto::Scalar(0.8));
  run.write(rec);
}
```
will write a file `events.out.tfevents.<isotime>` to the working directory.

## Installation
https://github.com/google/protobuf/releases/download/v3.6.1/protobuf-cpp-3.6.1.tar.gz
```
#cd into protobuf source dir
mkdir build
cd build
cmake -DBUILD_SHARED_LIBS=1 -DCMAKE_INSTALL_PREFIX=/usr ../cmake
cmake --build . -- -j 8
cmake --build . --target install
```
```
#cd into source dir
mkdir build
cd build
cmake ..
cmake --build .
```
or in container
```
docker run -it --rm --mount type=bind,source=$(pwd),target=/tbproto philipplang/gccprotobuf:latest /bin/bash
```
```
docker run -it --rm -p 6006:6006 --mount type=bind,source=$(pwd),target=/tb tensorboardprotobuf:latest
tensorboard --logdir /tb
http://localhost:6006
```

## References

https://github.com/tensorflow/tensorflow/
https://github.com/lanpa/tensorboardX
https://github.com/PaddlePaddle/board/tree/fileWriterTensorBoard/PaddleTensorBoardDemo 


## License

Under MIT license. From Tensorflow components (`tbproto/tensorflow/*`) taken from 
https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/framework/resource_handle.proto
https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/framework/tensor.proto
https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/framework/types.proto
https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/framework/event.proto
https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/framework/summary.proto
https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/lib/hash/crc32c.cc, https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/lib/io/record_writer.cc, https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/lib/core/raw_coding.h, https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/lib/core/coding.h
commit 2cbcc471d6340fac1ceca9b9559f62ec2d71a769 we inherit the following 

```
Copyright 2015 The TensorFlow Authors. All Rights Reserved.
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
    http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
