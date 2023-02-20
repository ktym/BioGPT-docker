# BioGPT-docker

Docker file for the [BioGPT](https://github.com/microsoft/BioGPT).

## Build

```sh
% git clone https://github.com/ktym/BioGPT-docker.git
% cd BioGPT-docker/docker
% docker image build -t biogpt:1.0 .
% cd ..
```

## Data

Download and extract pre-trained models from [BioGPT](https://github.com/microsoft/BioGPT).

```sh
% cd models
% sh download.sh
% cd ..
```

## Run

Mount and link the directory containing downloaded models as `${APP_DIR}/BioGPT/checkpoints`.
Suppose if you have stored pre-trained models under the ./models directory on the host OS,

```sh
% docker run --rm -it -v $(pwd):/mnt biogpt:1.0 /bin/bash
```

then log in to the guest OS and create a symbolic link from the mounted directory.

```
root:/app/BioGPT# ln -sf /mnt/models checkpoints
```

## GPUs

Confirm your server is equipped with GPUs.

```sh
% nvidia-smi
```

Install NVIDIA Container Toolkit on the host OS.

```sh
% distribution=$(source /etc/os-release; echo $ID$VERSION_ID)
% curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
% curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
% sudo apt update && sudo apt install -y nvidia-container-toolkit
% sudo systemctl restart docker
```

Confirm your installation is successful.

```sh
% nvidia-container-cli info
```

Run the docker container with a GPU option and confirm the host GPUs are available from the guest OS.

```sh
% docker run --gpu --rm -it -v $(pwd):/mnt biogpt:1.0 nvidia-smi
```

Note: make sure the NVIDIA driver versions of the host and guest OSs should be matched.

## Test

Example codes listed in the BioGPT repository are stored in the test directory for ease.

```sh
% docker run --gpu --rm -it -v $(pwd):/mnt biogpt:1.0 nvidia-smi
root:/app/BioGPT# python3.10 /mnt/test/test1.py
root:/app/BioGPT# python3.10 /mnt/test/test2.py
```

## License

The dockerfile and scripts provided in this repository are MIT-licensed.

### Author

Toshiaki Katayama (Database Center for Life Science, Japan)

### Acknowledgements

* Dockerfile is customized from https://zenn.dev/miyatsuki/articles/db220deeb28900
* Docker with host GPUs https://blog.mahoroi.com/posts/2019/12/docker-gpus-nvidia/

