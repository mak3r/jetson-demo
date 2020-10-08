# jetson-demo
Various demos of kubernetes with the NVIDIA jetson family of SBC

## CSI Camera Support for JetPack 4.3
* https://forums.developer.nvidia.com/t/jetpack-4-3-csi-camera-support-from-within-containers/111125
    ```
    Thanks jimwormold, can you try launching Docker with these flags?

    sudo docker run --net=host --runtime nvidia --rm --ipc=host -v /tmp/.X11-unix/:/tmp/.X11-unix/ -v /tmp/argus_socket:/tmp/argus_socket --cap-add SYS_PTRACE -e DISPLAY=$DISPLAY -it nvcr.io/nvidia/l4t-base:r32.2.1
    Note the –ipc=host and -v /tmp/argus_socket:/tmp/argus_socket arguments.	
    ```

## Configure sources and sinks and other deepstream settings
* https://docs.nvidia.com/metropolis/deepstream/dev-guide/index.html#page/DeepStream_Development_Guide/deepstream_app_config.3.2.html#
* [sample configs](./nano/config)

## Patch the deepstream src app to enable camera rotation
* [forum solution](https://forums.developer.nvidia.com/t/jetson-nano-csi-raspberry-pi-camera-v2-upside-down-video-when-run-an-example-with-deepstream-app/82077/6) 
* [patch for deepstream_source_bin.c](./nano/deepstream/patches/deepstream_source_bin.patch)
* [patch for Makefile](./nano/deepstream/patches/Makefile.patch)

## Docker standalone
* We may not need the DISPLAY env variable since the sink is rtsp
    ```
    docker run -it --rm --net=host --ipc=host -v /tmp/argus_socket:/tmp/argus_socket --runtime nvidia  -e DISPLAY=$DISPLAY -w /opt/nvidia/deepstream/deepstream-5.0 -v /home/mark/dev/config:/root -v /tmp/.X11-unix/:/tmp/.X11-unix nvcr.io/nvidia/deepstream-l4t:5.0-20.07-samples
    ```
* Inside the container
    ```
    cp /root/source5_sink-rtsp.txt /opt/nvidia/deepstream/deepstream-5.0/samples/configs/deepstream-app/source5_sink-rtsp.txt
    deepstream-app -c  /opt/nvidia/deepstream/deepstream-5.0/samples/configs/deepstream-app/source5_sink-rtsp.txt
    ``` 
