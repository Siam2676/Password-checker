# Password-checker
import subprocess

# ONLY works on Windows
try:
    data = subprocess.check_output(['netsh', 'wlan', 'show', 'profiles']).decode('utf-8').split('\n')
    profiles = [i.split(":")[1][1:-1] for i in data if "All User Profile" in i]

    for i in profiles:
        # Fixed 'wlar' to 'wlan' and '/n' to '\n'
        results = subprocess.check_output(['netsh', 'wlan', 'show', 'profile', i.strip(), 'key=clear']).decode('utf-8').split('\n')
        
        # Fixed "key content" to "Key Content"
        passwords = [b.split(":")[1][1:-1] for b in results if "Key Content" in b]
        
        try:
            print("{:<30}| {:<}".format(i.strip(), passwords[0]))
        except IndexError:
            print("{:<30}| {:<}".format(i.strip(), "No Password Found"))

except FileNotFoundError:
    print("Error: This script only works on Windows computers.")
