
## Run your CI on the operating systems GitHub does not offer

GitHub-hosted runners give you Ubuntu, Windows and macOS. VM Actions gives you
the rest. Each action in this organization boots a real, fully installed
operating system under QEMU inside an ordinary `ubuntu-latest` runner, syncs
your checked-out source tree into the guest, and runs your build and test
commands there -- on the real kernel, the real libc, the real package manager,
and, if you want, a real non-x86 CPU. No self-hosted runner, no container
trickery, no cross-compilation guesswork.

A job is one step:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: vmactions/freebsd-vm@v1
        with:
          usesh: true
          prepare: |
            pkg install -y gmake
          run: |
            ./configure
            gmake
            gmake check
```

Everything else is optional: pick a `release` and an `arch`, choose how the
workspace is shared (`rsync`, `sshfs`, `nfs` or `scp`), pass environment
variables and secrets in with `envs`, set `mem` / `cpu`, forward ports with
`nat`, run later workflow steps directly inside the VM with a custom `shell`,
cache the VM image after `prepare` so repeat builds skip it, and switch on
`debug-on-error` to get a VNC shell into the machine at the exact moment your
build broke.

Writing the workflow is optional too: install the
[vmactions-ci skill](https://github.com/vmactions/vmactions-skill) and ask your
coding agent for "run my tests on NetBSD aarch64" -- it knows the actions, the
releases, the per-OS default shells and the footguns.

Pick a system from the table below. Images are built and booted by
[AnyVM](https://anyvm.org).



-----------------------------------------------------------------------------------------------------------------------------------------------------
| VM                |  GitHub Actions                    | Arch            |                       Debug Shell              
|-------------------------------|-----------------------------------|-------------|---------------------------------------------------------------
| FreeBSD <br/> ![Test](https://github.com/vmactions/freebsd-vm/workflows/Test/badge.svg) | [vmactions/freebsd-vm@v1](https://github.com/vmactions/freebsd-vm) | x86_64, aarch64, riscv64, powerpc64|  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error) | 
| OpenBSD <br/>  ![Test](https://github.com/vmactions/openbsd-vm/workflows/Test/badge.svg) | [vmactions/openbsd-vm@v1](https://github.com/vmactions/openbsd-vm) |  x86_64,  aarch64, riscv64, sparc64|  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error) | 
| NetBSD  <br/> ![Test](https://github.com/vmactions/netbsd-vm/workflows/Test/badge.svg) | [vmactions/netbsd-vm@v1](https://github.com/vmactions/netbsd-vm)  | x86_64, aarch64, riscv64, sparc64| [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)    |
| Solaris <br/> ![Test](https://github.com/vmactions/solaris-vm/workflows/Test/badge.svg) | [vmactions/solaris-vm@v1](https://github.com/vmactions/solaris-vm)|  x86_64 |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  |
| DragonFlyBSD <br/> ![Test](https://github.com/vmactions/dragonflybsd-vm/workflows/Test/badge.svg) | [vmactions/dragonflybsd-vm@v1](https://github.com/vmactions/dragonflybsd-vm)| x86_64 | [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  |
| MidnightBSD <br/> ![Test](https://github.com/vmactions/midnightbsd-vm/workflows/Test/badge.svg) | [vmactions/midnightbsd-vm@v1](https://github.com/vmactions/midnightbsd-vm)| x86_64 | [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  |
| GhostBSD <br/> ![Test](https://github.com/vmactions/ghostbsd-vm/workflows/Test/badge.svg) | [vmactions/ghostbsd-vm@v1](https://github.com/vmactions/ghostbsd-vm)| x86_64 | [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  |
| NextBSD <br/> ![Test](https://github.com/vmactions/nextbsd-vm/workflows/Test/badge.svg) | [vmactions/nextbsd-vm@v1](https://github.com/vmactions/nextbsd-vm)| x86_64, aarch64 | [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  |
| HardenedBSD <br/> ![Test](https://github.com/vmactions/hardenedbsd-vm/workflows/Test/badge.svg) | [vmactions/hardenedbsd-vm@v1](https://github.com/vmactions/hardenedbsd-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| HyprBSD <br/> ![Test](https://github.com/vmactions/hyprbsd-vm/workflows/Test/badge.svg) | [vmactions/hyprbsd-vm@v1](https://github.com/vmactions/hyprbsd-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| OPNsense <br/> ![Test](https://github.com/vmactions/opnsense-vm/workflows/Test/badge.svg) | [vmactions/opnsense-vm@v1](https://github.com/vmactions/opnsense-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| OmniOS <br/> ![Test](https://github.com/vmactions/omnios-vm/workflows/Test/badge.svg) | [vmactions/omnios-vm@v1](https://github.com/vmactions/omnios-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  |
| OpenIndiana <br/> ![Test](https://github.com/vmactions/openindiana-vm/workflows/Test/badge.svg) | [vmactions/openindiana-vm@v1](https://github.com/vmactions/openindiana-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| Haiku OS <br/> ![Test](https://github.com/vmactions/haiku-vm/workflows/Test/badge.svg) | [vmactions/haiku-vm@v1](https://github.com/vmactions/haiku-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| Tribblix <br/> ![Test](https://github.com/vmactions/tribblix-vm/workflows/Test/badge.svg) | [vmactions/tribblix-vm@v1](https://github.com/vmactions/tribblix-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| Blissos(Android) <br/> ![Test](https://github.com/vmactions/blissos-vm/workflows/Test/badge.svg) | [vmactions/blissos-vm@v1](https://github.com/vmactions/blissos-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| GNU Hurd <br/> ![Test](https://github.com/vmactions/hurd-vm/workflows/Test/badge.svg) | [vmactions/hurd-vm@v1](https://github.com/vmactions/hurd-vm)|  x86_64, i386  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| OpenEuler <br/> ![Test](https://github.com/vmactions/openeuler-vm/workflows/Test/badge.svg) | [vmactions/openeuler-vm@v1](https://github.com/vmactions/openeuler-vm)|  x86_64, aarch64, riscv64, loongarch64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| Ubuntu <br/> ![Test](https://github.com/vmactions/ubuntu-vm/workflows/Test/badge.svg) | [vmactions/ubuntu-vm@v1](https://github.com/vmactions/ubuntu-vm)|  x86_64, aarch64, riscv64, s390x, ppc64le  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| Debian <br/> ![Test](https://github.com/vmactions/debian-vm/workflows/Test/badge.svg) | [vmactions/debian-vm@v1](https://github.com/vmactions/debian-vm)|  x86_64, aarch64, riscv64, ppc64le  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| Alpine <br/> ![Test](https://github.com/vmactions/alpine-vm/workflows/Test/badge.svg) | [vmactions/alpine-vm@v1](https://github.com/vmactions/alpine-vm)|  x86_64, aarch64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| AlmaLinux <br/> ![Test](https://github.com/vmactions/almalinux-vm/workflows/Test/badge.svg) | [vmactions/almalinux-vm@v1](https://github.com/vmactions/almalinux-vm)|  x86_64, aarch64, s390x, ppc64le  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| Rocky Linux <br/> ![Test](https://github.com/vmactions/rockylinux-vm/workflows/Test/badge.svg) | [vmactions/rockylinux-vm@v1](https://github.com/vmactions/rockylinux-vm)|  x86_64, aarch64, s390x, ppc64le  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| ReactOS <br/> ![Test](https://github.com/vmactions/reactos-vm/workflows/Test/badge.svg) | [vmactions/reactos-vm@v1](https://github.com/vmactions/reactos-vm)|  i386  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| Redox OS <br/> ![Test](https://github.com/vmactions/redox-vm/workflows/Test/badge.svg) | [vmactions/redox-vm@v1](https://github.com/vmactions/redox-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
| RISC OS <br/> ![Test](https://github.com/vmactions/riscos-vm/workflows/Test/badge.svg) | [vmactions/riscos-vm@v1](https://github.com/vmactions/riscos-vm)|  armv7  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  |
| Plan 9 <br/> ![Test](https://github.com/vmactions/plan9-vm/workflows/Test/badge.svg) | [vmactions/plan9-vm@v1](https://github.com/vmactions/plan9-vm)|  x86_64  |  [debug on error](https://github.com/vmactions/.github/wiki/debug%E2%80%90on%E2%80%90error)  | 
------------------------------------------------------------------------------------------------------------------------------------------















