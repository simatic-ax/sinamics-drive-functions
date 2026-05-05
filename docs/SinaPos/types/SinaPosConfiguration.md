
# SinaPosConfiguration

## Description

Defines a configuration structure that contains the bits from STW1 and STW2 of telegram 110/111.

## Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| OFF2 | `BOOL` | TRUE = enable is possible, FALSE = immediate pulse suppression and switching on inhibited |
| OFF3 | `BOOL` | TRUE = enable is possible, FALSE = braking with OFF3 ramp, then pulse suppression and switching on inhibited |
| ExternalBlockChange | `BOOL` | TRUE = external block change active |
| STW1_14 | `BOOL` | Reserved bit in STW1 |
| STW1_15 | `BOOL` | Reserved bit in STW1 |
| DDS0 | `BOOL` | Drive Data Set bit 0 |
| DDS1 | `BOOL` | Drive Data Set bit 1 |
| DDS2 | `BOOL` | Drive Data Set bit 2 |
| DDS3 | `BOOL` | Drive Data Set bit 3 |
| DDS4 | `BOOL` | Drive Data Set bit 4 |
| STW2_5 | `BOOL` | Reserved bit in STW2 |
| STW2_6 | `BOOL` | Reserved bit in STW2 |
| ParkingAxis | `BOOL` | Parking axis |
| TraverseToFixedEndstop | `BOOL` | Traverse to fixed end stop |
| STW2_9 | `BOOL` | Reserved bit in STW2 |
| POS_STW1_6 | `BOOL` | Reserved bit in POS_STW1 |
| POS_STW1_7 | `BOOL` | Reserved bit in POS_STW1 |
| POS_STW1_11 | `BOOL` | Reserved bit in POS_STW1 |
| ContinousTransfer | `BOOL` | TRUE = enable continous transfer of MDI block change, FALSE = Activates MDI block change via rising edge |
| POS_STW1_13 | `BOOL` | Reserved bit POS_STW1 |
| ReferenceCamActive | `BOOL` | TRUE = enable reference cam, FALSE = disable reference cam |
| POS_STW2_3 | `BOOL` | Reserved bit in POS_STW2 |
| POS_STW2_4 | `BOOL` | Reserved bit in POS_STW2 |
| POS_STW2_6 | `BOOL` | Reserved bit in POS_STW2 |
| POS_STW2_7 | `BOOL` | Reserved bit in POS_STW2 |
| MeasuringProbeSelection | `BOOL` | TRUE = Select measuring probe 2, FALSE = Select measuring probe 1 |
| MeasuringProbeEdgeDetection | `BOOL` | TRUE = falling edge of measuring probe, FALSE = Rising edge of measuring probe |
| POS_STW2_12 | `BOOL` | Reserved bit in POS_STW2 |
| POS_STW2_13 | `BOOL` | Reserved bit in POS_STW2 |
| SoftwareLimitSwitch | `BOOL` | TRUE = enable software limit switch, FALSE = disable software limit switch |
| StopCam | `BOOL` | TRUE = enable stop cam, FALSE = disable stop cam |

## Usage

The type is used as a configuration structure for the function block [SinaPos](../blocks/sinapos.md).
