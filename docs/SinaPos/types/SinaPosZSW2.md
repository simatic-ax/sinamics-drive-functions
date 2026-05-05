# SinaPosZSW2

## Description

Defines a structure that contains the bits from ZSW2 of telegram 110/111.

## Structure

| Field | Type | Description |
| ----- | ------ | --------- |
| TrackingModeActive | `BOOL` | TRUE = tracking mode active |
| VelocityLimitingActive | `BOOL` | TRUE = velocity limiting active |
| SetpointAvailable | `BOOL` | TRUE = setpoint available |
| PrintingMarkOutsideOuterWindow | `BOOL` | TRUE = printing mark outside outer window |
| AxisMovesForward | `BOOL` | TRUE = axis moves forward |
| AxisMovesBackward | `BOOL` | TRUE = axis moves backward |
| SoftwareLimitSwitchMinusReached | `BOOL` | TRUE = software limit switch minus reached |
| SoftwareLimitSwitchPlusReached | `BOOL` | TRUE = software limit switch plus reached |
| PositionBehindCam1SwitichingPosition | `BOOL` | TRUE = actual position <= cam switching position 1 |
| PositionBehindCam2SwitchingPosition | `BOOL` | TRUE = actual position <= cam switching position 2 |
| DirectOutput1ViaTraversingBlock | `BOOL` | TRUE = direct output 1 via traversing block |
| DirectOutput2ViaTraversingBlock | `BOOL` | TRUE = direct output 2 via traversing block |
| FixedStopReached | `BOOL` | TRUE = fixed stop reached |
| ClampingTorqueReached | `BOOL` | TRUE = fixed stop clamping torque reached |
| TravelToFixedStopActive | `BOOL` | TRUE = travel to fixed stop active |
| TraversingCommandActive | `BOOL` | TRUE = traversing command active |

## Usage

The type is used as a diagnostics structure for the function block [SinaPos](../blocks/sinapos.md).
