# Docker + Kitana + Plex server + Trakt plugin + Nvidia device

## Links

http://127.0.0.1:32400/ - Plex

http://127.0.0.1:31337/ - Kitana

<br />

## Installation Nvidia (Debian) - If you have nvidia device

For other OS you can get instruction on https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker

1. Setup the package repository and the GPG key:

    ```
    distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
        && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
        && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
                sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
                sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    ```

2. Install the nvidia-container-toolkit package (and dependencies) after updating the package listing:

    ```
    sudo apt-get update
    ```

    ```
    sudo apt-get install -y nvidia-container-toolkit
    ```

3. Configure the Docker daemon to recognize the NVIDIA Container Runtime:

    ```
    sudo nvidia-ctk runtime configure --runtime=docker
    ```

4. Restart the Docker daemon to complete the installation after setting the default runtime:

    ```
    sudo systemctl restart docker
    ```

5. At this point, a working setup can be tested by running a base CUDA container:

    ```
    sudo docker run --rm --runtime=nvidia --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
    ```

6. This should result in a console output shown below:

    ```
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 450.51.06    Driver Version: 450.51.06    CUDA Version: 11.0     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |===============================+======================+======================|
    |   0  Tesla T4            On   | 00000000:00:1E.0 Off |                    0 |
    | N/A   34C    P8     9W /  70W |      0MiB / 15109MiB |      0%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=============================================================================|
    |  No running processes found                                                 |
    +-----------------------------------------------------------------------------+
    ```

<br />

## Installation

1. Download folder.

2. Create the file `./.docker/.env` using `./.docker/.env.example` as template.<br />
    `PLEX_CLAIM` - Optionally you can obtain a claim token from https://plex.tv/claim and input here. Keep in mind that the claim tokens expire within 4 minutes.

3. If you don't use nvidia devices, you need delete `runtime: nvidia` and `NVIDIA_VISIBLE_DEVICES=all` line in `.docker/docker-compose.yml`.

4. Change `../../../pliki/plex:/data` to your path with files for plex.

5. Go inside folder `./docker` and run `docker-compose up -d --build` to start containers.

<br />

## Installation Plugins

New plugins you need insert to `/app/config/Library/Application Support/Plex Media Server/Plug-ins` folder.

<br />

## Configuration Trakt plugin

Configure the plugin by clicking on the plugin settings button at Plex/Web -> Channels:

Use this to help set up http://trakt-for-plex.github.io/configuration/#/connect or Kitana page