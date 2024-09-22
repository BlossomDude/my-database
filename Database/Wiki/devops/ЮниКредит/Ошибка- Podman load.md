---
Owner: Blossom Dude
Last edited time: 2024-01-22T16:25
Created time: 2023-12-20T15:00
---
## Обращение

Добрый день!  
Столкнулись с проблемой при разворачивании на препрод-среде:  

```Bash
[testdocshousepodman@vs3105 conf]$ podman load -i /csp/containers/ucb-web_ucb-pipe-31.tar
Getting image source signatures
Copying blob bd6174cfdfde done
Copying blob eafe6e032dbd done
Copying blob eb6ee5b9581f done
Copying blob e3abdc2e9252 done
Copying blob b7707ded6ebf done
Copying blob 92a4e8a3140f done
Copying blob ff673cbef06c done
Error: payload does not match any of the supported image formats:
* oci: initializing source oci:/csp/containers/ucb-web_ucb-pipe-31.tar:: open /csp/containers/ucb-web_ucb-pipe-31.tar/index.json: not a directory
* oci-archive: loading index: open /var/tmp/oci1519590072/index.json: no such file or directory
* docker-archive: writing blob: adding layer with blob "sha256:ff673cbef06cbbf210be341cb5bfc0d4ffb3896a1d494f8707c661237f8ea258": processing tar file(write /app/ucb-ecm-webapp.jar: no space left on device): exit status 1
* dir: open /csp/containers/ucb-web_ucb-pipe-31.tar/manifest.json: not a directory @
```

---

Продолжение:

Переназначили временную папку, текст ошибки изменился:

```Bash

testdocshousepodman@vs3105 ~]$ TMPDIR=/csp/tmp podman load -i /csp/containers/ucb-web_ucb-pipe-31.tar
Getting image source signatures
Copying blob bd6174cfdfde skipped: already exists
Copying blob eafe6e032dbd skipped: already exists
Copying blob eb6ee5b9581f skipped: already exists
Copying blob e3abdc2e9252 skipped: already exists
Copying blob 92a4e8a3140f skipped: already exists
Copying blob ff673cbef06c done
Copying blob b7707ded6ebf skipped: already exists
Error: payload does not match any of the supported image formats: 
* oci: initializing source oci:/csp/containers/ucb-web_ucb-pipe-31.tar:: open /
 csp/containers/ucb-web_ucb-pipe-31.tar/index.json: not a directory 
* oci-archive: loading index: open /csp/tmp/oci3223512768/index.json: no such file or directory 
* docker-archive: writing blob: adding layer with blob "sha256:210be341cb5bfc0d4ffb3896a1d494f8707c661237f8ea258": \
processing tar file(write /app/ucb-ecm-webapp.jar: no space left on device): exit status 1 
* dir: open /csp/containers/ucb-web_ucb-pipe-31.tar/manifest.json: not a direct
```

### Запрошенные данные

  

- os-release
    
    ```Bash
    NAME="Red Hat Enterprise Linux"
    VERSION="8.8 (Ootpa)"
    ID="rhel"
    ID_LIKE="fedora"
    VERSION_ID="8.8"
    PLATFORM_ID="platform:el8"
    PRETTY_NAME="Red Hat Enterprise Linux 8.8 (Ootpa)"
    ANSI_COLOR="0;31"
    CPE_NAME="cpe:/o:redhat:enterprise_linux:8::baseos"
    HOME_URL="https://www.redhat.com/"
    DOCUMENTATION_URL="https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8"
    BUG_REPORT_URL="https://bugzilla.redhat.com/"
    REDHAT_BUGZILLA_PRODUCT="Red Hat Enterprise Linux 8"
    REDHAT_BUGZILLA_PRODUCT_VERSION=8.8
    REDHAT_SUPPORT_PRODUCT="Red Hat Enterprise Linux"
    REDHAT_SUPPORT_PRODUCT_VERSION="8.8"
    Red Hat Enterprise Linux release 8.8 (Ootpa)
    Red Hat Enterprise Linux release 8.8 (Ootpa)
    ```
    
      
    
