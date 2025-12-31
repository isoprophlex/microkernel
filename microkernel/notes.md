# Notes

The --all-targets flag is completely unrelated to the --target argument. There are two different meanings of the term “target” in cargo:

The --target flag specifies the compilation target that should be passed to the rustc compiler. This should be set to the target triple of the machine that should run our code.
The --all-targets flag references the package target of Cargo. Cargo packages can be a library and binary at the same time, so you can specify in which way you like to build your crate. In addition, Cargo also has package targets for examples, tests, and benchmarks. These package targets can co-exist, so you can build/check the same crate in e.g. library or test mode.




cargo check --all-targets para compilar con todos los gargets


# Beginning
When you turn on a computer, it loads the BIOS from some special flash memory located on the motherboard. The BIOS runs self-test and initialization routines of the hardware, then it looks for bootable disks. If it finds one, control is transferred to its bootloader, which is a 512-byte portion of executable code stored at the disk’s beginning. Most bootloaders are larger than 512 bytes, so bootloaders are commonly split into a small first stage, which fits into 512 bytes, and a second stage, which is subsequently loaded by the first stage.

The bootloader has to determine the location of the kernel image on the disk and load it into memory. It also needs to switch the CPU from the 16-bit real mode first to the 32-bit protected mode, and then to the 64-bit long mode, where 64-bit registers and the complete main memory are available. Its third job is to query certain information (such as a memory map) from the BIOS and pass it to the OS kernel.

# Memory-Related Intrinsics
The Rust compiler assumes that a certain set of built-in functions is available for all systems. Most of these functions are provided by the compiler_builtins crate that we just recompiled. However, there are some memory-related functions in that crate that are not enabled by default because they are normally provided by the C library on the system. These functions include memset, which sets all bytes in a memory block to a given value, memcpy, which copies one memory block to another, and memcmp, which compares two memory blocks. While we didn’t need any of these functions to compile our kernel right now, they will be required as soon as we add some more code to it (e.g. when copying structs around).

Since we can’t link to the C library of the operating system, we need an alternative way to provide these functions to the compiler. One possible approach for this could be to implement our own memset etc. functions and apply the #[unsafe(no_mangle)] attribute to them (to avoid the automatic renaming during compilation). However, this is dangerous since the slightest mistake in the implementation of these functions could lead to undefined behavior. For example, implementing memcpy with a for loop may result in an infinite recursion because for loops implicitly call the IntoIterator::into_iter trait method, which may call memcpy again. So it’s a good idea to reuse existing, well-tested implementations instead.

# TLDR:
We first created the json with the required info.
Then the config.toml to make core recompiled. 

Runs with:
cargo bootimage   
qemu-system-x86_64 -drive format=raw,file=/Users/francocuppari/microkernel/microkernel/target/x86_64-microkernel/debug/bootimage-microkernel.bin
