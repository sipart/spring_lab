Check kernel in use
```
uname -a

Linux eve-ng 4.15.0-1037-azure #39~16.04.1-Ubuntu SMP Tue Jan 15 17:20:47 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```
Move to `/boot` create new directory and move cloud image kernels
```
root@eve-ng:~# cd /boot
root@eve-ng:/boot# mkdir ./old/
```
```
root@eve-ng:/boot# ls
config-4.15.0-1037-azure          old
config-4.9.40-eve-ng-ukms-2+      System.map-4.15.0-1037-azure
grub                              System.map-4.9.40-eve-ng-ukms-2+
initrd.img-4.15.0-1037-azure      vmlinuz-4.15.0-1037-azure
initrd.img-4.9.40-eve-ng-ukms-2+  vmlinuz-4.9.40-eve-ng-ukms-2+
```
```
root@eve-ng:/boot# mv *4.15* ./old/
```

```
update-grub

Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.9.40-eve-ng-ukms-2+
Found initrd image: /boot/initrd.img-4.9.40-eve-ng-ukms-2+
done
```
Reboot
```
shutdown -r now
```

** ```DO NOT DO THE BELOW IN AZURE - LEAVE THE KERNEL IN THE VM USING THE AZURE ONE ``` **

* Move of GCP or Azure kernel - DOES NOT WORK (works in GCP) - makes VM unbootable - info' for future reference if I can find out how to get this working!!

Check kernel in use
```
uname -a

Linux eve-ng 4.15.0-1037-azure #39~16.04.1-Ubuntu SMP Tue Jan 15 17:20:47 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```
Create new directory `/boot` and move the 'cloud' image kernels then update grub 
```
root@eve-ng:~# cd /boot
root@eve-ng:/boot# mkdir ./old/
```
```
root@eve-ng:/boot# ls
config-4.15.0-1037-azure          old
config-4.9.40-eve-ng-ukms-2+      System.map-4.15.0-1037-azure
grub                              System.map-4.9.40-eve-ng-ukms-2+
initrd.img-4.15.0-1037-azure      vmlinuz-4.15.0-1037-azure
initrd.img-4.9.40-eve-ng-ukms-2+  vmlinuz-4.9.40-eve-ng-ukms-2+
```
```
root@eve-ng:/boot# mv *4.15* ./old/
```

```
update-grub

Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.9.40-eve-ng-ukms-2+
Found initrd image: /boot/initrd.img-4.9.40-eve-ng-ukms-2+
done
```
Reboot
```
shutdown -r now
```
Check kernel in use
```
uname -a

Linux sipart-eve 4.9.40-eve-ng-ukms-2+ #4 SMP Fri Sep 15 02:07:02 CEST 2017 x86_64 x86_64 x86_64 GNU/Linux
```

* Doing the above in Azure will render the VM unbootable - see below to get the VM bootable and reverse the kernel move

Go into serial console after starting the VM

Stab the ESC key until you get the grub menu (you may have to reboot the VM to catch the grub menu)

Go into advanced options and press 'e' to edit the grub file and alter the paths to the 'old' kernel and initrd files.

Once booted you can login, move the files back and update-grub - phewww!