- df
    
    ```Bash
    Filesystem                    Type      Size  Used Avail Use% Mounted on
    devtmpfs                      devtmpfs  7.7G     0  7.7G   0% /dev
    tmpfs                         tmpfs     7.7G  252K  7.7G   1% /dev/shm
    tmpfs                         tmpfs     7.7G  793M  7.0G  11% /run
    tmpfs                         tmpfs     7.7G     0  7.7G   0% /sys/fs/cgroup
    /dev/mapper/rootvg-root_lv    xfs       4.0G  1.9G  2.2G  46% /
    /dev/mapper/appvg-csp_lv      xfs       185G  2.4G  183G   2% /csp
    /dev/mapper/rootvg-var_lv     xfs       3.0G  1.1G  2.0G  35% /var
    /dev/mapper/rootvg-opt_lv     xfs       4.0G  294M  3.8G   8% /opt
    /dev/mapper/rootvg-tmp_lv     xfs       2.0G   47M  2.0G   3% /tmp
    /dev/mapper/appvg-csp_logs_lv xfs        15G  140M   15G   1% /csp/logs
    /dev/mapper/rootvg-home_lv    xfs      1014M  889M  126M  88% /home
    /dev/sda1                     xfs       495M  290M  206M  59% /boot
    /dev/sda2                     vfat      200M  8.0K  200M   1% /boot/efi
    tmpfs                         tmpfs     1.6G     0  1.6G   0% /run/user/37195
    tmpfs                         tmpfs     1.6G     0  1.6G   0% /run/user/19242
    tmpfs                         tmpfs     1.6G     0  1.6G   0% /run/user/0
    ```
    
- podman version
    
    ```Bash
    Client:       Podman Engine
    Version:      4.4.1
    API Version:  4.4.1
    Go Version:   go1.19.10
    Built:        Thu Aug 17 18:23:14 2023
    OS/Arch:      linux/amd64
    ```
    
- subgid
    
    ```Bash
    am17525:100000:65536
    am14203:165536:65536
    am24557:231072:65536
    am26457:296608:65536
    am24206:362144:65536
    am29987:427680:65536
    am29377:493216:65536
    am27334:558752:65536
    srvcmsud:624288:65536
    srvnexposeunix:689824:65536
    am30755:755360:65536
    am37779:820896:65536
    am37798:886432:65536
    testdocshousepodman:951968:65536
    am27974:1017504:65536
    am19242:1083040:65536
    am37195:1148576:65536
    ```
    
- subuid
    
    ```Bash
    am17525:100000:65536
    am14203:165536:65536
    am24557:231072:65536
    am26457:296608:65536
    am24206:362144:65536
    am29987:427680:65536
    am29377:493216:65536
    am27334:558752:65536
    srvcmsud:624288:65536
    srvnexposeunix:689824:65536
    am30755:755360:65536
    am37779:820896:65536
    am37798:886432:65536
    testdocshousepodman:951968:65536
    am27974:1017504:65536
    am19242:1083040:65536
    am37195:1148576:65536
    ```
    
