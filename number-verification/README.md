# Python CAMARA Number Verification API (Mock)

This project is a Python-based mock server for the CAMARA Number Verification API, built with FastAPI.

It simulates the "southbound" logic of checking against a 5G core (like free5gc) by:

1. Receiving the API call (e.g., `/verify`)
2. Checking the request's source IP address
3. Comparing that IP against a mock database (simulating the UDM/SMF) to see if the phone number and IP match an active PDU session

## 🚀 Setup & Run (using uv)

This project uses `uv` for package management.

### 1. Install dependencies:

```bash
# uv will automatically create a virtual environment and install packages
uv sync
```

**Or if you don't have a pyproject.toml yet, install packages directly:**

```bash
uv add fastapi uvicorn
```

### 2. Run the API server:

```bash
# uv run automatically uses the virtual environment
uv run python camara_api.py
```

The server will be running at http://127.0.0.1:8000.

## 🧪 Testing the Mock API

You can test this using `curl` from your terminal.

### Mock User

The mock 5G core is pre-provisioned with one user (from `camara_api.py`):

- **Phone Number:** `+15551234567`
- **Expected IP (from UERANSIM):** `192.168.1.10`

### Test 1: Successful Verification (Correct IP)

This simulates the request coming from the device with the correct IP. We use `curl`'s `--interface` option to fake our source IP (or you can test this from a machine with that IP).

> **Note:** Faking a source IP often requires admin/sudo privileges and varies by OS. The easiest test is to change `MOCK_UERANSIM_IP` in `camara_api.py` to `127.0.0.1` and test from your local machine.

Assuming you've set `MOCK_UERANSIM_IP = "127.0.0.1"`:

```bash
# Test the /verify endpoint
curl -X POST "http://127.0.0.1:8000/verify" \
     -H "Content-Type: application/json" \
     -d '{"phoneNumber": "+15551234567"}'

# Expected Response:
# {"verified":true}
```

```bash
# Test the /device-phone-number endpoint
curl "http://127.0.0.1:8000/device-phone-number"

# Expected Response:
# {"phoneNumber":"+15551234567"}
```

### Test 2: Failed Verification (Wrong Number)

```bash
curl -X POST "http://127.0.0.1:8000/verify" \
     -H "Content-Type: application/json" \
     -d '{"phoneNumber": "+15559990000"}'

# Expected Response:
# {"verified":false}
```

### Test 3: Failed Verification (Wrong IP)

If you run the test from a machine other than `127.0.0.1` (or change the mock IP back to `192.168.1.10`), the request's source IP won't match the database, and the verification will fail.

```bash
# Request from a different IP (e.g., 192.168.1.20)
curl -X POST "http://<server-ip>:8000/verify" \
     -H "Content-Type: application/json" \
     -d '{"phoneNumber": "+15551234567"}'

# Expected Response:
# {"verified":false}
```
