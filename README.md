# HarvestGuard — Phase 1 Skeleton

This is the very first working piece: a terminal and a server talking to
each other over a socket, with a hardcoded PIN check. No cryptography yet —
the whole point is to prove the message flow (`code in -> ACCESS GRANTED /
DENIED out`) works before anything else gets layered on top.

## Run it

Open two terminal windows in this folder.

**Window 1 — start the server first:**
```
python3 server.py
```

**Window 2 — run the terminal (client):**
```
python3 terminal.py
```
Enter `1234` or `0000` for ACCESS GRANTED, anything else for ACCESS DENIED.

## What this proves

- The two-process (terminal / server) shape you'll keep for the whole project.
- A real network round-trip, so swapping in Wi-Fi + an actual ESP32 later is
  just a transport change, not a redesign.

## Next steps (in order)

1. **Weak RSA handshake** — replace the plaintext `code` with a real RSA key
   exchange, but pick a small key size (a few hundred bits) so it's
   factorable live on stage in well under a minute.
2. **Attacker script** — a third script that sniffs the handshake between
   `terminal.py` and `server.py`, factors the small RSA modulus, and forges
   its own ACCESS GRANTED. Get this reliable (5/5 runs) before moving on.
3. **Kyber swap** — install `liboqs-python` and replace the RSA step with a
   Kyber key exchange. Re-run the same attacker script unmodified and
   confirm it now fails.
4. Only after 1–3 are solid: decide the ESP32 crypto path (wolfSSL's Kyber
   support, or a dedicated ESP32 Kyber port) and start the hardware build.
