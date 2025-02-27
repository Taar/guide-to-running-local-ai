> [!NOTE]
> This repository is licensed under the [Creative Command by-nc-sa 4.0 license](https://creativecommons.org/licenses/by-nc-sa/4.0/) unless otherwise specified.

# Guide for running a local AI using Podman

This guide will walk you through utilizing containers to run [Ollama](https://ollama.com/) and [Open WebUI](https://github.com/open-webui/open-webui) as a privacy focused local AI solution.

The goal of this guide is to show how easy it is to run a local AI.

- [The Guide](Guide.md) - The actual guide
- [Extras](Extras.md) - Additional information and configuration

## What this guide will not cover

This guide will not go into details on what containers are nor go into great details about the commands used in this guide. If you'd would like to know more about containers or would like to know more about the specified command options used in this guide, refer to the [Podman documentation](https://docs.podman.io/en/latest/).

It will also not go into details on how large language models work nor will it recommend specific models. For a complete list of model, see the [Ollama model search page](https://ollama.com/search).

> [!NOTE]
> If you'd would like to learn more about LLMs check out [3Brown1Blue's Neural networks playlist](https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi) or [3Brown1Blue's Large Language Models explained briefly video](https://youtu.be/LPZh9BOjkQs?si=ja14FuDxIJMzExrH).

By default, the Ollama container will run the models using the resources it has available which will only be the CPU and system memory. This guide will not be going into details on how to use a GPU nor other AI specific hardware.

## What you'll get from this guide

At the end of the guide, you'll have two containers running. The first being Ollama which will be used to download and manage models. The second being Open WebUI which will interface with Ollama so that you can run the downloaded models in a more user friendly environment.

## Why create this guide?

This guide was created to compliment a presentation done at [Hackforge](https://www.hackf.org/) for the Linux User Group. I also created it as a document that can be given to people that are interested in running a local AI but aren't sure where to start.
