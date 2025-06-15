# Qemu Patch
This project is fully inspired from [Hypervisor-Phantom](https://github.com/Scrut1ny/Hypervisor-Phantom).
As it doesn't support nixos, and the main part I love about this project is "patch" file. so I got it and using it for nixos qemu.

To explain why not just fetch the file from Hypervisor-Phantom directly, I tried it and it causes issues with virtual machine manager, so I slowly fix patch part by part, might even fetch other patches and work on them too. (Time will show)

## How to use?
It is easy to use patch you have 2 options, download patch or directly fetch using nixos configuration like that:

To only patch "qemu" (not virt-machine's qemu"):
```
  environment.systemPackages = with pkgs; [
    (qemu.overrideAttrs (oldAttrs: {
      version = "9.2.4";
      src = pkgs.fetchurl {
        url = "https://download.qemu.org/qemu-9.2.4.tar.xz";
        sha256 = "sha256-88wcTqv9soghisPjN2Pb6eJ22LyJC4Z6IzXVjeLd05o=";
      };
      patches = (oldAttrs.patches or []) ++ [
          (pkgs.fetchurl {
            url = "https://github.com/Scrut1ny/Hypervisor-Phantom/raw/refs/heads/main/Hypervisor-Phantom/patches/QEMU/intel-qemu-9.2.4.patch"; #change with amd, if you have amd cpu
            sha256 = "sha256-AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="; # Replace with actual hash
          })
          (pkgs.fetchurl {
            url = "https://github.com/Scrut1ny/Hypervisor-Phantom/raw/refs/heads/main/Hypervisor-Phantom/patches/QEMU/libnfs6-qemu-9.2.4.patch";
            sha256 = "sha256-AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="; # Replace with actual hash
          })
      ];
    }))
  ];

  virtualisation.libvirtd = {
    enable = true;
    qemu.package = pkgs.qemu.overrideAttrs (oldAttrs: {  # Use the SAME override here
      version = "9.2.4";
      src = pkgs.fetchurl {
        url = "https://download.qemu.org/qemu-9.2.4.tar.xz";
        sha256 = "sha256-88wcTqv9soghisPjN2Pb6eJ22LyJC4Z6IzXVjeLd05o=";
      };
      patches = (oldAttrs.patches or []) ++ [
        <path_to_my_patch>  #Ex: /home/basicacc/Documents/my_intel_patch.patch #I have only intel patch
      ];
    });
  };
```

Now, if you just going to use qemu without "virtual machine manager" fetch it directly from Hypervisor-Phantom but if you love virtual machine manager like me, do the second one I gave example of.
Note: 10.0.2 patches doesn't work, (for now) don't waste time on them.
Note: I will 100% work on the patch more, might even write script for it too.
