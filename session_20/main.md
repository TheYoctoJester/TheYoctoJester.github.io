# Live Coding Session 20 - bare metal

When: Thursday September 2nd, 2021 - 4:00 PM Europe/Berlin

Topic: Instead of a linux distribution, this time we will create a bare metal build and run it in qemu.

## Shownotes

An example recipe for building a baremetal "Hello, World" Application to be run on QEMU is shipped right with poky. It is located in the meta-skeleton layer and supports those architectures:
- qemuarm
- qemuarmv5
- qemuarm64
- qemuriscv32
- qemuriscv64

In order to build, `TCLIBC` must be set to either `baremetal` or `newlib`, we used baremetal. A baremetal application substatially differs from a standard userland application visibly as it brings at least a rudimentary startup routine which is usually written in assembly and a custom linker script that makes sure all important pieces end up in exactly the memory locations that the platform needs.

Usecases for doing this in Yocto might be:
- mitigating development setups that rely on a specific toolchain and configuration to be present, sometimes even conflicting. If you use the Yocto approach, the build process will in itself take care of generating and using the correct tooling.
- in multistage/multiconfig builds, firmware blobs can be generated and piped directly into subsequent (Linux) builds that can put them on coprocessors. This is highly platform specific and requires additional handling, though.

## Recording

[YouTube](https://youtu.be/DQ-TTMUstNw)

## External Resources

[baremetal-helloworld.bb (on master)](https://git.yoctoproject.org/cgit/cgit.cgi/poky/tree/meta-skeleton/recipes-baremetal/baremetal-examples/baremetal-helloworld_git.bb)

[baremetal-helloworld sources](https://github.com/aehs29/baremetal-helloqemu)

### LinkedIn Event

[Yocto Livecoding - bare metal](https://www.linkedin.com/events/yoctolivecoding-baremetal6835840902821380096/)


### Google Calendar

[Yocto Livecoding - bare metal](https://calendar.google.com/event?action=TEMPLATE&tmeid=NnZuZnNrODg2ZjJmdHI3ZWxiZTJiZGozOWogamVzdGVyQHRoZXlvY3RvamVzdGVyLmluZm8&tmsrc=jester%40theyoctojester.info)