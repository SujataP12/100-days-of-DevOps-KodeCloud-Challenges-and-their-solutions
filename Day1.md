## Day 1 – Linux User Setup with Non-Interactive Shell

A Linux user with a non-interactive shell is typically used for service accounts or restricted users who should not be able to log in interactively (e.g., via SSH). 
They can still be used for running background processes, file transfers, or automation.

### To create a user with a non-interactive shell (preventing login access):

```bash
# Create user with /sbin/nologin shell
sudo useradd -m -s /sbin/nologin rahul

# OR using /bin/false
sudo useradd -m -s /bin/false rahul

### For verification check this 
grep rahul /etc/passwd

### and the output will be like
rahul:x:1001:1001::/home/rahul:/sbin/nologin ```

Here rahul is my user


