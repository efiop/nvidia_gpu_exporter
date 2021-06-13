# nvidia_gpu_exporter

[![build](https://github.com/utkuozdemir/nvidia_gpu_exporter/actions/workflows/build.yml/badge.svg)](https://github.com/utkuozdemir/nvidia_gpu_exporter/actions/workflows/build.yml)
[![Coverage Status](https://coveralls.io/repos/github/utkuozdemir/nvidia_gpu_exporter/badge.svg?branch=master)](https://coveralls.io/github/utkuozdemir/nvidia_gpu_exporter?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/utkuozdemir/nvidia_gpu_exporter)](https://goreportcard.com/report/github.com/utkuozdemir/nvidia_gpu_exporter)
![Latest GitHub release](https://img.shields.io/github/release/utkuozdemir/nvidia_gpu_exporter.svg)
[![GitHub license](https://img.shields.io/github/license/utkuozdemir/nvidia_gpu_exporter)](https://github.com/utkuozdemir/nvidia_gpu_exporter/blob/master/LICENSE)
![GitHub all releases](https://img.shields.io/github/downloads/utkuozdemir/nvidia_gpu_exporter/total)
![Docker Pulls](https://img.shields.io/docker/pulls/utkuozdemir/nvidia_gpu_exporter)

This is yet another an Nvidia GPU exporter for prometheus, using `nvidia-smi` binary to gather metrics.

This approach has the following advantages:
- It will work on any system that has `nvidia-smi(.exe)?` binary - be it Windows or Linux. No C bindings required
- No need for a Kubernetes environment or even a container, unlike DCGM
- It can even be customized to run nvidia-smi over SSH using command-line arguments

This project is based on the project [a0s/nvidia-smi-exporter](https://github.com/a0s/nvidia-smi-exporter).
However, this one is written in Go to produce a single, static binary.  

This makes it possible to run it on Windows and get GPU metrics while gaming - no Docker or Linux required.

## Installation

### Installing as a Windows Service

Requirements:
- [Scoop package manager](https://scoop.sh)
- [NSSM](https://nssm.cc/download) (get the latest pre-release version)

Installation steps:
1. Open a privileged powershell prompt (right click - Run as administrator)
2. Run the following commands:

```powershell
scoop bucket add nvidia_gpu_exporter https://github.com/utkuozdemir/scoop_nvidia_gpu_exporter.git
scoop install nvidia_gpu_exporter/nvidia_gpu_exporter --global
New-NetFirewallRule -DisplayName "Nvidia GPU Exporter" -Direction Inbound -Action Allow -Protocol TCP -LocalPort 9835
nssm install nvidia_gpu_exporter nvidia_gpu_exporter "C:\ProgramData\scoop\apps\nvidia_gpu_exporter\current\nvidia_gpu_exporter.exe"
Start-Service nvidia_gpu_exporter
```

### By downloading the binaries (MacOS/Linux/Windows)

1. Go to the [releases](https://github.com/utkuozdemir/nvidia_gpu_exporter/releases) and download
   the latest release archive for your platform.
2. Extract the archive.
3. Move the binary to somewhere in your `PATH`.

Sample steps for Linux 64-bit:
```bash
$ VERSION=0.1.2
$ wget https://github.com/utkuozdemir/nvidia_gpu_exporter/releases/download/v${VERSION}/nvidia_gpu_exporter_${VERSION}_linux_x86_64.tar.gz
$ tar -xvzf nvidia_gpu_exporter_${VERSION}_linux_x86_64.tar.gz
$ mv nvidia_gpu_exporter /usr/local/bin
$ nvidia_gpu_exporter --help
```

## Usage

The usage of the binary is the following:

```
usage: nvidia_gpu_exporter [<flags>]

Flags:
  -h, --help                Show context-sensitive help (also try --help-long and --help-man).
      --web.config.file=""  [EXPERIMENTAL] Path to configuration file that can enable TLS or authentication.
      --web.listen-address=":9835"
                            Address to listen on for web interface and telemetry.
      --web.telemetry-path="/metrics"
                            Path under which to expose metrics.
      --nvidia-smi-command="nvidia-smi"
                            Path or command to be used for the nvidia-smi executable
      --query-field-names="AUTO"
                            Comma-separated list of the query fields. You can find out possible fields by running `nvidia-smi --help-query-gpus`. The value `AUTO` will
                            automatically detect the fields to query.
      --log.level=info      Only log messages with the given severity or above. One of: [debug, info, warn, error]
      --log.format=logfmt   Output format of log messages. One of: [logfmt, json]
      --version             Show application version.
```
