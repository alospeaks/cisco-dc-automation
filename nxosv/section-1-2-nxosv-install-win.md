#NX-OSv in VMWare Player on Windows 7/10

This guide is based on NXOSv on VM Fusion document.
Owners of Windows 7 machines, please familiarize yourself with the above mentioned document first.

##Prerequisites
1. download NX-OSv OVA file from the box link provided (you will only need the .ova file)
2. Download and install VMWare player from here ( download VMWare Player ). It is free to use for non-commercial purposes.
    ![nxos-w](/images/nxosv-w-1.png)
3. Download and install PuTTY: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

###Step 1: Deploy NX-OSv OVA file
VMWare Player is shipped with OVA deployment called ovftool.exe. Locate it in your VMWare player installation folder:
![nxos-w](/images/nxosv-w-2.png)

You will need to launch the command line or powershell and execute command according to your specific file folder path:
![nxos-w](/images/nxosv-w-3.png)

###Step 2: Edit NX-OSv VM settings
Open the newly deployed NX-OSv VM from VMWare player.
![nxos-w](/images/nxosv-w-4.png)  
![nxos-w](/images/nxosv-w-5.png)

Edit virtual machine settings.
![nxos-w](/images/nxosv-w-6.png)

Disconnect CD-ROM:   

![nxos-w](/images/nxosv-w-7.png)

Change Network adapter to NAT:
![nxos-w](/images/nxosv-w-8.png)

Disconnect all other NICs:

![nxos-w](/images/nxosv-w-9.png)

Enable Serial port (console) connectivity to NX-OSv, by adding a "Serial Port" device, and configuring named pipe.

![nxos-w](/images/nxosv-w-10.png)  

![nxos-w](/images/nxosv-w-11.png)

![nxos-w](/images/nxosv-w-12.png)

Please remember how you named the pipe, as this name will be used by PuTTY in next steps.
Press Finish and proceed to next step.

###Step 3. Launch NX-OSv Virtual machine
Press "Play virtual machine" button and monitor the boot process
![nxos-w](/images/nxosv-w-14.png)  
![nxos-w](/images/nxosv-w-15.png)

Once you see "Leaving grub land" in VM console, you should start seeing bootup output in the console line, that is configured in the next step.

###Step 4. Attach to console of NX-OSv VM
You can start executing this step once you have launched the NX-OSv VM. That is, the named pipe configured in step 2 will be active as long as the VM is up and running.
You will need to use PuTTY in order to attach to the NX-OSv virtual machine.  If you don't have putty, you can download it from here https://the.earth.li/~sgtatham/putty/latest/x86/putty.exe

Please select connection type as "Serial" and specify the full pipe name you configured in step 2.
![nxos-w](/images/nxosv-w-16.png)
You can optionally save the settings into "Saved Sessions".
The output in PuTTY should look something like this:
![nxos-w](/images/nxosv-w-17.png)

You can now proceed  to "Initial Switch Configuration" section on NXOSv on VM Fusion guide.
