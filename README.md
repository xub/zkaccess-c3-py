# zkaccess-c3

A pure Python library for communicating with ZKTeco C3 access control panels.

**Fork with user management write support** — adds `set_user()` method for adding users to the panel (cmd 0x07, validated).

## Features

- Connect to C3 panels via TCP
- Read device configuration and parameters
- Get user database
- **Write users to panel** (original library was read-only)
- Control door relays
- Read transaction/access logs
- Device restart

## Installation

```bash
pip install zkaccess-c3 @ git+https://github.com/xub/zkaccess-c3-py.git@main
```

## Usage

```python
from zkaccess_c3 import C3

panel = C3("<PANEL_IP>", port=4370)
panel.connect()

# Read users
users = panel.get_device_data("user")

# Add user (write support)
panel.set_user(card=777000000, pin=999)

panel.disconnect()
```

## Note

Delete user support is not yet implemented (no validated wire found).
