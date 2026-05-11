# SinaSpeedZSW1

## Description

Defines a structure that contains the bits from ZSW1 of telegram 1.

## Structure

| Name | Type | Description |
| ---- | ---- | ----------- |
| ReadyForSwitchOn | `BOOL` | TRUE = ready for switching on |
| ReadyForOperation | `BOOL` | TRUE = ready for operation |
| OperationEnabled | `BOOL` | TRUE = operation enabled |
| FaultPresent | `BOOL` | TRUE = fault present |
| NoCoastDownActive | `BOOL` | TRUE = no coast down active (OFF2 inactive) |
| NoQuickStopActive | `BOOL` | TRUE = no quick stop active (OFF3 inactive) |
| SwitchOnInhibitedActive | `BOOL` | TRUE = switch on inhibited active |
| AlarmPresent | `BOOL` | TRUE = alarm present |
| ActualSpeedDeviation | `BOOL` | TRUE = actual speed value differs to setpoint |
| ControlRequested | `BOOL` | TRUE = PLC control requested |
| SpeedSetpointReached | `BOOL` | TRUE = speed setpoint is reached |
| DriveOperatingWithinLimits | `BOOL` | TRUE = power/torque/current limit not reached |
| HoldingBrakeOpen | `BOOL` | TRUE = holding brake is open |
| NoMotorOvertemperature | `BOOL` | TRUE = no motor overtemperature alarm |
| CurrentMotorDirection | `BOOL` | TRUE =  Motor rotates forwards, FALSE = Motor rotates backwards |
| NoCriticalAlarms | `BOOL` | TRUE = No alarm, thermal overload, power unit |

## Usage

The type is used as a diagnostics structure for the function block [SinaSpeed](../blocks/sinaspeed.md).
