# Linux User Creation Bash Script

This Bash script automates the process of creating users, setting passwords, and assigning groups on a Unix-like system. It reads user information from an input file and logs the operations performed.

## Prerequisites

- Ensure you have `sudo` privileges on the system where you are running this script.
- Ensure `openssl` is installed for password generation.

## Script Usage

### Input File Format

The input file should have the following format:

```
username1;group1,group2
username2;group1,group3
...
```

Each line contains a username and a comma-separated list of groups.

### Running the Script

```sh
./user_management.sh inputfile.txt
```

- Replace `inputfile.txt` with the path to your input file.

## Script Details

### Logging and Password Storage

- **Log File**: `/var/log/user_management.log`
  - Logs all actions performed by the script.
- **Password File**: `/var/secure/user_passwords.csv`
  - Stores generated passwords securely with restricted permissions (`600`).

### Functions

- **user_exists**: Checks if a user already exists.
- **group_exists**: Checks if a group already exists.
- **user_in_group**: Checks if a user is a member of a group.

### Operations

1. **Check Input File**: The script checks if the input file exists. If not, it exits with an error message.
2. **Initialize Log and Password Files**: Creates and sets appropriate permissions for the log and password files if they do not already exist.
3. **Read and Process Input File**: For each line in the input file:
   - Trims whitespace from the username and group list.
   - Checks if the user exists.
     - If the user exists, logs the information.
     - If the user does not exist, creates the user and generates a random password.
   - Ensures the user's home directory exists.
   - Checks each group in the group list:
     - If the group does not exist, creates it.
     - Adds the user to the group if they are not already a member.

## Example

```
Foster;developers,admins
Jayson;designers,managers
Doris;developers
```

### Running the Example

```sh
./user_management.sh example_input.txt
```

This will create users `Foster`, `Jayson`, and `Doris`, set their passwords, and add them to the specified groups.

## Notes

- Ensure the script has execute permissions:

  ```sh
  chmod +x user_management.sh
  ```

- This script must be run with `sudo` to ensure it has the necessary permissions to create users, groups, and write to log and password files.

## Security

- Passwords are generated using `openssl rand -base64 12`.
- Passwords are stored in a file with restricted permissions (`600`).
- Log file permissions are also restricted to maintain security.
