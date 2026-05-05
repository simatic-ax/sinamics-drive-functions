# SinaInfeedConfiguration

## Description

Defines a configuration structure that contains the bits from STW1 of telegram 370 and telegram 371.

## Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| OFF2 | `BOOL` | TRUE = enable is possible, FALSE = immediate pulse suppression and switching on inhibited |
| DisableMotorOperation | `BOOL` | TRUE = disable motor operation |
| DisableGeneratorOperation | `BOOL` | TRUE = disable motor operation |
| STW1_2 | `BOOL` | Reserved bit in STW1 |
| STW1_4 | `BOOL` | Reserved bit in STW1 |
| STW1_8 | `BOOL` | Reserved bit in STW1 |
| STW1_9 | `BOOL` | Reserved bit in STW1 |
| STW1_11 | `BOOL` | Reserved bit in STW1 |
| STW1_12 | `BOOL` | Reserved bit in STW1 |
| STW1_13 | `BOOL` | Reserved bit in STW1 |
| STW1_14 | `BOOL` | Reserved bit in STW1 |
| STW1_15 | `BOOL` | Reserved bit in STW1 |

## Usage

The type is used as a configuration structure for the function blocks [SinaInfeed](../blocks/sinainfeed.md) and [SinaInfeedTlg371](../blocks/sinainfeedtlg371.md).
