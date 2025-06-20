# Write a bash script that create 20 users from a csv file with default password.

10 users to sys-admin group
5 users to data-admin group
5 users to database group

## Implement Q1 using ansible playbook



### Solution

```
#!/bin/bash'
```
# Tells the script to exit immediately if any command fails. This prevents further steps from running on error.

```
set -e
```

# Extract unique groups from users.csv and create them if they don't exist

```
   cut -d',' -f2 users.csv | sort -u | while read group; do
    if ! getent group "$group" > /dev/null; then
   echo "Creating group $group"
    groupadd "$group"
   fi
   done
```
# Read each line of users.csv

```
while IFS=',' read -r username group; do
  # Create the user without specifying the group (use default group)
  if ! id "$username" &>/dev/null; then
    useradd -m "$username"
    echo "$username:P@ssw0rd" | chpasswd
    echo "User $username created."
  else
    echo "User $username already exists. Skipping creation."
  fi
```

  # Add user to the group as a secondary group
  ```
  usermod -aG "$group" "$username"
  echo "User $username added to group $group."
done < users.csv
```
#Result
![image](https://github.com/user-attachments/assets/cc6cb2a3-dc0f-4307-a099-3728f9e1c992)