```
[ 1906.162600] reboot: Restarting system
[H[J[1;1H[H[J[1;1H[    0.000000] Linux version 4.9.40-eve-ng-ukms-2+ (root@eve-ng) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4) ) #4 SMP Fri Sep 15 02:07:02 CEST 2017
[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-4.9.40-eve-ng-ukms-2+ root=UUID=73e9659d-2fd9-46ca-a341-5a8637c416ee ro net.ifnames=0 console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300
[    0.000000] KERNEL supported cpus:
[    0.000000]   Intel GenuineIntel
[    0.000000]   AMD AuthenticAMD
[    0.000000]   Centaur CentaurHauls
[    0.000000] x86/fpu: Supporting XSAVE feature 0x001: 'x87 floating point registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x002: 'SSE registers'
[    0.000000] x86/fpu: Supporting XSAVE feature 0x004: 'AVX registers'
[    0.000000] x86/fpu: xstate_offset[2]:  576, xstate_sizes[2]:  256
[    0.000000] x86/fpu: Enabled xstate features 0x7, context size is 832 bytes, using 'standard' format.
[    0.000000] x86/fpu: Using 'eager' FPU context switches.
[    0.000000] e820: BIOS-provided physical RAM map:
[    0.000000] BIOS-e820: [mem 0x0000000000000000-0x000000000009fbff] usable
[    0.000000] BIOS-e820: [mem 0x000000000009fc00-0x000000000009ffff] reserved
[    0.000000] BIOS-e820: [mem 0x00000000000e0000-0x00000000000fffff] reserved
[    0.000000] BIOS-e820: [mem 0x0000000000100000-0x000000003ffeffff] usable
[    0.000000] BIOS-e820: [mem 0x000000003fff0000-0x000000003fffefff] ACPI data
[    0.000000] BIOS-e820: [mem 0x000000003ffff000-0x000000003fffffff] ACPI NVS
[    0.000000] BIOS-e820: [mem 0x0000000100000000-0x0000000fdfffffff] usable
[    0.000000] BIOS-e820: [mem 0x0000001000000000-0x00000028dfffffff] usable
[    0.000000] bootconsole [earlyser0] enabled
[    0.000000] NX (Execute Disable) protection: active
[    0.000000] SMBIOS 2.3 present.
[    0.000000] Hypervisor detected: Microsoft HyperV
[    0.000000] HyperV: features 0x2e7f, hints 0x4c2c
[    0.000000] HyperV: LAPIC Timer Frequency: 0xc3500
[    0.000000] clocksource: hyperv_clocksource: mask: 0xffffffffffffffff max_cycles: 0x24e6a1710, max_idle_ns: 440795202120 ns
[    0.000000] tsc: Marking TSC unstable due to running on Hyper-V
[    0.000000] e820: last_pfn = 0x28e0000 max_arch_pfn = 0x400000000
[    0.000000] x86/PAT: Configuration [0-7]: WB  WC  UC- UC  WB  WC  UC- WT  
Memory KASLR using RDRAND RDTSC...
[    0.000000] e820: last_pfn = 0x3fff0 max_arch_pfn = 0x400000000
[    0.000000] found SMP MP-table at [mem 0x000ff780-0x000ff78f] mapped at [ffff8833400ff780]
[    0.000000] Scanning 1 areas for low memory corruption
[    0.000000] Using GB pages for direct mapping
[    0.000000] RAMDISK: [mem 0x34340000-0x36197fff]
[    0.000000] ACPI: Early table checksum verification disabled
[    0.000000] ACPI: RSDP 0x00000000000F5BF0 000014 (v00 ACPIAM)
[    0.000000] ACPI: RSDT 0x000000003FFF0000 000040 (v01 VRTUAL MICROSFT 06001702 MSFT 00000097)
[    0.000000] ACPI: FACP 0x000000003FFF0200 000081 (v02 VRTUAL MICROSFT 06001702 MSFT 00000097)
[    0.000000] ACPI: DSDT 0x000000003FFF1D24 003CBE (v01 MSFTVM MSFTVM02 00000002 INTL 02002026)
[    0.000000] ACPI: FACS 0x000000003FFFF000 000040
[    0.000000] ACPI: WAET 0x000000003FFF1A80 000028 (v01 VRTUAL MICROSFT 06001702 MSFT 00000097)
[    0.000000] ACPI: SLIC 0x000000003FFF1AC0 000176 (v01 VRTUAL MICROSFT 06001702 MSFT 00000097)
[    0.000000] ACPI: OEM0 0x000000003FFF1CC0 000064 (v01 VRTUAL MICROSFT 06001702 MSFT 00000097)
[    0.000000] ACPI: SRAT 0x000000003FFF0800 000260 (v02 VRTUAL MICROSFT 00000001 MSFT 00000001)
[    0.000000] ACPI: APIC 0x000000003FFF0300 000452 (v01 VRTUAL MICROSFT 06001702 MSFT 00000097)
[    0.000000] ACPI: OEMB 0x000000003FFFF040 000064 (v01 VRTUAL MICROSFT 06001702 MSFT 00000097)
[    0.000000] SRAT: PXM 0 -> APIC 0x00 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x01 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x02 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x03 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x04 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x05 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x06 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x07 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x08 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x09 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x0a -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x0b -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x0c -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x0d -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x0e -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x0f -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x10 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x11 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x12 -> Node 0
[    0.000000] SRAT: PXM 0 -> APIC 0x13 -> Node 0
[    0.000000] ACPI: SRAT: Node 0 PXM 0 [mem 0x00000000-0x3fffffff] hotplug
[    0.000000] ACPI: SRAT: Node 0 PXM 0 [mem 0x100000000-0xfdfffffff] hotplug
[    0.000000] ACPI: SRAT: Node 0 PXM 0 [mem 0x1000000000-0x28dfffffff] hotplug
[    0.000000] ACPI: SRAT: Node 0 PXM 0 [mem 0x28e0200000-0xffffffffff] hotplug
[    0.000000] ACPI: SRAT: Node 0 PXM 0 [mem 0x10000200000-0x1ffffffffff] hotplug
[    0.000000] ACPI: SRAT: Node 0 PXM 0 [mem 0x20000200000-0x3ffffffffff] hotplug
[    0.000000] NUMA: Node 0 [mem 0x00000000-0x3fffffff] + [mem 0x100000000-0xfdfffffff] -> [mem 0x00000000-0xfdfffffff]
[    0.000000] NUMA: Node 0 [mem 0x00000000-0xfdfffffff] + [mem 0x1000000000-0x28dfffffff] -> [mem 0x00000000-0x28dfffffff]
[    0.000000] NODE_DATA(0) allocated [mem 0x28dffd5000-0x28dfffffff]
[    0.000000] Zone ranges:
[    0.000000]   DMA      [mem 0x0000000000001000-0x0000000000ffffff]
[    0.000000]   DMA32    [mem 0x0000000001000000-0x00000000ffffffff]
[    0.000000]   Normal   [mem 0x0000000100000000-0x00000028dfffffff]
[    0.000000]   Device   empty
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000000001000-0x000000000009efff]
[    0.000000]   node   0: [mem 0x0000000000100000-0x000000003ffeffff]
[    0.000000]   node   0: [mem 0x0000000100000000-0x0000000fdfffffff]
[    0.000000]   node   0: [mem 0x0000001000000000-0x00000028dfffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000000001000-0x00000028dfffffff]
[    0.000000] ACPI: PM-Timer IO Port: 0x408
[    0.000000] ACPI: LAPIC_NMI (acpi_id[0xff] dfl dfl lint[0x1])
[    0.000000] IOAPIC[0]: apic_id 0, version 17, address 0xfec00000, GSI 0-23
[    0.000000] ACPI: INT_SRC_OVR (bus 0 bus_irq 0 global_irq 2 dfl dfl)
[    0.000000] ACPI: INT_SRC_OVR (bus 0 bus_irq 9 global_irq 9 high level)
[    0.000000] Using ACPI (MADT) for SMP configuration information
[    0.000000] smpboot: Allowing 128 CPUs, 108 hotplug CPUs
[    0.000000] PM: Registered nosave memory: [mem 0x00000000-0x00000fff]
[    0.000000] PM: Registered nosave memory: [mem 0x0009f000-0x0009ffff]
[    0.000000] PM: Registered nosave memory: [mem 0x000a0000-0x000dffff]
[    0.000000] PM: Registered nosave memory: [mem 0x000e0000-0x000fffff]
[    0.000000] PM: Registered nosave memory: [mem 0x3fff0000-0x3fffefff]
[    0.000000] PM: Registered nosave memory: [mem 0x3ffff000-0x3fffffff]
[    0.000000] PM: Registered nosave memory: [mem 0x40000000-0xffffffff]
[    0.000000] PM: Registered nosave memory: [mem 0xfe0000000-0xfffffffff]
[    0.000000] e820: [mem 0x40000000-0xffffffff] available for PCI devices
[    0.000000] Booting paravirtualized kernel on bare hardware
[    0.000000] clocksource: refined-jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645519600211568 ns
[    0.000000] setup_percpu: NR_CPUS:8192 nr_cpumask_bits:128 nr_cpu_ids:128 nr_node_ids:1
[    0.000000] percpu: Embedded 36 pages/cpu @ffff885b7d600000 s107800 r8192 d31464 u262144
[    0.000000] Built 1 zonelists in Node order, mobility grouping on.  Total pages: 41285497
[    0.000000] Policy zone: Normal
[    0.000000] Kernel command line: BOOT_IMAGE=/boot/vmlinuz-4.9.40-eve-ng-ukms-2+ root=UUID=73e9659d-2fd9-46ca-a341-5a8637c416ee ro net.ifnames=0 console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300
[    0.000000] log_buf_len individual max cpu contribution: 4096 bytes
[    0.000000] log_buf_len total cpu_extra contributions: 520192 bytes
[    0.000000] log_buf_len min size: 262144 bytes
[    0.000000] log_buf_len: 1048576 bytes
[    0.000000] early log buf free: 250620(95%)
[    0.000000] PID hash table entries: 4096 (order: 3, 32768 bytes)
[    0.000000] Memory: 165012812K/167771704K available (8846K kernel code, 1642K rwdata, 3772K rodata, 2152K init, 2360K bss, 2758892K reserved, 0K cma-reserved)
[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=128, Nodes=1
[    0.000000] Hierarchical RCU implementation.
[    0.000000] 	Build-time adjustment of leaf fanout to 64.
[    0.000000] 	RCU restricting CPUs from NR_CPUS=8192 to nr_cpu_ids=128.
[    0.000000] RCU: Adjusting geometry for rcu_fanout_leaf=64, nr_cpu_ids=128
[    0.000000] NR_IRQS:524544 nr_irqs:1448 16
[    0.000000] Console: colour VGA+ 80x25
[    0.000000] console [tty1] enabled
[    0.000000] console [ttyS0] enabled
[    0.000000] console [ttyS0] enabled
[    0.000000] bootconsole [earlyser0] disabled
[    0.000000] bootconsole [earlyser0] disabled
[    0.000000] tsc: Fast TSC calibration failed
[    0.000000] tsc: Unable to calibrate against PIT
[    0.000000] tsc: using PMTIMER reference calibration
[    0.000000] tsc: Detected 2294.684 MHz processor
[    0.020043] Calibrating delay loop (skipped), value calculated using timer frequency.. 4589.36 BogoMIPS (lpj=9178736)
[    0.026045] pid_max: default: 131072 minimum: 1024
[    0.028065] ACPI: Core revision 20160831
[    0.033672] ACPI: 1 ACPI AML tables successfully acquired and loaded
[    0.040180] Security Framework initialized
[    0.043342] Yama: becoming mindful.
[    0.044030] AppArmor: AppArmor initialized
[    0.059517] Dentry cache hash table entries: 33554432 (order: 16, 268435456 bytes)
[    0.104413] Inode-cache hash table entries: 16777216 (order: 15, 134217728 bytes)
[    0.129757] Mount-cache hash table entries: 524288 (order: 10, 4194304 bytes)
[    0.132177] Mountpoint-cache hash table entries: 524288 (order: 10, 4194304 bytes)
[    0.141663] CPU: Physical Processor ID: 0
[    0.144015] CPU: Processor Core ID: 0
[    0.146594] mce: CPU supports 1 MCE banks
[    0.148041] Last level iTLB entries: 4KB 64, 2MB 8, 4MB 8
[    0.152016] Last level dTLB entries: 4KB 64, 2MB 0, 4MB 0, 1GB 4
[    0.157309] Freeing SMP alternatives memory: 32K (ffffffff9cdb6000 - ffffffff9cdbe000)
[    0.165482] ftrace: allocating 33429 entries in 131 pages
[    0.176123] smpboot: Max logical packages: 8
[    0.180061] Switched APIC routing to physical flat.
[    0.202412] ..TIMER: vector=0x30 apic1=0 pin1=2 apic2=-1 pin2=-1
[    0.204169] smpboot: CPU0: Intel(R) Xeon(R) CPU E5-2673 v4 @ 2.30GHz (family: 0x6, model: 0x4f, stepping: 0x1)
[    0.212001] Performance Events: unsupported p6 CPU model 79 no PMU driver, software events only.
[    0.220008] NMI watchdog: disabled (cpu0): hardware events not enabled
[    0.224001] NMI watchdog: Shutting down hard lockup detector on all cpus
[    0.228567] x86: Booting SMP configuration:
[    0.232002] .... node  #0, CPUs:          #1   #2   #3   #4   #5   #6   #7   #8   #9  #10  #11  #12  #13  #14  #15  #16  #17  #18  #19[    1.756153] x86: Booted up 1 node, 20 CPUs
[    1.759485] smpboot: Total of 20 processors activated (91786.78 BogoMIPS)
[    1.770934] devtmpfs: initialized
[    1.772061] x86/mm: Memory block size: 2048MB
[    1.778068] evm: security.selinux
[    1.780002] evm: security.SMACK64
[    1.784002] evm: security.SMACK64EXEC
[    1.786404] evm: security.SMACK64TRANSMUTE
[    1.788002] evm: security.SMACK64MMAP
[    1.792001] evm: security.ima
[    1.796003] evm: security.capability
[    1.798542] PM: Registering ACPI NVS region [mem 0x3ffff000-0x3fffffff] (4096 bytes)
[    1.800391] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645041785100000 ns
[    1.804129] futex hash table entries: 32768 (order: 9, 2097152 bytes)
[    1.812365] pinctrl core: initialized pinctrl subsystem
[    1.836410] RTC time:  9:52:22, date: 02/20/19
[    1.841168] NET: Registered protocol family 16
[    1.864026] cpuidle: using governor ladder
[    1.880075] cpuidle: using governor menu
[    1.882765] PCCT header not found.
[    1.884054] ACPI: bus type PCI registered
[    1.888002] acpiphp: ACPI Hot Plug PCI Controller Driver version: 0.5
[    1.892430] PCI: Using configuration type 1 for base access
[    1.916120] HugeTLB registered 1 GB page size, pre-allocated 0 pages
[    1.920004] HugeTLB registered 2 MB page size, pre-allocated 0 pages
[    1.928596] ACPI: Added _OSI(Module Device)
[    1.932003] ACPI: Added _OSI(Processor Device)
[    1.936001] ACPI: Added _OSI(3.0 _SCP Extensions)
[    1.940001] ACPI: Added _OSI(Processor Aggregator Device)
[    1.947041] ACPI: Interpreter enabled
[    1.948013] ACPI: (supports S0 S5)
[    1.952002] ACPI: Using IOAPIC for interrupt routing
[    1.956064] PCI: Using host bridge windows from ACPI; if necessary, use "pci=nocrs" and report a bug
[    1.982045] ACPI: PCI Root Bridge [PCI0] (domain 0000 [bus 00-ff])
[    1.984006] acpi PNP0A03:00: _OSC: OS supports [ASPM ClockPM Segments MSI]
[    1.988005] acpi PNP0A03:00: _OSC failed (AE_NOT_FOUND); disabling ASPM
[    1.996012] acpi PNP0A03:00: fail to add MMCONFIG information, can't access extended PCI configuration space under this bridge.
[    2.004181] PCI host bridge to bus 0000:00
[    2.006998] pci_bus 0000:00: root bus resource [mem 0xfe0000000-0xfffffffff window]
[    2.012002] pci_bus 0000:00: root bus resource [io  0x0000-0x0cf7 window]
[    2.016002] pci_bus 0000:00: root bus resource [io  0x0d00-0xffff window]
[    2.020002] pci_bus 0000:00: root bus resource [mem 0x000a0000-0x000bffff window]
[    2.024002] pci_bus 0000:00: root bus resource [mem 0x40000000-0xfffbffff window]
[    2.032003] pci_bus 0000:00: root bus resource [bus 00-ff]
[    2.043375] pci 0000:00:07.1: legacy IDE quirk: reg 0x10: [io  0x01f0-0x01f7]
[    2.048005] pci 0000:00:07.1: legacy IDE quirk: reg 0x14: [io  0x03f6]
[    2.052002] pci 0000:00:07.1: legacy IDE quirk: reg 0x18: [io  0x0170-0x0177]
[    2.056001] pci 0000:00:07.1: legacy IDE quirk: reg 0x1c: [io  0x0376]
[    2.060646] * Found PM-Timer Bug on the chipset. Due to workarounds for a bug,
[    2.060646] * this clock source is slow. Consider trying other clock sources
[    2.072146] pci 0000:00:07.3: quirk: [io  0x0400-0x043f] claimed by PIIX4 ACPI
[    2.094163] ACPI: PCI Interrupt Link [LNKA] (IRQs 3 4 5 7 9 10 *11 12 14 15)
[    2.104839] ACPI: PCI Interrupt Link [LNKB] (IRQs 3 4 5 7 9 10 11 12 14 15) *0, disabled.
[    2.117993] ACPI: PCI Interrupt Link [LNKC] (IRQs 3 4 5 7 9 10 11 12 14 15) *0, disabled.
[    2.130393] ACPI: PCI Interrupt Link [LNKD] (IRQs 3 4 5 7 9 10 11 12 14 15) *0, disabled.
[    2.142986] ACPI: Enabled 1 GPEs in block 00 to 0F
[    2.148544] vgaarb: setting as boot device: PCI:0000:00:08.0
[    2.151856] vgaarb: device added: PCI:0000:00:08.0,decodes=io+mem,owns=io+mem,locks=none
[    2.156002] vgaarb: loaded
[    2.158038] vgaarb: bridge control possible 0000:00:08.0
[    2.160296] SCSI subsystem initialized
[    2.164290] ACPI: bus type USB registered
[    2.168017] usbcore: registered new interface driver usbfs
[    2.172009] usbcore: registered new interface driver hub
[    2.176371] usbcore: registered new device driver usb
[    2.180285] PCI: Using ACPI for IRQ routing
[    2.184535] NetLabel: Initializing
[    2.188002] NetLabel:  domain hash size = 128
[    2.191118] NetLabel:  protocols = UNLABELED CIPSOv4
[    2.192015] NetLabel:  unlabeled traffic allowed by default
[    2.196312] clocksource: Switched to clocksource hyperv_clocksource
[    2.211436] VFS: Disk quotas dquot_6.6.0
[    2.214751] VFS: Dquot-cache hash table entries: 512 (order 0, 4096 bytes)
[    2.219329] AppArmor: AppArmor Filesystem Enabled
[    2.223330] pnp: PnP ACPI init
[    2.228183] system 00:06: [io  0x01e0-0x01ef] has been reserved
[    2.232501] system 00:06: [io  0x0160-0x016f] has been reserved
[    2.236934] system 00:06: [io  0x0278-0x027f] has been reserved
[    2.240675] system 00:06: [io  0x0378-0x037f] has been reserved
[    2.244586] system 00:06: [io  0x0678-0x067f] has been reserved
[    2.248389] system 00:06: [io  0x0778-0x077f] has been reserved
[    2.252305] system 00:06: [io  0x04d0-0x04d1] has been reserved
[    2.256498] system 00:07: [io  0x0400-0x043f] has been reserved
[    2.260578] system 00:07: [io  0x0370-0x0371] has been reserved
[    2.264579] system 00:07: [io  0x0440-0x044f] has been reserved
[    2.268320] system 00:07: [mem 0xfec00000-0xfec00fff] could not be reserved
[    2.273014] system 00:07: [mem 0xfee00000-0xfee00fff] has been reserved
[    2.277603] system 00:08: [mem 0x00000000-0x0009ffff] could not be reserved
[    2.281945] system 00:08: [mem 0x000c0000-0x000dffff] could not be reserved
[    2.286649] system 00:08: [mem 0x000e0000-0x000fffff] could not be reserved
[    2.291064] system 00:08: [mem 0x00100000-0x3fffffff] could not be reserved
[    2.295416] system 00:08: [mem 0xfffc0000-0xffffffff] has been reserved
[    2.299909] pnp: PnP ACPI: found 9 devices
[    2.312410] clocksource: acpi_pm: mask: 0xffffff max_cycles: 0xffffff, max_idle_ns: 2085701024 ns
[    2.318037] NET: Registered protocol family 2
[    2.321886] TCP established hash table entries: 524288 (order: 10, 4194304 bytes)
[    2.327316] TCP bind hash table entries: 65536 (order: 8, 1048576 bytes)
[    2.331945] TCP: Hash tables configured (established 524288 bind 65536)
[    2.336467] UDP hash table entries: 65536 (order: 9, 2097152 bytes)
[    2.340868] UDP-Lite hash table entries: 65536 (order: 9, 2097152 bytes)
[    2.345959] NET: Registered protocol family 1
[    2.349261] pci 0000:00:00.0: Limiting direct PCI/PCI transfers
[    2.353751] pci 0000:00:08.0: Video device with shadowed ROM at [mem 0x000c0000-0x000dffff]
[    2.359582] Trying to unpack rootfs image as initramfs...
[    2.796085] Freeing initrd memory: 31072K (ffff883374340000 - ffff883376198000)
[    2.800708] PCI-DMA: Using software bounce buffering for IO (SWIOTLB)
[    2.804619] software IO TLB [mem 0x3bff0000-0x3fff0000] (64MB) mapped at [ffff88337bff0000-ffff88337ffeffff]
[    2.810877] Scanning for low memory corruption every 60 seconds
[    2.814679] clocksource: tsc: mask: 0xffffffffffffffff max_cycles: 0x21139776891, max_idle_ns: 440795250948 ns
[    2.822079] audit: initializing netlink subsys (disabled)
[    2.826499] audit: type=2000 audit(1550656342.824:1): initialized
[    2.831605] Initialise system trusted keyrings
[    2.836124] workingset: timestamp_bits=36 max_order=26 bucket_order=0
[    2.841636] zbud: loaded
[    2.845389] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    2.849942] fuse init (API version 7.26)
[    2.853342] Allocating IMA blacklist keyring.
[    2.861181] Key type asymmetric registered
[    2.864207] Asymmetric key parser 'x509' registered
[    2.868044] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 248)
[    2.873462] io scheduler noop registered
[    2.876283] io scheduler deadline registered (default)
[    2.879789] io scheduler cfq registered
[    2.882559] pci_hotplug: PCI Hot Plug PCI Core version: 0.5
[    2.886199] pciehp: PCI Express Hot Plug Controller Driver version: 0.4
[    2.890535] input: Power Button as /devices/LNXSYSTM:00/LNXPWRBN:00/input/input0
[    2.895394] ACPI: Power Button [PWRF]
[    2.898829] GHES: HEST is not enabled!
[    2.901790] Serial: 8250/16550 driver, 32 ports, IRQ sharing enabled
[    2.933461] 00:03: ttyS0 at I/O 0x3f8 (irq = 4, base_baud = 115200) is a 16550A
[    2.965602] 00:04: ttyS1 at I/O 0x2f8 (irq = 3, base_baud = 115200) is a 16550A
[    2.973602] Linux agpgart interface v0.103
[    2.988819] brd: module loaded
[    2.996849] loop: module loaded
[    2.999361] ata_piix 0000:00:07.1: Hyper-V Virtual Machine detected, ATA device ignore set
[    3.007357] scsi host0: ata_piix
[    3.010092] scsi host1: ata_piix
[    3.012623] ata1: PATA max UDMA/33 cmd 0x1f0 ctl 0x3f6 bmdma 0xffa0 irq 14
[    3.017155] ata2: PATA max UDMA/33 cmd 0x170 ctl 0x376 bmdma 0xffa8 irq 15
[    3.021705] libphy: Fixed MDIO Bus: probed
[    3.024952] tun: Universal TUN/TAP device driver, 1.6
[    3.028513] tun: (C) 1999-2004 Max Krasnyansky <maxk@qualcomm.com>
[    3.032851] PPP generic driver version 2.4.2
[    3.035973] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
[    3.040327] ehci-pci: EHCI PCI platform driver
[    3.043514] ehci-platform: EHCI generic platform driver
[    3.047060] ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
[    3.051174] ohci-pci: OHCI PCI platform driver
[    3.054147] ohci-platform: OHCI generic platform driver
[    3.057688] uhci_hcd: USB Universal Host Controller Interface driver
[    3.062637] i8042: PNP: PS/2 Controller [PNP0303:PS2K,PNP0f03:PS2M] at 0x60,0x64 irq 1,12
[    3.073779] serio: i8042 KBD port at 0x60,0x64 irq 1
[    3.078055] serio: i8042 AUX port at 0x60,0x64 irq 12
[    3.084702] mousedev: PS/2 mouse device common for all mice
[    3.089055] rtc_cmos 00:00: RTC can wake from S4
[    3.097637] input: AT Translated Set 2 keyboard as /devices/platform/i8042/serio0/input/input1
[    3.113865] rtc_cmos 00:00: rtc core: registered rtc_cmos as rtc0
[    3.119621] rtc_cmos 00:00: alarms up to one month, 114 bytes nvram
[    3.124260] i2c /dev entries driver
[    3.127243] device-mapper: uevent: version 1.0.3
[    3.130681] device-mapper: ioctl: 4.35.0-ioctl (2016-06-23) initialised: dm-devel@redhat.com
[    3.137479] ledtrig-cpu: registered to indicate activity on CPUs
[    3.143173] NET: Registered protocol family 10
[    3.147715] NET: Registered protocol family 17
[    3.150886] Key type dns_resolver registered
[    3.154791] microcode: sig=0x406f1, pf=0x1, revision=0xffffffff
[    3.161146] microcode: Microcode Update Driver: v2.01 <tigran@aivazian.fsnet.co.uk>, Peter Oruba
[    3.167153] registered taskstats version 1
[    3.170282] Loading compiled-in X.509 certificates
[    3.177103] Loaded X.509 cert 'Build time autogenerated kernel key: b4dd6f89d257e4171a42fa83c1cc5a35245710d0'
[    3.183256] zswap: loaded using pool lzo/zbud
[    3.657987] UKSM: relative memcmp_cost = 174 hash=2660834889 cmp_ret=0.
[    3.676533] Key type big_key registered
[    3.681327] Key type trusted registered
[    3.686143] Key type encrypted registered
[    3.689290] AppArmor: AppArmor sha1 policy hashing enabled
[    3.693215] ima: No TPM chip found, activating TPM-bypass!
[    3.696979] evm: HMAC attrs: 0x1
[    3.699593]   Magic number: 7:769:879
[    3.702871] rtc_cmos 00:00: setting system clock to 2019-02-20 09:52:24 UTC (1550656344)
[    3.708460] BIOS EDD facility v0.16 2004-Jun-25, 0 devices found
[    3.712279] EDD information not available.
[    3.718578] Freeing unused kernel memory: 2152K (ffffffff9cb9c000 - ffffffff9cdb6000)
[    3.723458] Write protecting the kernel read-only data: 14336k
[    3.728636] Freeing unused kernel memory: 1376K (ffff8858c58a8000 - ffff8858c5a00000)
[    3.735192] Freeing unused kernel memory: 324K (ffff8858c5daf000 - ffff8858c5e00000)
[    3.747076] x86/mm: Checked W+X mappings: passed, no W+X pages found.
Loading, please wait...
starting version[    3.768329] random: systemd-udevd: uninitialized urandom read (16 bytes read)
 229
[    3.769048] random: udevadm: uninitialized urandom read (16 bytes read)
[    3.769060] random: udevadm: uninitialized urandom read (16 bytes read)
[    3.776468] random: udevadm: uninitialized urandom read (16 bytes read)
[    3.776488] random: udevadm: uninitialized urandom read (16 bytes read)
[    3.780617] random: udevadm: uninitialized urandom read (16 bytes read)
[    3.780707] random: udevadm: uninitialized urandom read (16 bytes read)
[    3.780717] random: udevadm: uninitialized urandom read (16 bytes read)
[    3.780895] random: udevadm: uninitialized urandom read (16 bytes read)
[    3.780932] random: udevadm: uninitialized urandom read (16 bytes read)
[    3.848237] FUJITSU Extended Socket Network Device Driver - version 1.1 - Copyright (c) 2015 FUJITSU LIMITED
[    3.874117] clocksource: hyperv_clocksource_tsc_page: mask: 0xffffffffffffffff max_cycles: 0x24e6a1710, max_idle_ns: 440795202120 ns
[    3.875833] Floppy drive(s): fd0 is 1.44M
[    3.891418] clocksource: Switched to clocksource hyperv_clocksource_tsc_page
[    3.898892] FDC 0 is an 82078.
[    3.904847] hv_vmbus: Hyper-V Host Build:14393-10.0-0-0.274; Vmbus version:4.0
[    3.921459] hv_vmbus: registering driver hyperv_fb
[    3.933574] hyperv_fb: Screen resolution: 1152x864, Color depth: 32
[    3.942912] hv_utils: Registering HyperV Utility Driver
[    3.942913] hv_vmbus: registering driver hv_util
[    3.943906] Console: switching to colour frame buffer device 144x54
[    3.945971] hv_vmbus: registering driver hyperv_keyboard
[    3.964434] AVX2 version of gcm_enc/dec engaged.
[    3.967162] AES CTR mode by8 optimization enabled
[    3.968385] hv_utils: Using TimeSync version 4.0
[    3.980473] hv_vmbus: registering driver hv_netvsc
[    3.980476] hv_vmbus: registering driver hv_storvsc
[    3.982669] scsi host2: storvsc_host_t
[    3.983888] scsi 2:0:0:0: Direct-Access     Msft     Virtual Disk     1.0  PQ: 0 ANSI: 5
[    4.000285] hidraw: raw HID events driver (C) Jiri Kosina
[    4.008321] hv_vmbus: registering driver hid_hyperv
[    4.028253] sd 2:0:0:0: Attached scsi generic sg0 type 0
[    4.028279] sd 2:0:0:0: [sda] 209715200 512-byte logical blocks: (107 GB/100 GiB)
[    4.028281] sd 2:0:0:0: [sda] 4096-byte physical blocks
[    4.028535] sd 2:0:0:0: [sda] Write Protect is off
[    4.028650] sd 2:0:0:0: [sda] Write cache: enabled, read cache: enabled, supports DPO and FUA
[    4.030421]  sda: sda1
[    4.031059] sd 2:0:0:0: [sda] Attached SCSI disk
[    4.055920] scsi host3: storvsc_host_t
[    4.059001] scsi 3:0:1:0: Direct-Access     Msft     Virtual Disk     1.0  PQ: 0 ANSI: 5
[    4.088384] sd 3:0:1:0: Attached scsi generic sg1 type 0
[    4.088417] sd 3:0:1:0: [sdb] 671088640 512-byte logical blocks: (344 GB/320 GiB)
[    4.088418] sd 3:0:1:0: [sdb] 4096-byte physical blocks
[    4.089364] sd 3:0:1:0: [sdb] Write Protect is off
[    4.089717] sd 3:0:1:0: [sdb] Write cache: disabled, read cache: enabled, supports DPO and FUA
[    4.091451] random: fast init done
[    4.091831]  sdb: sdb1
[    4.092916] sd 3:0:1:0: [sdb] Attached SCSI disk
[    4.130438] scsi host4: storvsc_host_t
[    4.137743] scsi host5: storvsc_host_t
[    4.142242] hv_netvsc: hv_netvsc channel opened successfully
[    4.170057] hv_netvsc 000d3a43-d6d4-000d-3a43-d6d4000d3a43: Send section size: 6144, Section count:2560
[    4.177594] hv_netvsc 000d3a43-d6d4-000d-3a43-d6d4000d3a43: Device MAC 00:0d:3a:43:d6:d4 link state up
[    4.344084] psmouse serio1: trackpoint: failed to get extended button data
[    4.672400] input: Microsoft Vmbus HID-compliant Mouse as /devices/0006:045E:0621.0001/input/input4
[    4.681366] hid 0006:045E:0621.0001: input: <UNKNOWN> HID v0.01 Mouse [Microsoft Vmbus HID-compliant Mouse] on 
Begin: Loading essential drivers ... [    6.675463] md: linear personality registered for level -1
[    6.683327] md: multipath personality registered for level -4
[    6.691416] md: raid0 personality registered for level 0
[    6.699467] md: raid1 personality registered for level 1
[    6.776073] raid6: sse2x1   gen()  9226 MB/s
[    6.844060] raid6: sse2x1   xor()  6946 MB/s
[    6.900070] random: crng init done
[    6.912070] raid6: sse2x2   gen() 11860 MB/s
[    6.980052] raid6: sse2x2   xor()  7347 MB/s
[    7.048083] raid6: sse2x4   gen() 13718 MB/s
[    7.116061] raid6: sse2x4   xor()  9298 MB/s
[    7.184075] raid6: avx2x1   gen() 17964 MB/s
[    7.252052] raid6: avx2x2   gen() 21001 MB/s
[    7.320069] raid6: avx2x4   gen() 23930 MB/s
[    7.323117] raid6: using algorithm avx2x4 gen() 23930 MB/s
[    7.326876] raid6: using avx2x2 recovery algorithm
[    7.332953] xor: automatically using best checksumming function   avx       
[    7.340078] async_tx: api initialized (async)
[    7.355243] md: raid6 personality registered for level 6
[    7.359044] md: raid5 personality registered for level 5
[    7.362657] md: raid4 personality registered for level 4
[    7.373042] md: raid10 personality registered for level 10
done.
Begin: Running /scripts/init-premount ... done.
Begin: Mounting root file system ... Begin: Running /scripts/local-top ... done.
Begin: Running /scripts/local-premount ... [    7.409096] Btrfs loaded, crc32c=crc32c-intel
Scanning for Btrfs filesystems
[    7.500323] blk_update_request: I/O error, dev fd0, sector 0
[    7.504058] floppy: error -5 while reading block 0
done.
Warning: fsck not present, so skipping root file system
[    7.548480] EXT4-fs (sda1): mounted filesystem with ordered data mode. Opts: (null)
done.
Begin: Running /scripts/local-bottom ... done.
Begin: Running /scripts/init-bottom ... done.
[    7.956333] systemd[1]: systemd 229 running in system mode. (+PAM +AUDIT +SELINUX +IMA +APPARMOR +SMACK +SYSVINIT +UTMP +LIBCRYPTSETUP +GCRYPT +GNUTLS +ACL +XZ -LZ4 +SECCOMP +BLKID +ELFUTILS +KMOD -IDN)
[    7.969922] systemd[1]: Detected virtualization microsoft.
[    7.973842] systemd[1]: Detected architecture x86-64.

Welcome to [1mUbuntu 16.04.5 LTS[0m!

[    7.979817] systemd[1]: Set hostname to <eve-ng>.
[    8.040257] blk_update_request: I/O error, dev fd0, sector 0
[    8.044022] floppy: error -5 while reading block 0
[    8.105878] systemd[1]: [/etc/systemd/system/cpulimit.service:7] Executable path is not absolute, ignoring: (/usr/bin/killall -9 cpulimit-daemon.php & /usr/bin/killall -TERM cpulimit )
[    8.123239] systemd[1]: Reached target Encrypted Volumes.
[[0;32m  OK  [0m] Reached target Encrypted Volumes.
[    8.129472] systemd[1]: Reached target Swap.
[[0;32m  OK  [0m] Reached target Swap.
[    8.134461] systemd[1]: Started Forward Password Requests to Wall Directory Watch.
[[0;32m  OK  [0m] Started Forward Password Requests to Wall Directory Watch.
[    8.144147] systemd[1]: Set up automount Arbitrary Executable File Formats File System Automount Point.
[[0;32m  OK  [0m] Set up automount Arbitrary Executab...ats File System Automount Point.
[    8.153868] systemd[1]: Listening on LVM2 metadata daemon socket.
[[0;32m  OK  [0m] Listening on LVM2 metadata daemon socket.
[[0;32m  OK  [0m] Listening on Syslog Socket.
[[0;32m  OK  [0m] Listening on udev Kernel Socket.
[[0;32m  OK  [0m] Listening on Device-mapper event daemon FIFOs.
[[0;32m  OK  [0m] Listening on Journal Audit Socket.
[[0;32m  OK  [0m] Listening on /dev/initctl Compatibility Named Pipe.
[[0;32m  OK  [0m] Listening on Journal Socket (/dev/log).
[[0;32m  OK  [0m] Created slice System Slice.
[[0;32m  OK  [0m] Created slice system-serial\x2dgetty.slice.
[[0;32m  OK  [0m] Created slice system-systemd\x2dfsck.slice.
[[0;32m  OK  [0m] Listening on Journal Socket.
         Starting Journal Service...
         Mounting POSIX Message Queue File System...
         Mounting Huge Pages File System...
         Starting Load Kernel Modules...
[[0;32m  OK  [0m] Listening on LVM2 poll daemon socket.
[[0;32m  OK  [0m] Listening on fsck to fsckd communication Socket.
[[0;32m  OK  [0m] Started Trigger resolvconf update for networkd DNS.
         Mounting Debug File System...
         Starting Uncomplicated firewall...
         Starting Set console keymap...
         Starting Remount Root and Kernel File Systems...
[[0;32m  OK  [0m] Listening on udev Control Socket.
[[0;32m  OK  [0m] Created slice User and Session Slice.
[[0;32m  OK  [0m] Reached target Slices.
[[0;32m  OK  [0m] Reached target User and Group Name Lookups.
[    8.244951] EXT4-fs (sda1): re-mounted. Opts: discard
         Starting Create list of required st... nodes for the current kernel...
         Starting Monitoring of LVM2 mirrors... dmeventd or progress polling...
         Starting Nameserver information manager...
[[0;32m  OK  [0m] Mounted Debug File System.
[[0;32m  OK  [0m] Mounted POSIX Message Queue File System.
[[0;32m  OK  [0m] Mounted Huge Pages File System.
[[0;32m  OK  [0m] Started Journal Service.
[[0;32m  OK  [0m] Started Uncomplicated firewall.
[[0;32m  OK  [0m] Started Set console keymap.
[[0;32m  OK  [0m] Started Remount Root and Kernel File Systems.
[[0;32m  OK  [0m] Started Create list of required sta...ce nodes for the current kernel.
[[0;32m  OK  [0m] Started Nameserver information manager.
[    8.335595] Loading iSCSI transport class v2.0-870.
[    8.403868] iscsi: registered transport (tcp)
[[0;32m  OK  [0m] Started LVM2 metadata daemon.
         Starting Create Static Device Nodes in /dev...
         Starting udev Coldplug all Devices...
         Starting Load/Save Random Seed...
[[0;32m  OK  [0m] Started Hyper-V KVP Protocol Daemon.
         Starting Flush Journal to Persistent Storage...
[[0;32m  OK  [0m] Started Create Static Device Nodes in /dev.
[[0;32m  OK  [0m] Started Load/Save Random Seed.
[    8.497922] systemd-journald[651]: Received request to flush runtime journal from PID 1
         Starting udev Kernel Device Manager...
[[0;32m  OK  [0m] Started Flush Journal to Persistent Storage.
[    8.655969] iscsi: registered transport (iser)
[[0;32m  OK  [0m] Started udev Kernel Device Manager.
[[0;32m  OK  [0m] Started Load Kernel Modules.
[[0;32m  OK  [0m] Started udev Coldplug all Devices.
[[0;32m  OK  [0m] Found device /dev/ttyS0.
[    8.700546] RAPL PMU: API unit is 2^-32 Joules, 3 fixed counters, 10737418240 ms ovfl timer
[    8.706626] RAPL PMU: hw unit of domain pp0-core 2^-0 Joules
[    8.712787] RAPL PMU: hw unit of domain package 2^-0 Joules
[    8.716911] RAPL PMU: hw unit of domain dram 2^-16 Joules
[[0;32m  OK  [0m] Listening on Load/Save RF Kill Switch Status /dev/rfkill Watch.
[    8.730744] hv_vmbus: registering driver hv_balloon
[    8.818119] piix4_smbus 0000:00:07.3: SMBus base address uninitialized - upgrade BIOS or use force_addr=0xaddr
[    8.830389] hv_vmbus: registering driver hv_pci
[    8.838248] ACPI: \: failed to evaluate _DSM (0x1001)
[    8.842266] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: PCI host bridge to bus 95bf:00
[    8.842268] pci_bus 95bf:00: root bus resource [mem 0xfe0000000-0xfe07fffff window]
         Mounting FUSE Control File System...
         Mounting Configuration File System...
         Starting Apply Kernel Variables...
         Starting LSB: QEMU KVM module loading script...
[[0;32m  OK  [0m] Started Dispatch Password Requests to Console Directory Watch.
[[0;32m  OK  [0m] Mounted FUSE Control File System.
[[0;32m  OK  [0m] Mounted Configuration File System.
[[0;32m  OK  [0m] Started Apply Kernel Variables.
[[0;32m  OK  [0m] Found device Virtual_Disk 1.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet0.
[[0;32m  OK  [0m] Started Monitoring of LVM2 mirrors,...ng dmeventd or progress polling.
[[0;32m  OK  [0m] Reached target Local File Systems (Pre).
         Starting File System Check on /dev/disk/cloud/azure_resource-part1...
[[0;32m  OK  [0m] Reached target Local File Systems.
         Starting Initial cloud-init job (pre-networking)[    9.070297] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
...
[    9.080187] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.086459] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
         Starting Set console font and keymap...
[    9.096113] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.103290] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.113762] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.121689] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.129150] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.135770] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.142424] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
         Startin[    9.149144] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
g Create Volatile Files and Dire[    9.155735] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
ctories...
[    9.162626] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.169504] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.177103] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
         Starting LSB: AppArmor initialization...
[    9.189802] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.189849] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.189892] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.189935] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.189976] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
[    9.190020] hv_pci 22038bae-a07e-439d-95bf-1728ef60ecc1: Request for interrupt failed: 0xc0350005
         Starting Tell Plymouth To Write Out Runtime Data...
[[0;32m  OK  [0m] Started LSB: QEMU KVM module loading script.
[[0;32m  OK  [0m] Started File System Check on /dev/disk/cloud/azure_resource-part1.
[[0;32m  OK  [0m] Started Create Volatile Files and Directories.
[[0;32m  OK  [0m] Started Tell Plymouth To Write Out Runtime Data.
[[0;32m  OK  [0m] Started File System Check Daemon to report status.
         Starting Update UTMP about System Boot/Shutdown...
[[0;32m  OK  [0m] Reached target System Time Synchronized.
[[0;32m  OK  [0m] Started Set console font and keymap.
[[0;32m  OK  [0m] Created slice system-getty.slice.
[[0;32m  OK  [0m] Started Update UTMP about System Boot/Shutdown.
[    9.780005] general protection fault: 0000 [#1] SMP
[    9.780005] Modules linked in: intel_rapl(-) sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[    9.780005] CPU: 7 PID: 735 Comm: systemd-udevd Not tainted 4.9.40-eve-ng-ukms-2+ #4
[    9.780005] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[    9.780005] task: ffff885b5ffe5b00 task.stack: ffffa9989a168000
[    9.780005] RIP: 0010:[<ffffa998988e9000>]  [<ffffa998988e9000>] 0xffffa998988e9000
[    9.780005] RSP: 0018:ffffa9989a16b7a8  EFLAGS: 00000086
[    9.780005] RAX: ffffffffffffffff RBX: 0000000000000080 RCX: 000000000000007e
[    9.780005] RDX: 000021655a16b7d0 RSI: 000021655a16b7d0 RDI: 000000000000007e
[    9.780005] RBP: ffffa9989a16b7c0 R08: 0000000000000000 R09: 0000000000000000
[    9.780005] R10: ffff885b6061f990 R11: 0000000000000246 R12: ffff885b5c755020
[    9.780005] R13: 0000000000000001 R14: ffff885b5eb27220 R15: ffffa9989a16b7d0
[    9.780005] FS:  00007f63884748c0(0000) GS:ffff885b7d7c0000(0000) knlGS:0000000000000000
[    9.780005] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[    9.780005] CR2: 00007ffc6ef38858 CR3: 000000281fff7000 CR4: 00000000003406e0
[    9.780005] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[    9.780005] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[    9.780005] Stack:
[    9.780005]  ffffffffc01bf8c6 ffffa998988e9000 21f74772ebc7f4b6 ffffa9989a16b838
[    9.780005]  ffffffffc03fe1be ffffffffffffffff 00000000a07e4398 0000000000000001
[    9.780005]  0000000000000000 0000000000000000 0000000000000041 00000000000fffff
[    9.780005] Call Trace:
[    9.780005]  [<ffffffffc01bf8c6>] ? hv_do_hypercall+0x76/0xc0 [hv_vmbus]
[    9.780005]  [<ffffffffc03fe1be>] hv_irq_unmask+0x10e/0x140 [pci_hyperv]
[    9.780005]  [<ffffffff9bce69ea>] irq_enable+0x3a/0x50
[    9.780005]  [<ffffffff9bce6a77>] irq_startup+0x77/0x80
[    9.780005]  [<ffffffff9c0230f0>] ? alloc_cpumask_var_node+0x20/0x30
[    9.780005]  [<ffffffff9bce523b>] __setup_irq+0x5cb/0x660
[    9.780005]  [<ffffffff9bce54a2>] request_threaded_irq+0x112/0x1e0
[    9.780005]  [<ffffffffc149661b>] mlx4_init_eq_table+0x3db/0x620 [mlx4_core]
[    9.780005]  [<ffffffffc149f888>] mlx4_setup_hca+0x1f8/0x7a0 [mlx4_core]
[    9.780005]  [<ffffffffc14a2c74>] mlx4_load_one+0xb34/0x1690 [mlx4_core]
[    9.780005]  [<ffffffffc14a3ce0>] mlx4_init_one+0x510/0x6e0 [mlx4_core]
[    9.780005]  [<ffffffff9c07b605>] local_pci_probe+0x45/0xa0
[    9.780005]  [<ffffffff9c07ca6d>] pci_device_probe+0xfd/0x140
[    9.780005]  [<ffffffff9c1a5894>] driver_probe_device+0x224/0x430
[    9.780005]  [<ffffffff9c1a5b7f>] __driver_attach+0xdf/0xf0
[    9.780005]  [<ffffffff9c1a5aa0>] ? driver_probe_device+0x430/0x430
[    9.780005]  [<ffffffff9c1a33cc>] bus_for_each_dev+0x6c/0xc0
[    9.780005]  [<ffffffff9c1a4fce>] driver_attach+0x1e/0x20
[    9.780005]  [<ffffffff9c1a4a8d>] bus_add_driver+0x1fd/0x270
[    9.780005]  [<ffffffffc062b000>] ? 0xffffffffc062b000
[    9.780005]  [<ffffffff9c1a64f0>] driver_register+0x60/0xe0
[    9.780005]  [<ffffffff9c07aefc>] __pci_register_driver+0x4c/0x50
[    9.780005]  [<ffffffffc062b115>] mlx4_init+0x115/0x1000 [mlx4_core]
[    9.780005]  [<ffffffff9bc02190>] do_one_initcall+0x50/0x180
[    9.780005]  [<ffffffff9be19aab>] ? kmem_cache_alloc_trace+0xdb/0x1c0
[    9.780005]  [<ffffffff9bda639b>] do_init_module+0x5f/0x1f6
[    9.780005]  [<ffffffff9bd14905>] load_module+0x2575/0x2930
[    9.780005]  [<ffffffff9bd11190>] ? __symbol_put+0x60/0x60
[    9.780005]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[    9.780005]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[    9.780005]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[    9.780005]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[    9.780005]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[    9.780005] Code: <0f> 01 c1 c3 8b c8 b8 11 00 00 00 0f 01 c1 c3 48 8b c1 48 c7 c1 11 
[    9.780005] RIP  [<ffffa998988e9000>] 0xffffa998988e9000
[    9.780005]  RSP <ffffa9989a16b7a8>
[    9.780005] ---[ end trace 629a28f2342edeaa ]---
[[0;32m  OK  [0m] [   10.135212] cloud-init[1225]: Cloud-init v. 18.4-0ubuntu1~16.04.2 running 'init-local' at Wed, 20 Feb 2019 09:52:29 +0000. Up 9.53 seconds.
Started LSB: AppArmor initialization.
[[0;32m  OK  [0m] Started Initial cloud-init job (pre-networking).
[[0;32m  OK  [0m] Reached target Network (Pre).
         Starting Raise network interfaces...
[[0;32m  OK  [0m] Started ifup for pnet0.
[   10.169944] pnet0: port 1(eth0) entered blocking state
[   10.176172] pnet0: port 1(eth0) entered forwarding state
[[0;32m  OK  [0m] Started ifup for pnet1.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet1.
[[0;32m  OK  [0m] Started ifup for pnet2.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet2.
[[0;32m  OK  [0m] Started ifup for pnet3.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet3.
[[0;32m  OK  [0m] Started ifup for pnet4.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet4.
[[0;32m  OK  [0m] Started ifup for pnet5.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet5.
[[0;32m  OK  [0m] Started ifup for pnet6.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet6.
[[0;32m  OK  [0m] Started ifup for pnet7.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet7.
[[0;32m  OK  [0m] Started ifup for pnet8.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet8.
[[0;32m  OK  [0m] Started ifup for pnet9.
[[0;32m  OK  [0m] Found device /sys/subsystem/net/devices/pnet9.
[[0;32m  OK  [0m] Started Raise network interfaces.
[[0;32m  OK  [0m] Reached target Network.
         Starting Initial cloud-init job (metadata service crawler)...
[   13.864200] blk_update_request: I/O error, dev fd0, sector 0
[   13.868023] floppy: error -5 while reading block 0
[   13.963406] cloud-init[3650]: Cloud-init v. 18.4-0ubuntu1~16.04.2 running 'init' at Wed, 20 Feb 2019 09:52:34 +0000. Up 13.65 seconds.
[   13.970770] cloud-init[3650]: ci-info: +++++++++++++++++++++++++++++++++++++++Net device info+++++++++++++++++++++++++++++++++++++++
[   13.977459] cloud-init[3650]: ci-info: +--------+------+------------------------------+---------------+--------+-------------------+
[   13.983105] cloud-init[3650]: ci-info: | Device |  Up  |           Address            |      Mask     | Scope  |     Hw-Address    |
[   13.988867] cloud-init[3650]: ci-info: +--------+------+------------------------------+---------------+--------+-------------------+
[   13.994942] cloud-init[3650]: ci-info: |  eth0  | True | fe80::20d:3aff:fe43:d6d4/64  |       .       |  link  | 00:0d:3a:43:d6:d4 |
[[0;32m  OK  [0m[   14.000504] ] cloud-init[3650]: ci-info: |   lo   | True |          127.0.0.1           |   255.0.0.0   |  host  |         .         |
Started Initial cloud-init job (metadata service crawler).
[   14.000688] cloud-init[3650]: ci-info: |   lo   | True |           ::1/128            |       .       |  host  |         .         |
[[0;32m  OK  [0m] Reached target Cloud-config availability.[   14.022418] cloud-init[3650]: ci-info: | pnet0  | True |           10.0.1.5           | 255.255.255.0 | global | 00:0d:3a:43:d6:d4 |

[   14.022542] cloud-init[3650]: ci-info: | pnet0  | True | fe80::20d:3aff:fe43:d6d4/64  |       .       |  link  | 00:0d:3a:43:d6:d4 |
[[   14.035143] cloud-init[3650]: ci-info: | pnet1  | True | fe80::c888:67ff:fe63:64f2/64 |       .       |  link  | ca:88:67:63:64:f2 |
[0;32m  OK  [0m] Reached target System Initialization.
[   14.035842] cloud-init[3650]: ci-info: | pnet2  | True | fe80::a49a:b7ff:fe1a:6402/64 |       .       |  link  | a6:9a:b7:1a:64:02 |
[   14.061526] cloud-init[3650]: ci-info: | pnet3  | True | fe80::84b4:eeff:fea3:182b/64 |       .       |  link  | 86:b4:ee:a3:18:2b |
         Starting LXD - unix socket.
[   14.067341] cloud-init[3650]: ci-info: | pnet4  | True | fe80::e4c9:d8ff:fe72:c5f6/64 |       .       |  link  | e6:c9:d8:72:c5:f6 |
[   14.074695] [cloud-init[0;32m  OK  [0m[3650]: ] ci-info: | pnet5  | True | fe80::bc91:2aff:fe46:238e/64 |       .       |  link  | be:91:2a:46:23:8e |Listening on UUID daemon activation socket.

[   14.074795] cloud-init[3650]: ci-info: | pnet6  | True | fe80::a065:e1ff:fe5b:d371/64 |       .       |  link  | a2:65:e1:5b:d3:71 |
[   14.088313] cloud-init[3650]: ci-info: | pnet7  | True | fe80::fc77:1fff:fe2c:3016/64 |       .       |  link  | fe:77:1f:2c:30:16 |
[   14.094149] cloud-init[3650]: ci-info: | pnet8  | True | fe80::a444:52ff:fe16:b841/64 |       .       |  link  | a6:44:52:16:b8:41 |
[[0;32m  OK  [0m] Started ACPI Events Check.[   14.100808] 
cloud-init[3650]: ci-info: | pnet9  | True |         10.132.0.10          | 255.255.240.0 | global | 32:97:be:c8:27:c9 |
[[   14.108134] [0;32m  OK  [0mcloud-init] [3650]: Reached target Paths.ci-info: | pnet9  | True | fe80::3097:beff:fec8:27c9/64 |       .       |  link  | 32:97:be:c8:27:c9 |

         Starting Socket activation for snappy daemon.[   14.118297] cloud-init[3650]: ci-info: +--------+------+------------------------------+---------------+--------+-------------------+

[   14.119860] cloud-init[3650]: ci-info: ++++++++++++++++++++++++++++++Route IPv4 info+++++++++++++++++++++++++++++++
[   14.131335] cloud-init[3650]: ci-info: +-------+-----------------+----------+-----------------+-----------+-------+
[   14.136098] cloud-init[3650]: ci-info: | Route |   Destination   | Gateway  |     Genmask     | Interface | Flags |
[   14.140647] cloud-init[3650]: ci-info: +-------+-----------------+----------+-----------------+-----------+-------+
[   14.140778] cloud-init[3650]: ci-info: |   0   |     0.0.0.0     | 10.0.1.1 |     0.0.0.0     |   pnet0   |   UG  |
[   14.141286] cloud-init[3650]: ci-info: |   1   |     10.0.1.0    | 0.0.0.0  |  255.255.255.0  |   pnet0   |   U   |
[   14.141871] cloud-init[3650]: ci-info: |   2   |    10.132.0.0   | 0.0.0.0  |  255.255.240.0  |   pnet9   |   U   |
[   14.142413] cloud-init[3650]: ci-info: |   3   |  168.63.129.16  | 10.0.1.1 | 255.255.255.255 |   pnet0   |  UGH  |
[   14.142995] cloud-init[3650]: ci-info: |   4   | 169.254.169.254 | 10.0.1.1 | 255.255.255.255 |   pnet0   |  UGH  |
[   14.143475] cloud-init[3650]: ci-info: +-------+-----------------+----------+-----------------+-----------+-------+
[   14.144825] cloud-init[3650]: ci-info: +++++++++++++++++++Route IPv6 info+++++++++++++++++++
[   14.145429] cloud-init[3650]: ci-info: +-------+-------------+---------+-----------+-------+
[   14.146134] cloud-init[3650]: ci-info: | Route | Destination | Gateway | Interface | Flags |
[   14.146795] cloud-init[3650]: ci-info: +-------+-------------+---------+-----------+-------+
[   14.147309] cloud-init[3650]: ci-info: |   0   |  fe80::/64  |    ::   |    eth0   |   U   |
[   14.147963] cloud-init[3650]: ci-info: |   1   |  fe80::/64  |    ::   |   pnet0   |   U   |
[   14.148552] cloud-init[3650]: ci-info: |   2   |  fe80::/64  |    ::   |   pnet1   |   U   |
[   14.149126] cloud-init[3650]: ci-info: |   3   |  fe80::/64  |    ::   |   pnet2   |   U   |
[   14.149551] cloud-init[3650]: ci-info: |   4   |  fe80::/64  |    ::   |   pnet3   |   U   |
[   14.150099] cloud-init[3650]: ci-info: |   5   |  fe80::/64  |    ::   |   pnet4   |   U   |
[   14.150645] cloud-init[3650]: ci-info: |   6   |  fe80::/64  |    ::   |   pnet5   |   U   |
[   14.151113] cloud-init[3650]: ci-info: |   7   |  fe80::/64  |    ::   |   pnet6   |   U   |
[   14.151539] cloud-init[3650]: ci-info: |   8   |  fe80::/64  |    ::   |   pnet7   |   U   |
[   14.152122] cloud-init[3650]: ci-info: |   9   |  fe80::/64  |    ::   |   pnet8   |   U   |
[   14.152763] cloud-init[3650]: ci-info: |   10  |  fe80::/64  |    ::   |   pnet9   |   U   |
[   14.153459] cloud-init[3650]: ci-info: |   19  |   ff00::/8  |    ::   |    eth0   |   U   |
[   14.153889] cloud-init[3650]: ci-info: |   20  |   ff00::/8  |    ::   |   pnet0   |   U   |
[   14.154439] cloud-init[3650]: ci-info: |   21  |   ff00::/8  |    ::   |   pnet1   |   U   |
[   14.155003] cloud-init[3650]: ci-info: |   22  |   ff00::/8  |    ::   |   pnet2   |   U   |
[   14.155486] cloud-init[3650]: ci-info: |   23  |   ff00::/8  |    ::   |   pnet3   |   U   |
[   14.156071] cloud-init[3650]: ci-info: |   24  |   ff00::/8  |    ::   |   pnet4   |   U   |
[   14.157183] cloud-init[3650]: ci-info: |   25  |   ff00::/8  |    ::   |   pnet5   |   U   |
[   14.157780] cloud-init[3650]: ci-info: |   26  |   ff00::/8  |    ::   |   pnet6   |   U   |
[   14.158329] cloud-init[3650]: ci-info: |   27  |   ff00::/8  |    ::   |   pnet7   |   U   |
[   14.158852] cloud-init[3650]: ci-info: |   28  |   ff00::/8  |    ::   |   pnet8   |   U   |
[   14.159352] cloud-init[3650]: ci-info: |   29  |   ff00::/8  |    ::   |   pnet9   |   U   |
[   14.159855] cloud-init[3650]: ci-info: +-------+-------------+---------+-----------+-------+
[[0;32m  OK  [0m] Started Daily apt download activities.
[[0;32m  OK  [0m] Started Daily apt upgrade and clean activities.
[[0;32m  OK  [0m] Started Daily Cleanup of Temporary Directories.
[[0;32m  OK  [0m] Listening on ACPID Listen Socket.
[[0;32m  OK  [0m] Listening on D-Bus System Message Bus Socket.
[[0;32m  OK  [0m] Reached target Timers.
         Mounting /mnt...
[[0;32m  OK  [0m] Reached target Network is Online.
         Starting iSCSI initiator daemon (iscsid)...
[[0;32m  OK  [0m] Listening on LXD - unix socket.
[[0;32m  OK  [0m] Listening on Socket activation for snappy daemon.
[[0;32m  OK  [0m] Started iSCSI initiator daemon (iscsid).
[   14.314734] EXT4-fs (sdb1): mounted filesystem with ordered data mode. Opts: (null)
         Starting Login to default iSCSI targets...
[[0;32m  OK  [0m] Reached target Sockets.
[[0;32m  OK  [0m] Reached target Basic System.
         Starting Login Service...
[[0;32m  OK  [0m] Started CPU Limit for Qemu.
[[0;32m  OK  [0m] Started ACPI event daemon.
[[0;32m  OK  [0m] Started Hyper-V File Copy Protocol Daemon.
         Starting MySQL Community Server...
[[0;32m  OK  [0m] Started OS configuration for EVE-NG.
         Starting /etc/rc.local Compatibility...
[[0;32m  OK  [0m] Started D-Bus System Message Bus.
[[0;32m  OK  [0m] Started Unattended Upgrades Shutdown.
         Starting LSB: MD monitoring daemon...
[[0;32m  OK  [0m] Started Azure Linux Agent.
         Starting LSB: Guacamole proxy daemon...
[[0;32m  OK  [0m] Started FUSE filesystem for LXC.
         Starting LSB: Re[   14.407068] cgroup: new mount options do not match the existing superblock, will be ignored
cord successful boot for GRUB...
         Starting System Logging Service...
         Starting Open vSwitch Internal Unit...
[[0;32m  OK  [0m] Started Deferred execution scheduler.
[[0;32m  OK  [0m] Started Regular background program processing daemon.
         Starting Accounts Service...
         Starting OpenBSD Secure Shell server...
         Starting Snappy daemon...
         Starting LXD - container startup/shutdown...
[[0;32m  OK  [0m] Started Hyper-V VSS Protocol Daemon.
[[0;32m  OK  [0m] Started Name Service Cache Daemon.
[[0;32m  OK  [0m] Mounted /mnt.
[[0;32m  OK  [0m] Started System Logging Service.
[[0;32m  OK  [0m] Started /etc/rc.local Compatibility.
[[0;32m  OK  [0m] Started LSB: Guacamole proxy daemon.
[[0;32m  OK  [0m] Started OpenBSD Secure Shell server.
[   14.510145] audit_printk_skb: 15 callbacks suppressed
[   14.515542] audit: type=1400 audit(1550656354.913:17): apparmor="DENIED" operation="open" profile="/usr/sbin/mysqld" name="/proc/4004/status" pid=4004 comm="mysqld" requested_mask="r" denied_mask="r" fsuid=113 ouid=113
[   14.530303] audit: type=1400 audit(1550656354.933:18): apparmor="DENIED" operation="open" profile="/usr/sbin/mysqld" name="/sys/devices/system/node/" pid=4004 comm="mysqld" requested_mask="r" denied_mask="r" fsuid=113 ouid=0
[   14.544200] audit: type=1400 audit(1550656354.949:19): apparmor="DENIED" operation="open" profile="/usr/sbin/mysqld" name="/proc/4004/status" pid=4004 comm="mysqld" requested_mask="r" denied_mask="r" fsuid=113 ouid=113
2019/02/20 09:52:35.006480 INFO Daemon Azure Linux Agent Version:2.2.32.2
2019/02/20 09:52:35.009968 INFO Daemon OS: ubuntu 16.04
2019/02/20 09:52:35.012831 INFO Daemon Python: 3.5.2
2019/02/20 09:52:35.036985 INFO Daemon Creating cgroup directory /sys/fs/cgroup/cpu/system.slice/walinuxagent
2019/02/20 09:52:35.042776 INFO Daemon Creating cgroup directory /sys/fs/cgroup/memory/system.slice/walinuxagent
2019/02/20 09:52:35.050530 INFO Daemon Add daemon process pid 3849 to walinuxagent systemd cgroup
2019/02/20 09:52:35.056037 INFO Daemon CGroups: ok
[[0;32m  OK  [0m] Started Login to default iSCSI targets.
2019/02/20 09:52:35.062144 INFO Daemon Run daemon
[[0;32m  OK  [0m] Started LSB: MD monitoring daemon.
2019/02/20 09:52:35.065996 INFO Daemon Clean protocol
2019/02/20 09:52:35.071017 INFO Daemon Provisioning already completed, skipping.
2019/02/20 09:52:35.075076 INFO Daemon RDMA capabilities are not enabled, skipping
[[0;32m  OK  [0m] Started Login Service.
         Starting Authenticate and Authorize Users to Run Privileged Tasks...
[[0;32m  OK  [0m] Reached target Remote File Systems (Pre).
[[0;32m  OK  [0m] Reached target Remote File Systems.
         Starting LSB: Start Tomcat....
2019/02/20 09:52:35.096626 INFO Daemon Determined Agent WALinuxAgent-2.2.36 to be the latest agent
         Starting LSB: Start NTP daemon...
         Starting LSB: automatic crash report generation...
         Starting LSB: Simple OpenFlow controller for testing...
         Starting Permit User Sessions...
         Starting LSB: Set the CPU Frequency Scaling governor to "ondemand"...
         Starting LSB: daemon to balance interrupts for SMP systems...
         Starting LSB: Apache2 web server...
         Starting LSB: start and stop UML networking services...
[[0;32m  OK  [0m] Started LSB: Record successful boot for GRUB.
[[0;32m  OK  [0m] Started LXD - container startup/shutdown.
[[0;32m  OK  [0m] Started Permit User Sessions.
[[0;32m  OK  [0m] Started Authenticate and Authorize Users to Run Privileged Tasks.
[[0;32m  OK  [0m] Started Accounts Service.
[[0;32m  OK  [0m] Started Snappy daemon.
2019/02/20 09:52:35.464763 INFO ExtHandler Agent WALinuxAgent-2.2.36 is running as the goal state agent
2019/02/20 09:52:35.489092 INFO ExtHandler Detect protocol endpoints
2019/02/20 09:52:35.493285 INFO ExtHandler Clean protocol
2019/02/20 09:52:35.496950 INFO ExtHandler WireServer endpoint is not found. Rerun dhcp handler
2019/02/20 09:52:35.501924 INFO ExtHandler Test for route to 168.63.129.16
2019/02/20 09:52:35.506140 INFO ExtHandler Route to 168.63.129.16 exists
2019/02/20 09:52:35.509351 INFO ExtHandler Wire server endpoint:168.63.129.16
2019/02/20 09:52:35.520689 INFO ExtHandler Fabric preferred wire protocol version:2015-04-05
2019/02/20 09:52:35.531983 INFO ExtHandler Wire protocol version:2012-11-30
2019/02/20 09:52:35.537188 INFO ExtHandler Server preferred version:2015-04-05


[   40.636002] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[   40.636002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[   40.636002] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D         4.9.40-eve-ng-ukms-2+ #4
[   40.636002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[   40.636002] Workqueue: events netstamp_clear
[   40.636002] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[   40.636002] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[   40.636002] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[   40.636002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[   40.636002] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[   40.636002] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[   40.636002] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[   40.636002] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[   40.636002] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[   40.636002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[   40.636002] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[   40.636002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[   40.636002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[   40.636002] Stack:
[   40.636002]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[   40.636002]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[   40.636002]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[   40.636002] Call Trace:
[   40.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[   40.636002]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[   40.636002]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[   40.636002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[   40.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[   40.636002]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[   40.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[   40.636002]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[   40.636002]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[   40.636002]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[   40.636002]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[   40.636002]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[   40.636002]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[   40.636002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[   40.636002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[   40.636002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[   40.636002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[   40.636002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[   40.636002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[   40.636002] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[   41.436008] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[   41.436008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[   41.436008] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[   41.436008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[   41.436008] task: ffff885b12860000 task.stack: ffffa998a8f58000
[   41.436008] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[   41.436008] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[   41.436008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[   41.436008] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[   41.436008] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[   41.436008] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[   41.436008] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[   41.436008] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[   41.436008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[   41.436008] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[   41.436008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[   41.436008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[   41.436008] Stack:
[   41.436008]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[   41.436008]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[   41.436008]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[   41.436008] Call Trace:
[   41.436008]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[   41.436008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[   41.436008]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[   41.436008]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[   41.436008]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[   41.436008]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[   41.436008]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[   41.436008]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[   41.436008]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[   41.436008]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[   41.436008]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[   41.436008]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[   41.436008]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[   41.436008]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[   41.436008]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[   41.436008]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[   41.436008] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[   44.228010] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 23s! [kworker/0:3:4418]
[   44.228010] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[   44.228010] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[   44.228010] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[   44.228010] Workqueue: events hv_set_host_time [hv_utils]
[   44.228010] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[   44.228010] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[   44.228010] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[   44.228010] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[   44.228010] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[   44.228010] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[   44.228010] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[   44.228010] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[   44.228010] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[   44.228010] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[   44.228010] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[   44.228010] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[   44.228010] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[   44.228010] Stack:
[   44.228010]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[   44.228010]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[   44.228010]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[   44.228010] Call Trace:
[   44.228010]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[   44.228010]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[   44.228010]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[   44.228010]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[   44.228010]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[   44.228010]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[   44.228010]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[   44.228010]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[   44.228010]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[   44.228010]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[   44.228010]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[   44.228010] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[   68.636005] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[   68.636005] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[   68.636005] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[   68.636005] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[   68.636005] Workqueue: events netstamp_clear
[   68.636005] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[   68.636005] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[   68.636005] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[   68.636005] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[   68.636005] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[   68.636005] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[   68.636005] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[   68.636005] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[   68.636005] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[   68.636005] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[   68.636005] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[   68.636005] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[   68.636005] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[   68.636005] Stack:
[   68.636005]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[   68.636005]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[   68.636005]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[   68.636005] Call Trace:
[   68.636005]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[   68.636005]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[   68.636005]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[   68.636005]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[   68.636005]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[   68.636005]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[   68.636005]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[   68.636005]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[   68.636005]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[   68.636005]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[   68.636005]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[   68.636005]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[   68.636005]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[   68.636005]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[   68.636005]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[   68.636005]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[   68.636005]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[   68.636005]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[   68.636005]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[   68.636005] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[   69.436009] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[   69.436009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[   69.436009] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[   69.436009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[   69.436009] task: ffff885b12860000 task.stack: ffffa998a8f58000
[   69.436009] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[   69.436009] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[   69.436009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[   69.436009] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[   69.436009] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[   69.436009] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[   69.436009] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[   69.436009] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[   69.436009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[   69.436009] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[   69.436009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[   69.436009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[   69.436009] Stack:
[   69.436009]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[   69.436009]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[   69.436009]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[   69.436009] Call Trace:
[   69.436009]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[   69.436009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[   69.436009]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[   69.436009]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[   69.436009]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[   69.436009]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[   69.436009]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[   69.436009]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[   69.436009]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[   69.436009]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[   69.436009]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[   69.436009]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[   69.436009]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[   69.436009]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[   69.436009]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[   69.436009]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[   69.436009] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[   72.228002] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 23s! [kworker/0:3:4418]
[   72.228002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[   72.228002] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[   72.228002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[   72.228002] Workqueue: events hv_set_host_time [hv_utils]
[   72.228002] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[   72.228002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[   72.228002] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[   72.228002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[   72.228002] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[   72.228002] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[   72.228002] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[   72.228002] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[   72.228002] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[   72.228002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[   72.228002] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[   72.228002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[   72.228002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[   72.228002] Stack:
[   72.228002]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[   72.228002]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[   72.228002]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[   72.228002] Call Trace:
[   72.228002]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[   72.228002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[   72.228002]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[   72.228002]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[   72.228002]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[   72.228002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[   72.228002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[   72.228002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[   72.228002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[   72.228002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[   72.228002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[   72.228002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[   74.928030] INFO: rcu_sched detected stalls on CPUs/tasks:
[   74.932025] 	6-...: (1 GPs behind) idle=df3/140000000000000/0 softirq=930/931 fqs=6544 
[   74.932025] 	(detected by 11, t=15004 jiffies, g=242, c=241, q=26991)
[   74.932025] Task dump for CPU 6:
[   74.932025] irqbalance      R  running task        0  4286   4204 0x00000008
[   74.932025]  ffff885c1ffd6240 0000000000000001 fffffffffffffff8 ffffa998a8e5b8e0
[   74.932025]  ffff885c1ffd7080 0000000000000200 0000000000000012 0000000000000001
[   74.932025]  ffff885c1ffd5d00 0000000000000246 1bea1e9cd88269b0 ffff885c1ffd7080
[   74.932025] Call Trace:
[   74.932025]  [<ffffffff9be33316>] ? memcg_kmem_charge_memcg+0x76/0x90
[   74.932025]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[   74.932025]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[   74.932025]  [<ffffffff9be19cf7>] ? kmem_cache_alloc+0xd7/0x1b0
[   74.932025]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[   74.932025]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[   74.932025]  [<ffffffff9bcd1505>] ? mutex_optimistic_spin+0x145/0x190
[   74.932025]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[   74.932025]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[   74.932025]  [<ffffffff9be19aab>] ? kmem_cache_alloc_trace+0xdb/0x1c0
[   74.932025]  [<ffffffff9c02d628>] ? put_dec+0x18/0xa0
[   74.932025]  [<ffffffff9c02d9a0>] ? number+0x2f0/0x310
[   74.932025]  [<ffffffff9bec0df8>] ? interrupts_open+0x18/0x20
[   74.932025]  [<ffffffff9c02e203>] ? string+0x63/0x70
[   74.932025]  [<ffffffff9c0306aa>] ? vsnprintf+0x12a/0x4c0
[   74.932025]  [<ffffffff9be689f5>] ? seq_vprintf+0x35/0x50
[   74.932025]  [<ffffffff9be68a63>] ? seq_printf+0x53/0x70
[   74.932025]  [<ffffffff9c49eba7>] ? _raw_spin_lock_irqsave+0x37/0x3f
[   74.932025]  [<ffffffff9bceae24>] ? show_interrupts+0x114/0x330
[   74.932025]  [<ffffffff9be692f1>] ? seq_read+0x301/0x410
[   74.932025]  [<ffffffff9beb7ac2>] ? proc_reg_read+0x42/0x70
[   74.932025]  [<ffffffff9be41c47>] ? __vfs_read+0x37/0x150
[   74.932025]  [<ffffffff9bf848db>] ? security_file_permission+0x9b/0xc0
[   74.932025]  [<ffffffff9be42123>] ? vfs_read+0x93/0x130
[   74.932025]  [<ffffffff9be43645>] ? SyS_read+0x55/0xc0
[   74.932025]  [<ffffffff9c49ec3b>] ? entry_SYSCALL_64_fastpath+0x1e/0xad
[   96.636001] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[   96.636001] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[   96.636001] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[   96.636001] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[   96.636001] Workqueue: events netstamp_clear
[   96.636001] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[   96.636001] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[   96.636001] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[   96.636001] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[   96.636001] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[   96.636001] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[   96.636001] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[   96.636001] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[   96.636001] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[   96.636001] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[   96.636001] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[   96.636001] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[   96.636001] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[   96.636001] Stack:
[   96.636001]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[   96.636001]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[   96.636001]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[   96.636001] Call Trace:
[   96.636001]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[   96.636001]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[   96.636001]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[   96.636001]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[   96.636001]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[   96.636001]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[   96.636001]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[   96.636001]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[   96.636001]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[   96.636001]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[   96.636001]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[   96.636001]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[   96.636001]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[   96.636001]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[   96.636001]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[   96.636001]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[   96.636001]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[   96.636001]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[   96.636001]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[   96.636001] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[   97.436008] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[   97.436008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[   97.436008] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[   97.436008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[   97.436008] task: ffff885b12860000 task.stack: ffffa998a8f58000
[   97.436008] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[   97.436008] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[   97.436008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[   97.436008] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[   97.436008] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[   97.436008] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[   97.436008] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[   97.436008] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[   97.436008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[   97.436008] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[   97.436008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[   97.436008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[   97.436008] Stack:
[   97.436008]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[   97.436008]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[   97.436008]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[   97.436008] Call Trace:
[   97.436008]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[   97.436008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[   97.436008]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[   97.436008]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[   97.436008]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[   97.436008]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[   97.436008]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[   97.436008]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[   97.436008]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[   97.436008]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[   97.436008]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[   97.436008]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[   97.436008]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[   97.436008]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[   97.436008]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[   97.436008]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[   97.436008] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  100.228008] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 23s! [kworker/0:3:4418]
[  100.228008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  100.228008] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  100.228008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  100.228008] Workqueue: events hv_set_host_time [hv_utils]
[  100.228008] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  100.228008] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  100.228008] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  100.228008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  100.228008] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  100.228008] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  100.228008] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  100.228008] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  100.228008] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  100.228008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  100.228008] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  100.228008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  100.228008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  100.228008] Stack:
[  100.228008]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  100.228008]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  100.228008]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  100.228008] Call Trace:
[  100.228008]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  100.228008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  100.228008]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  100.228008]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  100.228008]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  100.228008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  100.228008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  100.228008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  100.228008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  100.228008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  100.228008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  100.228008] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  124.636008] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 23s! [kworker/5:1:278]
[  124.636008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  124.636008] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  124.636008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  124.636008] Workqueue: events netstamp_clear
[  124.636008] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  124.636008] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  124.636008] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  124.636008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  124.636008] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  124.636008] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  124.636008] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  124.636008] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  124.636008] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  124.636008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  124.636008] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  124.636008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  124.636008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  124.636008] Stack:
[  124.636008]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  124.636008]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  124.636008]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  124.636008] Call Trace:
[  124.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  124.636008]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  124.636008]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  124.636008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  124.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  124.636008]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  124.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  124.636008]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  124.636008]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  124.636008]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  124.636008]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  124.636008]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  124.636008]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  124.636008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  124.636008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  124.636008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  124.636008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  124.636008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  124.636008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  124.636008] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  125.436002] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  125.436002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  125.436002] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  125.436002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  125.436002] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  125.436002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  125.436002] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  125.436002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  125.436002] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  125.436002] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  125.436002] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  125.436002] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  125.436002] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  125.436002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  125.436002] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  125.436002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  125.436002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  125.436002] Stack:
[  125.436002]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  125.436002]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  125.436002]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  125.436002] Call Trace:
[  125.436002]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  125.436002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  125.436002]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  125.436002]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  125.436002]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  125.436002]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  125.436002]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  125.436002]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  125.436002]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  125.436002]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  125.436002]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  125.436002]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  125.436002]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  125.436002]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  125.436002]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  125.436002]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  125.436002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  128.228002] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  128.228002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  128.228002] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  128.228002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  128.228002] Workqueue: events hv_set_host_time [hv_utils]
[  128.228002] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  128.228002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  128.228002] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  128.228002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  128.228002] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  128.228002] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  128.228002] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  128.228002] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  128.228002] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  128.228002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  128.228002] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  128.228002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  128.228002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  128.228002] Stack:
[  128.228002]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  128.228002]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  128.228002]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  128.228002] Call Trace:
[  128.228002]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  128.228002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  128.228002]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  128.228002]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  128.228002]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  128.228002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  128.228002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  128.228002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  128.228002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  128.228002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  128.228002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  128.228002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  152.636008] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 23s! [kworker/5:1:278]
[  152.636008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  152.636008] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  152.636008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  152.636008] Workqueue: events netstamp_clear
[  152.636008] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  152.636008] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  152.636008] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  152.636008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  152.636008] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  152.636008] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  152.636008] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  152.636008] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  152.636008] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  152.636008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  152.636008] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  152.636008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  152.636008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  152.636008] Stack:
[  152.636008]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  152.636008]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  152.636008]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  152.636008] Call Trace:
[  152.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  152.636008]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  152.636008]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  152.636008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  152.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  152.636008]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  152.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  152.636008]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  152.636008]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  152.636008]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  152.636008]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  152.636008]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  152.636008]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  152.636008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  152.636008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  152.636008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  152.636008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  152.636008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  152.636008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  152.636008] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  153.436009] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  153.436009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  153.436009] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  153.436009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  153.436009] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  153.436009] RIP: 0010:[<ffffffff9bd0e74f>]  [<ffffffff9bd0e74f>] smp_call_function_many+0x1ef/0x250
[  153.436009] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  153.436009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  153.436009] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  153.436009] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  153.436009] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  153.436009] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  153.436009] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  153.436009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  153.436009] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  153.436009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  153.436009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  153.436009] Stack:
[  153.436009]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  153.436009]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  153.436009]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  153.436009] Call Trace:
[  153.436009]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  153.436009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  153.436009]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  153.436009]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  153.436009]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  153.436009]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  153.436009]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  153.436009]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  153.436009]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  153.436009]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  153.436009]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  153.436009]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  153.436009]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  153.436009]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  153.436009]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  153.436009]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  153.436009] Code: 08 48 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 <f3> 90 8b 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 
[  156.228010] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  156.228010] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  156.228010] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  156.228010] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  156.228010] Workqueue: events hv_set_host_time [hv_utils]
[  156.228010] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  156.228010] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  156.228010] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  156.228010] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  156.228010] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  156.228010] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  156.228010] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  156.228010] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  156.228010] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  156.228010] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  156.228010] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  156.228010] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  156.228010] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  156.228010] Stack:
[  156.228010]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  156.228010]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  156.228010]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  156.228010] Call Trace:
[  156.228010]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  156.228010]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  156.228010]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  156.228010]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  156.228010]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  156.228010]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  156.228010]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  156.228010]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  156.228010]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  156.228010]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  156.228010]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  156.228010] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  180.636004] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 23s! [kworker/5:1:278]
[  180.636004] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  180.636004] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  180.636004] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  180.636004] Workqueue: events netstamp_clear
[  180.636004] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  180.636004] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  180.636004] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  180.636004] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  180.636004] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  180.636004] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  180.636004] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  180.636004] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  180.636004] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  180.636004] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  180.636004] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  180.636004] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  180.636004] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  180.636004] Stack:
[  180.636004]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  180.636004]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  180.636004]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  180.636004] Call Trace:
[  180.636004]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  180.636004]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  180.636004]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  180.636004]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  180.636004]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  180.636004]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  180.636004]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  180.636004]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  180.636004]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  180.636004]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  180.636004]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  180.636004]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  180.636004]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  180.636004]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  180.636004]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  180.636004]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  180.636004]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  180.636004]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  180.636004]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  180.636004] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  181.436011] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  181.436011] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  181.436011] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  181.436011] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  181.436011] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  181.436011] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  181.436011] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  181.436011] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  181.436011] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  181.436011] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  181.436011] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  181.436011] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  181.436011] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  181.436011] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  181.436011] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  181.436011] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  181.436011] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  181.436011] Stack:
[  181.436011]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  181.436011]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  181.436011]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  181.436011] Call Trace:
[  181.436011]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  181.436011]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  181.436011]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  181.436011]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  181.436011]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  181.436011]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  181.436011]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  181.436011]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  181.436011]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  181.436011]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  181.436011]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  181.436011]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  181.436011]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  181.436011]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  181.436011]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  181.436011]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  181.436011] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  184.228002] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  184.228002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  184.228002] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  184.228002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  184.228002] Workqueue: events hv_set_host_time [hv_utils]
[  184.228002] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  184.228002] RIP: 0010:[<ffffffff9bd0e74f>]  [<ffffffff9bd0e74f>] smp_call_function_many+0x1ef/0x250
[  184.228002] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  184.228002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  184.228002] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  184.228002] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  184.228002] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  184.228002] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  184.228002] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  184.228002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  184.228002] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  184.228002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  184.228002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  184.228002] Stack:
[  184.228002]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  184.228002]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  184.228002]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  184.228002] Call Trace:
[  184.228002]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  184.228002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  184.228002]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  184.228002]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  184.228002]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  184.228002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  184.228002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  184.228002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  184.228002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  184.228002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  184.228002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  184.228002] Code: 08 48 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 <f3> 90 8b 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 
[  208.636005] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 23s! [kworker/5:1:278]
[  208.636005] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  208.636005] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  208.636005] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  208.636005] Workqueue: events netstamp_clear
[  208.636005] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  208.636005] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  208.636005] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  208.636005] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  208.636005] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  208.636005] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  208.636005] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  208.636005] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  208.636005] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  208.636005] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  208.636005] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  208.636005] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  208.636005] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  208.636005] Stack:
[  208.636005]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  208.636005]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  208.636005]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  208.636005] Call Trace:
[  208.636005]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  208.636005]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  208.636005]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  208.636005]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  208.636005]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  208.636005]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  208.636005]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  208.636005]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  208.636005]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  208.636005]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  208.636005]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  208.636005]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  208.636005]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  208.636005]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  208.636005]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  208.636005]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  208.636005]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  208.636005]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  208.636005]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  208.636005] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  209.436002] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 23s! [modprobe:4346]
[  209.436002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  209.436002] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  209.436002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  209.436002] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  209.436002] RIP: 0010:[<ffffffff9bd0e74f>]  [<ffffffff9bd0e74f>] smp_call_function_many+0x1ef/0x250
[  209.436002] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  209.436002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  209.436002] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  209.436002] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  209.436002] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  209.436002] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  209.436002] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  209.436002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  209.436002] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  209.436002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  209.436002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  209.436002] Stack:
[  209.436002]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  209.436002]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  209.436002]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  209.436002] Call Trace:
[  209.436002]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  209.436002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  209.436002]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  209.436002]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  209.436002]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  209.436002]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  209.436002]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  209.436002]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  209.436002]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  209.436002]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  209.436002]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  209.436002]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  209.436002]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  209.436002]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  209.436002]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  209.436002]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  209.436002] Code: 08 48 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 <f3> 90 8b 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 
[  212.228009] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  212.228009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  212.228009] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  212.228009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  212.228009] Workqueue: events hv_set_host_time [hv_utils]
[  212.228009] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  212.228009] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  212.228009] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  212.228009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  212.228009] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  212.228009] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  212.228009] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  212.228009] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  212.228009] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  212.228009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  212.228009] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  212.228009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  212.228009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  212.228009] Stack:
[  212.228009]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  212.228009]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  212.228009]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  212.228009] Call Trace:
[  212.228009]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  212.228009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  212.228009]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  212.228009]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  212.228009]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  212.228009]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  212.228009]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  212.228009]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  212.228009]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  212.228009]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  212.228009]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  212.228009] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  236.636005] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  236.636009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  236.636009] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  236.636009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  236.636009] Workqueue: events netstamp_clear
[  236.636009] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  236.636009] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  236.636009] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  236.636009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  236.636009] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  236.636009] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  236.636009] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  236.636009] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  236.636009] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  236.636009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  236.636009] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  236.636009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  236.636009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  236.636009] Stack:
[  236.636009]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  236.636009]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  236.636009]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  236.636009] Call Trace:
[  236.636009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  236.636009]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  236.636009]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  236.636009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  236.636009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  236.636009]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  236.636009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  236.636009]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  236.636009]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  236.636009]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  236.636009]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  236.636009]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  236.636009]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  236.636009]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  236.636009]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  236.636009]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  236.636009]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  236.636009]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  236.636009]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  236.636009] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  237.436002] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 23s! [modprobe:4346]
[  237.436002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  237.436002] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  237.436002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  237.436002] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  237.436002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  237.436002] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  237.436002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  237.436002] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  237.436002] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  237.436002] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  237.436002] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  237.436002] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  237.436002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  237.436002] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  237.436002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  237.436002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  237.436002] Stack:
[  237.436002]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  237.436002]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  237.436002]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  237.436002] Call Trace:
[  237.436002]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  237.436002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  237.436002]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  237.436002]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  237.436002]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  237.436002]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  237.436002]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  237.436002]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  237.436002]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  237.436002]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  237.436002]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  237.436002]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  237.436002]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  237.436002]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  237.436002]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  237.436002]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  237.436002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  240.228008] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  240.228008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  240.228008] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  240.228008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  240.228008] Workqueue: events hv_set_host_time [hv_utils]
[  240.228008] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  240.228008] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  240.228008] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  240.228008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  240.228008] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  240.228008] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  240.228008] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  240.228008] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  240.228008] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  240.228008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  240.228008] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  240.228008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  240.228008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  240.228008] Stack:
[  240.228008]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  240.228008]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  240.228008]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  240.228008] Call Trace:
[  240.228008]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  240.228008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  240.228008]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  240.228008]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  240.228008]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  240.228008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  240.228008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  240.228008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  240.228008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  240.228008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  240.228008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  240.228008] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  254.948082] INFO: rcu_sched detected stalls on CPUs/tasks:
[  254.952078] 	6-...: (1 GPs behind) idle=df3/140000000000000/0 softirq=930/931 fqs=26182 
[  254.952078] 	(detected by 16, t=60009 jiffies, g=242, c=241, q=32191)
[  254.952078] Task dump for CPU 6:
[  254.952078] irqbalance      R  running task        0  4286   4204 0x00000008
[  254.952078]  ffff885c1ffd6240 0000000000000001 fffffffffffffff8 ffffa998a8e5b8e0
[  254.952078]  ffff885c1ffd7080 0000000000000200 0000000000000012 0000000000000001
[  254.952078]  ffff885c1ffd5d00 0000000000000246 1bea1e9cd88269b0 ffff885c1ffd7080
[  254.952078] Call Trace:
[  254.952078]  [<ffffffff9be33316>] ? memcg_kmem_charge_memcg+0x76/0x90
[  254.952078]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  254.952078]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  254.952078]  [<ffffffff9be19cf7>] ? kmem_cache_alloc+0xd7/0x1b0
[  254.952078]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  254.952078]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  254.952078]  [<ffffffff9bcd1505>] ? mutex_optimistic_spin+0x145/0x190
[  254.952078]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  254.952078]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  254.952078]  [<ffffffff9be19aab>] ? kmem_cache_alloc_trace+0xdb/0x1c0
[  254.952078]  [<ffffffff9c02d628>] ? put_dec+0x18/0xa0
[  254.952078]  [<ffffffff9c02d9a0>] ? number+0x2f0/0x310
[  254.952078]  [<ffffffff9bec0df8>] ? interrupts_open+0x18/0x20
[  254.952078]  [<ffffffff9c02e203>] ? string+0x63/0x70
[  254.952078]  [<ffffffff9c0306aa>] ? vsnprintf+0x12a/0x4c0
[  254.952078]  [<ffffffff9be689f5>] ? seq_vprintf+0x35/0x50
[  254.952078]  [<ffffffff9be68a63>] ? seq_printf+0x53/0x70
[  254.952078]  [<ffffffff9c49eba7>] ? _raw_spin_lock_irqsave+0x37/0x3f
[  254.952078]  [<ffffffff9bceae24>] ? show_interrupts+0x114/0x330
[  254.952078]  [<ffffffff9be692f1>] ? seq_read+0x301/0x410
[  254.952078]  [<ffffffff9beb7ac2>] ? proc_reg_read+0x42/0x70
[  254.952078]  [<ffffffff9be41c47>] ? __vfs_read+0x37/0x150
[  254.952078]  [<ffffffff9bf848db>] ? security_file_permission+0x9b/0xc0
[  254.952078]  [<ffffffff9be42123>] ? vfs_read+0x93/0x130
[  254.952078]  [<ffffffff9be43645>] ? SyS_read+0x55/0xc0
[  254.952078]  [<ffffffff9c49ec3b>] ? entry_SYSCALL_64_fastpath+0x1e/0xad
[  264.636002] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  264.636002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  264.636002] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  264.636002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  264.636002] Workqueue: events netstamp_clear
[  264.636002] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  264.636002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  264.636002] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  264.636002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  264.636002] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  264.636002] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  264.636002] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  264.636002] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  264.636002] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  264.636002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  264.636002] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  264.636002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  264.636002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  264.636002] Stack:
[  264.636002]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  264.636002]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  264.636002]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  264.636002] Call Trace:
[  264.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  264.636002]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  264.636002]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  264.636002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  264.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  264.636002]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  264.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  264.636002]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  264.636002]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  264.636002]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  264.636002]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  264.636002]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  264.636002]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  264.636002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  264.636002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  264.636002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  264.636002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  264.636002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  264.636002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  264.636002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  265.436002] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 23s! [modprobe:4346]
[  265.436002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  265.436002] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  265.436002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  265.436002] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  265.436002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  265.436002] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  265.436002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  265.436002] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  265.436002] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  265.436002] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  265.436002] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  265.436002] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  265.436002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  265.436002] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  265.436002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  265.436002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  265.436002] Stack:
[  265.436002]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  265.436002]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  265.436002]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  265.436002] Call Trace:
[  265.436002]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  265.436002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  265.436002]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  265.436002]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  265.436002]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  265.436002]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  265.436002]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  265.436002]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  265.436002]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  265.436002]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  265.436002]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  265.436002]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  265.436002]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  265.436002]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  265.436002]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  265.436002]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  265.436002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  268.228006] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  268.228010] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  268.228010] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  268.228010] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  268.228010] Workqueue: events hv_set_host_time [hv_utils]
[  268.228010] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  268.228010] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  268.228010] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  268.228010] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  268.228010] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  268.228010] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  268.228010] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  268.228010] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  268.228010] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  268.228010] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  268.228010] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  268.228010] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  268.228010] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  268.228010] Stack:
[  268.228010]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  268.228010]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  268.228010]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  268.228010] Call Trace:
[  268.228010]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  268.228010]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  268.228010]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  268.228010]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  268.228010]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  268.228010]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  268.228010]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  268.228010]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  268.228010]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  268.228010]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  268.228010]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  268.228010] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  292.636008] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  292.636008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  292.636008] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  292.636008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  292.636008] Workqueue: events netstamp_clear
[  292.636008] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  292.636008] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  292.636008] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  292.636008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  292.636008] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  292.636008] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  292.636008] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  292.636008] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  292.636008] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  292.636008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  292.636008] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  292.636008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  292.636008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  292.636008] Stack:
[  292.636008]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  292.636008]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  292.636008]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  292.636008] Call Trace:
[  292.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  292.636008]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  292.636008]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  292.636008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  292.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  292.636008]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  292.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  292.636008]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  292.636008]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  292.636008]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  292.636008]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  292.636008]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  292.636008]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  292.636008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  292.636008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  292.636008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  292.636008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  292.636008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  292.636008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  292.636008] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  293.436002] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 23s! [modprobe:4346]
[  293.436002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  293.436002] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  293.436002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  293.436002] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  293.436002] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  293.436002] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  293.436002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  293.436002] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  293.436002] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  293.436002] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  293.436002] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  293.436002] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  293.436002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  293.436002] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  293.436002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  293.436002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  293.436002] Stack:
[  293.436002]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  293.436002]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  293.436002]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  293.436002] Call Trace:
[  293.436002]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  293.436002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  293.436002]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  293.436002]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  293.436002]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  293.436002]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  293.436002]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  293.436002]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  293.436002]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  293.436002]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  293.436002]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  293.436002]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  293.436002]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  293.436002]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  293.436002]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  293.436002]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  293.436002] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  296.228002] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  296.228002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  296.228002] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  296.228002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  296.228002] Workqueue: events hv_set_host_time [hv_utils]
[  296.228002] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  296.228002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  296.228002] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  296.228002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  296.228002] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  296.228002] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  296.228002] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  296.228002] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  296.228002] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  296.228002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  296.228002] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  296.228002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  296.228002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  296.228002] Stack:
[  296.228002]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  296.228002]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  296.228002]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  296.228002] Call Trace:
[  296.228002]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  296.228002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  296.228002]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  296.228002]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  296.228002]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  296.228002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  296.228002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  296.228002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  296.228002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  296.228002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  296.228002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  296.228002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  320.636002] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  320.636002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  320.636002] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  320.636002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  320.636002] Workqueue: events netstamp_clear
[  320.636002] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  320.636002] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  320.636002] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  320.636002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  320.636002] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  320.636002] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  320.636002] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  320.636002] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  320.636002] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  320.636002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  320.636002] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  320.636002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  320.636002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  320.636002] Stack:
[  320.636002]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  320.636002]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  320.636002]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  320.636002] Call Trace:
[  320.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  320.636002]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  320.636002]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  320.636002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  320.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  320.636002]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  320.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  320.636002]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  320.636002]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  320.636002]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  320.636002]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  320.636002]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  320.636002]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  320.636002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  320.636002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  320.636002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  320.636002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  320.636002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  320.636002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  320.636002] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  321.436003] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  321.436003] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  321.436003] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  321.436003] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  321.436003] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  321.436003] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  321.436003] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  321.436003] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  321.436003] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  321.436003] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  321.436003] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  321.436003] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  321.436003] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  321.436003] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  321.436003] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  321.436003] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  321.436003] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  321.436003] Stack:
[  321.436003]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  321.436003]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  321.436003]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  321.436003] Call Trace:
[  321.436003]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  321.436003]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  321.436003]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  321.436003]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  321.436003]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  321.436003]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  321.436003]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  321.436003]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  321.436003]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  321.436003]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  321.436003]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  321.436003]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  321.436003]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  321.436003]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  321.436003]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  321.436003]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  321.436003] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  324.228002] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  324.228002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  324.228002] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  324.228002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  324.228002] Workqueue: events hv_set_host_time [hv_utils]
[  324.228002] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  324.228002] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  324.228002] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  324.228002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  324.228002] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  324.228002] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  324.228002] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  324.228002] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  324.228002] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  324.228002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  324.228002] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  324.228002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  324.228002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  324.228002] Stack:
[  324.228002]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  324.228002]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  324.228002]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  324.228002] Call Trace:
[  324.228002]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  324.228002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  324.228002]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  324.228002]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  324.228002]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  324.228002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  324.228002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  324.228002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  324.228002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  324.228002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  324.228002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  324.228002] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  343.624006] INFO: rcu_bh self-detected stall on CPU[  343.624010] INFO: rcu_bh self-detected stall on CPU
[  343.624011] 	0-...: (1 GPs behind) idle=949/140000000000001/0 softirq=0/2584 fqs=6783 
[  343.624011] 	
[  343.624011]  (t=15000 jiffies g=-299 c=-300 q=9)
[  343.624011] Task dump for CPU 0:
[  343.624011] kworker/0:3     R
[  343.624011]   running task        0  4418      2 0x00000008
[  343.624011] Workqueue: events hv_set_host_time [hv_utils]
[  343.624011]  ffff885b7d603df8
[  343.624011]  ffffffff9bcb37dd 0000000000000000 ffffffff9ca5d240 ffff885b7d603e10
[  343.624011]  ffffffff9bcb64c7 0000000000000000 ffff885b7d603e40 ffffffff9bda61a8
[  343.624011]  ffff885b7d619f00 ffffffff9ca5d240 0000000000000000Call Trace:
[  343.624011]  <IRQ> 
[  343.624011]  [<ffffffff9bcb37dd>] sched_show_task+0xcd/0x130
[  343.624011]  [<ffffffff9bcb64c7>] dump_cpu_task+0x37/0x40
[  343.624011]  [<ffffffff9bda61a8>] rcu_dump_cpu_stacks+0x94/0xba
[  343.624011]  [<ffffffff9bcf1c27>] rcu_check_callbacks+0x747/0x890
[  343.624011]  [<ffffffff9bd4cf5c>] ? acct_account_cputime+0x1c/0x20
[  343.624011]  [<ffffffff9bcb6f3f>] ? account_system_time+0x7f/0x110
[  343.624011]  [<ffffffff9bd08c10>] ? tick_sched_handle.isra.12+0x60/0x60
[  343.624011]  [<ffffffff9bcf8b5f>] update_process_times+0x2f/0x60
[  343.624011]  [<ffffffff9bd08bd5>] tick_sched_handle.isra.12+0x25/0x60
[  343.624011]  [<ffffffff9bd08c4d>] tick_sched_timer+0x3d/0x70
[  343.624011]  [<ffffffff9bcf9603>] __hrtimer_run_queues+0xf3/0x280
[  343.624011]  [<ffffffff9bcf9dd8>] hrtimer_interrupt+0xa8/0x1a0
[  343.624011]  [<ffffffffc01bf560>] vmbus_isr+0xb0/0xf0 [hv_vmbus]
[  343.624011]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  343.624011]  [<ffffffff9bc4f889>] hyperv_vector_handler+0x39/0x50
[  343.624011]  [<ffffffff9c49fde2>] hyperv_callback_vector+0x82/0x90
[  343.624011]  <EOI> 
[  343.624011]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  343.624011]  [<ffffffff9bd0e754>] ? smp_call_function_many+0x1f4/0x250
[  343.624011]  [<ffffffff9bd0e72d>] ? smp_call_function_many+0x1cd/0x250
[  343.624011]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  343.624011]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  343.624011]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  343.624011]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  343.624011]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  343.624011]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  343.624011]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  343.624011]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  343.624011]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  343.624011]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  343.624011]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  343.624011] Task dump for CPU 5:
[  343.624011] kworker/5:1     R
[  343.624011]   running task        0   278      2 0x00000008
[  343.624011] Workqueue: events netstamp_clear
[  343.624011]  ffffffff9bda47e8
[  343.624011]  ffffffff9cb969a0 ffff885b7d758b00 ffff885b7d75d200 ffffa99899d27df0
[  343.624011]  ffffffff9bda4a45 ffff885b5bfc1740 ffffa99899d27e00 ffffffff9bda4a6d
[  343.624011]  ffffa99899d27e10 ffffffff9c37a5bd ffffa99899d27e50Call Trace:
[  343.624011]  [<ffffffff9bda47e8>] ? jump_label_update+0x88/0x90
[  343.624011]  [<ffffffff9bda4a45>] ? static_key_slow_inc+0x95/0xa0
[  343.624011]  [<ffffffff9bda4a6d>] ? static_key_enable+0x1d/0x50
[  343.624011]  [<ffffffff9c37a5bd>] ? netstamp_clear+0x2d/0x40
[  343.624011]  [<ffffffff9bc9ebbb>] ? process_one_work+0x16b/0x4a0
[  343.624011]  [<ffffffff9bc9ef3b>] ? worker_thread+0x4b/0x500
[  343.624011]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  343.624011]  [<ffffffff9bca5466>] ? kthread+0xe6/0x100
[  343.624011]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  343.624011]  [<ffffffff9c49eeb5>] ? ret_from_fork+0x25/0x30
[  343.624011] Task dump for CPU 6:
[  343.624011] irqbalance      R
[  343.624011]   running task        0  4286   4204 0x00000008
 ffff885c1ffd6240
[  343.624011]  0000000000000001 fffffffffffffff8 ffffa998a8e5b8e0 ffff885c1ffd7080
[  343.624011]  0000000000000200 0000000000000012 0000000000000001 ffff885c1ffd5d00
[  343.624011]  0000000000000246 1bea1e9cd88269b0 ffff885c1ffd7080Call Trace:
[  343.624011]  [<ffffffff9be33316>] ? memcg_kmem_charge_memcg+0x76/0x90
[  343.624011]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  343.624011]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  343.624011]  [<ffffffff9be19cf7>] ? kmem_cache_alloc+0xd7/0x1b0
[  343.624011]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  343.624011]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  343.624011]  [<ffffffff9bcd1505>] ? mutex_optimistic_spin+0x145/0x190
[  343.624011]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  343.624011]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  343.624011]  [<ffffffff9be19aab>] ? kmem_cache_alloc_trace+0xdb/0x1c0
[  343.624011]  [<ffffffff9c02d628>] ? put_dec+0x18/0xa0
[  343.624011]  [<ffffffff9c02d9a0>] ? number+0x2f0/0x310
[  343.624011]  [<ffffffff9bec0df8>] ? interrupts_open+0x18/0x20
[  343.624011]  [<ffffffff9c02e203>] ? string+0x63/0x70
[  343.624011]  [<ffffffff9c0306aa>] ? vsnprintf+0x12a/0x4c0
[  343.624011]  [<ffffffff9be689f5>] ? seq_vprintf+0x35/0x50
[  343.624011]  [<ffffffff9be68a63>] ? seq_printf+0x53/0x70
[  343.624011]  [<ffffffff9c49eba7>] ? _raw_spin_lock_irqsave+0x37/0x3f
[  343.624011]  [<ffffffff9bceae24>] ? show_interrupts+0x114/0x330
[  343.624011]  [<ffffffff9be692f1>] ? seq_read+0x301/0x410
[  343.624011]  [<ffffffff9beb7ac2>] ? proc_reg_read+0x42/0x70
[  343.624011]  [<ffffffff9be41c47>] ? __vfs_read+0x37/0x150
[  343.624011]  [<ffffffff9bf848db>] ? security_file_permission+0x9b/0xc0
[  343.624011]  [<ffffffff9be42123>] ? vfs_read+0x93/0x130
[  343.624011]  [<ffffffff9be43645>] ? SyS_read+0x55/0xc0
[  343.624011]  [<ffffffff9c49ec3b>] ? entry_SYSCALL_64_fastpath+0x1e/0xad
[  343.624009] 	5-...: (1 GPs behind) idle=e51/140000000000001/0 softirq=0/1500 fqs=6826 
[  343.624009] 	 (t=15097 jiffies g=-299 c=-300 q=9)
[  343.624009] Task dump for CPU 0:
[  343.624009] kworker/0:3     R  running task        0  4418      2 0x00000008
[  343.624009] Workqueue: events hv_set_host_time [hv_utils]
[  343.624009]  f81ce9ab408c818f 01d4c90203f17fe7 ffffffffc0368b40 ffff885b7d61d200
[  343.624009]  ffffa998a90abe10 ffffffffc0364193 000000005c6d2367 000000001706b03c
[  343.624009]  f81ce9ab408c818f ffff885b0f3d9080 ffff885b7d618b00 ffffa998a90abe50
[  343.624009] Call Trace:
[  343.624009]  [<ffffffffc0364193>] ? hv_set_host_time+0x73/0xa0 [hv_utils]
[  343.624009]  [<ffffffff9bc9ebbb>] ? process_one_work+0x16b/0x4a0
[  343.624009]  [<ffffffff9bc9ef3b>] ? worker_thread+0x4b/0x500
[  343.624009]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  343.624009]  [<ffffffff9bca5466>] ? kthread+0xe6/0x100
[  343.624009]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  343.624009]  [<ffffffff9c49eeb5>] ? ret_from_fork+0x25/0x30
[  343.624009] Task dump for CPU 5:
[  343.624009] kworker/5:1     R  running task        0   278      2 0x00000008
[  343.624009] Workqueue: events netstamp_clear
[  343.624009]  ffff885b7d743df8 ffffffff9bcb37dd 0000000000000005 ffffffff9ca5d240
[  343.624009]  ffff885b7d743e10 ffffffff9bcb64c7 0000000000000005 ffff885b7d743e40
[  343.624009]  ffffffff9bda61a8 ffff885b7d759f00 ffffffff9ca5d240 0000000000000000
[  343.624009] Call Trace:
[  343.624009]  <IRQ> [  343.624009]  [<ffffffff9bcb37dd>] sched_show_task+0xcd/0x130
[  343.624009]  [<ffffffff9bcb64c7>] dump_cpu_task+0x37/0x40
[  343.624009]  [<ffffffff9bda61a8>] rcu_dump_cpu_stacks+0x94/0xba
[  343.624009]  [<ffffffff9bcf1c27>] rcu_check_callbacks+0x747/0x890
[  343.624009]  [<ffffffff9bd4cf5c>] ? acct_account_cputime+0x1c/0x20
[  343.624009]  [<ffffffff9bcb6f3f>] ? account_system_time+0x7f/0x110
[  343.624009]  [<ffffffff9bd08c10>] ? tick_sched_handle.isra.12+0x60/0x60
[  343.624009]  [<ffffffff9bcf8b5f>] update_process_times+0x2f/0x60
[  343.624009]  [<ffffffff9bd08bd5>] tick_sched_handle.isra.12+0x25/0x60
[  343.624009]  [<ffffffff9bd08c4d>] tick_sched_timer+0x3d/0x70
[  343.624009]  [<ffffffff9bcf9603>] __hrtimer_run_queues+0xf3/0x280
[  343.624009]  [<ffffffff9bcf9dd8>] hrtimer_interrupt+0xa8/0x1a0
[  343.624009]  [<ffffffffc01bf560>] vmbus_isr+0xb0/0xf0 [hv_vmbus]
[  343.624009]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  343.624009]  [<ffffffff9bc4f889>] hyperv_vector_handler+0x39/0x50
[  343.624009]  [<ffffffff9c49fde2>] hyperv_callback_vector+0x82/0x90
[  343.624009]  <EOI> [  343.624009]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  343.624009]  [<ffffffff9bd0e754>] ? smp_call_function_many+0x1f4/0x250
[  343.624009]  [<ffffffff9bd0e72d>] ? smp_call_function_many+0x1cd/0x250
[  343.624009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  343.624009]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  343.624009]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  343.624009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  343.624009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  343.624009]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  343.624009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  343.624009]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  343.624009]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  343.624009]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  343.624009]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  343.624009]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  343.624009]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  343.624009]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  343.624009]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  343.624009]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  343.624009]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  343.624009]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  343.624009]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  343.624009] Task dump for CPU 6:
[  343.624009] irqbalance      R  running task        0  4286   4204 0x00000008
[  343.624009]  ffff885c1ffd6240 0000000000000001 fffffffffffffff8 ffffa998a8e5b8e0
[  343.624009]  ffff885c1ffd7080 0000000000000200 0000000000000012 0000000000000001
[  343.624009]  ffff885c1ffd5d00 0000000000000246 1bea1e9cd88269b0 ffff885c1ffd7080
[  343.624009] Call Trace:
[  343.624009]  [<ffffffff9be33316>] ? memcg_kmem_charge_memcg+0x76/0x90
[  343.624009]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  343.624009]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  343.624009]  [<ffffffff9be19cf7>] ? kmem_cache_alloc+0xd7/0x1b0
[  343.624009]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  343.624009]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  343.624009]  [<ffffffff9bcd1505>] ? mutex_optimistic_spin+0x145/0x190
[  343.624009]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  343.624009]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  343.624009]  [<ffffffff9be19aab>] ? kmem_cache_alloc_trace+0xdb/0x1c0
[  343.624009]  [<ffffffff9c02d628>] ? put_dec+0x18/0xa0
[  343.624009]  [<ffffffff9c02d9a0>] ? number+0x2f0/0x310
[  343.624009]  [<ffffffff9bec0df8>] ? interrupts_open+0x18/0x20
[  343.624009]  [<ffffffff9c02e203>] ? string+0x63/0x70
[  343.624009]  [<ffffffff9c0306aa>] ? vsnprintf+0x12a/0x4c0
[  343.624009]  [<ffffffff9be689f5>] ? seq_vprintf+0x35/0x50
[  343.624009]  [<ffffffff9be68a63>] ? seq_printf+0x53/0x70
[  343.624009]  [<ffffffff9c49eba7>] ? _raw_spin_lock_irqsave+0x37/0x3f
[  343.624009]  [<ffffffff9bceae24>] ? show_interrupts+0x114/0x330
[  343.624009]  [<ffffffff9be692f1>] ? seq_read+0x301/0x410
[  343.624009]  [<ffffffff9beb7ac2>] ? proc_reg_read+0x42/0x70
[  343.624009]  [<ffffffff9be41c47>] ? __vfs_read+0x37/0x150
[  343.624009]  [<ffffffff9bf848db>] ? security_file_permission+0x9b/0xc0
[  343.624009]  [<ffffffff9be42123>] ? vfs_read+0x93/0x130
[  343.624009]  [<ffffffff9be43645>] ? SyS_read+0x55/0xc0
[  343.624009]  [<ffffffff9c49ec3b>] ? entry_SYSCALL_64_fastpath+0x1e/0xad
[  349.436009] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  349.436009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  349.436009] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  349.436009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  349.436009] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  349.436009] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  349.436009] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  349.436009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  349.436009] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  349.436009] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  349.436009] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  349.436009] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  349.436009] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  349.436009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  349.436009] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  349.436009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  349.436009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  349.436009] Stack:
[  349.436009]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  349.436009]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  349.436009]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  349.436009] Call Trace:
[  349.436009]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  349.436009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  349.436009]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  349.436009]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  349.436009]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  349.436009]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  349.436009]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  349.436009]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  349.436009]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  349.436009]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  349.436009]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  349.436009]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  349.436009]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  349.436009]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  349.436009]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  349.436009]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  349.436009] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  368.228008] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  368.228008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  368.228008] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  368.228008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  368.228008] Workqueue: events hv_set_host_time [hv_utils]
[  368.228008] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  368.228008] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  368.228008] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  368.228008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  368.228008] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  368.228008] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  368.228008] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  368.228008] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  368.228008] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  368.228008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  368.228008] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  368.228008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  368.228008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  368.228008] Stack:
[  368.228008]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  368.228008]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  368.228008]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  368.228008] Call Trace:
[  368.228008]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  368.228008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  368.228008]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  368.228008]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  368.228008]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  368.228008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  368.228008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  368.228008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  368.228008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  368.228008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  368.228008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  368.228008] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  368.636002] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 23s! [kworker/5:1:278]
[  368.636002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  368.636002] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  368.636002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  368.636002] Workqueue: events netstamp_clear
[  368.636002] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  368.636002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  368.636002] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  368.636002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  368.636002] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  368.636002] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  368.636002] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  368.636002] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  368.636002] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  368.636002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  368.636002] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  368.636002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  368.636002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  368.636002] Stack:
[  368.636002]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  368.636002]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  368.636002]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  368.636002] Call Trace:
[  368.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  368.636002]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  368.636002]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  368.636002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  368.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  368.636002]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  368.636002]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  368.636002]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  368.636002]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  368.636002]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  368.636002]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  368.636002]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  368.636002]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  368.636002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  368.636002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  368.636002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  368.636002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  368.636002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  368.636002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  368.636002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  377.436010] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  377.436010] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  377.436010] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  377.436010] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  377.436010] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  377.436010] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  377.436010] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  377.436010] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  377.436010] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  377.436010] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  377.436010] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  377.436010] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  377.436010] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  377.436010] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  377.436010] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  377.436010] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  377.436010] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  377.436010] Stack:
[  377.436010]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  377.436010]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  377.436010]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  377.436010] Call Trace:
[  377.436010]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  377.436010]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  377.436010]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  377.436010]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  377.436010]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  377.436010]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  377.436010]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  377.436010]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  377.436010]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  377.436010]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  377.436010]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  377.436010]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  377.436010]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  377.436010]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  377.436010]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  377.436010]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  377.436010] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  396.228002] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 23s! [kworker/0:3:4418]
[  396.228002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  396.228002] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  396.228002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  396.228002] Workqueue: events hv_set_host_time [hv_utils]
[  396.228002] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  396.228002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  396.228002] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  396.228002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  396.228002] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  396.228002] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  396.228002] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  396.228002] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  396.228002] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  396.228002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  396.228002] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  396.228002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  396.228002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  396.228002] Stack:
[  396.228002]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  396.228002]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  396.228002]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  396.228002] Call Trace:
[  396.228002]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  396.228002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  396.228002]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  396.228002]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  396.228002]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  396.228002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  396.228002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  396.228002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  396.228002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  396.228002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  396.228002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  396.228002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  396.636009] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  396.636009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  396.636009] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  396.636009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  396.636009] Workqueue: events netstamp_clear
[  396.636009] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  396.636009] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  396.636009] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  396.636009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  396.636009] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  396.636009] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  396.636009] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  396.636009] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  396.636009] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  396.636009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  396.636009] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  396.636009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  396.636009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  396.636009] Stack:
[  396.636009]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  396.636009]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  396.636009]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  396.636009] Call Trace:
[  396.636009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  396.636009]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  396.636009]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  396.636009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  396.636009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  396.636009]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  396.636009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  396.636009]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  396.636009]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  396.636009]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  396.636009]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  396.636009]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  396.636009]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  396.636009]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  396.636009]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  396.636009]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  396.636009]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  396.636009]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  396.636009]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  396.636009] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  405.436003] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  405.436003] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  405.436003] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  405.436003] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  405.436003] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  405.436003] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  405.436003] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  405.436003] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  405.436003] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  405.436003] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  405.436003] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  405.436003] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  405.436003] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  405.436003] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  405.436003] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  405.436003] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  405.436003] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  405.436003] Stack:
[  405.436003]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  405.436003]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  405.436003]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  405.436003] Call Trace:
[  405.436003]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  405.436003]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  405.436003]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  405.436003]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  405.436003]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  405.436003]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  405.436003]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  405.436003]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  405.436003]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  405.436003]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  405.436003]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  405.436003]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  405.436003]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  405.436003]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  405.436003]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  405.436003]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  405.436003] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  424.228003] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 23s! [kworker/0:3:4418]
[  424.228003] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  424.228003] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  424.228003] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  424.228003] Workqueue: events hv_set_host_time [hv_utils]
[  424.228003] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  424.228003] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  424.228003] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  424.228003] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  424.228003] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  424.228003] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  424.228003] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  424.228003] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  424.228003] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  424.228003] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  424.228003] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  424.228003] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  424.228003] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  424.228003] Stack:
[  424.228003]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  424.228003]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  424.228003]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  424.228003] Call Trace:
[  424.228003]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  424.228003]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  424.228003]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  424.228003]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  424.228003]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  424.228003]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  424.228003]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  424.228003]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  424.228003]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  424.228003]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  424.228003]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  424.228003] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  424.636008] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  424.636008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  424.636008] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  424.636008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  424.636008] Workqueue: events netstamp_clear
[  424.636008] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  424.636008] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  424.636008] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  424.636008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  424.636008] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  424.636008] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  424.636008] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  424.636008] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  424.636008] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  424.636008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  424.636008] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  424.636008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  424.636008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  424.636008] Stack:
[  424.636008]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  424.636008]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  424.636008]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  424.636008] Call Trace:
[  424.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  424.636008]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  424.636008]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  424.636008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  424.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  424.636008]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  424.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  424.636008]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  424.636008]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  424.636008]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  424.636008]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  424.636008]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  424.636008]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  424.636008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  424.636008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  424.636008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  424.636008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  424.636008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  424.636008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  424.636008] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  433.436010] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  433.436010] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  433.436010] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  433.436010] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  433.436010] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  433.436010] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  433.436010] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  433.436010] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  433.436010] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  433.436010] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  433.436010] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  433.436010] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  433.436010] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  433.436010] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  433.436010] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  433.436010] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  433.436010] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  433.436010] Stack:
[  433.436010]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  433.436010]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  433.436010]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  433.436010] Call Trace:
[  433.436010]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  433.436010]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  433.436010]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  433.436010]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  433.436010]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  433.436010]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  433.436010]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  433.436010]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  433.436010]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  433.436010]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  433.436010]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  433.436010]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  433.436010]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  433.436010]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  433.436010]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  433.436010]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  433.436010] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  434.968028] INFO: rcu_sched detected stalls on CPUs/tasks:
[  434.972024] 	6-...: (1 GPs behind) idle=df3/140000000000000/0 softirq=930/931 fqs=45709 
[  434.972024] 	(detected by 16, t=105014 jiffies, g=242, c=241, q=34680)
[  434.972024] Task dump for CPU 6:
[  434.972024] irqbalance      R  running task        0  4286   4204 0x00000008
[  434.972024]  ffff885c1ffd6240 0000000000000001 fffffffffffffff8 ffffa998a8e5b8e0
[  434.972024]  ffff885c1ffd7080 0000000000000200 0000000000000012 0000000000000001
[  434.972024]  ffff885c1ffd5d00 0000000000000246 1bea1e9cd88269b0 ffff885c1ffd7080
[  434.972024] Call Trace:
[  434.972024]  [<ffffffff9be33316>] ? memcg_kmem_charge_memcg+0x76/0x90
[  434.972024]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  434.972024]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  434.972024]  [<ffffffff9be19cf7>] ? kmem_cache_alloc+0xd7/0x1b0
[  434.972024]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  434.972024]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  434.972024]  [<ffffffff9bcd1505>] ? mutex_optimistic_spin+0x145/0x190
[  434.972024]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  434.972024]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  434.972024]  [<ffffffff9be19aab>] ? kmem_cache_alloc_trace+0xdb/0x1c0
[  434.972024]  [<ffffffff9c02d628>] ? put_dec+0x18/0xa0
[  434.972024]  [<ffffffff9c02d9a0>] ? number+0x2f0/0x310
[  434.972024]  [<ffffffff9bec0df8>] ? interrupts_open+0x18/0x20
[  434.972024]  [<ffffffff9c02e203>] ? string+0x63/0x70
[  434.972024]  [<ffffffff9c0306aa>] ? vsnprintf+0x12a/0x4c0
[  434.972024]  [<ffffffff9be689f5>] ? seq_vprintf+0x35/0x50
[  434.972024]  [<ffffffff9be68a63>] ? seq_printf+0x53/0x70
[  434.972024]  [<ffffffff9c49eba7>] ? _raw_spin_lock_irqsave+0x37/0x3f
[  434.972024]  [<ffffffff9bceae24>] ? show_interrupts+0x114/0x330
[  434.972024]  [<ffffffff9be692f1>] ? seq_read+0x301/0x410
[  434.972024]  [<ffffffff9beb7ac2>] ? proc_reg_read+0x42/0x70
[  434.972024]  [<ffffffff9be41c47>] ? __vfs_read+0x37/0x150
[  434.972024]  [<ffffffff9bf848db>] ? security_file_permission+0x9b/0xc0
[  434.972024]  [<ffffffff9be42123>] ? vfs_read+0x93/0x130
[  434.972024]  [<ffffffff9be43645>] ? SyS_read+0x55/0xc0
[  434.972024]  [<ffffffff9c49ec3b>] ? entry_SYSCALL_64_fastpath+0x1e/0xad
[  452.228010] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 23s! [kworker/0:3:4418]
[  452.228010] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  452.228010] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  452.228010] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  452.228010] Workqueue: events hv_set_host_time [hv_utils]
[  452.228010] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  452.228010] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  452.228010] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  452.228010] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  452.228010] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  452.228010] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  452.228010] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  452.228010] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  452.228010] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  452.228010] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  452.228010] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  452.228010] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  452.228010] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  452.228010] Stack:
[  452.228010]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  452.228010]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  452.228010]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  452.228010] Call Trace:
[  452.228010]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  452.228010]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  452.228010]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  452.228010]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  452.228010]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  452.228010]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  452.228010]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  452.228010]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  452.228010]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  452.228010]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  452.228010]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  452.228010] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  452.636011] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  452.636011] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  452.636011] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  452.636011] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  452.636011] Workqueue: events netstamp_clear
[  452.636011] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  452.636011] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  452.636011] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  452.636011] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  452.636011] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  452.636011] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  452.636011] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  452.636011] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  452.636011] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  452.636011] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  452.636011] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  452.636011] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  452.636011] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  452.636011] Stack:
[  452.636011]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  452.636011]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  452.636011]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  452.636011] Call Trace:
[  452.636011]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  452.636011]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  452.636011]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  452.636011]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  452.636011]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  452.636011]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  452.636011]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  452.636011]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  452.636011]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  452.636011]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  452.636011]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  452.636011]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  452.636011]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  452.636011]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  452.636011]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  452.636011]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  452.636011]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  452.636011]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  452.636011]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  452.636011] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  461.436002] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  461.436002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  461.436002] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  461.436002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  461.436002] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  461.436002] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  461.436002] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  461.436002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  461.436002] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  461.436002] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  461.436002] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  461.436002] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  461.436002] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  461.436002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  461.436002] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  461.436002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  461.436002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  461.436002] Stack:
[  461.436002]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  461.436002]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  461.436002]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  461.436002] Call Trace:
[  461.436002]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  461.436002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  461.436002]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  461.436002]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  461.436002]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  461.436002]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  461.436002]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  461.436002]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  461.436002]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  461.436002]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  461.436002]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  461.436002]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  461.436002]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  461.436002]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  461.436002]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  461.436002]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  461.436002] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  480.228005] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 23s! [kworker/0:3:4418]
[  480.228005] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  480.228005] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  480.228005] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  480.228005] Workqueue: events hv_set_host_time [hv_utils]
[  480.228005] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  480.228005] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  480.228005] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  480.228005] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  480.228005] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  480.228005] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  480.228005] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  480.228005] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  480.228005] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  480.228005] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  480.228005] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  480.228005] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  480.228005] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  480.228005] Stack:
[  480.228005]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  480.228005]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  480.228005]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  480.228005] Call Trace:
[  480.228005]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  480.228005]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  480.228005]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  480.228005]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  480.228005]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  480.228005]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  480.228005]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  480.228005]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  480.228005]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  480.228005]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  480.228005]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  480.228005] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  480.636013] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  480.636016] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  480.636016] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  480.636016] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  480.636016] Workqueue: events netstamp_clear
[  480.636016] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  480.636016] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  480.636016] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  480.636016] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  480.636016] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  480.636016] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  480.636016] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  480.636016] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  480.636016] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  480.636016] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  480.636016] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  480.636016] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  480.636016] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  480.636016] Stack:
[  480.636016]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  480.636016]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  480.636016]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  480.636016] Call Trace:
[  480.636016]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  480.636016]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  480.636016]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  480.636016]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  480.636016]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  480.636016]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  480.636016]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  480.636016]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  480.636016]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  480.636016]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  480.636016]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  480.636016]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  480.636016]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  480.636016]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  480.636016]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  480.636016]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  480.636016]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  480.636016]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  480.636016]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  480.636016] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  489.436009] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  489.436009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  489.436009] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  489.436009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  489.436009] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  489.436009] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  489.436009] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  489.436009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  489.436009] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  489.436009] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  489.436009] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  489.436009] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  489.436009] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  489.436009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  489.436009] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  489.436009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  489.436009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  489.436009] Stack:
[  489.436009]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  489.436009]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  489.436009]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  489.436009] Call Trace:
[  489.436009]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  489.436009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  489.436009]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  489.436009]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  489.436009]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  489.436009]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  489.436009]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  489.436009]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  489.436009]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  489.436009]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  489.436009]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  489.436009]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  489.436009]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  489.436009]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  489.436009]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  489.436009]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  489.436009] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  508.228008] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 23s! [kworker/0:3:4418]
[  508.228008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  508.228008] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  508.228008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  508.228008] Workqueue: events hv_set_host_time [hv_utils]
[  508.228008] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  508.228008] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  508.228008] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  508.228008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  508.228008] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  508.228008] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  508.228008] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  508.228008] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  508.228008] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  508.228008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  508.228008] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  508.228008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  508.228008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  508.228008] Stack:
[  508.228008]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  508.228008]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  508.228008]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  508.228008] Call Trace:
[  508.228008]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  508.228008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  508.228008]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  508.228008]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  508.228008]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  508.228008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  508.228008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  508.228008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  508.228008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  508.228008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  508.228008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  508.228008] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  508.636008] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  508.636008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  508.636008] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  508.636008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  508.636008] Workqueue: events netstamp_clear
[  508.636008] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  508.636008] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  508.636008] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  508.636008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  508.636008] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  508.636008] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  508.636008] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  508.636008] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  508.636008] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  508.636008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  508.636008] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  508.636008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  508.636008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  508.636008] Stack:
[  508.636008]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  508.636008]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  508.636008]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  508.636008] Call Trace:
[  508.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  508.636008]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  508.636008]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  508.636008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  508.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  508.636008]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  508.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  508.636008]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  508.636008]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  508.636008]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  508.636008]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  508.636008]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  508.636008]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  508.636008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  508.636008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  508.636008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  508.636008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  508.636008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  508.636008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  508.636008] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  517.436002] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  517.436002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  517.436002] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  517.436002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  517.436002] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  517.436002] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  517.436002] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  517.436002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  517.436002] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  517.436002] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  517.436002] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  517.436002] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  517.436002] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  517.436002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  517.436002] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  517.436002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  517.436002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  517.436002] Stack:
[  517.436002]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  517.436002]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  517.436002]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  517.436002] Call Trace:
[  517.436002]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  517.436002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  517.436002]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  517.436002]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  517.436002]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  517.436002]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  517.436002]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  517.436002]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  517.436002]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  517.436002]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  517.436002]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  517.436002]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  517.436002]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  517.436002]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  517.436002]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  517.436002]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  517.436002] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  523.640001] INFO: rcu_bh self-detected stall on CPU[  523.640009] INFO: rcu_bh self-detected stall on CPU
[  523.640009] 	5-...: (1 GPs behind) idle=e51/140000000000001/0 softirq=0/1500 fqs=26666 
[  523.640009] 	
[  523.640009]  (t=60004 jiffies g=-299 c=-300 q=9)
[  523.640009] Task dump for CPU 0:
[  523.640009] kworker/0:3     R
[  523.640009]   running task        0  4418      2 0x00000008
[  523.640009] Workqueue: events hv_set_host_time [hv_utils]
[  523.640009]  f81ce9ab408c818f
[  523.640009]  01d4c90203f17fe7 ffffffffc0368b40 ffff885b7d61d200 ffffa998a90abe10
[  523.640009]  ffffffffc0364193 000000005c6d2367 000000001706b03c f81ce9ab408c818f
[  523.640009]  ffff885b0f3d9080 ffff885b7d618b00 ffffa998a90abe50Call Trace:
[  523.640009]  [<ffffffffc0364193>] ? hv_set_host_time+0x73/0xa0 [hv_utils]
[  523.640009]  [<ffffffff9bc9ebbb>] ? process_one_work+0x16b/0x4a0
[  523.640009]  [<ffffffff9bc9ef3b>] ? worker_thread+0x4b/0x500
[  523.640009]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  523.640009]  [<ffffffff9bca5466>] ? kthread+0xe6/0x100
[  523.640009]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  523.640009]  [<ffffffff9c49eeb5>] ? ret_from_fork+0x25/0x30
[  523.640009] Task dump for CPU 5:
[  523.640009] kworker/5:1     R
[  523.640009]   running task        0   278      2 0x00000008
[  523.640009] Workqueue: events netstamp_clear
[  523.640009]  ffff885b7d743df8
[  523.640009]  ffffffff9bcb37dd 0000000000000005 ffffffff9ca5d240 ffff885b7d743e10
[  523.640009]  ffffffff9bcb64c7 0000000000000005 ffff885b7d743e40 ffffffff9bda61a8
[  523.640009]  ffff885b7d759f00 ffffffff9ca5d240 0000000000000000Call Trace:
[  523.640009]  <IRQ> 
[  523.640009]  [<ffffffff9bcb37dd>] sched_show_task+0xcd/0x130
[  523.640009]  [<ffffffff9bcb64c7>] dump_cpu_task+0x37/0x40
[  523.640009]  [<ffffffff9bda61a8>] rcu_dump_cpu_stacks+0x94/0xba
[  523.640009]  [<ffffffff9bcf1c27>] rcu_check_callbacks+0x747/0x890
[  523.640009]  [<ffffffff9bd4cf5c>] ? acct_account_cputime+0x1c/0x20
[  523.640009]  [<ffffffff9bcb6f3f>] ? account_system_time+0x7f/0x110
[  523.640009]  [<ffffffff9bd08c10>] ? tick_sched_handle.isra.12+0x60/0x60
[  523.640009]  [<ffffffff9bcf8b5f>] update_process_times+0x2f/0x60
[  523.640009]  [<ffffffff9bd08bd5>] tick_sched_handle.isra.12+0x25/0x60
[  523.640009]  [<ffffffff9bd08c4d>] tick_sched_timer+0x3d/0x70
[  523.640009]  [<ffffffff9bcf9603>] __hrtimer_run_queues+0xf3/0x280
[  523.640009]  [<ffffffff9bcf9dd8>] hrtimer_interrupt+0xa8/0x1a0
[  523.640009]  [<ffffffffc01bf560>] vmbus_isr+0xb0/0xf0 [hv_vmbus]
[  523.640009]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  523.640009]  [<ffffffff9bc4f889>] hyperv_vector_handler+0x39/0x50
[  523.640009]  [<ffffffff9c49fde2>] hyperv_callback_vector+0x82/0x90
[  523.640009]  <EOI> 
[  523.640009]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  523.640009]  [<ffffffff9bd0e754>] ? smp_call_function_many+0x1f4/0x250
[  523.640009]  [<ffffffff9bd0e72d>] ? smp_call_function_many+0x1cd/0x250
[  523.640009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  523.640009]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  523.640009]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  523.640009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  523.640009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  523.640009]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  523.640009]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  523.640009]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  523.640009]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  523.640009]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  523.640009]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  523.640009]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  523.640009]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  523.640009]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  523.640009]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  523.640009]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  523.640009]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  523.640009]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  523.640009]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  523.640009] Task dump for CPU 6:
[  523.640009] irqbalance      R
[  523.640009]   running task        0  4286   4204 0x00000008
 ffff885c1ffd6240
[  523.640009]  0000000000000001 fffffffffffffff8 ffffa998a8e5b8e0 ffff885c1ffd7080
[  523.640009]  0000000000000200 0000000000000012 0000000000000001 ffff885c1ffd5d00
[  523.640009]  0000000000000246 1bea1e9cd88269b0 ffff885c1ffd7080Call Trace:
[  523.640009]  [<ffffffff9be33316>] ? memcg_kmem_charge_memcg+0x76/0x90
[  523.640009]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  523.640009]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  523.640009]  [<ffffffff9be19cf7>] ? kmem_cache_alloc+0xd7/0x1b0
[  523.640009]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  523.640009]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  523.640009]  [<ffffffff9bcd1505>] ? mutex_optimistic_spin+0x145/0x190
[  523.640009]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  523.640009]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  523.640009]  [<ffffffff9be19aab>] ? kmem_cache_alloc_trace+0xdb/0x1c0
[  523.640009]  [<ffffffff9c02d628>] ? put_dec+0x18/0xa0
[  523.640009]  [<ffffffff9c02d9a0>] ? number+0x2f0/0x310
[  523.640009]  [<ffffffff9bec0df8>] ? interrupts_open+0x18/0x20
[  523.640009]  [<ffffffff9c02e203>] ? string+0x63/0x70
[  523.640009]  [<ffffffff9c0306aa>] ? vsnprintf+0x12a/0x4c0
[  523.640009]  [<ffffffff9be689f5>] ? seq_vprintf+0x35/0x50
[  523.640009]  [<ffffffff9be68a63>] ? seq_printf+0x53/0x70
[  523.640009]  [<ffffffff9c49eba7>] ? _raw_spin_lock_irqsave+0x37/0x3f
[  523.640009]  [<ffffffff9bceae24>] ? show_interrupts+0x114/0x330
[  523.640009]  [<ffffffff9be692f1>] ? seq_read+0x301/0x410
[  523.640009]  [<ffffffff9beb7ac2>] ? proc_reg_read+0x42/0x70
[  523.640009]  [<ffffffff9be41c47>] ? __vfs_read+0x37/0x150
[  523.640009]  [<ffffffff9bf848db>] ? security_file_permission+0x9b/0xc0
[  523.640009]  [<ffffffff9be42123>] ? vfs_read+0x93/0x130
[  523.640009]  [<ffffffff9be43645>] ? SyS_read+0x55/0xc0
[  523.640009]  [<ffffffff9c49ec3b>] ? entry_SYSCALL_64_fastpath+0x1e/0xad
[  523.640001] 	0-...: (1 GPs behind) idle=949/140000000000001/0 softirq=0/2584 fqs=26714 
[  523.640001] 	 (t=60116 jiffies g=-299 c=-300 q=9)
[  523.640001] Task dump for CPU 0:
[  523.640001] kworker/0:3     R  running task        0  4418      2 0x00000008
[  523.640001] Workqueue: events hv_set_host_time [hv_utils]
[  523.640001]  ffff885b7d603df8 ffffffff9bcb37dd 0000000000000000 ffffffff9ca5d240
[  523.640001]  ffff885b7d603e10 ffffffff9bcb64c7 0000000000000000 ffff885b7d603e40
[  523.640001]  ffffffff9bda61a8 ffff885b7d619f00 ffffffff9ca5d240 0000000000000000
[  523.640001] Call Trace:
[  523.640001]  <IRQ> [  523.640001]  [<ffffffff9bcb37dd>] sched_show_task+0xcd/0x130
[  523.640001]  [<ffffffff9bcb64c7>] dump_cpu_task+0x37/0x40
[  523.640001]  [<ffffffff9bda61a8>] rcu_dump_cpu_stacks+0x94/0xba
[  523.640001]  [<ffffffff9bcf1c27>] rcu_check_callbacks+0x747/0x890
[  523.640001]  [<ffffffff9bd4cf5c>] ? acct_account_cputime+0x1c/0x20
[  523.640001]  [<ffffffff9bcb6f3f>] ? account_system_time+0x7f/0x110
[  523.640001]  [<ffffffff9bd08c10>] ? tick_sched_handle.isra.12+0x60/0x60
[  523.640001]  [<ffffffff9bcf8b5f>] update_process_times+0x2f/0x60
[  523.640001]  [<ffffffff9bd08bd5>] tick_sched_handle.isra.12+0x25/0x60
[  523.640001]  [<ffffffff9bd08c4d>] tick_sched_timer+0x3d/0x70
[  523.640001]  [<ffffffff9bcf9603>] __hrtimer_run_queues+0xf3/0x280
[  523.640001]  [<ffffffff9bcf9dd8>] hrtimer_interrupt+0xa8/0x1a0
[  523.640001]  [<ffffffffc01bf560>] vmbus_isr+0xb0/0xf0 [hv_vmbus]
[  523.640001]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  523.640001]  [<ffffffff9bc4f889>] hyperv_vector_handler+0x39/0x50
[  523.640001]  [<ffffffff9c49fde2>] hyperv_callback_vector+0x82/0x90
[  523.640001]  <EOI> [  523.640001]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  523.640001]  [<ffffffff9bd0e754>] ? smp_call_function_many+0x1f4/0x250
[  523.640001]  [<ffffffff9bd0e72d>] ? smp_call_function_many+0x1cd/0x250
[  523.640001]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  523.640001]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  523.640001]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  523.640001]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  523.640001]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  523.640001]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  523.640001]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  523.640001]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  523.640001]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  523.640001]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  523.640001]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  523.640001] Task dump for CPU 5:
[  523.640001] kworker/5:1     R  running task        0   278      2 0x00000008
[  523.640001] Workqueue: events netstamp_clear
[  523.640001]  ffffffff9bda47e8 ffffffff9cb969a0 ffff885b7d758b00 ffff885b7d75d200
[  523.640001]  ffffa99899d27df0 ffffffff9bda4a45 ffff885b5bfc1740 ffffa99899d27e00
[  523.640001]  ffffffff9bda4a6d ffffa99899d27e10 ffffffff9c37a5bd ffffa99899d27e50
[  523.640001] Call Trace:
[  523.640001]  [<ffffffff9bda47e8>] ? jump_label_update+0x88/0x90
[  523.640001]  [<ffffffff9bda4a45>] ? static_key_slow_inc+0x95/0xa0
[  523.640001]  [<ffffffff9bda4a6d>] ? static_key_enable+0x1d/0x50
[  523.640001]  [<ffffffff9c37a5bd>] ? netstamp_clear+0x2d/0x40
[  523.640001]  [<ffffffff9bc9ebbb>] ? process_one_work+0x16b/0x4a0
[  523.640001]  [<ffffffff9bc9ef3b>] ? worker_thread+0x4b/0x500
[  523.640001]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  523.640001]  [<ffffffff9bca5466>] ? kthread+0xe6/0x100
[  523.640001]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  523.640001]  [<ffffffff9c49eeb5>] ? ret_from_fork+0x25/0x30
[  523.640001] Task dump for CPU 6:
[  523.640001] irqbalance      R  running task        0  4286   4204 0x00000008
[  523.640001]  ffff885c1ffd6240 0000000000000001 fffffffffffffff8 ffffa998a8e5b8e0
[  523.640001]  ffff885c1ffd7080 0000000000000200 0000000000000012 0000000000000001
[  523.640001]  ffff885c1ffd5d00 0000000000000246 1bea1e9cd88269b0 ffff885c1ffd7080
[  523.640001] Call Trace:
[  523.640001]  [<ffffffff9be33316>] ? memcg_kmem_charge_memcg+0x76/0x90
[  523.640001]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  523.640001]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  523.640001]  [<ffffffff9be19cf7>] ? kmem_cache_alloc+0xd7/0x1b0
[  523.640001]  [<ffffffff9be5cc87>] ? __d_alloc+0x27/0x1e0
[  523.640001]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  523.640001]  [<ffffffff9bcd1505>] ? mutex_optimistic_spin+0x145/0x190
[  523.640001]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  523.640001]  [<ffffffff9be33157>] ? memcg_kmem_get_cache+0x57/0x150
[  523.640001]  [<ffffffff9be19aab>] ? kmem_cache_alloc_trace+0xdb/0x1c0
[  523.640001]  [<ffffffff9c02d628>] ? put_dec+0x18/0xa0
[  523.640001]  [<ffffffff9c02d9a0>] ? number+0x2f0/0x310
[  523.640001]  [<ffffffff9bec0df8>] ? interrupts_open+0x18/0x20
[  523.640001]  [<ffffffff9c02e203>] ? string+0x63/0x70
[  523.640001]  [<ffffffff9c0306aa>] ? vsnprintf+0x12a/0x4c0
[  523.640001]  [<ffffffff9be689f5>] ? seq_vprintf+0x35/0x50
[  523.640001]  [<ffffffff9be68a63>] ? seq_printf+0x53/0x70
[  523.640001]  [<ffffffff9c49eba7>] ? _raw_spin_lock_irqsave+0x37/0x3f
[  523.640001]  [<ffffffff9bceae24>] ? show_interrupts+0x114/0x330
[  523.640001]  [<ffffffff9be692f1>] ? seq_read+0x301/0x410
[  523.640001]  [<ffffffff9beb7ac2>] ? proc_reg_read+0x42/0x70
[  523.640001]  [<ffffffff9be41c47>] ? __vfs_read+0x37/0x150
[  523.640001]  [<ffffffff9bf848db>] ? security_file_permission+0x9b/0xc0
[  523.640001]  [<ffffffff9be42123>] ? vfs_read+0x93/0x130
[  523.640001]  [<ffffffff9be43645>] ? SyS_read+0x55/0xc0
[  523.640001]  [<ffffffff9c49ec3b>] ? entry_SYSCALL_64_fastpath+0x1e/0xad
[  545.436009] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 22s! [modprobe:4346]
[  545.436009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  545.436009] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  545.436009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  545.436009] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  545.436009] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  545.436009] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  545.436009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  545.436009] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  545.436009] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  545.436009] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  545.436009] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  545.436009] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  545.436009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  545.436009] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  545.436009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  545.436009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  545.436009] Stack:
[  545.436009]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  545.436009]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  545.436009]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  545.436009] Call Trace:
[  545.436009]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  545.436009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  545.436009]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  545.436009]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  545.436009]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  545.436009]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  545.436009]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  545.436009]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  545.436009]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  545.436009]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  545.436009]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  545.436009]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  545.436009]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  545.436009]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  545.436009]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  545.436009]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  545.436009] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  548.228009] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  548.228009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  548.228009] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  548.228009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  548.228009] Workqueue: events hv_set_host_time [hv_utils]
[  548.228009] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  548.228009] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  548.228009] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  548.228009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  548.228009] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  548.228009] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  548.228009] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  548.228009] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  548.228009] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  548.228009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  548.228009] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  548.228009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  548.228009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  548.228009] Stack:
[  548.228009]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  548.228009]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  548.228009]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  548.228009] Call Trace:
[  548.228009]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  548.228009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  548.228009]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  548.228009]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  548.228009]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  548.228009]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  548.228009]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  548.228009]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  548.228009]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  548.228009]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  548.228009]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  548.228009] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  548.636004] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 22s! [kworker/5:1:278]
[  548.636004] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  548.636004] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  548.636004] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  548.636004] Workqueue: events netstamp_clear
[  548.636004] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  548.636004] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  548.636004] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  548.636004] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  548.636004] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  548.636004] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  548.636004] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  548.636004] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  548.636004] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  548.636004] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  548.636004] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  548.636004] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  548.636004] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  548.636004] Stack:
[  548.636004]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  548.636004]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  548.636004]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  548.636004] Call Trace:
[  548.636004]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  548.636004]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  548.636004]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  548.636004]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  548.636004]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  548.636004]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  548.636004]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  548.636004]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  548.636004]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  548.636004]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  548.636004]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  548.636004]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  548.636004]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  548.636004]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  548.636004]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  548.636004]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  548.636004]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  548.636004]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  548.636004]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  548.636004] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  573.436002] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 23s! [modprobe:4346]
[  573.436002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  573.436002] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  573.436002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  573.436002] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  573.436002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  573.436002] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  573.436002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  573.436002] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  573.436002] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  573.436002] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  573.436002] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  573.436002] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  573.436002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  573.436002] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  573.436002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  573.436002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  573.436002] Stack:
[  573.436002]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  573.436002]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  573.436002]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  573.436002] Call Trace:
[  573.436002]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  573.436002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  573.436002]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  573.436002]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  573.436002]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  573.436002]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  573.436002]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  573.436002]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  573.436002]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  573.436002]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  573.436002]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  573.436002]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  573.436002]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  573.436002]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  573.436002]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  573.436002]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  573.436002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  576.228002] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  576.228002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  576.228002] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  576.228002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  576.228002] Workqueue: events hv_set_host_time [hv_utils]
[  576.228002] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  576.228002] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  576.228002] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  576.228002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  576.228002] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  576.228002] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  576.228002] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  576.228002] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  576.228002] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  576.228002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  576.228002] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  576.228002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  576.228002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  576.228002] Stack:
[  576.228002]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  576.228002]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  576.228002]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  576.228002] Call Trace:
[  576.228002]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  576.228002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  576.228002]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  576.228002]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  576.228002]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  576.228002]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  576.228002]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  576.228002]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  576.228002]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  576.228002]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  576.228002]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  576.228002] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
[  576.636008] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 23s! [kworker/5:1:278]
[  576.636008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  576.636008] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  576.636008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  576.636008] Workqueue: events netstamp_clear
[  576.636008] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  576.636008] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  576.636008] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  576.636008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  576.636008] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  576.636008] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  576.636008] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  576.636008] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  576.636008] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  576.636008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  576.636008] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  576.636008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  576.636008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  576.636008] Stack:
[  576.636008]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  576.636008]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  576.636008]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  576.636008] Call Trace:
[  576.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  576.636008]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  576.636008]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  576.636008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  576.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  576.636008]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  576.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  576.636008]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  576.636008]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  576.636008]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  576.636008]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  576.636008]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  576.636008]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  576.636008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  576.636008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  576.636008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  576.636008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  576.636008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  576.636008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  576.636008] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  601.436002] NMI watchdog: BUG: soft lockup - CPU#15 stuck for 23s! [modprobe:4346]
[  601.436002] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  601.436002] CPU: 15 PID: 4346 Comm: modprobe Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  601.436002] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  601.436002] task: ffff885b12860000 task.stack: ffffa998a8f58000
[  601.436002] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  601.436002] RSP: 0018:ffffa998a8f5bae8  EFLAGS: 00000202
[  601.436002] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  601.436002] RDX: ffff885b7d79d8a0 RSI: 0000000000000080 RDI: ffff885b643b3290
[  601.436002] RBP: ffffa998a8f5bb20 R08: 0000000000000000 R09: 00000000000f7fff
[  601.436002] R10: ffffd225a070ad00 R11: ffff885b643b3290 R12: ffffffff9bc72670
[  601.436002] R13: 0000000000000000 R14: ffff885b7d9da280 R15: 000000000001a240
[  601.436002] FS:  00007f160b6b9700(0000) GS:ffff885b7d9c0000(0000) knlGS:0000000000000000
[  601.436002] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  601.436002] CR2: 00007fe4e8808750 CR3: 00000027d2fda000 CR4: 00000000003406e0
[  601.436002] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  601.436002] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  601.436002] Stack:
[  601.436002]  000000000000000f 01ff885b00000001 ffff885b51a7ad20 ffffffff9bc72670
[  601.436002]  0000000000000000 ffffa998a8f5bbf8 ffffa998a8f5bb50 ffffa998a8f5bb48
[  601.436002]  ffffffff9bd0e80d ffff885b51a7ad20 ffff885b7f5d43a8 ffffa998a8f5bbf0
[  601.436002] Call Trace:
[  601.436002]  [<ffffffff9bc72670>] ? leave_mm+0xd0/0xd0
[  601.436002]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  601.436002]  [<ffffffff9bc72f2b>] flush_tlb_kernel_range+0x4b/0x80
[  601.436002]  [<ffffffff9c03aa0e>] ? bsearch+0x5e/0x90
[  601.436002]  [<ffffffff9bdf0e18>] __purge_vmap_area_lazy+0x2d8/0x320
[  601.436002]  [<ffffffff9bdf0f83>] vm_unmap_aliases+0x123/0x150
[  601.436002]  [<ffffffff9bc6e44b>] change_page_attr_set_clr+0xeb/0x520
[  601.436002]  [<ffffffff9bc6f4bf>] set_memory_ro+0x2f/0x40
[  601.436002]  [<ffffffff9bd10bd0>] frob_text.isra.32+0x20/0x30
[  601.436002]  [<ffffffff9bd11d65>] module_enable_ro+0x35/0x90
[  601.436002]  [<ffffffff9bd142ae>] load_module+0x1f1e/0x2930
[  601.436002]  [<ffffffff9bfc95ad>] ? ima_post_read_file+0x7d/0xa0
[  601.436002]  [<ffffffff9bf82f4b>] ? security_kernel_post_read_file+0x6b/0x80
[  601.436002]  [<ffffffff9bd14f2f>] SYSC_finit_module+0xdf/0x110
[  601.436002]  [<ffffffff9bd14f7e>] SyS_finit_module+0xe/0x10
[  601.436002]  [<ffffffff9c49ec3b>] entry_SYSCALL_64_fastpath+0x1e/0xad
[  601.436002] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  604.228009] NMI watchdog: BUG: soft lockup - CPU#0 stuck for 22s! [kworker/0:3:4418]
[  604.228009] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  604.228009] CPU: 0 PID: 4418 Comm: kworker/0:3 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  604.228009] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  604.228009] Workqueue: events hv_set_host_time [hv_utils]
[  604.228009] task: ffff885b231796c0 task.stack: ffffa998a90a8000
[  604.228009] RIP: 0010:[<ffffffff9bd0e754>]  [<ffffffff9bd0e754>] smp_call_function_many+0x1f4/0x250
[  604.228009] RSP: 0018:ffffa998a90abd28  EFLAGS: 00000202
[  604.228009] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  604.228009] RDX: ffff885b7d79cc40 RSI: 0000000000000080 RDI: ffff885b7d09a840
[  604.228009] RBP: ffffa998a90abd60 R08: 0000000000000000 R09: 00000000000ffffe
[  604.228009] R10: ffffd2259f887a40 R11: ffff885b7d09a840 R12: ffffffff9bcf9490
[  604.228009] R13: 0000000000000000 R14: ffff885b7d61a280 R15: 000000000001a240
[  604.228009] FS:  0000000000000000(0000) GS:ffff885b7d600000(0000) knlGS:0000000000000000
[  604.228009] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  604.228009] CR2: 0000009a58960c30 CR3: 0000002585e07000 CR4: 00000000003406f0
[  604.228009] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  604.228009] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  604.228009] Stack:
[  604.228009]  0000000000000000 01ffffff00000001 0000000000000000 ffffffff9bcf9490
[  604.228009]  0000000000000000 0000000000000000 ffffffffc0368b40 ffffa998a90abd88
[  604.228009]  ffffffff9bd0e80d 0000000000000000 ffffa998a90abde8 0000000000000283
[  604.228009] Call Trace:
[  604.228009]  [<ffffffff9bcf9490>] ? hrtimer_cancel+0x20/0x20
[  604.228009]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  604.228009]  [<ffffffff9bcf9c4c>] clock_was_set+0x1c/0x30
[  604.228009]  [<ffffffff9bcff803>] do_settimeofday64+0xf3/0x160
[  604.228009]  [<ffffffffc0364193>] hv_set_host_time+0x73/0xa0 [hv_utils]
[  604.228009]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  604.228009]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  604.228009]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  604.228009]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  604.228009]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  604.228009]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  604.228009] Code: 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 8b 42 18 <a8> 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 f7 e8 b4 
[  604.636008] NMI watchdog: BUG: soft lockup - CPU#5 stuck for 23s! [kworker/5:1:278]
[  604.636008] Modules linked in: sb_edac edac_core kvm_intel mlx4_core(+) kvm devlink bridge input_leds stp serio_raw joydev llc irqbypass i2c_piix4 pci_hyperv mac_hid hv_balloon intel_rapl_perf ib_iser rdma_cm iw_cm ib_cm ib_core configfs iscsi_tcp libiscsi_tcp libiscsi scsi_transport_iscsi autofs4 btrfs raid10 raid456 async_raid6_recov async_memcpy async_pq async_xor async_tx xor raid6_pq libcrc32c raid1 raid0 multipath linear hid_generic hid_hyperv crct10dif_pclmul crc32_pclmul ghash_clmulni_intel hv_storvsc aesni_intel hv_netvsc scsi_transport_fc hid hyperv_keyboard hv_utils aes_x86_64 hyperv_fb lrw glue_helper ablk_helper psmouse cryptd hv_vmbus pata_acpi floppy fjes
[  604.636008] CPU: 5 PID: 278 Comm: kworker/5:1 Tainted: G      D      L  4.9.40-eve-ng-ukms-2+ #4
[  604.636008] Hardware name: Microsoft Corporation Virtual Machine/Virtual Machine, BIOS 090007  06/02/2017
[  604.636008] Workqueue: events netstamp_clear
[  604.636008] task: ffff885b5c9e0000 task.stack: ffffa99899d24000
[  604.636008] RIP: 0010:[<ffffffff9bd0e751>]  [<ffffffff9bd0e751>] smp_call_function_many+0x1f1/0x250
[  604.636008] RSP: 0018:ffffa99899d27ca8  EFLAGS: 00000202
[  604.636008] RAX: 0000000000000003 RBX: 0000000000000080 RCX: 0000000000000006
[  604.636008] RDX: ffff885b7d79d760 RSI: 0000000000000080 RDI: ffff885b643b3440
[  604.636008] RBP: ffffa99899d27ce0 R08: 0000000000000000 R09: 00000000000fffdf
[  604.636008] R10: ffffd225a0727540 R11: ffff885b643b3440 R12: ffffffff9bc34f30
[  604.636008] R13: 0000000000000000 R14: ffff885b7d75a280 R15: 000000000001a240
[  604.636008] FS:  0000000000000000(0000) GS:ffff885b7d740000(0000) knlGS:0000000000000000
[  604.636008] CS:  0010 DS: 0000 ES: 0000 CR0: 0000000080050033
[  604.636008] CR2: 00000000010e1af8 CR3: 0000002585e07000 CR4: 00000000003406e0
[  604.636008] DR0: 0000000000000000 DR1: 0000000000000000 DR2: 0000000000000000
[  604.636008] DR3: 0000000000000000 DR6: 00000000fffe0ff0 DR7: 0000000000000400
[  604.636008] Stack:
[  604.636008]  0000000000000005 01ffffff00000001 ffffffff9c3808c0 ffffffff9bc34f30
[  604.636008]  0000000000000000 ffffffff9c3808c1 ffffffff9cb29ac0 ffffa99899d27d08
[  604.636008]  ffffffff9bd0e80d ffffffff9c3808c0 0000000000000005 ffffa99899d27d5b
[  604.636008] Call Trace:
[  604.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  604.636008]  [<ffffffff9bc34f30>] ? arch_unregister_cpu+0x30/0x30
[  604.636008]  [<ffffffff9c3808c1>] ? netif_receive_skb_internal+0x21/0xa0
[  604.636008]  [<ffffffff9bd0e80d>] on_each_cpu+0x2d/0x60
[  604.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  604.636008]  [<ffffffff9bc35f5a>] text_poke_bp+0x6a/0xf0
[  604.636008]  [<ffffffff9c3808c0>] ? netif_receive_skb_internal+0x20/0xa0
[  604.636008]  [<ffffffff9bc32b5b>] arch_jump_label_transform+0x9b/0x120
[  604.636008]  [<ffffffff9bda4746>] __jump_label_update+0x76/0x90
[  604.636008]  [<ffffffff9bda47e8>] jump_label_update+0x88/0x90
[  604.636008]  [<ffffffff9bda4a45>] static_key_slow_inc+0x95/0xa0
[  604.636008]  [<ffffffff9bda4a6d>] static_key_enable+0x1d/0x50
[  604.636008]  [<ffffffff9c37a5bd>] netstamp_clear+0x2d/0x40
[  604.636008]  [<ffffffff9bc9ebbb>] process_one_work+0x16b/0x4a0
[  604.636008]  [<ffffffff9bc9ef3b>] worker_thread+0x4b/0x500
[  604.636008]  [<ffffffff9bc9eef0>] ? process_one_work+0x4a0/0x4a0
[  604.636008]  [<ffffffff9bca5466>] kthread+0xe6/0x100
[  604.636008]  [<ffffffff9bca5380>] ? kthread_park+0x60/0x60
[  604.636008]  [<ffffffff9c49eeb5>] ret_from_fork+0x25/0x30
[  604.636008] Code: 63 d2 e8 43 c4 32 00 3b 05 d1 02 e8 00 89 c1 0f 8d 9a fe ff ff 48 98 49 8b 16 48 03 14 c5 c0 c3 93 9c 8b 42 18 a8 01 74 09 f3 90 <8b> 42 18 a8 01 75 f7 eb bd 0f b6 4d d0 4c 89 ea 4c 89 e6 44 89 
```
