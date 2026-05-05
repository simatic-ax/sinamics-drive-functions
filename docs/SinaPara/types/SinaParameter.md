# SinaParameter

## Description

The InOut variable `parameter` acts as a container for the individual parameters that are read or written with the `SinaPara` function block.
The following values have to be supplied always by the user:

- `parameter[i].siParaNo`: parameter number
- `parameter[i].siIndex`: parameter index

The following values havt to be supplied for writing operation:

- `parameter[i].srValue`: parameter value for `REAL` type fields
- `parameter[i].sdValue`: parameter value for `DWORD`/`DINT` type fields

## Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| siParaNo | `UINT` | Number of parameter (Number 1..65535) |
| siIndex | `UINT` | Subindex (Number 1..65535) |
| srValue | `REAL` | Value of parameter |
| sdValue | `DINT` | Value of parameter |
| slValue | `LREAL` | Value of parameter |
| syFormat | `BYTE` | Format of value (Format 0x40..0x44) |
| swErrorNo | `WORD` | Error number (see table below) |

## Usage

This type is used in arrays to specify datasets (parameter numbers and their values) that can be read or written to the drive in [SinaPara](../blocks/sinapara.md).
