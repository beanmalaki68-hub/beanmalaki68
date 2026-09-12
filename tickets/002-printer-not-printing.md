# Ticket #002 — Printer Not Printing

## Ticket Information

- Ticket Number: 002
- Priority: Normal
- Department: IT Support
- User: Sarah Mitchell
- Device: Windows 11 Workstation
- Printer: Office-Printer-01

## User Report

Sarah reported that her documents were not printing. Windows showed the printer as offline. The printer was powered on and showed Ready. Other employees were able to print successfully.

## Troubleshooting Performed

1. Checked printer status in Windows.
2. Confirmed the printer was showing Offline.
3. Checked the print queue and found three stuck print jobs.
4. Cancelled the stuck print jobs.
5. Retried printing and confirmed the issue remained.
6. Confirmed the printer was using a Standard TCP/IP Port.
7. Confirmed the printer IP address was `192.168.4.50`.
8. Pinged `192.168.4.50` and received successful replies.
9. Checked the Print Spooler service and confirmed it was running.
10. Opened the printer queue and discovered that **Use Printer Offline** was enabled.
11. Disabled **Use Printer Offline**.
12. Asked Sarah to retry printing.

## Root Cause

The **Use Printer Offline** setting was enabled in Windows, causing the computer to treat the printer as offline even though the printer was reachable over the network.

## Resolution

Disabled **Use Printer Offline** for Office-Printer-01.

## Verification

Sarah successfully printed her document after the setting was disabled.

## Final Status

**Resolved**

## Skills Practiced

- Windows printer troubleshooting
- Print queue management
- PowerShell
- Print Spooler
- TCP/IP networking
- Ping
- Printer configuration
- Root-cause analysis
- Troubleshooting documentation
