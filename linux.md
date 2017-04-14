# Hardware & System Information

`uname -a`

> Linux cloudy 4.4.0-72-generic #93-Ubuntu SMP Fri Mar 31 14:07:41 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux

---

Use the lshw tool to gather vast information about your hardware components such as cpu, disks, memory, usb controllers etc.

`sudo lshw`

Pass the `-short` for a neater print

<details>
    <summary>lshw -short</summary>
    
    ```
    H/W path        Device     Class          Description
    =====================================================
                               system         Kona (LENOVO_BI_IDEAPAD76)
    /0                         bus            Yoga2
    /0/0                       memory         128KiB BIOS
    /0/4                       processor      Intel(R) Core(TM) i7-4510U CPU @ 2.00GHz
    /0/4/b                     memory         32KiB L1 cache
    /0/4/c                     memory         256KiB L2 cache
    /0/4/d                     memory         4MiB L3 cache
    /0/a                       memory         32KiB L1 cache
    /0/2a                      memory         8GiB System Memory
    /0/2a/0                    memory         4GiB DIMM
    /0/2a/1                    memory         DIMM [empty]
    /0/2a/2                    memory         4GiB DIMM
    /0/2a/3                    memory         DIMM [empty]
    /0/100                     bridge         Haswell-ULT DRAM Controller
    /0/100/2                   display        Haswell-ULT Integrated Graphics Controller
    /0/100/3                   multimedia     Haswell-ULT HD Audio Controller
    /0/100/4                   generic        Intel Corporation
    /0/100/14                  bus            8 Series USB xHCI HC
    /0/100/14/0     usb3       bus            xHCI Host Controller
    /0/100/14/1     usb2       bus            xHCI Host Controller
    /0/100/14/1/1              input          USB Optical Mouse
    /0/100/14/1/4              communication  Bluetooth wireless interface
    /0/100/14/1/6              input          Lenovo Yoga
    /0/100/14/1/7              input          Touchscreen
    /0/100/14/1/8              multimedia     Lenovo EasyCamera
    /0/100/16                  communication  8 Series HECI #0
    /0/100/1b                  multimedia     8 Series HD Audio Controller
    /0/100/1c                  bridge         8 Series PCI Express Root Port 1
    /0/100/1c/0     wlp1s0     network        Wireless 7260
    /0/100/1d                  bus            8 Series USB EHCI #1
    /0/100/1d/1     usb1       bus            EHCI Host Controller
    /0/100/1d/1/1              bus            USB hub
    /0/100/1f                  bridge         8 Series LPC Controller
    /0/100/1f.2                storage        8 Series SATA Controller 1 [AHCI mode]
    /0/100/1f.3                bus            8 Series SMBus Controller
    /0/100/1f.6                generic        8 Series Thermal
    /0/1            scsi1      storage        
    /0/1/0.0.0      /dev/sda   disk           512GB SAMSUNG MZMTE512
    /0/1/0.0.0/1    /dev/sda1  volume         500MiB Windows NTFS volume
    /0/1/0.0.0/2    /dev/sda2  volume         117GiB Windows NTFS volume
    /0/1/0.0.0/3    /dev/sda3  volume         359GiB Extended partition
    /0/1/0.0.0/3/5  /dev/sda5  volume         38GiB Linux filesystem partition
    /0/1/0.0.0/3/6  /dev/sda6  volume         321GiB Linux filesystem partition
    /1                         power          CRB Battery 0
    /2                         power          OEM_Define5
    ```
</details>

---

Use the `lscpu` command as it shows information about your CPU architecture such as number of CPU’s, cores, CPU family model, CPU caches, threads, etc from sysfs and /proc/cpuinfo.

<details>
    <summary>lscpu</summary>
    ```
    
    Architecture:          x86_64
    CPU op-mode(s):        32-bit, 64-bit
    Byte Order:            Little Endian
    CPU(s):                4
    On-line CPU(s) list:   0-3
    Thread(s) per core:    2
    Core(s) per socket:    2
    Socket(s):             1
    NUMA node(s):          1
    Vendor ID:             GenuineIntel
    CPU family:            6
    Model:                 69
    Model name:            Intel(R) Core(TM) i7-4510U CPU @ 2.00GHz
    Stepping:              1
    CPU MHz:               1802.125
    CPU max MHz:           3100,0000
    CPU min MHz:           800,0000
    BogoMIPS:              5187.81
    Virtualization:        VT-x
    L1d cache:             32K
    L1i cache:             32K
    L2 cache:              256K
    L3 cache:              4096K
    NUMA node0 CPU(s):     0-3
    Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4_1 sse4_2 movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm epb tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 avx2 smep bmi2 erms invpcid xsaveopt dtherm ida arat pln pts
    
    ```
</details>

---

`lsblk` command is used to report information about block devices. (-a = all)

---


