# Zephyr Example Application
# STM32_LCD_display_Zephyr_app
ESP32 ZephyrOS App: 3.2 inch TFT LCD display


## Getting Started

Before getting started, make sure you have a proper Zephyr development
environment. Follow the official
[Zephyr Getting Started Guide](https://docs.zephyrproject.org/latest/getting_started/index.html).

### Setup Zephyr Template/Example app

The first step is to initialize the workspace folder (``zephyr_workspace``) where
the ``example-application`` and all Zephyr modules will be cloned. Run the following
command:

```shell
# initialize `zephyr_workspace` workspace (not repo) for the example-application (main branch)
cd ~/MatasPC/coding/
mkdir zephyr_workspace
cd zephyr_workspace
# initialize `stm32_lcd_repo` git repo within the workspace, which contains the example-application
git clone https://github.com/zephyrproject-rtos/example-application stm32_lcd_repo
west init -l stm32_lcd_repo
west update
```

### Git Setup

```shell
git init
git remote add origin git@github.com:matassabaliauskas/STM32_LCD_display_Zephyr_app.git
git status
```

### Environment Setup

The first step is to create a Python virtual environment (venv). 
Run the following command:

```shell
# Create venv
python3 -m venv ~/MatasPC/coding/zephyr_workspace/.venv
source ~/MatasPC/coding/zephyr_workspace/.venv/bin/activate
deactivate
```

Verify:
```shell
matas@matas-lenovo-15ACH6:~/MatasPC/coding/zephyr_workspace$ which python
/home/matas/MatasPC/coding/zephyr_workspace/.venv/bin/python
```

Setup direnv to automate this:
```shell
nano .envrc
```

Contents of .envrc:
```shell
# ~/MatasPC/coding/zephyr_workspace
source ~/MatasPC/coding/zephyrproject/.venv/bin/activate
source ~/MatasPC/coding/zephyrproject/zephyr/zephyr-env.sh
```
and then:
```shell
direnv allow
```

Optional step if facing an issue with west:
```shell
# ...
pip install -r zephyr/scripts/requirements.txt
```
Because Zephyr docs assume you followed this exact flow:
```shell
west init
west update
west zephyr-export
pip install -r zephyr/scripts/requirements.txt
```


### Building and running

To build the application, run the following command:

```shell
cd stm32_lcd_repo/
west build -p always -b black_f407ve app
```

[west build -b $BOARD app]
where `$BOARD` is the target board.

You can use the `custom_plank` board found in this
repository. Note that Zephyr sample boards may be used if an
appropriate overlay is provided (see `app/boards`).

A sample debug configuration is also provided. To apply it, run the following
command:

```shell
west build -b $BOARD app -- -DEXTRA_CONF_FILE=debug.conf
```

Once you have built the application, run the following command to flash it:

```shell
west flash
```

### Testing

To execute Twister integration tests, run the following command:

```shell
west twister -T tests --integration
```

### Documentation

A minimal documentation setup is provided for Doxygen and Sphinx. To build the
documentation first change to the ``doc`` folder:

```shell
cd doc
```

Before continuing, check if you have Doxygen installed. It is recommended to
use the same Doxygen version used in [CI](.github/workflows/docs.yml). To
install Sphinx, make sure you have a Python installation in place and run:

```shell
pip install -r requirements.txt
```

API documentation (Doxygen) can be built using the following command:

```shell
doxygen
```

The output will be stored in the ``_build_doxygen`` folder. Similarly, the
Sphinx documentation (HTML) can be built using the following command:

```shell
make html
```

The output will be stored in the ``_build_sphinx`` folder. You may check for
other output formats other than HTML by running ``make help``.
