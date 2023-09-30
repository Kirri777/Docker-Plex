# Docker + Kitana + Plex server + Trakt plugin + Nvidia device + Port Forward

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

## Installation Plex

1. Download folder.

2. Create the file `./.docker/.env` using `./.docker/.env.example` as template.<br />
    `PLEX_CLAIM` - Optionally you can obtain a claim token from https://plex.tv/claim and input here. Keep in mind that the claim tokens expire within 4 minutes.

3. If you don't use nvidia devices, you need delete `runtime: nvidia` and `NVIDIA_VISIBLE_DEVICES=all` line in `.docker/docker-compose.yml`.

4. Change `../../../pliki/plex:/data` to your path with files for plex.

5. Go inside folder `./docker` and run `docker-compose up -d --build` to start containers.

6. To get acces from other devices, you need change Settings / Network / List of IP addresses and networks that are allowed without auth to `192.168.1.1/255.255.255.0`

    ![238138263-674aea0d-1a2b-425b-83f5-cc3dd56dc8f1](https://github.com/Kirri777/Docker-Plex/assets/32429348/218e0871-a2c7-4f4f-a067-7fae47389e6d)



<br />

## Installation Plugins

New plugins you need insert to `/app/config/Library/Application Support/Plex Media Server/Plug-ins` folder.

<br />

## Configuration Trakt plugin

Configure the plugin by clicking on the plugin settings button at Plex/Web -> Channels:

Use this to help set up http://trakt-for-plex.github.io/configuration/#/connect or Kitana page

![238136918-40396799-8347-4226-908a-3e025c64ccad](https://github.com/Kirri777/Docker-Plex/assets/32429348/e0b6337d-d40d-4c7c-aaf2-43867a55de24)

![238136829-78cf427d-1d93-4deb-a635-fea2c806b42c](https://github.com/Kirri777/Docker-Plex/assets/32429348/8ca34b9b-576e-46b9-ba42-e5a1f35ca351)

To add more tract account for home user, On the "Plex" authentication tab you need change to the "Home" mode:

![238137030-fbabe9c5-6d59-4d47-8dc5-f3ffa34472f4](https://github.com/Kirri777/Docker-Plex/assets/32429348/73945210-67bb-4c04-ba57-9104fb725ebc)

If new Plex Home User have updated you tract, you can exclude him in Trak module settings (Plex / Modules / Trakt.tv)

![238138016-d1690dfd-b51a-47f8-81ec-f0848ecc701a](https://github.com/Kirri777/Docker-Plex/assets/32429348/de136607-a32e-43dc-a48a-3cd543c08f80)

<br />

## Port Forward

https://portforward.com/router.htm - Guide for every one router

https://canyouseeme.org/ - Check if your port is open

### TP-Link Archer 6

You need add Virtual server for plex (port need to be the same like in plex settings / remote)

![238137257-f62dcd18-f943-453c-9ceb-8fc28d95e878](https://github.com/Kirri777/Docker-Plex/assets/32429348/5b1cf42f-7966-4793-a124-65dde8e4b5ee)

### Orange FunBox

1. First you need set static ip address for router:

   ![238137448-078c630a-9166-4afe-97e9-c77c48a377ca](https://github.com/Kirri777/Docker-Plex/assets/32429348/87fe4b65-7cec-421a-8e9a-2fe3188b6775)
    
3. Nex you need add port forward on NAT/PAT tab:

   ![238137705-e6893d3e-8545-444d-a86f-b796fbd5485c](https://github.com/Kirri777/Docker-Plex/assets/32429348/1992aebb-9b6f-48a1-ae35-9b77269fc716)


<br />

## Plex Media Scanner via Command Line

https://support.plex.tv/articles/201242707-plex-media-scanner-via-command-line/

```
Plex Media Scanner (c) 2010-2014 Plex Development Team.

  -h, --help           Display this message.
  -v, --verbose        Show more output.
  -p, --progress       Show special progress output.
  --log-file-suffix    Specify suffix for log file.

 Actions:

  -r, --refresh        Refresh the metadata.
  -a, --analyze        Analyze media information.
  --analyze-deeply     Fully read and perform deep media analysis.
  -b, --index          Generate a media index file. (Video Preview Thumbnails)
  -i, --info           Get information.
  -l, --list           List.
  -g, --generate       Regenerate thumbnails/fanart.
  -t, --tree           Show a section tree.
  -w, --reset          Delete all media out of a section.
  -n, --add-section  --type <type:1,2,8> --agent  --location  --lang  Add a new section.
  -D, --del-section    Delete a section.

 Items to which actions apply:

  -c, --section        A library section ID.
  -o, --item           An item ID.
  -d, --directory      A directory path.
  -f, --file           A file.

 Modifiers to actions:

  -x, --force          Force an operation (e.g. refresh).
  --no-thumbs          Do not regenerate thumbs when analyzing.
  --chapter-thumbs-only   Only generate chapter thumbnails during generate pass
  --thumbOffset  Percent offset into video for thumbnail image generated during media analysis.
  --artOffset    Percent offset into video for fanart image generated during media analysis.
  ```

### Regerate all `Video Preview Thumbnails` for one library

1. Check id of library section:

    ```
    "/usr/lib/plexmediaserver/Plex Media Scanner" --list
    ```

    This returns a list of Libraries and their ID:

    ```
    "/usr/lib/plexmediaserver/Plex Media Scanner" --list
    29: Movies
    31: Music
    30: TV Shows
   ```

2. Run command:

    ```
    "/usr/lib/plexmediaserver/Plex Media Scanner" -c 1 -b
    ```

    Then you can check if plex started generate new thumbails on web panel.
