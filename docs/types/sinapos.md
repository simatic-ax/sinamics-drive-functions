# Types

## SinaPosConfiguration

| Name                          | Type   | Description                                                                                                  |
| ----------------------------- | ------ | ------------------------------------------------------------------------------------------------------------ |
| OFF2                          | `BOOL` | TRUE = enable is possible, FALSE = immediate pulse suppression and switching on inhibited                    |
| OFF3                          | `BOOL` | TRUE = enable is possible, FALSE = braking with OFF3 ramp, then pulse suppression and switching on inhibited |
| ExternalBlockChange           | `BOOL` | TRUE = external block change active                                                                          |
| STW1_14                       | `BOOL` | Reserved bit in STW1                                                                                         |
| STW1_15                       | `BOOL` | Reserved bit in STW1                                                                                         |
| DDS0                          | `BOOL` | Drive Data Set bit 0                                                                                         |
| DDS1                          | `BOOL` | Drive Data Set bit 1                                                                                         |
| DDS2                          | `BOOL` | Drive Data Set bit 2                                                                                         |
| DDS3                          | `BOOL` | Drive Data Set bit 3                                                                                         |
| DDS4                          | `BOOL` | Drive Data Set bit 4                                                                                         |
| STW2_5                        | `BOOL` | Reserved bit in STW2                                                                                         |
| STW2_6                        | `BOOL` | Reserved bit in STW2                                                                                         |
| ParkingAxis                   | `BOOL` | Parking axis                                                                                                 |
| TraverseToFixedEndstop        | `BOOL` | Traverse to fixed end stop                                                                                   |
| STW2_9                        | `BOOL` | Reserved bit in STW2                                                                                         |
| POS_STW1_6                    | `BOOL` | Reserved bit in POS_STW1                                                                                     |
| POS_STW1_7                    | `BOOL` | Reserved bit in POS_STW1                                                                                     |
| POS_STW1_11                   | `BOOL` | Reserved bit in POS_STW1                                                                                     |
| ContinousTransfer             | `BOOL` | TRUE = enable continous transfer of MDI block change, FALSE = Activates MDI block change via rising edge     |
| POS_STW1_13                   | `BOOL` | Reserved bit POS_STW1                                                                                        |
| ReferenceCamActive            | `BOOL` | TRUE = enable reference cam, FALSE = disable reference cam                                                   |
| POS_STW2_3                    | `BOOL` | Reserved bit in POS_STW2                                                                                     |
| POS_STW2_4                    | `BOOL` | Reserved bit in POS_STW2                                                                                     |
| POS_STW2_6                    | `BOOL` | Reserved bit in POS_STW2                                                                                     |
| POS_STW2_7                    | `BOOL` | Reserved bit in POS_STW2                                                                                     |
| MeasuringProbeSelection       | `BOOL` | TRUE = Select measuring probe 2, FALSE = Select measuring probe 1                                            |
| MeasuringProbeEdgeDetection   | `BOOL` | TRUE = falling edge of measuring probe, FALSE = Rising edge of measuring probe                               |
| POS_STW2_12                   | `BOOL` | Reserved bit in POS_STW2                                                                                     |
| POS_STW2_13                   | `BOOL` | Reserved bit in POS_STW2                                                                                     |
| SoftwareLimitSwitch           | `BOOL` | TRUE = enable software limit switch, FALSE = disable software limit switch                                   |
| StopCam                       | `BOOL` | TRUE = enable stop cam, FALSE = disable stop cam                                                             |

## SinaPosMode

| Name                | Description                                                                                                                                                                                                                         |
| --------------------| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RelativePositioning | This mode is implemented via the drive function "MDI relative positioning". It permits executing relative traversing paths via the integrated position controller of the SINAMICS drive                                             |
| AbsolutePositioning | This mode is implemented via the drive function "MDI absolute positioing". It permits executing absolute travsing paths via the integrated position controller of the SINAMICS drive                                                |
| PositioningAsSetup  | This mode is implemented via the drive function "MDI setup". It permits the position-controlled traversing of the axis in a positive or negative travel direction at constant speed without the specification of a target position. |
| Homing              | This mode is implemented via the drive function "Active homing". It permits a homing procedure of the axis in a positive or negative direction with pre-configured velocity.                                                        |
| SetHomePosition     | This mode is implemented via the drive function "Set reference point". It permits setting directly the home position of the axis.                                                                                                   |
| TraversingBlocks    | This mode is implemented via the drive function "Traversing blocks". It permits the creation of automation programs, trave to fixed stop and setting/resetting of outputs.                                                          |
| Jog                 | This mode is implemented via the drive function "Jog". It permits the position-controlled jogging of the axis in positive/negative direction.                                                                                       |
| JogIncremental      | This mode is implemented via the drive function "Jog". It permits the position-controlled incremental jogging of the axis in positive/negative direction.                                                                           |

