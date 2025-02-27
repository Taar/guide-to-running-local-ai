# Extras

This section isn't part of the guide but rather contains additional information that I felt would be helpful to include.

## Hardware Considerations

Looking at the [smollm2 model tags](https://ollama.com/library/smollm2/tags), we can see that there are three available parameter sizes: 135M, 360M, and 1.7B. To be able to run these models (using Ollama), the model need to be able to fit into memory. The parameter sizes are *not always* the true size of the model. For example, the [`1.7B` tag](https://ollama.com/library/smollm2:1.7b) is about `~1.8 GB` while the [`1.7b-instruct-fp16` tag](https://ollama.com/library/smollm2:1.7b-instruct-fp16) is about `~3.4GB`. Therefore, to be able to run the `smollm2:1.7b-instruct-fp16` model, there would need to be a *minimum* of `3.4GB` of memory available. That's the minimum because it does not account for the prompt or any other information provided.

If you would like to use a GPU, then the same principals apply. The model, prompt, context, etc needs to be able to video into the GPU's VRAM. If it doesn't fit, ollama will partially use the CPU and system memory but at a performance cost.

This also means that running multiple models at the same time is possible but only if the required resources are available.

The ollama CLI `ps` command will show the exact resources the model is using. Below is an example output:

```
NAME                          ID              SIZE      PROCESSOR    UNTIL
smollm2:135m-instruct-q6_K    6e8e767c0cee    507 MB    100% CPU     4 minutes from now
```

The `SIZE` is the amount of memory the model is using and the `PROCESSOR` is where the model will run. This the case above, it will run completely on the CPU.


### Using a Nvidia GPU with Rootless Podman

In order for the Ollama container to use a GPU, it needs to be given assess to the GPU using additional command line flags or configuration.

> [!WARNING]
> The next commands will only work on a Linux based system. I don't have a Windows nor a Mac machine to test these with.

First the [Nvidia container toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installation) will need to be installed on the system. And the following change needs to be made to it's configuration file:

In the following file, `/etc/nvidia-container-runtime/config.toml` under the `[nvidia-container-cli]` section change the following:

```toml
# Change
no-cgroups = false
# to
no-cgroups = true
```

A system reboot is required for the changes to be applied.

The following command can be used to start Ollama with access to all the GPUs installed in the system.

```bash
podman run -d --network ollama --device nvidia.com/gpu=all --security-opt=label=disable --hooks-dir=/usr/share/containers/oci/hooks.d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

If the command fails to because it is not able to find the GPU CDI device, the CDI file needs to be updated.

> [!WARNING]
> Run the following commands at your own risk!
> Creating a backup is always a good idea!

```bash
# Need to be root in order to run these commands
sudo su
# Lists the GPUs installed on the system
nvidia-ctk list
# Backup the CDI file
cp /etc/cdi/nvidia.yaml /etc/cdi/nvidia.yaml.bck
# This command will update the CDI, a reboot will be required
nvidia-ctk cdi generate > /etc/cdi/nvidia.yaml
```

After a system reboot, the container *should* run without issue. If not then something else is wrong.

## Show running models

Being able to show the running models and the resource they are using.

```bash
podman exec ollama ollama ps
```

```
NAME                          ID              SIZE      PROCESSOR    UNTIL
smollm2:135m-instruct-q6_K    6e8e767c0cee    507 MB    100% CPU     4 minutes from now
```

## Stopping a running model

If you need to stop a model because it's using too much resources and/or taking to long to respond, then you can use the `stop` command to stop the model.

```bash
podman exec ollama ollama stop smollm2:135m-instruct-q6_K
```
