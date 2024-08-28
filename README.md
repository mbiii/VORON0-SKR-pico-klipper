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
