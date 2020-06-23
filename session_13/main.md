# Live Coding Session #13 - Building a kernel module

When: Tuesday June 23th, 2020 - 5:00 PM Europe/Berlin

Topic: Building an out-of-tree kernel module.

## Notes from session

We have used ${AUTOREV} during the session, for the easy of explaining. This is strongly discouraged in real production code. If you really think you need it, please adhere to [the manual](https://www.yoctoproject.org/docs/3.1/ref-manual/ref-manual.html#var-AUTOREV)

## Recording

[YouTube](https://youtu.be/zYyE-xah7jg)

## External resources

[meta-praseodymium](https://github.com/TheYoctoJester/meta-praseodymium): the layer created during the session

[mod-praseodymium](https://github.com/TheYoctoJester/mod-praseodymium): the module created during the session

[checkpatch.pl](https://github.com/torvalds/linux/blob/master/scripts/checkpatch.pl): the tool you should use to check your code

[printk](https://www.kernel.org/doc/html/latest/core-api/printk-basics.html): in-depth information on how to print stuff from the kernel context