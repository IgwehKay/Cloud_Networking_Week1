# Tells the script to exit immediately if any command fails. This prevents further steps from running on error.
```
#!/bin/bash'
set -e
```

# Extract unique groups from users.csv and create them if they don't exist

```
   cut -d',' -f2 users.csv | sort -u | while read group; do`
    if ! getent group "$group" > /dev/null; then`
   echo "Creating group $group"`
    groupadd "$group"`
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