- podman --log-level debug load -i /csc/containers/ucb-web_ucb-pipe-31.tar
    
    ```Bash
    [testdocshousepodman@vs3105 containers]$ podman --log-level debug load -i /csc/containers/ucb-web_ucb-pipe-31.tar > /csp/load_debug.out
    INFO[0000] podman filtering at log level debug
    DEBU[0000] Called load.PersistentPreRunE(podman --log-level debug load -i /csc/containers/ucb-web_ucb-pipe-31.tar)
    DEBU[0000] Using conmon: "/usr/bin/conmon"
    DEBU[0000] Initializing boltdb state at /home/testdocshousepodman/.local/share/containers/storage/libpod/bolt_state.db
    DEBU[0000] Using graph driver overlay
    DEBU[0000] Using graph root /home/testdocshousepodman/.local/share/containers/storage
    DEBU[0000] Using run root /tmp/containers-user-1690/containers
    DEBU[0000] Using static dir /home/testdocshousepodman/.local/share/containers/storage/libpod
    DEBU[0000] Using tmp dir /tmp/podman-run-1690/libpod/tmp
    DEBU[0000] Using volume path /home/testdocshousepodman/.local/share/containers/storage/volumes
    DEBU[0000] Using transient store: false
    DEBU[0000] Set libpod namespace to ""
    DEBU[0000] Not configuring container store
    DEBU[0000] Initializing event backend file
    DEBU[0000] Configured OCI runtime crun-wasm initialization failed: no valid executable found for OCI runtime crun-wasm: invalid argument
    DEBU[0000] Configured OCI runtime kata initialization failed: no valid executable found for OCI runtime kata: invalid argument
    DEBU[0000] Configured OCI runtime youki initialization failed: no valid executable found for OCI runtime youki: invalid argument
    DEBU[0000] Configured OCI runtime krun initialization failed: no valid executable found for OCI runtime krun: invalid argument
    DEBU[0000] Configured OCI runtime ocijail initialization failed: no valid executable found for OCI runtime ocijail: invalid argument
    DEBU[0000] Configured OCI runtime crun initialization failed: no valid executable found for OCI runtime crun: invalid argument
    DEBU[0000] Configured OCI runtime runsc initialization failed: no valid executable found for OCI runtime runsc: invalid argument
    DEBU[0000] Configured OCI runtime runj initialization failed: no valid executable found for OCI runtime runj: invalid argument
    DEBU[0000] Using OCI runtime "/usr/bin/runc"
    INFO[0000] Setting parallel job count to 31
    INFO[0000] podman filtering at log level debug
    DEBU[0000] Called load.PersistentPreRunE(podman --log-level debug load -i /csc/containers/ucb-web_ucb-pipe-31.tar)
    DEBU[0000] Using conmon: "/usr/bin/conmon"
    DEBU[0000] Initializing boltdb state at /home/testdocshousepodman/.local/share/containers/storage/libpod/bolt_state.db
    DEBU[0000] Overriding run root "/tmp/podman-run-1690/containers" with "/tmp/containers-user-1690/containers" from database
    DEBU[0000] Using graph driver overlay
    DEBU[0000] Using graph root /home/testdocshousepodman/.local/share/containers/storage
    DEBU[0000] Using run root /tmp/containers-user-1690/containers
    DEBU[0000] Using static dir /home/testdocshousepodman/.local/share/containers/storage/libpod
    DEBU[0000] Using tmp dir /tmp/podman-run-1690/libpod/tmp
    DEBU[0000] Using volume path /home/testdocshousepodman/.local/share/containers/storage/volumes
    DEBU[0000] Using transient store: false
    DEBU[0000] Set libpod namespace to ""
    DEBU[0000] [graphdriver] trying provided driver "overlay"
    DEBU[0000] Cached value indicated that overlay is supported
    DEBU[0000] Cached value indicated that overlay is supported
    DEBU[0000] Cached value indicated that metacopy is not being used
    DEBU[0000] Cached value indicated that native-diff is usable
    DEBU[0000] backingFs=xfs, projectQuotaSupported=false, useNativeDiff=true, usingMetacopy=false
    DEBU[0000] Initializing event backend file
    DEBU[0000] Configured OCI runtime crun initialization failed: no valid executable found for OCI runtime crun: invalid argument
    DEBU[0000] Configured OCI runtime runsc initialization failed: no valid executable found for OCI runtime runsc: invalid argument
    DEBU[0000] Configured OCI runtime krun initialization failed: no valid executable found for OCI runtime krun: invalid argument
    DEBU[0000] Configured OCI runtime ocijail initialization failed: no valid executable found for OCI runtime ocijail: invalid argument
    DEBU[0000] Configured OCI runtime crun-wasm initialization failed: no valid executable found for OCI runtime crun-wasm: invalid argument
    DEBU[0000] Configured OCI runtime runj initialization failed: no valid executable found for OCI runtime runj: invalid argument
    DEBU[0000] Configured OCI runtime kata initialization failed: no valid executable found for OCI runtime kata: invalid argument
    DEBU[0000] Configured OCI runtime youki initialization failed: no valid executable found for OCI runtime youki: invalid argument
    DEBU[0000] Using OCI runtime "/usr/bin/runc"
    INFO[0000] Setting parallel job count to 31
    DEBU[0000] Failed to add podman to systemd sandbox cgroup: dial unix /run/user/0/bus: connect: permission denied
    Error: stat /csc/containers/ucb-web_ucb-pipe-31.tar: no such file or directory
    DEBU[0000] Shutting down engines
    ```
    
