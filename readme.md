# OLKB Planck QMK Keymap for the Anne Pro 2

This keymap is approximately the same as the default keymap for the OLKB Planck
if for some reason you want the 40% experience in a 60% format. I find this
works a bit better than setting up the same keymap in ObinsKit because this
way fn2 can take you to numbers and fn1 can take you to punctuation
without having to hold down the shift key. ObinsKit doesn't make the Anne Pro
fully customizable. QMK does!

## Anne Pro 2 Model

This assumes you have the C18 version of the Anne Pro 2, which you can determine by look at the bottom of your keyboard. If "Anne Pro" is printed inside the circle, then you're good to go. Otherwise, you will need to make some modifications.

## Setup

You can follow the instructions on the [Open Anne Pro Install Instructions](https://openannepro.github.io/install/) page.

Alternatively, here is a quick guide. You will need a second keyboard to flash the firmware:

Install the dependencies including git, rustup, and the build requirements:
```
sudo apt install gcc-arm-none-eabi binutils-arm-none-eabi git build-essential libusb-1.0-0-dev 
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Make a directory to house everything and clone each of the necessary git repos including Anne Pro 2 Tools, the OpenAnnePro fork of the QMK firmware, AnnePro2 Shine, and my config:
```
mkdir github
cd github
git clone https://github.com/OpenAnnePro/AnnePro2-Tools.git
git clone https://github.com/OpenAnnePro/qmk_firmware.git annepro-qmk --recursive --depth 1
git clone https://github.com/amorris28/annepro-qmk-layout.git
git clone https://github.com/OpenAnnePro/annepro2-shine.git --recursive
```

Build Anne Pro 2 Tools:
```
cd AnnePro2-Tools
cargo build --release
cd ..
```

Build the Anne Pro 2 Shine binary:
```
cd annepro2-shine
make C18
cp builds/annepro2-shine-C18.bin ../AnnePro2-Tools/target/release
```

I like to symlink my git directory to the qmk keymap directory so that I can
make edits in my personal git repo and then easily recompile in the qmk
directory. But you could also just create this directory and copy the files to it.
```
symlink -s annepro-qmk-layout annepro-qmk/keyboards/annepro2/keymaps/custom
```

Now, put the Anne Pro 2 into IAP mode by unplugging it, holding down the Esc key, replugging it, and then releasing the escape key. Now, you can simply run the "reflash" script. This will build a binary from the custom keymap, flash the keyboard with the custom binary, flash the keyboard with AnnePro2 Shine, and reboot the keyboard.
```
annepro-qmk-layout/reflash
```

