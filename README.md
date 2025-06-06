# Kvand

This is the backend component of [KVantage app](https://github.com/kosail/KVantage), a minimal control center for Lenovo laptops on Linux.

---
KVantage Battery Daemon (`kvand`) is a daemon written in Go specifically for Linux systems. Its purpose is to provide a secure and efficient way to interact with Lenovo-specific ACPI interfaces exposed through `/proc/acpi/call`.

> As you may already know, these interfaces allow reading and writing settings such as battery conservation mode, rapid charging, and performance profiles, but they require root privileges to access.

<br>
The daemon runs as a long-lived background process launched once at application startup. I decided to do it in this way. This is because otherwise and working as a CLI tool like batmanager, it would require asking for the user's password at startup and at every action related to the battery or performance options. Even though CLI tools are great, they are not as intuitive or attractive for standard users or new linux users. This daemon would be a better fit, asking for the user's password at the startup of the program, and having it as a service executed until the GUI is closed.

But going back to the daemon thing. It is responsible for receiving commands from the graphical interface via standard input (stdin), executing the appropriate system-level actions, and returning results through standard output (stdout). This model avoids running the full GUI application with elevated privileges, which is considered unsafe and bad practice. Instead, only the minimal backend runs as root, reducing attack surface and improving system integrity.

At launch, `kvand` checks whether it is running with root permissions. If it is not, it will attempt to re-execute itself using `pkexec`, prompting the user for their password through a standard system authentication dialog. This ensures that the daemon has the necessary permissions without forcing the entire application to start or run with escalated privileges.

The communication between the GUI and the daemon uses a simple, CLI-style text protocol such as `get conservation` or `set performance 2` (syntax inspired by batmanager), making the interaction lightweight, debuggable, and secure. This also allows the daemon to validate all commands internally, ignoring anything unexpected or malformed and improving the security a little bit. I think I made a great work enforcing checks and avoiding any undesired situation.


---

## 🤝 Contributing
Contributions are welcome! Feel free to fork the repository and submit pull requests. If you have ideas, suggestions, or bug reports, open an issue on GitHub.


[GPLv3 (GNU General Public License v3)](LICENSE.txt) – Free to use, modify, and distribute as long as this remains open source, and it is not use for profitable purposes.

    GPLv3 Logos:
    Copyright © 2012 Christian Cadena
    Available under the Creative Commons Attribution 3.0 Unported License.


---
> **Note:** KVantage is a personal learning project and is not affiliated with Lenovo or any other brand or product.
---
KVantage - kvand Copyright © 2025, kosail
<br>
With love, from Honduras.