#    T2Linux_Suspend_Enhanced_Fix
T2-Suspend-Fix (MacBook Pro 16,1 Optimized)
A Hardware-Aware Suspend/Resume Service for Intel T2 Macs on Fedora

This repository contains a specialized systemd service designed to solve the "Black Screen of Death" and "Touch Bar Hang" issues common on the MacBook Pro 16,1 (2019 i9) running Linux.

Credits & Inspiration:

    This implementation is heavily inspired by deqrock's original T2 suspend logic.

    Refined with the "André Eikmeyer Handshake" protocol for i9-specific BridgeOS stability.

    Optimized for Fedora 40+ and Pipewire audio stacks.

🚀 Key Features

    BridgeOS Stabilization: Implements a calculated 2-second delay to allow the T2 chip to initialize before driver attachment (prevents "Interrupt Storms").

    Module Lifecycle Management: Cleanly unloads and reloads apple-bce and appletbdrm to prevent kernel panics.

    Audio Stack Kickstart: Automatically restarts Pipewire/Wireplumber services on resume to eliminate audio latency and "Device Busy" errors.

    HID/Backlight Recovery: Forces the keyboard backlight and Touch Bar (tiny-dfr) back to an active state after resume.

🧠 Why the 2-Second Delay?

On the i9 16,1 model, the BridgeOS (T2) requires more time than other models to re-initialize its security and audio buffers during the power transition. Shaving this delay below 2 seconds often leads to:

    Stuttering Audio: The CPU is flooded with "Wait" interrupts from the T2 chip.

    UI Lag: The PCI bus rescan happens before the chip is ready, causing a lockup.

    Black Touch Bar: The driver fails to claim the device as it is still "Busy" internally.

📋 Installation

    Create the service file:
    Bash

    sudo nano /etc/systemd/system/t2-suspend.service

    Paste the script content and save (Ctrl+O, Enter, Ctrl+X).

    Enable the service:
    Bash

    sudo systemctl enable t2-suspend.service
    sudo systemctl daemon-reload
