# zkaccess-c3

A pure Python library for communicating with ZKTeco C3 access control panels.

**Fork with user management write support** — adds `set_user()` (cmd 0x07) and `delete_user()` (cmd 0x09), both validated against a live C3 panel.

## Features

- Connect to C3 panels via TCP
- Read device configuration and parameters
- Get user database
- **Write users to panel** (original library was read-only)
- **Delete users from panel**
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

# Delete user (by card, or pass pin=... to match on PIN instead)
panel.delete_user(card=777000000)

panel.disconnect()
```

## Write Operations

Both `set_user()` and `delete_user()` are **validated against a live C3 panel**:

- **`set_user(card, pin, ...)`** (cmd 0x07): Adds a user. Tested: 0→1 users confirmed.
- **`delete_user(card=None, pin=None)`** (cmd 0x09): Deletes a user by CardNo (default) or Pin. 
  Tested: 31→30 users confirmed deleted and verified via `get_device_data("user")` readback.

Both operations use proper little-endian per-field encoding matching ZKTeco's wire format.