## SinaPosStatus

| Name                              | Value          | Description                                    | Remedy                                                                                                                           |
| --------------------------------- | -------------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| NO_FAULT                          | `WORD#16#7002` | No fault active                                |                                                                                                                                  |
| INCORRECT_SETPOINTS               | `WORD#16#8203` | Incorrect paramaeterization of override inputs | Check the settings of the override inputs in the accordance of their limits                                                      |
| INCORRECT_TRAVERSING_BLOCK_NUMBER | `WORD#16#8204` | Invalid traversing block number                | Enter a traversing block number from 0 63                                                                                        |
| DRIVE_FAULT_ACTIVE                | `WORD#16#8401` | Drive fault active                             | Evaluate active errors of the SINAMICS drive via output "actualFault                                                             |
| ON_INHIBIT_ACTIVE                 | `WORD#16#8402` | Switching on of drive is inhibited             | Check for parking axis, safety function triggered, check input enableAxis or OFF2, OFF3 or ParkingAxis from SinaPosConfiguration |
| FLYING_REFERENCING_NOT_POSSIBLE   | `WORD#16#8403` | Flying homing could not be started             | Check for active alarms in the drive                                                                                             |
| READ_ERROR                        | `WORD#16#8600` | Error reading data from drive                  | Check the diagnosticsId output and clear communication fault                                                                     |
| WRITE_ERROR                       | `WORD#16#8601` | Error writing data to drive                    | Check the diagnosticsId output and clear communication fault                                                                     |

## SinaPosZSW1

| Name                         | Type   | Description                            |
| ---------------------------- | ------ | -------------------------------------- |
| ActiveTraversingBlockBit0    | `BOOL` | TRUE = traversing block bit 0 active   |
| ActiveTraversingBlockBit1    | `BOOL` | TRUE = traversing block bit 1 active   |
| ActiveTraversingBlockBit2    | `BOOL` | TRUE = traversing block bit 2 active   |
| ActiveTraversingBlockBit3    | `BOOL` | TRUE = traversing block bit 3 active   |
| ActiveTraversingBlockBit4    | `BOOL` | TRUE = traversing block bit 4 active   |
| ActiveTraversingBlockBit5    | `BOOL` | TRUE = traversing block bit 5 active   |
| StopCamMinusActive           | `BOOL` | TRUE = STOP cam minus active           |
| StopCamPlusActive            | `BOOL` | TRUE = STOP cam plus active            |
| JoggingActive                | `BOOL` | TRUE = jogging active                  |
| ReferencePointApproachActive | `BOOL` | TRUE = reference point approach active |
| FlyingReferencingActive      | `BOOL` | TRUE = flying referencing active       |
| TraversingBlockActive        | `BOOL` | TRUE = traversing block active         |
| SetUpActive                  | `BOOL` | TRUE = set-up mode active              |
| MDI                          | `BOOL` | TRUE = MDI active                      |

## SinaPosZSW2

| Name                                 | Type   | Description                                        |
| ------------------------------------ | ------ | -------------------------------------------------- |
| TrackingModeActive                   | `BOOL` | TRUE = tracking mode active                        |
| VelocityLimitingActive               | `BOOL` | TRUE = velocity limiting active                    |
| SetpointAvailable                    | `BOOL` | TRUE = setpoint available                          |
| PrintingMarkOutsideOuterWindow       | `BOOL` | TRUE = printing mark outside outer window          |
| AxisMovesForward                     | `BOOL` | TRUE = axis moves forward                          |
| AxisMovesBackward                    | `BOOL` | TRUE = axis moves backward                         |
| SoftwareLimitSwitchMinusReached      | `BOOL` | TRUE = software limit switch minus reached         |
| SoftwareLimitSwitchPlusReached       | `BOOL` | TRUE = software limit switch plus reached          |
| PositionBehindCam1SwitichingPosition | `BOOL` | TRUE = actual position <= cam switching position 1 |
| PositionBehindCam2SwitchingPosition  | `BOOL` | TRUE = actual position <= cam switching position 2 |
| DirectOutput1ViaTraversingBlock      | `BOOL` | TRUE = direct output 1 via traversing block        |
| DirectOutput2ViaTraversingBlock      | `BOOL` | TRUE = direct output 2 via traversing block        |
| FixedStopReached                     | `BOOL` | TRUE = fixed stop reached                          |
| ClampingTorqueReached                | `BOOL` | TRUE = fixed stop clamping torque reached          |
| TravelToFixedStopActive              | `BOOL` | TRUE = travel to fixed stop active                 |
| TraversingCommandActive              | `BOOL` | TRUE = traversing command active                   |