- /etc/passwd
    
    ```Bash
    root:x:0:0:root:/home/root:/bin/bash
    bin:x:1:1:bin:/bin:/sbin/nologin
    daemon:x:2:2:daemon:/sbin:/sbin/nologin
    adm:x:3:4:adm:/var/adm:/sbin/nologin
    lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    sync:x:5:0:sync:/sbin:/bin/sync
    shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
    halt:x:7:0:halt:/sbin:/sbin/halt
    mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    operator:x:11:0:operator:/home/root:/sbin/nologin
    games:x:12:100:games:/usr/games:/sbin/nologin
    ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
    dbus:x:81:81:System message bus:/:/sbin/nologin
    tss:x:59:59:Account used for TPM access:/dev/null:/sbin/nologin
    systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
    systemd-resolve:x:193:193:systemd Resolver:/:/sbin/nologin
    polkitd:x:998:994:User for polkitd:/:/sbin/nologin
    unbound:x:997:993:Unbound DNS resolver:/etc/unbound:/sbin/nologin
    rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
    sssd:x:996:992:User for sssd:/:/sbin/nologin
    rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
    chrony:x:995:991::/var/lib/chrony:/sbin/nologin
    sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
    tcpdump:x:72:72::/:/sbin/nologin
    am17525:x:17525:17525:Andrey.Kopenkin@unicredit.ru:/home/am17525:/bin/bash
    am14203:x:14203:14203:Igor.Kukin@unicredit.ru:/home/am14203:/bin/bash
    am24557:x:24557:24557:Evgeny.Golobokov@unicredit.ru:/home/am24557:/bin/bash
    am26457:x:26457:26457:Matvey.Spivak@unicredit.ru:/home/am26457:/bin/bash
    am24206:x:24206:24206:Evgeny.Pekishev@unicredit.ru:/home/am24206:/bin/bash
    am29987:x:29987:29987:Igor.Makushev@unicredit.ru:/home/am29987:/bin/bash
    am29377:x:29377:29377:Artem.Chudin@unicredit.ru:/home/am29377:/bin/bash
    am27334:x:27334:27334:Andrei.Leontiev@unicredit.ru:/home/am27334:/bin/bash
    srvcmsud:x:1515:1515:Configuration Management System Universal Discovery:/home/srvcmsud:/bin/bash
    srvnexposeunix:x:1422:1422:Sergey.Marin@unicredit.ru:/home/srvnexposeunix:/bin/bash
    am30755:x:30755:30755:Arseny.Ratanin@unicredit.ru:/home/am30755:/bin/bash
    am37779:x:37779:37779:Dmitry.Artyushin@unicredit.ru:/home/am37779:/bin/bash
    am37798:x:37798:37798:Andrei.An.Petrov@unicredit.ru:/home/am37798:/bin/bash
    testdocshousepodman:x:1690:1690:CHG000797464:/home/testdocshousepodman:/bin/bash
    am27974:x:27974:27974:Lyudmila.Tsukanova@unicredit.ru:/home/am27974:/bin/bash
    am19242:x:19242:19242:Aleksey.Pryamikov@unicredit.ru:/home/am19242:/bin/bash
    am37195:x:37195:37195:Dmitry.Ulasov@unicredit.ru:/home/am37195:/bin/bash
    ```
    
