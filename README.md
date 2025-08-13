Clone this repo first:

```
git clone https://github.com/ant9000/evernode
cd evernode
```

Define `BASE` as current dir and `DEVICE` as the correct serial port:

```
BASE=`pwd`
DEVICE=/dev/ttyUSB0
```

Clone my Micropython repo and switch to `evernode` branch:

```
git clone https://github.com/ant9000/micropython
cd micropython
git checkout evernode
```

Move to Zephyr installation and compile:

```
cd ~/zephyrproject/zephyr
west build -b apollo3_evb \
   -d ~/zephyrproject/evernode-micropython/ \
   $BASE/micropython/ports/zephyr/ \
   -DEXTRA_CONF_FILE=thread.conf \
   -DEXTRA_CONF_FILE=$BASE/evernode.conf \
   -DEXTRA_DTC_OVERLAY_FILE=$BASE/evernode.overlay
```

For further work, just move to the build directory:

```
cd ~/zephyrproject/evernode-micropython/
west flash
```

To copy the radio driver to the Evernode:
```
$BASE/micropython/tools/pyboard.py --device $DEVICE -f cp $BASE/sx1262.py :/flash/sx1262.py
```

To launch the radio test script on the Evernode:
```
$BASE/micropython/tools/pyboard.py --device $DEVICE $BASE/test_radio.py
```

If you get the error `ModuleNotFoundError: No module named 'serial'`, then install pyserial with

```
sudo apt install python3-pyserial
```

For a complete Micropython IDE, you can install Thonny:

```
sudo apt install pipx
pipx install thonny
```

and then simply launch it as

```
thonny
```
