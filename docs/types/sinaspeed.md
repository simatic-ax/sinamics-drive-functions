# Types

## SinaSpeedConfiguration

| Name                          | Type   | Description                                                                                                  |
| ----------------------------- | ------ | ------------------------------------------------------------------------------------------------------------ |
| OFF2                          | `BOOL` | TRUE = enable is possible, FALSE = immediate pulse suppression and switching on inhibited                    |
| OFF3                          | `BOOL` | TRUE = enable is possible, FALSE = braking with OFF3 ramp, then pulse suppression and switching on inhibited |
| EnableOperation               | `BOOL` | TRUE = enable operation, FALSE = inhibit operation                                                           |
| EnableRampFunctionGenerator   | `BOOL` | TRUE = enable ramp-function generator, FALSE = inhibit ramp-function generator                               |
| ContinueRampFunctionGenerator | `BOOL` | TRUE = continue ramp-function generator, FALSE = stop ramp-function generator                                |
| EnableSetpoint                | `BOOL` | TRUE = enable setpoint, FALSE = inhibit setpoint                                                             |
| SetpointInversion             | `BOOL` | TRUE = setpoints inverted, FALSE = setpoints alike                                                           |
| OpenHoldingBrake              | `BOOL` | TRUE = opens the holding brake, FALSE = closes the holding brake                                             |
| RaiseMotorizedPotentiometer   | `BOOL` | TRUE = raises motorized potentiometer                                                                        |
| LowerMotorizedPotentiometer   | `BOOL` | TRUE = loweres motorized potentiometer                                                                       |

## SinaSpeedStatus

| Name               | Value          | Description                        | Remedy                                                                                                                                                     |
| ------------------ | -------------- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NO_FAULT           | `WORD#16#7002` | No fault active                    |                                                                                                                                                            |
| DRIVE_FAULT_ACTIVE | `WORD#16#8401` | Drive fault active                 | Evaluate active errors of the SINAMICS drive via acyclic communication                                                                                     |
| ON_INHIBIT_ACTIVE  | `WORD#16#8402` | Switching on of drive is inhibited | Check for parking axis, safety function triggered, check input enableAxis or OFF2, OFF3 and EnableOperation from SinaSpeedConfiguration                    |
| READ_ERROR         | `WORD#16#8600` | Error reading data from drive      | Check the diagnosticsId output and clear communication fault                                                                                               |
| WRITE_ERROR        | `WORD#16#8601` | Error reading data from drive      | Check the diagnosticsId output and clear communication fault                                                                                               |

## SinaSpeedZSW1

| Name                       | Type   | Description                                                     |
| -------------------------- | ------ | --------------------------------------------------------------- |
| ReadyForSwitchOn           | `BOOL` | TRUE = ready for switching on                                   |
| ReadyForOperation          | `BOOL` | TRUE = ready for operation                                      |
| OperationEnabled           | `BOOL` | TRUE = operation enabled                                        |
| FaultPresent               | `BOOL` | TRUE = fault present                                            |
| NoCoastDownActive          | `BOOL` | TRUE = no coast down active (OFF2 inactive)                     |
| NoQuickStopActive          | `BOOL` | TRUE = no quick stop active (OFF3 inactive)                     |
| SwitchOnInhibitedActive    | `BOOL` | TRUE = switch on inhibited active                               |
| AlarmPresent               | `BOOL` | TRUE = alarm present                                            |
| ActualSpeedDeviation       | `BOOL` | TRUE = actual speed value differs to setpoint                   |
| ControlRequested           | `BOOL` | TRUE = PLC control requested                                    |
| SpeedSetpointReached       | `BOOL` | TRUE = speed setpoint is reached                                |
| DriveOperatingWithinLimits | `BOOL` | TRUE = power/torque/current limit not reached                   |
| HoldingBrakeOpen           | `BOOL` | TRUE = holding brake is open                                    |
| NoMotorOvertemperature     | `BOOL` | TRUE = no motor overtemperature alarm                           |
| CurrentMotorDirection      | `BOOL` | TRUE =  Motor rotates forwards, FALSE = Motor rotates backwards |
| NoCriticalAlarms           | `BOOL` | TRUE = No alarm, thermal overload, power unit                   |
