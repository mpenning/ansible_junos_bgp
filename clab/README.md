This directory contains a [containerlab](https://github.com/srl-labs/containerlab) topology file to run `vJunos-switch` images and host them at 172.20.20.2 and  172.20.20.3

Before running `containerlab`, you need to dockerize the Junos images with [hellt/vrnetlab](https://github.com/hellt/vrnetlab).

The topology file assumes that the Junos images were tagged a certain way; you might need to adjust the image name or tags in the topology file before running it.
