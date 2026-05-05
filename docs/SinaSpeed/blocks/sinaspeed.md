# SinaSpeed

## Principle of Operation

You can use the function block `SinaSpeed` to cyclically control the speed of an axis via telegram 1. Connect the according hardware identifier of the drive telegram to the inputs `hwidSTW` and `hwidZSW`. Set the input `referenceSpeed` to the respective value configured in the drive system (For SINAMICS drive system the value is stored in parameter p2000).

## Interface

### Input Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| enableAxis | `BOOL` | TRUE = enable axis, FALSE = switch off drive (OFF1) |
| acknowledgeError | `BOOL` | Acknowledge drive errors with rising edge |
| speedSetpoint | `REAL` | Speed setpoint in rpm |
| referenceSpeed | `REAL` | Reference speed in rpm |
| config | [`SinaSpeedConfiguration`](../types/SinaSpeedConfiguration.md#structure) | Configuration for axis |
| hwidSTW | `UINT` | Hardware ID of the SIMATIC S7-1200/1500 setpoint slot |
| hwidZSW | `UINT` | Hardware ID of the SIMATIC S7-1200/1500 actual value slot |

### Output Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| axisEnabled | `BOOL` | TRUE = axis is enabled |
| lockout | `BOOL` | TRUE = switch-on inhibited |
| actualSpeed | `REAL` | Actual speed in rpm |
| ZSW1 | [`SinaSpeedZSW1`](../types/SinaSpeedZSW1.md#structure) | Status of ZSW1 |
| error | `BOOL` | TRUE = fault in function block present |
| status | [`SinaSpeedStatus`](../enums/SinaSpeedStatus.md) | Status of the function block |
| diagnosticsId | `WORD` | Return values of communication errors (ReadData and WriteData) |

## Example

```iec-st
USING Simatic.Ax.Sinamics;

PROGRAM SampleProgram
  VAR_EXTERNAL
    axisTelegram1: UINT;
  END_VAR

  VAR
    _enableAxis: BOOL;
    _acknowledgeError: BOOL;
    _moveForward: BOOL;
    _moveBackward: BOOL;
    _sinaSpeed: SinaSpeed;
  END_VAR

  VAR CONSTANT
    REFERENCE_SPEED: REAL := 6000;
  END_VAR

  // check axis is enabled and select speed and direction
  IF _sinaSpeed.axisEnabled THEN
    IF _moveForward AND NOT _moveBackward THEN
      _sinaSpeed.speedSetpoint := 3000;
    ELSIF _moveBackward AND NOT _moveForward THEN
      _sinaSpeed.speedSetpoint := -3000;
    ELSE
      _sinaSpeed.speedSetpoint := 0;
    END_IF;
  ELSE
    _sinaSpeed.speedSetpoint := 0;
  END_IF;

  // call sina speed cyclically
  _sinaSpeed(enableAxis := _enableAxis, acknowledgeError := _acknowledgeError, referenceSpeed := REFERENCE_SPEED, hwidSTW := axisTelegram1, hwidZSW := axisTelegram1);
END_PROGRAM
```
