# SinaPos

You use the function block `SinaPos` to control the axis via basic positioner technology function (EPOS) of SINAMICS drive systems. This requires that the basic positioner technology has been properly configured and telegram 111 has been selected in the drive. Connect the according hardware identifier of the drive telegram to the inputs `hwidSTW` and `hwidZSW`. With the `mode` input you can change beetween the operation modes such as positioning, setup mode, homing and jogging of the axis.

## Parameters

| Name                  | Section | Type                   | Description                                                                                                                 |
| --------------------- | ------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| mode                  | Input   | `SinaPosMode`          | Operating mode selection for MDI, see [SinaPosMode](../types/sinapos.md#sinaposmode) for further details                 |
| enableAxis            | Input   | `BOOL`                 | TRUE = enable axis, FALSE = switch off drive (OFF1)                                                                         |
| acknowledgeError      | Input   | `BOOL`                 | Acknowledge drive errors with rising edge                                                                                   |
| cancelTraversing      | Input   | `BOOL`                 | FALSE = Rejects the active traversing job                                                                                   |
| intermediateStop      | Input   | `BOOL`                 | FALSE = Interrupts the active traversing job                                                                                |
| positive              | Input   | `BOOL`                 | Positive traversing direction                                                                                               |
| negative              | Input   | `BOOL`                 | Negative traversing direction                                                                                               |
| jog1                  | Input   | `BOOL`                 | Jog signal source 1                                                                                                         |
| jog2                  | Input   | `BOOL`                 | Jog signal source 2                                                                                                         |
| flyingReferencing     | Input   | `BOOL`                 | Controls selection for flying referencing                                                                                   |
| executeMode           | Input   | `BOOL`                 | Activate traversing job/setpoint or activates homing or takes over home position                                            |
| position              | Input   | `DINT`                 | Position setpoint in LU for direct traversing operating modes or traversing block number in traversing block operation mode |
| velocity              | Input   | `DINT`                 | Velocity in 1000 LU/min for MDI operating mode                                                                              |
| overrideVelocity      | Input   | `INT`                  | Velocity override in %, effective from 0% to 199%                                                                           |
| overrideAcceleration  | Input   | `INT`                  | Acceleration override in %, effective from 0% to 100%                                                                       |
| overrideDeceleration  | Input   | `INT`                  | Deceleration override in %, effective from 0% to 100%                                                                       |
| config                | Input   | `SinaPosConfiguration` | Configuration for axis, see [SinaPosConfiguration](../types/sinapos.md#sinaposconfiguration) for further details            |
| hwidSTW               | Input   | `UINT`                 | Hardware ID of the SIMATIC S7-1200/1500 setpoint slot                                                                       |
| hwidZSW               | Input   | `UINT`                 | Hardware ID of the SIMATIC S7-1200/1500 actual value slot                                                                   |
| axisEnabled           | Output  | `BOOL`                 | TRUE = axis is enabled                                                                                                      |
| axisPositionOk        | Output  | `BOOL`                 | TRUE = target position of the axis reached                                                                                  |
| axisSetpointFixed     | Output  | `BOOL`                 | TRUE = setpoint is stationary (SINAMICS S/G120 FW < 4.8 / 4.7.9: r2199.0, SINAMICS S/G120 FW >= 4.8 / >= 4.7.9: r2683.2)    |
| axisReferenced        | Output  | `BOOL`                 | TRUE = home position is set                                                                                                 |
| axisWarning           | Output  | `BOOL`                 | TRUE = alarm of the drive pending                                                                                           |
| axisError             | Output  | `BOOL`                 | TRUE = drive fault                                                                                                          |
| lockout               | Output  | `BOOL`                 | TRUE = switch-on inhibited                                                                                                  |
| actualVeloctiy        | Output  | `DINT`                 | Actual veloctiy (standardized 40000000h = 100% p2000)                                                                       |
| actualPosition        | Output  | `DINT`                 | Actual position in LU                                                                                                       |
| actualTraversingBlock | Output  | `UINT`                 | Actual traversing block (0-63)                                                                                              |
| actualMode            | Output  | `SinaPosMode`          | Actual operating mode                                                                                                       |
| posZSW1               | Output  | `SinaPosZSW1`          | Status of POS_ZSW1, see [SinaPosZSW1](../types/sinapos.md#sinaposzsw1) for further details                                  |
| posZSW2               | Output  | `SinaPosZSW2`          | Status of POS_ZSW2, see [SinaPosZSW2](../types/sinapos.md#sinaposzsw2) for further detials                                  |
| actualWarning         | Output  | `WORD`                 | Current alarm number                                                                                                        |
| actualFault           | Output  | `WORD`                 | Current fault number                                                                                                        |
| error                 | Output  | `BOOL`                 | TRUE = fault in function block present                                                                                      |
| status                | Output  | `SinaPosStatus`        | Status of the function block, see [SinaPosStatus](../types/sinapos.md#sinaposstatus) for further details                    |
| diagnosticsId         | Output  | `WORD`                 | Return values of communication errors (ReadData and WriteData)                                                              |

## Example

```iec-st
PROGRAM SampleProgram
  VAR_EXTERNAL
    axisTelegram111: UINT;
  END_VAR

  VAR
    _enableAxis: BOOL;
    _acknowledgeError: BOOL;
    _moveDistance: BOOL;
    _sinaPos: SinaPos;
  END_VAR

  // check axis is enabled and select speed and direction
  IF _sinaPos.axisEnabled THEN
    // select mode when axis is in standstill
    IF _sinaPos.axisPositionOk THEN
      _sinaPos.mode := SinaPosMode#RelativePositioning;

      // check mode is active and execute traversing job
      IF _sinaPos.actualMode = SinaPosMode#RelativePositioning THEN
        _sinaPos.position := 10000;
        _sinaPos.velocity := 500;
        _sinaPos.executeMode := TRUE;
      END_IF;
    ELSE
      // reset job parameters whenever axis is moving
      _sinaPos.position := 0;
      _sinaPos.velocity := 0;
      _sinaPos.executeMode := FALSE;
    END_IF;
  END_IF;

  // call sina pos cyclically
  _sinaPos(enableAxis := _enableAxis, acknowledgeError := _acknowledgeError, hwidSTW := axisTelegram111, hwidZSW := axisTelegram111);
END_PROGRAM
```
