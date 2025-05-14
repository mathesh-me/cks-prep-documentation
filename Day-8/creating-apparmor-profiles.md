## Creating AppArmor Profiles

### Create a sample Script to which we want to create AppArmor profile for

```sh
#!/bin/bash
data_directory=/opt/app/data
mkdir -p "${data_directory}"
echo "=> File created at $(date)" | tee "${data_directory}/create.log"
```

### Create AppArmor Profile

- Install `apparmor-utils` package:
```bash
apt install apparmor-utils
```
- Run `aa-genprof` command to create a new AppArmor profile for the script:
```bash
sudo aa-genprof /path/to/script.sh
```
It will prompt you to run the script to collect information about the system calls it makes. Run the script in a separate terminal:
```bash
/path/to/script.sh
```
- After running the script, return to the terminal where you ran `aa-genprof` and follow the prompts to create the profile. It will ask you to allow or deny specific system calls and file accesses. After answering the prompts, click `s` to save and `f` to finish.
- This profile wil get stored in `/etc/apparmor.d/` directory with a name like `usr.bin.script.sh`.
- To confirm that the profile is in enforced mode, use the following command:
```bash
sudo aa-status
```

### Existing AppArmor Profiles

- To load a existing AppArmor profile, use the following command:
```bash
sudo apparmor_parser /etc/apparmor.d/usr.bin.script.sh
```
- To unload a AppArmor profile, use the following command:
```bash
sudo apparmor_parser -R /etc/apparmor.d/usr.bin.script.sh
```

Date of Commit: 14/05/2025