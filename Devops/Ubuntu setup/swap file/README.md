# How to Create Swap File

To see available Memory and swap file size, use this command. You can see there is no swap file.

      free -m

Create a swap file and allocate space. I’ve used 1024 MB but you can take more.

      sudo dd if=/dev/zero of=/mnt/swap.0 bs=1024 count=1048576

Next use this line to mount it.

      sudo mkswap /mnt/swap.0

Switch to root account to activate the swap.

      sudo su

Use this to activate it.

      echo "/mnt/swap.0 swap swap defaults 0 0" >> /etc/fstab

Next use this command.

      swapon /mnt/swap.0

check if it’s created successfully.

      sudo swapon -s

Check again the created main and swap size for memory. The swap file is created successfully.

      free -m