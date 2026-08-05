# Architecture Diagram

## Lab topology

```mermaid
flowchart LR
    Client["Client-01\nDomain-joined Windows client"]
    DC["DC-01\nWindows Server 2022\nAD DS + DNS + Print Server"]
    Office["KH-DAL-Office-Printer-01\nShared simulated office printer"]
    Plotter["KH-DAL-Engineering-Plotter-01\nShared simulated plotter"]
    Port["Standard TCP/IP port\n10.10.1.50:9100\nRaw protocol"]
    Group["KH-Printer-Users\nAD security group"]

    Client -->|DNS and domain connectivity| DC
    Client -->|UNC shared-printer connection| Office
    DC -->|Centralized Print Management| Office
    DC -->|Centralized Print Management| Plotter
    Plotter --> Port
    Group -.->|Print permission| Office
    Group -.->|Intended access control| Plotter
```

## Component relationships

- `Client-01` uses the lab domain and validates end-user access to the shared printer.
- `DC-01` provides the directory, DNS, and print-management services used in this time-limited lab.
- `KH-Printer-Users` is the access-control boundary for ordinary printer users.
- The office-printer queue represents a shared office device.
- The plotter queue represents an engineering device whose network path can be diagnosed independently.

## Production difference

The lab consolidates services on `DC-01` to reduce Azure cost and setup time. In production, a dedicated print server or managed print platform would normally be considered, along with tested vendor drivers, restricted administration, monitoring, redundancy, and change control.