- podman info
    
    ```Bash
    host:
      arch: amd64
      buildahVersion: 1.29.0
      cgroupControllers: []
      cgroupManager: cgroupfs
      cgroupVersion: v1
      conmon:
        package: conmon-2.1.6-1.module+el8.8.0+18098+9b44df5f.x86_64
        path: /usr/bin/conmon
        version: 'conmon version 2.1.6, commit: 8c4ab5a095127ecc96ef8a9c885e0e1b14aeb11b'
      cpuUtilization:
        idlePercent: 99.69
        systemPercent: 0.1
        userPercent: 0.21
      cpus: 10
      distribution:
        distribution: '"rhel"'
        version: "8.8"
      eventLogger: file
      hostname: vs3105.imb.ru
      idMappings:
        gidmap:
        - container_id: 0
          host_id: 1690
          size: 1
        - container_id: 1
          host_id: 951968
          size: 65536
        uidmap:
        - container_id: 0
          host_id: 1690
          size: 1
        - container_id: 1
          host_id: 951968
          size: 65536
      kernel: 4.18.0-477.27.1.el8_8.x86_64
      linkmode: dynamic
      logDriver: k8s-file
      memFree: 11510263808
      memTotal: 16525160448
      networkBackend: cni
      ociRuntime:
        name: runc
        package: runc-1.1.4-1.module+el8.8.0+18060+3f21f2cc.x86_64
        path: /usr/bin/runc
        version: |-
          runc version 1.1.4
          spec: 1.0.2-dev
          go: go1.19.4
          libseccomp: 2.5.2
      os: linux
      remoteSocket:
        path: /tmp/podman-run-1690/podman/podman.sock
      security:
        apparmorEnabled: false
        capabilities: CAP_SYS_CHROOT,CAP_NET_RAW,CAP_CHOWN,CAP_DAC_OVERRIDE,CAP_FOWNER,CAP_FSETID,CAP_KILL,CAP_NET_BIND_SERVICE,CAP_SETFCAP,CAP_SETGID,CAP_SETPCAP,CAP_SETUID
        rootless: true
        seccompEnabled: true
        seccompProfilePath: /usr/share/containers/seccomp.json
        selinuxEnabled: false
      serviceIsRemote: false
      slirp4netns:
        executable: /usr/bin/slirp4netns
        package: slirp4netns-1.2.0-2.module+el8.8.0+18060+3f21f2cc.x86_64
        version: |-
          slirp4netns version 1.2.0
          commit: 656041d45cfca7a4176f6b7eed9e4fe6c11e8383
          libslirp: 4.4.0
          SLIRP_CONFIG_VERSION_MAX: 3
          libseccomp: 2.5.2
      swapFree: 4294963200
      swapTotal: 4294963200
      uptime: 168h 9m 1.00s (Approximately 7.00 days)
    plugins:
      authorization: null
      log:
      - k8s-file
      - none
      - passthrough
      - journald
      network:
      - bridge
      - macvlan
      - ipvlan
      volume:
      - local
    registries:
      search:
      - registry.access.redhat.com
      - registry.redhat.io
      - docker.io
    store:
      configFile: /home/testdocshousepodman/.config/containers/storage.conf
      containerStore:
        number: 0
        paused: 0
        running: 0
        stopped: 0
      graphDriverName: overlay
      graphOptions: {}
      graphRoot: /home/testdocshousepodman/.local/share/containers/storage
      graphRootAllocated: 1063256064
      graphRootUsed: 931717120
      graphStatus:
        Backing Filesystem: xfs
        Native Overlay Diff: "true"
        Supports d_type: "true"
        Using metacopy: "false"
      imageCopyTmpDir: /var/tmp
      imageStore:
        number: 1
      runRoot: /tmp/containers-user-1690/containers
      transientStore: false
      volumePath: /home/testdocshousepodman/.local/share/containers/storage/volumes
    version:
      APIVersion: 4.4.1
      Built: 1692285794
      BuiltTime: Thu Aug 17 18:23:14 2023
      GitCommit: ""
      GoVersion: go1.19.10
      Os: linux
      OsArch: linux/amd64
      Version: 4.4.1
    ```
    
- podman info new
    
    ```Bash
    host:
      arch: amd64
      buildahVersion: 1.29.0
      cgroupControllers: []
      cgroupManager: cgroupfs
      cgroupVersion: v1
      conmon:
        package: conmon-2.1.6-1.module+el8.8.0+18098+9b44df5f.x86_64
        path: /usr/bin/conmon
        version: 'conmon version 2.1.6, commit: 8c4ab5a095127ecc96ef8a9c885e0e1b14aeb11b'
      cpuUtilization:
        idlePercent: 99.7
        systemPercent: 0.1
        userPercent: 0.21
      cpus: 10
      distribution:
        distribution: '"rhel"'
        version: "8.8"
      eventLogger: file
      hostname: vs3105.imb.ru
      idMappings:
        gidmap:
        - container_id: 0
          host_id: 1690
          size: 1
        - container_id: 1
          host_id: 951968
          size: 65536
        uidmap:
        - container_id: 0
          host_id: 1690
          size: 1
        - container_id: 1
          host_id: 951968
          size: 65536
      kernel: 4.18.0-477.27.1.el8_8.x86_64
      linkmode: dynamic
      logDriver: k8s-file
      memFree: 11631558656
      memTotal: 16525160448
      networkBackend: cni
      ociRuntime:
        name: runc
        package: runc-1.1.4-1.module+el8.8.0+18060+3f21f2cc.x86_64
        path: /usr/bin/runc
        version: |-
          runc version 1.1.4
          spec: 1.0.2-dev
          go: go1.19.4
          libseccomp: 2.5.2
      os: linux
      remoteSocket:
        path: /run/user/1690/podman/podman.sock
      security:
        apparmorEnabled: false
        capabilities: CAP_SYS_CHROOT,CAP_NET_RAW,CAP_CHOWN,CAP_DAC_OVERRIDE,CAP_FOWNER,CAP_FSETID,CAP_KILL,CAP_NET_BIND_SERVICE,CAP_SETFCAP,CAP_SETGID,CAP_SETPCAP,CAP_SETUID
        rootless: true
        seccompEnabled: true
        seccompProfilePath: /usr/share/containers/seccomp.json
        selinuxEnabled: false
      serviceIsRemote: false
      slirp4netns:
        executable: /usr/bin/slirp4netns
        package: slirp4netns-1.2.0-2.module+el8.8.0+18060+3f21f2cc.x86_64
        version: |-
          slirp4netns version 1.2.0
          commit: 656041d45cfca7a4176f6b7eed9e4fe6c11e8383
          libslirp: 4.4.0
          SLIRP_CONFIG_VERSION_MAX: 3
          libseccomp: 2.5.2
      swapFree: 4294963200
      swapTotal: 4294963200
      uptime: 641h 41m 49.00s (Approximately 26.71 days)
    plugins:
      authorization: null
      log:
      - k8s-file
      - none
      - passthrough
      - journald
      network:
      - bridge
      - macvlan
      - ipvlan
      volume:
      - local
    registries:
      search:
      - registry.access.redhat.com
      - registry.redhat.io
      - docker.io
    store:
      configFile: /home/testdocshousepodman/.config/containers/storage.conf
      containerStore:
        number: 0
        paused: 0
        running: 0
        stopped: 0
      graphDriverName: overlay
      graphOptions: {}
      graphRoot: /home/testdocshousepodman/.local/share/containers/storage
      graphRootAllocated: 1063256064
      graphRootUsed: 931721216
      graphStatus:
        Backing Filesystem: xfs
        Native Overlay Diff: "true"
        Supports d_type: "true"
        Using metacopy: "false"
      imageCopyTmpDir: /var/tmp
      imageStore:
        number: 1
      runRoot: /tmp/containers-user-1690/containers
      transientStore: false
      volumePath: /home/testdocshousepodman/.local/share/containers/storage/volumes
    version:
      APIVersion: 4.4.1
      Built: 1692285794
      BuiltTime: Thu Aug 17 18:23:14 2023
      GitCommit: ""
      GoVersion: go1.19.10
      Os: linux
      OsArch: linux/amd64
      Version: 4.4.1
    ```
    
