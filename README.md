# Wireshark Network Traffic Analysis

A basic project documenting how to analyze a packet capture in Wireshark — identify normal traffic, follow TCP streams, and detect a SYN flood.

---

## Overview

This project uses Wireshark to examine a provided capture file. The goal is to:

- Understand what protocols and hosts are present
- Identify the TCP three-way handshake
- Follow a TCP conversation
- Detect suspicious traffic (SYN flood)
- Record the key findings

---

## Requirements

- Wireshark 4.x
- A sample capture file (pcap or pcapng)

---

## Setup

1. Open Wireshark.
2. Go to **File → Open** and select the capture file.
3. Check the **status bar** at the bottom. It shows the total number of packets, the number currently displayed, and how many were dropped.
   - **Packets** = total captured
   - **Displayed** = packets matching the current filter

---

## Analysis

### 1. Protocol overview

**Statistics → Protocol Hierarchy**

Shows which protocols appear in the capture and their share of traffic. Note the top protocols.

---

### 2. Endpoints

**Statistics → Endpoints → IPv4**

- Lists every IP with packet counts.
- Click **Packets** to sort.
- The top talkers are the most active hosts.

---

### 3. TCP three-way handshake

Use a display filter to show SYN and ACK packets. Look for the three packets that start a connection:

| Packet | Flags | Meaning |
|---|---|---|
| 1 | SYN | Client requests connection |
| 2 | SYN-ACK | Server accepts |
| 3 | ACK | Client confirms |

Expand **Transmission Control Protocol** in the packet details to see the flags.

---

### 4. Follow a TCP stream

1. Right-click a packet → **Follow → TCP Stream**.
2. The window shows the full conversation.
3. The title bar shows the stream number. Note the number that follows "tcp.stream eq".

---

### 5. Detect a SYN flood

A SYN flood sends many connection requests without completing the handshake.

**Steps:**

1. Apply a display filter that shows only initial SYN packets (SYN set, ACK not set).
2. Read the **Displayed** count in the status bar — this is the number of SYN packets.
3. Open **Statistics → Endpoints → IPv4**.
4. Enable **"Limit to display filter"**.
5. Sort by **Packets** descending.
6. The top **source** IP with a large count is the **attacker**.
7. The common **destination** IP is the **victim**.

Normal hosts send 1–2 SYNs. The attacker sends a long block in a row.

---

### 6. Chat stream

If the capture contains chat traffic (commonly IRC on port 6667):

1. Apply a display filter for the chat port.
2. Right-click a chat packet → **Follow → TCP Stream**.
3. Read the conversation (JOIN, NICK, PRIVMSG).
4. Note the stream number from the window title.

---



---

## Conclusion

Summarize the analysis in a few lines:

- Most traffic was normal (e.g., DNS, HTTP).
- One host sent an abnormal number of SYN packets, indicating a SYN flood.
- The attacker's IP was __________, targeting __________.
- The chat conversation was found in stream __________.

---

## Notes

- Display filters are non-destructive — apply and remove them freely.
- If "Displayed" is lower than "Packets", a filter is hiding traffic; clear it with the **X**.
- Always analyze captures you are authorized to access.

---
