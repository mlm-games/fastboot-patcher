## `fastboot-patcher`
> This repository is basically an updated version of a script which is also an updated version of [Johx22/Patch-Recovery](https://github.com/Johx22/Patch-recovery)  
> Which is then based off of this script: [phhusson/samsung-galaxy-a51-gsi-boot](https://github.com/phhusson/samsung-galaxy-a51-gsi-boot)  
> the scripts have been revamped, etc.  
> All credit still goes to them.

## Instructions for patching!
#### In github (using Github Actions)
> Note: to use github actions, you need to fork this repository first, and also if using any other filehost than transfer.sh, please check if you can download the image using `wget` - do not open an issue if you haven't checked the link and made sure your image was not corrupted yourself (you can use the workflow logs to see that)

Upload recovery.img or recovery.img.lz4 to a file host website [like this](https://transfer.sh),  
go over to the `Actions` tab on your fork, select `Patch Image via URL` and click `Run Workflow`.  
It should open up a little window that allows you to enter a link,  
paste the file host link for your recovery image there
`(e.g. https://transfer.sh/xxxxxx/recovery.img)` -  
then click `Run Workflow` again to start the process.  

When it's done, you should have 2 files available to download - 1 being the keys, of which signifigance I'm not sure myself,  
and the other will hold your patched recovery image, and a version of it that you can use for odin.

#### Natively (Arch/Debian)
> Note: you're going to need the packages *(git, wget, lz4, tar, openssl, & python3)* available in your terminal,  
> and *recovery.img.lz4 or recovery.img.* Personally, I unzipped the AP archive of a stock rom and got my recovery image from that.
> Also, patcher-minimal has a bunch of workarounds for the workflow, so I'm not sure how good of an idea it'd be to run that instead of patcher locally.

If you have those, then clone the repository.
```bash
git clone https://github.com/mlm-games/fastboot-patcher.git && cd fastboot-patcher
```
Move the recovery image to the folder where the 'patcher' script is present (or to the fastboot-patcher folder at which the terminal is pointing at).

Then, simply run the script with `./patcher` and wait for it to finish, it shouldn't take longer than a minute.  
You'll find the original recovery image and 2 new images *(.img and .tar.md5 for odin)* in the top directory if successful.


To verify the integrity of the image using avbtool, you can add the following command after the `add_hash_footer` step:

```bash
python3 "$csd/avbtool" verify_image --image "$csd/output.img" --key "$csd/keys/phh.pem"
```

This command will verify that the image has been properly signed and that its integrity is intact according to the AVB (Android Verified Boot) standards.



# Fastboot Patcher (kinda noob friendly)

This script patches recovery images to enable fastboot functionality on certain Android devices.

## Prerequisites

- Bash shell
- Python 3
- OpenSSL
- LZ4 (for compressed images)
- Magiskboot (included)
- AVBTool (included)

## Usage

1. Place your `recovery.img` or `recovery.img.lz4` in the same directory as the script.
2. Run the script:
   ```
   ./patcher.sh
   ```
3. The script will produce two files:
   - `output.img`: The patched recovery image
   - `output.tar`: Packaged image for use with Odin

## Verification

After patching, the script will automatically verify the integrity of the image. If you want to manually verify, use:

```
python3 avbtool verify_image --image output.img --key keys/phh.pem
```

## Troubleshooting

- If fastboot commands fail after flashing the patched image, try re-running the script and pay attention to any warnings or errors.
- Ensure you're using the correct version of the script for your device and Android version.
- If issues persist, check the device-specific forums for any additional steps or modifications needed.
