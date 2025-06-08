#podman
To resolve the warning `WARN[0001] "/" is not a shared mount...` in rootless containers (e.g., Podman/Docker), follow these steps:

---

### 1. **Temporarily Fix Mount Propagation (Non-Persistent)**
   ```bash
   sudo mount --make-shared /
   ```
   This changes the root (`/`) mount propagation to "shared" for the current session.  
   **Note:** This resets on reboot.

---

### 2. **Make the Change Persistent**
   #### **Option A: Systemd Service (Recommended)**
   - Create a systemd mount unit:
     ```bash
     sudo systemctl edit --force --full root-shared.mount
     ```
   - Paste this configuration:
   ```bash
     [Unit]
     Description=Make root filesystem shared

     [Mount]
     What=/
     Where=/
     Options=shared

     [Install]
     WantedBy=local-fs.target
     ```
   - Enable the service:
     ```bash
     sudo systemctl enable --now root-shared.mount
     ```

   #### **Option B: Modify `/etc/fstab` (Advanced)**
   Add this line to `/etc/fstab`:
   ```bash
   /    /    none    bind,shared    0 0
   ```
   Then run:
   ```bash
   sudo mount -a
   ```
   **Caution:** Incorrect edits to `/etc/fstab` can break your system. Backup first!

---

### 3. **Configure User Namespaces (Podman/Docker Rootless)**
   - Ensure proper subuid/subgid mappings:
     ```bash
     sudo usermod --add-subuids 100000-165535 --add-subgids 100000-165535 $USER
     ```
   - Enable lingering for your user (allows user services to run at boot):
     ```bash
     sudo loginctl enable-linger $USER
     ```

---

### 4. **Verify the Fix**
   ```bash
   findmnt -o PROPAGATION /
   ```
   The output should show `shared` for `/`.

---

### 5. **Apply Changes to Containers**
   - For Podman, run:
     ```bash
     podman system migrate
     ```

---

### Why This Happens:
- Rootless containers require `shared` mount propagation to manage volumes and namespaces properly. Some Linux distributions default to `private` propagation for security, causing conflicts.

---

If you lack `sudo` access, contact your system administrator or use container volumes within your home directory (avoiding system-wide mounts). For more details, see the [Rootless Podman Docs](https://github.com/containers/podman/blob/main/docs/tutorials/rootless_tutorial.md).