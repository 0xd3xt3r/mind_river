---
tags:
  - qpsi/project
status: todo
created-date: 2025-08-01
completed-date: 2025-08-01
---

## Objective


## Code Setup

- [Android patch repos](https://github.qualcomm.com/rmalmain/qandroid)

## Build Instruction

```bash
https://github.qualcomm.com/rmalmain/qandroid

# download some bin and put in this directory
scripts/bin/<package>

./scripts/build.sh

#issue 1
sudo usermod -aG kvm,cvdnetwork,render $USER

# issue 2 - ubuntu bug
sudo chown -R _apt:root /var/lib/apt/lists
```

#### Bazel config

```bash
# copy this in ~/.bazelrc file
# This config will generate the bazel cache data on the specified directory
build --repository_cache=/local/mnt/workspace/qcom-dev/qandroid/bazel_cache
startup --output_base=/local/mnt/workspace/qcom-dev/qandroid/bazel_cache
```

## Notes

1. Running on kvm supported ARM server can significantly improve the speed of the execution.
