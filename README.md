To reproduce the issue run `pants export --resolve=requirements`. The following error message is returned:

```
21:03:42.56 [INFO] Canceled: Build pex for resolve `requirements`
21:03:50.20 [INFO] Completed: Build pex for resolve `requirements`
21:03:50.60 [INFO] Completed: Get interpreter version
21:03:50.60 [ERROR] 1 Exception encountered:

Engine traceback:
  in `export` goal

ProcessExecutionFailure: Process 'Get interpreter version' failed with exit code 1.
stdout:

stderr:
Failed to find a compatible PEX_PYTHON=/home/user/.pyenv/versions/3.11.3/bin/python3.11.

Examined the following interpreters:
1.) /home/user/.pyenv/versions/3.11.3/bin/python3.11 CPython==3.11.3

No interpreter compatible with the requested constraints was found:

  Failed to resolve requirements from PEX environment @ /home/user/.cache/pants/named_caches/pex_root/unzipped_pexes/4e612df67d342635c3353f40d3fd4a6410d1f832.
  Needed cp311-cp311-manylinux_2_35_x86_64 compatible dependencies for:
   1: nvidia-cuda-nvrtc-cu12==12.1.105; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-cuda-nvrtc-cu12', normalized='nvidia-cuda-nvrtc-cu12') distributions.
   2: nvidia-cuda-runtime-cu12==12.1.105; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-cuda-runtime-cu12', normalized='nvidia-cuda-runtime-cu12') distributions.
   3: nvidia-cuda-cupti-cu12==12.1.105; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-cuda-cupti-cu12', normalized='nvidia-cuda-cupti-cu12') distributions.
   4: nvidia-cudnn-cu12==8.9.2.26; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-cudnn-cu12', normalized='nvidia-cudnn-cu12') distributions.
   5: nvidia-cublas-cu12==12.1.3.1; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-cublas-cu12', normalized='nvidia-cublas-cu12') distributions.
   6: nvidia-cufft-cu12==11.0.2.54; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-cufft-cu12', normalized='nvidia-cufft-cu12') distributions.
   7: nvidia-curand-cu12==10.3.2.106; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-curand-cu12', normalized='nvidia-curand-cu12') distributions.
   8: nvidia-cusolver-cu12==11.4.5.107; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-cusolver-cu12', normalized='nvidia-cusolver-cu12') distributions.
   9: nvidia-cusparse-cu12==12.1.0.106; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-cusparse-cu12', normalized='nvidia-cusparse-cu12') distributions.
   10: nvidia-nccl-cu12==2.18.1; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-nccl-cu12', normalized='nvidia-nccl-cu12') distributions.
   11: nvidia-nvtx-cu12==12.1.105; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='nvidia-nvtx-cu12', normalized='nvidia-nvtx-cu12') distributions.
   12: triton==2.1.0; platform_system == "Linux" and platform_machine == "x86_64"
      Required by:
        torch 2.1.0
      But this pex had no ProjectName(raw='triton', normalized='triton') distributions.
```

As an attempt to resolve this I have also edited `pyproject.toml` to include the following:

```toml
[tool.poetry]
name = "pytorch-issue-repr"
version = "0.1.0"
description = ""
authors = []

[tool.poetry.dependencies]
python = "==3.11.3"
torch = "^2.1.0"
nvidia-cuda-nvrtc-cu12 = "12.1.105"
nvidia-cuda-runtime-cu12 = "12.1.105"
nvidia-cuda-cupti-cu12 = "12.1.105"
nvidia-cudnn-cu12 = "8.9.2.26"
nvidia-cublas-cu12 = "12.1.3.1"
nvidia-cufft-cu12 = "11.0.2.54"
nvidia-curand-cu12 = "10.3.2.106"
nvidia-cusolver-cu12 = "11.4.5.107"
nvidia-cusparse-cu12 = "12.1.0.106"
nvidia-nccl-cu12 = "2.18.1"
nvidia-nvtx-cu12 = "12.1.105"
triton = "2.1.0"
```

But this does not resolve the issue either.