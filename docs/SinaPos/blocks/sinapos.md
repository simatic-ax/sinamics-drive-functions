# SinaPos

## Principle of Operation

You use the function block `SinaPos` to control the axis via basic positioner technology function (EPOS) of SINAMICS drive systems. This requires that the basic positioner technology has been properly configured and telegram 110/111 has been selected in the drive. Connect the according hardware identifier of the drive telegram to the inputs `hwidSTW` and `hwidZSW`. With the `mode` input you can change beetween the operation modes such as positioning, setup mode, homing and jogging of the axis.

## Interface

### Input Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| mode | [`SinaPosMode`](../enums/SinaPosMode.md) | Operating mode selection for MDI |
| enableAxis | `BOOL` | TRUE = enable axis, FALSE = switch off drive (OFF1) |
| acknowledgeError | `BOOL` | Acknowledge drive errors with rising edge |
| cancelTraversing | `BOOL` | FALSE = Rejects the active traversing job |
| intermediateStop | `BOOL` | FALSE = Interrupts the active traversing job |
| positive | `BOOL` | Positive traversing direction |
| negative | `BOOL` | Negative traversing direction |
| jog1 | `BOOL` | Jog signal source 1 |
| jog2 | `BOOL` | Jog signal source 2 |
| flyingReferencing | `BOOL` | Controls selection for flying referencing |
| executeMode | `BOOL` | Activate traversing job/setpoint or activates homing or takes over home position |
| position | `DINT` | Position setpoint in LU for direct traversing operating modes or traversing block number in traversing block operation mode |
| velocity | `DINT` | Velocity in 1000 LU/min for MDI operating mode |
| overrideVelocity | `INT` | Velocity override in %, effective from 0% to 199% |
| overrideAcceleration | `INT` | Acceleration override in %, effective from 0% to 100% |
| overrideDeceleration | `INT` | Deceleration override in %, effective from 0% to 100% |
| config | [`SinaPosConfiguration`](../types/SinaPosConfiguration.md#structure) | Configuration for axis |
| hwidSTW | `UINT` | Hardware ID of the SIMATIC S7-1200/1500 setpoint slot |
| hwidZSW | `UINT` | Hardware ID of the SIMATIC S7-1200/1500 actual value slot |

### Output Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| axisEnabled | `BOOL` | TRUE = axis is enabled |
| axisPositionOk | `BOOL` | TRUE = target position of the axis reached |
| axisSetpointFixed | `BOOL` | TRUE = setpoint is stationary (SINAMICS S/G120 FW < 4.8 / 4.7.9: r2199.0, SINAMICS S/G120 FW >= 4.8 / >= 4.7.9: r2683.2) |
| axisReferenced | `BOOL` | TRUE = home position is set |
| axisWarning | `BOOL` | TRUE = alarm of the drive pending |
| axisError | `BOOL` | TRUE = drive fault |
| lockout | `BOOL` | TRUE = switch-on inhibited |
| actualVelocity | `DINT` | Actual veloctiy (standardized 40000000h = 100% p2000) |
| actualPosition | `DINT` | Actual position in LU |
| actualTraversingBlock | `UINT` | Actual traversing block (0-63) |
| actualMode | `SinaPosMode` | Actual operating mode |
| posZSW1 | [`SinaPosZSW1`](../types/SinaPosZSW1.md#structure) | Status of POS_ZSW1 |
| posZSW2 | [`SinaPosZSW2`](../types/SinaPosZSW2.md#structure) | Status of POS_ZSW2 |
| actualWarning | `WORD` | Current alarm number |
| actualFault | `WORD` | Current fault number |
| error | `BOOL` | TRUE = fault in function block present |
| status | [`SinaPosStatus`](../enums/SinaPosStatus.md) | Status of the function block |
| diagnosticsId | `WORD` | Return values of communication errors (ReadData and WriteData) |

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
