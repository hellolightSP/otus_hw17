
```
[root@client vagrant]# systemctl list-timers --all
NEXT                         LEFT          LAST                         PASSED       UNIT                         ACTIVATES
Tue 2023-06-27 21:38:47 UTC  1min 17s left Tue 2023-06-27 21:33:47 UTC  3min 42s ago borg-backup.timer            borg-backup.service
Wed 2023-06-28 21:30:47 UTC  23h left      Tue 2023-06-27 21:30:47 UTC  6min ago     systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service
n/a                          n/a           n/a                          n/a          systemd-readahead-done.timer systemd-readahead-done.service

3 timers listed.

[root@client vagrant]# borg list borg@192.168.56.160:/var/backup/
Remote: Warning: Permanently added '192.168.56.160' (ECDSA) to the list of known hosts.
etc-2023-06-27_21:33:48              Tue, 2023-06-27 21:33:49 [3e7f1e88fc81b27bc1be75e21c5a458ad79f3fd6368ecbea481f40baaac5bec1]
etc-2023-06-27_21:38:48              Tue, 2023-06-27 21:38:49 [29bf2239cac17581517e5936e4c92b28f7e289d70b3e0b8aaaf802bcb4661ddb]
[root@client vagrant]# systemctl list-timers --all
NEXT                         LEFT          LAST                         PASSED       UNIT                         ACTIVATES
Tue 2023-06-27 21:43:47 UTC  2min 35s left Tue 2023-06-27 21:38:47 UTC  2min 24s ago borg-backup.timer            borg-backup.service
Wed 2023-06-28 21:30:47 UTC  23h left      Tue 2023-06-27 21:30:47 UTC  10min ago    systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service
n/a                          n/a           n/a                          n/a          systemd-readahead-done.timer systemd-readahead-done.service

3 timers listed.
[root@client vagrant]# systemctl list-timers --all
NEXT                         LEFT          LAST                         PASSED       UNIT                         ACTIVATES
Tue 2023-06-27 21:43:47 UTC  1min 23s left Tue 2023-06-27 21:38:47 UTC  3min 36s ago borg-backup.timer            borg-backup.service
Wed 2023-06-28 21:30:47 UTC  23h left      Tue 2023-06-27 21:30:47 UTC  11min ago    systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service
n/a                          n/a           n/a                          n/a          systemd-readahead-done.timer systemd-readahead-done.service

3 timers listed.
[root@client vagrant]# systemctl list-timers --all
NEXT                         LEFT          LAST                         PASSED       UNIT                         ACTIVATES
Tue 2023-06-27 21:43:47 UTC  1min 22s left Tue 2023-06-27 21:38:47 UTC  3min 37s ago borg-backup.timer            borg-backup.service
Wed 2023-06-28 21:30:47 UTC  23h left      Tue 2023-06-27 21:30:47 UTC  11min ago    systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service
n/a                          n/a           n/a                          n/a          systemd-readahead-done.timer systemd-readahead-done.service

3 timers listed.
[root@client vagrant]# systemctl list-timers --all
NEXT                         LEFT          LAST                         PASSED       UNIT                         ACTIVATES
Tue 2023-06-27 21:49:47 UTC  1min 36s left Tue 2023-06-27 21:44:47 UTC  3min 23s ago borg-backup.timer            borg-backup.service
Wed 2023-06-28 21:30:47 UTC  23h left      Tue 2023-06-27 21:30:47 UTC  17min ago    systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service
n/a                          n/a           n/a                          n/a          systemd-readahead-done.timer systemd-readahead-done.service

3 timers listed.
[root@client vagrant]# borg list borg@192.168.56.160:/var/backup/
Remote: Warning: Permanently added '192.168.56.160' (ECDSA) to the list of known hosts.
etc-2023-06-27_21:33:48              Tue, 2023-06-27 21:33:49 [3e7f1e88fc81b27bc1be75e21c5a458ad79f3fd6368ecbea481f40baaac5bec1]
etc-2023-06-27_21:38:48              Tue, 2023-06-27 21:38:49 [29bf2239cac17581517e5936e4c92b28f7e289d70b3e0b8aaaf802bcb4661ddb]
etc-2023-06-27_21:44:48              Tue, 2023-06-27 21:44:49 [1ee3ecfbf43c95e7e9e5e11d3c3cf1ba53c52bbcaadb718a23d98cbc9084e2ad]

```
