# SinaSpeed

You use the function block `SinaSpeed` to cyclically control the speed of an axis. Connect the according hardware identifier of the drive telegram to the inputs `hwidSTW` and `hwidZSW`. Set the input `referenceSpeed` to the respective value configured in the drive system (For SINAMICS drive system the value is stored in parameter p2000).

## Parameters

| Name             | Section | Type                     | Description                                                                                                                  |
| ---------------- | ------- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| enableAxis       | Input   | `BOOL`                   | TRUE = enable axis, FALSE = switch off drive (OFF1)                                                                          |
| acknowledgeError | Input   | `BOOL`                   | Acknowledge drive errors with rising edge                                                                                    |
| speedSetpoint    | Input   | `REAL`                   | Speed setpoint in rpm                                                                                                        |
| referenceSpeed   | Input   | `REAL`                   | Reference speed in rpm                                                                                                       |
| config           | Input   | `SinaSpeedConfiguration` | Configuration for axis, see [SinaSpeedConfiguration](../types/sinaspeed.md#sinaspeedconfiguration) for further details       |
| hwidSTW          | Input   | `UINT`                   | Hardware ID of the SIMATIC S7-1200/1500 setpoint slot                                                                        |
| hwidZSW          | Input   | `UINT`                   | Hardware ID of the SIMATIC S7-1200/1500 actual value slot                                                                    |
| axisEnabled      | Output  | `BOOL`                   | TRUE = axis is enabled                                                                                                       |
| lockout          | Output  | `BOOL`                   | TRUE = switch-on inhibited                                                                                                   |
| actualSpeed      | Output  | `REAL`                   | Actual speed in rpm                                                                                                       |
| ZSW1             | Output  | `SinaSpeedZSW1`          | Status of ZSW1, see [SinaSpeedZSW1](../types/sinaspeed.md#sinaspeedzsw1)                                                     |
| error            | Output  | `BOOL`                   | TRUE = fault in function block present                                                                                       |
| status           | Output  | `SinaSpeedStatus`        | Status of the function block, see [SinaSpeedStatus](../types/sinaspeed.md#sinaspeedstatus) for further details               |
| diagnosticsId    | Output  | `WORD`                   | Return values of communication errors (ReadData and WriteData)                                                               |

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
