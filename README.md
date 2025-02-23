> [!NOTE]
> This repository is licensed under the [Creative Command by-nc-sa 4.0 license](https://creativecommons.org/licenses/by-nc-sa/4.0/) unless otherwise specified.

# Guide for running a local AI using Podman

This guide will walk you through utilizing containers to run ollama and Open WebUI.

- [The Guide](Guide.md) - The actual guide
- [Extras](Extras.md) - Additional information and configuration

## How to use this guide

This guide is meant to be used to get familiar with and/or test using ollama and Open WebUI as a privacy focused local AI solution.

Additional configuration options and recommendations can be found in the [Extras document](Extras.md).

## What this guide will not cover

This guide will not go into details on what containers are nor go into great details about the command used in this guide. If you'd would like to know more about containers, please refer to the [Podman documentation](https://docs.podman.io/en/latest/).

It will also not go into details on how large language models work nor will it recommend specific models. For a complete list of model, see the [ollama model search page](https://ollama.com/search).

> [!NOTE]
> If you'd would like to learn more about LLMs check out [3Brown1Blue's Neural networks playlist](https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi) or [3Brown1Blue's Large Language Models explained briefly video](https://youtu.be/LPZh9BOjkQs?si=ja14FuDxIJMzExrH).

By default, the ollama container will run the models using the resources it has available which will only be the CPU and system memory. This guide will not be going into details on how to use a GPU nor other AI specific hardware. The [Extras Section](#extras) will include some resources for configuring common consumer hardware.

## What you'll get from this guide

At the end of the guide, you'll have two container running. The first being ollama and the second being Open WebUI. The guide does not configure the containers to automatically start on system reboot. See the [Extras document](Extra.md) for more advanced configuration recommendations for using this stack in a production like environment.

## Why create this guide?

This guide was created to compliment a presentation done at [Hackforge](https://www.hackf.org/) for the Linux User Group. I also created it as a document that can be given to people that are interested in running a local AI but aren't sure where to start.
