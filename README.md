# device_samsung_a60q

[中文](./README_CN.md) / [English](./README.md)

20230808: Copied some stuff from A70, now `adb` command can be used in twrp! The main purpose is to push and pull files. Decryption has not been resolved yet. Currently, it can only be used to build twrp-9.0 (apparently because twrp-9.0 is the last version that can be used with non-dynamic partitions).

The research record from the branch "from-a70q" can be found at https://github.com/Jason-summer/twrp_samsung_a60q. Please raise any issues in **issues**.

## Flashing Twrp

Put the recovery.img into Odin's AP slot and connect the phone to flash it. In a Linux environment, you can also use the command of the odin program:

```
./odin4 -a recovery.img
```

## Building it yourself

1. Sync the source code

After downloading and expanding, at least 28GB of disk space is required.

```
repo init --depth=1 -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni.git -b twrp-9.0
repo sync
```

1. Download and place the device tree

```
git clone https://github.com/Jason-summer/device_samsung_a60q.git -b twrp-9.0
```

Place all the downloaded files in the `<source-dir>/samsung/a60q` directory! `<source-dir>` refers to the directory where the source code was downloaded.

2. Configure the Python environment

twrp-9.0 was born before python3 came out, so it uses python2. Here I use conda to configure the Python environment, you can configure it in your preferred way.

```
conda create -n py27 python=2.7
conda activate py27
```

3. Build

Make sure that the computer's memory + virtual memory > 16G before building, otherwise the build may fail. Since Samusng A6060 has a recovery partition, what we want to build is `recoveryimage`. It's okay to have warnings during the build process as long as there are no red errors.

```
cd <source-dir>
export ALLOW_MISSING_DEPENDENCIES=true
. build/envsetup.sh
lunch omni_a60q-eng
mka recoveryimage
```

4. Obtain the generated files

When the bash displays something similar to the following content, it means that the build is completed.

```
[100% 12881/12881] ----- Making recovery image ------
#### build completed successfully (03:10 (mm:ss)) ####
```

At this point, you can find the generated `recovery.img` in the `<source-dir>/out/target/product/a60q` directory.
