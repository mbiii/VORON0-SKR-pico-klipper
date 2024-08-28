# Steps to Flash BTT SKR Pico v1.0 with Canboot & Klipper in UART mode

## Install CanBoot if you haven't already
- Run the following from your klipper host to copy the canboot github files
```
cd ~/
git clone https://github.com/Arksine/CanBoot
```

- Add Canboot to the moonraker update manager by adding the following lines to your moonraker.cof file.
```
[update_manager CanBoot]
type: git_repo
path: ~/CanBoot
origin: https://github.com/Arksine/CanBoot.git
is_system_service: False
```
## Canboot for SKR Pico in UART mode 

- Update to the latest Canboot and Klipper codabase as of 11-8-2022
  - Canboot v0.0.1-20-g600967e or newer
  - Klipper v0.10.0-623-g5b1a6676 or newer
- SSH to  your Klipper host
- CD to Canboot
  ```
  cd ~/CanBoot/
  ```
  
- Run Make Clean
  ```
  make clean
  ```

- Run Make Menuconfig
  ```
  make menuconfig
  ```

- Settings
    - MCU Raspberry Pi RP2040
    - Build CanBoot deployment 16Kib Bootloader
    - Com interface Serial on UART

![Canboot in UART mode](/img/skr_pico_canboot_uart.png)

- Press Q to quit, and Y to save

- Run make
  ```
  make -j 4
  ``` 
- Connect the Pico by USB to the Klipper Host
- Install boot jumper on the SKR Pico and press the reset button

![SKR Pico boot jumpers](/img/skr-pico-boot-jumpers.png)

- Mount the Pico file system
  ```
  sudo mount /dev/sda1 /mnt
  ```

- Copy Canboot.uf2 to the Pico
  ```
  sudo cp ~/CanBoot/out/canboot.uf2 /mnt
  ``` 

- This will copy the firmware image we compiled earlier onto the SKR-Pico's storage and it will be flashed automatically.

  - This take a minute or so.

- When the /mnt unmounts remove the boot jumpers. 

- Shutdown the host and disconnect USB cable from the Pico.

## Connect SKR Pico over UART to Raspberry Pi

- Connect the UART cable between GPIO headers of the Raspberry Pi and the Raspberry Pi headers on the SKR Pico board.

![SKR Pico UART connection](/img/skr-pico-uart-connection.png)

### Flashing Klpper via UART the first time

- SSH into the Klipper host and CD to klipper
  ```
  cd ~/klipper/
  ```

- Run Make Clean
  ```
  make clean
  ```

- Run Make Menuconfig
  
  ```
  make menuconfig
  ```

- Settings
    - MCU Raspberry Pi RP2040
    - bootloader offset 16Kib Bootloader
    - Com interface Serial on UART


![Klipper UART](/img/skr_pico_klipper_canbus_uart.png)    
  
 - Run make
  ```
  make -j 4
  ``` 

 - Connect the SKR Pico UART pins to the klipper host.
 - Flash the klipper.bin file to the SKR Pico
  ```
  python3 ~/CanBoot/scripts/flash_can.py -f ~/klipper/out/klipper.bin -d  /dev/ttyAMA0
  ```

- Reboot and your Pico should come back up in Serial UART mode.