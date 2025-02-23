# The Guide

The actual guide starts here!

> [!NOTE]
> No specialized hardware is required.

This guide uses [Podman](https://podman.io/) and the terminal/console to get the stack up and running. Some familiarity with using the terminal on your system is required. All the commands commands will work on any system (Linux, Windows, and MacOS).

> [!NOTE]
> If you're using Windows or MacOS, then [Podman Desktop](https://podman-desktop.io/) is required.

A deep understanding of how containers work or how podman works is *not* required but will be helpful when expanding on what you've learned from this guide.

## Ollama

The ollama container image contains the ollama CLI which can be used to download and run models.

The first thing that we'll need to do is create a volume to store our downloaded models.

```bash
podman volume create ollama
```

After create the volume, the container can be created and started.

```bash
podman run -d -v ollama:/root/.ollama --name ollama docker.io/ollama/ollama
```

Now that the container is running, let's verify that we can execute the ollama CLI by displaying the version number.

```bash
podman exec ollama ollama --version
```

Executing that command should output something like below:

```
ollama version is 0.4.3
```

Now that we are able to run the CLI, let's download out first model. We'll be downloading the [smollm2:135m-instruct-q6_K](https://ollama.com/library/smollm2:135m-instruct-q6_K) because it's a very small model which means it needs very little resources to be able to run.

> [!NOTE]
> To download any model using ollama, the model name and tag needs to be provided. The format is, `model_name:tag_name`. Eg. `smollm2:135m-instruct-q6_K` the model name is `smollm2` and the tag is `135m-instruct-q6_K`.

```bash
podman exec ollama ollama pull smollm2:135m-instruct-q6_K
```

Once it down downloading, we can run the model using the command below.

```bash
podman exec -it ollama ollama run smollm2:135m-instruct-q6_K
```

Let's ask the model something!

```
Why is the sky blue?
```

When are done, we can close the chat by typeing in `/bye` or using the keyboard short cut Control + d.

## Running Open WebUI

The ollama CLI lacks a fair bit of quality of life features which is were Open WebUI comes in. It's a web application that can be used as a wrapper around ollama.

First we'll need to stop and remove our current ollama container so that we can make some modifications.

```bash
podman container stop ollama
podman container rm ollama
```

Next we'll need to create a network so that the Open WebUI container and the new ollama container can communicate with each other.

```bash
podman network create ollama
```

We'll use a slightly different command to re-create the ollama container.

```bash
podman run -d --network ollama -v ollama:/root/.ollama -p 11434:11434 --name ollama docker.io/ollama/ollama
```

Before we can start the Open WebUI container, we need to create another volume so that settings and other user data will persist when the container is stopped.

```bash
podman volume create open-webui
```


Now we can start the container. It will take a moment or two to fully initialize.

```bash
podman run -d -p 3000:8080 --network ollama -e OLLAMA_BASE_URL=http://ollama:11434 -v open-webui:/app/backend/data --name open-webui ghcr.io/open-webui/open-webui:main
```

Navigate to `localhost:3000`. If it doesn't load, wait a few more seconds. Once you see the intro page, click on the "Get Started" button to continue.

Create an admin account with a secure password.

Near the top left, there should be a drop down with the label of "Area Model", click on that and select the model we downloaded earlier.

Ask it the same question we asked earlier, "Why is the sky blue?".

For a complete list of [features](https://docs.openwebui.com/features/), [pipelines](https://docs.openwebui.com/pipelines/), [tutorials](https://docs.openwebui.com/category/-tutorials), etc, see the [official Open WebUI documentation](https://docs.openwebui.com/).