- ls -lah /home/testdocshousepodman/.local/share/containers/
    
    ```Bash
    total 0
    drwx------ 4 testdocshousepodman testdocshousepodman  34 Dec 18 16:23 .
    drwx------ 3 testdocshousepodman testdocshousepodman  24 Dec 18 16:23 ..
    drwx------ 2 testdocshousepodman testdocshousepodman  39 Dec 18 16:23 cache
    drwx------ 7 testdocshousepodman testdocshousepodman 144 Dec 18 16:23 storage
    ```
    
- conmon --version
    
    ```Bash
    conmon version 2.1.6
    commit: 8c4ab5a095127ecc96ef8a9c885e0e1b14aeb11b
    ```
    

  

  

  

Возможное решение:

- удаление rm `-rf /home/testdocshousepodman/.local/share/containers`- удалит все объекты подман пользователей не имеющих root-прав ([ссылка](https://github.com/containers/podman/issues/18483))

---

Проблема сертов при запуске фронта

```Bash
[testdocshousepodman@vs3105 containers]$ podman run -d -p 443:443 --name front-ucb -v /csp/conf:/ssl -v /csp/conf:/config -v /csp/conf/httpd-ssl.conf:/usr/local/apache2/conf/extra/httpd-ssl.conf registry.test.ucb:9080/front-ucb:ucb-pipe-35
31158e253ddce71c0d2c419021203e4932d8de146ec7b73bdc5bf2b0d5eab123
[testdocshousepodman@vs3105 containers]$ podman logs -t --since 10m front-ucb > /csp/front-ucb.log
2024-01-22T15:49:06.655606495+03:00 AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.0.2.100. Set the 'ServerName' directive globally to suppress this message
2024-01-22T15:49:06.681209491+03:00 AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 10.0.2.100. Set the 'ServerName' directive globally to suppress this message
2024-01-22T15:49:06.696248064+03:00 [Mon Jan 22 12:49:06.683468 2024] [mpm_event:notice] [pid 1:tid 139728726302016] AH00489: Apache/2.4.54 (Unix) OpenSSL/1.1.1n configured -- resuming normal operations
2024-01-22T15:49:06.696248064+03:00 [Mon Jan 22 12:49:06.696228 2024] [core:notice] [pid 1:tid 139728726302016] AH00094: Command line: 'httpd -D FOREGROUND -C PidFile /tmp/httpd.pid'
```