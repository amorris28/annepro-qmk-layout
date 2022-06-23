# OLKB Planck QMK Keymap for the Anne Pro 2

This keymap is approximately the same as the default keymap for the OLKB Planck
if for some reason you want the 40% experience in a 60% format. I find this
works a bit better than setting up the same keymap in ObinsKit because this
way fn2 can take you to numbers and fn1 can take you to punctuation
without having to hold down the shift key. ObinsKit doesn't make the Anne Pro
fully customizable. QMK does!

## Setup

You can follow the instructions on the [Open Anne Pro Install Instructions](https://openannepro.github.io/install/) page.

Alternatively, here is a quick guide. You will need a second keyboard to flash the firmware:

Install the dependencies:
```
sudo apt install gcc-arm-none-eabi binutils-arm-none-eabi git build-essential libusb-1.0-0-dev 
```

Install rustup:
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Make a directory to house everything:
```
mkdir github
cd github
```

Clone the AnnePro2-Tools git repo:
```
git clone https://github.com/OpenAnnePro/AnnePro2-Tools.git
```

Change directory into the Anne Pro 2 Tools directory and build the tool:
```
cd AnnePro2-Tools
cargo build --release
cd ..
```

Clone the OpenAnnePro fork of the QMK firmware:
```
git clone https://github.com/OpenAnnePro/qmk_firmware.git annepro-qmk --recursive --depth 1
```

Clone this repo into a directory:
```
git clone
```

I like to symlink my git directory to the qmk keymap directory so that I can
make edits in my personal git repo and then easily recompile in the qmk
directory. But you could also just copy it.
```
symlink -s annepro-qmk-layout annepro-qmk/keyboards/annepro2/keymaps/custom
```

Change directory into the annepro-qmk directory and compile the custom layout:
```
cd annepro-qmk
make annepro/c18:custom
```
Use "c18" if your keyboard has "Anne Pro" printed in the circle on the bottom of the keyboard. Otherwise, use "c15".

Copy the compiled binary into the AnnePro2-Tools directory and change to that directory:
```
cp annepro2_c18_custom.bin ../AnnePro2-Tools/target/release
cd ../AnnePro2-Tools/target/release
```

Now, use an on-screen keyboard or a second physical keyboard.
Unplug the Anne Pro 2, hold down the Esc key, plug in the cable, then release the Esc key.
Then, run this command:
```
./annepro2_tools annepro2_c18_custom.bin
```

It should run without failure. Then, you can install AnnePro2 Shine to get the LEDs working.
First, return to our github directory and clone the repo:
```
cd ~/github
git clone https://github.com/OpenAnnePro/annepro2-shine.git --recursive
```

Change directory to the annepro2-shine directory and compile the firmware:
```
make C18
```

Do the same procedure to unplug, hold Esc, and replug. Alternatively, just keep it in IAP mode from the first time and then you can flash the firmware:
```
annepro2_tools --boot -t led build/annepro2-shine-C18.bin
```

After that, you should be good to go!

