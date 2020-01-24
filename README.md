# qnap-ubuntu-server
Step-by-step guide how to create usable Ubuntu server base image for QNAP Virtualization Station

Install VM helper tools (qemu-guest-agent) to able host to communicate with guest VM.
"The QEMU guest agent is a daemon that runs on the virtual machine. The agent passes network information on the virtual machine, notably the IP address of additional networks, to the host."

For more information, see https://wiki.qemu.org/Features/GuestAgent

$ sudo apt install qemu-guest-agent
