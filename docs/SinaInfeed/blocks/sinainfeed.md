# SinaInfeed

## Principle of Operation

You can use the function block `SinaInfeed` to control the infeed of a SINAMICS S120 drive system via telegram 370. Connect the according hardware identifier of the drive telegram to the inputs `hwidSTW` and `hwidZSW`.

### Enabling infeed

The infeed can be enabled with the input `enableInfeed`. The precharching of the infeed can be selected with the input `enablePrecharging`.

### Acknowledge faults

With a rising edge at the input`ackFault` the user can acknolwedge pending alarms/faults of the infeed.

## Interface

### Input Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| enablePrecharging | `BOOL` | 0->1 = Setup the DC Link (BLM/SLM/ALM) (OFF1 = 0->1) |
| enableInfeed | `BOOL` | 1 = Enable line modules  (only SLM / ALM) |
| ackFault | `BOOL` | 1 = Acknowledge infeed error |
| config | [`SinaInfeedConfiguration`](../types/SinaInfeedConfiguration.md#structure) | Configuration of axis |
| hwidSTW | `UINT` | Hardware ID of the SIMATIC S7-1200/1500 setpoint slot |
| hwidZSW | `UINT` | Hardware ID of the SIMATIC S7-1200/1500 actual value slot |

### Output Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| ready | `BOOL` | 1 = Infeed ready to power up |
| operation | `BOOL` | 1 = Infeed ready for operation |
| run | `BOOL` | 1 = Infeed in operation (only SLM / ALM) |
| fault | `BOOL` | 1 = Infeed error active |
| lockout | `BOOL` | 1 = Infeed lockout active |
| warning | `BOOL` | 1 = Infeed warning active |
| ZSW1 | [`SinaInfeedZSW1`](../types/SinaInfeedZSW1.md#structure) | Infeed Status |
| error | `BOOL` | Rising edge informs that an error occurred during the execution of the function block |
| status | `WORD` | Status output (7002 = FB in operation; 8xxx = error description  - read the manual) |
| diagID | `WORD` | Error codes of the cyclic system funtion blocks ReadData / WriteData |

## Example

```iec-st
PROGRAM SampleProgram
    VAR_EXTERNAL
      infeedTelegram370: UINT;
    END_VAR

    VAR
        _sinaInfeed : SinaInfeed;
    END_VAR

    // enable pre-charging
    _sinaInfeed.enablePrecharging := TRUE;

    // acknowledge errors of infeed
    IF _sinaInfeed.fault OR _sinaInfeed.warning THEN
        _sinaInfeed.ackFault := TRUE;
    END_IF;

    // enable infeed when its ready
    IF _sinaInfeed.ready AND NOT _sinaInfeed.lockout THEN
        _sinaInfeed.enableInfeed := TRUE;
    END_IF;

    // call sina infeed cyclically
    _sinaInfeed(hwidSTW := infeedTelegram370, hwidZSW := infeedTelegram370);
END_PROGRAM
```
