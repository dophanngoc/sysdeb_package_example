### Run
`&user@:~# sudo dpkg-buildpackage -us -uc -b`

### Debug
```
&user@:~# journalctl -u <servicename>.service

&user@:~# systemctl daemon-reload
&user@:~# LANG=C systemctl list-dependencies --after <servicename>.service
&user@:~# LANG=C systemctl list-dependencies --before <servicename>.service

&user@:~# systemd-analyze plot > timing_test.svg

```
open timming_test by any browser

### Modify to add start order, dependency service
```
&user@:~# vi /etc/systemd/system/app-test-init.service
[Unit]
Description = system_init_test daemon
After = before_test.service   #add this line
...
```
