# SinaSpeedConfiguration

## Description

Defines a configuration structure that contains the bits from STW1 of telegram 1.

## Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| OFF2 | `BOOL` | TRUE = enable is possible, FALSE = immediate pulse suppression and switching on inhibited |
| OFF3 | `BOOL` | TRUE = enable is possible, FALSE = braking with OFF3 ramp, then pulse suppression and switching on inhibited |
| EnableOperation | `BOOL` | TRUE = enable operation, FALSE = inhibit operation |
| EnableRampFunctionGenerator | `BOOL` | TRUE = enable ramp-function generator, FALSE = inhibit ramp-function generator |
| ContinueRampFunctionGenerator | `BOOL` | TRUE = continue ramp-function generator, FALSE = stop ramp-function generator |
| EnableSetpoint | `BOOL` | TRUE = enable setpoint, FALSE = inhibit setpoint |
| SetpointInversion | `BOOL` | TRUE = setpoints inverted, FALSE = setpoints alike |
| OpenHoldingBrake | `BOOL` | TRUE = opens the holding brake, FALSE = closes the holding brake |
| RaiseMotorizedPotentiometer | `BOOL` | TRUE = raises motorized potentiometer |
| LowerMotorizedPotentiometer | `BOOL` | TRUE = loweres motorized potentiometer |

## Usage

The type is used as a configuration structure for the function block [SinaSpeed](../blocks/sinaspeed.md).
