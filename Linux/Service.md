A service is a **background program (daemon)** that starts automatically or on demand to provide a function to the system or other programs.
- **Runs in background**:
    
	No UI, no user interaction. Works silently.
    
- **Managed by init system**:
    
	On modern Linux, services are managed by **systemd**.
    
- **Examples**:
- `sshd` → handles SSH connections
- `cron` → runs scheduled jobs
- `nginx` / `apache2` → web servers
- `docker` → container engine
- **Common operations**:
- Start: `systemctl start service_name`
- Stop: `systemctl stop service_name`
- Status: `systemctl status service_name`
- Enable at boot: `systemctl enable service_name`
- **Where defined**:
- Unit files: `/usr/lib/systemd/system/` or `/etc/systemd/system/`
- **Simple analogy**:
    
	A service is like a **server-side worker** that’s always ready to do its job when needed.
    
**Sources**:
- systemd docs: https://www.freedesktop.org/wiki/Software/systemd/
- Red Hat Linux Services: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/
- Arch Wiki – systemd: https://wiki.archlinux.org/title/systemd