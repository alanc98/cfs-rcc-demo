Support to run the RKI image on the renode.io simulator.

To run the LEON3 model in renode you just need to install renode.io.
Follow the instructions here:
https://github.com/renode/renode#installation

In this directory:
- leon3_rki.resc - the renode script to load and start the rtems kernel image on the leon3 model.
- prom.bin - a minimal leon3 prom that jumps to the kernel in RAM

What you need:
- An installation of renode
- The rki.elf file compiled from the rki2 project in this repository. It has to be built for the leon3 and not the leon4

Once you have renode installed, you can open a shell in this directory, run renode, and in the renode console, type:

```
(monitor) s @leon3_rki.resc
```

This will load, open a console window, and start the rki.elf binary.
 
