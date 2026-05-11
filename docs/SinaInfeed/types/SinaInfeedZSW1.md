# SinaInfeedZSW1

## Description

Defines a structure that contains the bits from ZSW1 of telegram 370 and telegram 371.

## Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| ReadyForSwitchOn | `BOOL` | TRUE: Drive is ready to be switched on |
| ReadyForOperation | `BOOL` | TRUE: Drive is ready for operation |
| OperationEnabled | `BOOL` | TRUE: Operation of drive is enabled |
| FaultPresent | `BOOL` | TRUE: A fault is present at the drive |
| NoOFF2 | `BOOL` | TRUE: No OFF2 is active |
| SwitchingOnInhibited | `BOOL` | TRUE: Switching drive on is inhibited |
| AlarmPresent | `BOOL` | TRUE: An alarm is present at the drive |
| PlcRequestsControl | `BOOL` | TRUE: PLC requests control |
| PrechargingCompleted | `BOOL` | TRUE: Precharging of the drive is complete |
| LineContactorClosed | `BOOL` | TRUE: Line contactor is closed |

## Usage

The type is used as a diagnostics structure for the function blocks [SinaInfeed](../blocks/sinainfeed.md) and [SinaInfeedTlg371](../blocks/sinainfeedtlg371.md).
