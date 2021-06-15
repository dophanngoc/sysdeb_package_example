### Setup autostart
- Copy unit file to systemd directory:
```
user@:~# cp sim7600-handler.service /etc/systemd/system/
user@:~# systemctl list-unit-files | grep app-test-init.service
app-test-init.service                       disable
```
- Enable service at boot
```
&user@:~# systemctl enable app-test-init.Service
Created symlink /etc/systemd/system/multi-user.target.wants/app-test-init.service → /etc/systemd/system/app-test-init.service.
user@:~# systemctl list-unit-files | grep system_init_test
system_init_test.service                    enabled
```
### Setup order to start at boot
- For example, we have before_test.service, we want to set app-teset-init.service to start after starting before_test.service
We modify app-test-init.service file like following:
```
&user@:~# vi /etc/systemd/system/app-test-init.service
[Unit]
Description = system_init_test daemon
After = before_test.service   #add this line
 
[Service]
ExecStart = /root/app-test.sh
Restart = always
Type = simple
 
[Install]
WantedBy = multi-user.target

&user@:~# systemctl daemon-reload
&user@:~# systemctl start app-test-init.service
```
- Check list
```
&user@:~# LANG=C systemctl list-dependencies --after system_init_test.service
&user@:~# LANG=C systemctl list-dependencies --before system_init_test.service
```

- Using systemd-analyze command to extract detail information.
```
&user@:~# systemd-analyze plot > timing_test.svg
```
- Open svg file by a web browser


