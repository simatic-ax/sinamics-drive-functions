# SinaPara

## Principle of Operation

The function block `SinaPara` is used to acyclically read or write up to 16 parameters from/to a SINAMICS S/G drive. Connect the hardware identifier of the drive object to the input `hardwareId`.

### Writing parameters

The write action results in the parameter value and the format of the set parameter first being read from the SINAMICS drive and written into the parameter structure. After successfully reading, the parameter value specified by the user is sent to the SINAMICS drive.
During this operation, the `busy` output is set to `TRUE`.
If the parameter to be written is erroneous, the assiociated parameter error number is also read and entered into the parameter structure. Additionally, the corresponding error bit in the first `WORD` of the `errorId(DWORD)` is set.
A successful write operation ends with a falling edge of the `busy` output and a rising edge of the `done` output.

### Reading parameters

The read action results in the parameter value and the format of the set parameter first being read from the SINAMICS drive and written into the parameter structure. Then, the value to be read is saved in the structure.
During this operation, the `busy` output is set to `TRUE`.
If the parameter to be written is erroneous, the assiociated parameter error number is also read and entered into the parameter structure. Additionally, the corresponding error bit in the first `WORD` of the `errorId(DWORD)` is set.
A successful write operation ends with a falling edge of the `busy` output and a rising edge of the `done` output.

## Interface

### Input Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| start | `BOOL` | Start of the job (0 = no job or cancel the actual job; 1 = start and perform the job) |
| readWrite | [`SinaParaMode`](../enums/SinaParaMode.md) | Type of job: SinaParaMode#Read (FALSE), SinaParaMode#Write (TRUE) |
| paraNo | `INT` | Number of parameters to read or write (1..16) |
| axisNo | `BYTE` | Axis Number / Object ID of the drive |
| hardwareId | `UINT` | Hardware ID for WriteRecord and ReadRecord |

### Output Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| ready | `BOOL` | Read or write parameter order ready |
| busy | `BOOL` | Job in progress |
| done | `BOOL` | Operation finished successfully |
| error | `BOOL` | Error during operation |
| errorId | `DWORD` | Error information |
| diagId | `WORD` | Additional communication information |
| parameter | ARRAY[1..16] OF [SinaParameter](../types/SinaParameter.md#structure) | List of parameters (max. 16 parameter) |

## Example

```iec-st
USING Simatic.Ax.Sinamics;
PROGRAM SampleProgram
    VAR_EXTERNAL
      HW_ID_DRIVE: UINT;
    END_VAR

    VAR_INPUT
      _execute: BOOL;
    END_VAR

    VAR
      _executeOld : BOOL;
      _readParameters : ARRAY[1..16] OF SinaParameter;
      _sinaPara: SinaPara;
    END_VAR

    IF _execute AND NOT _executeOld THEN
        // setup parameters for read access
        IF NOT _sinaPara.busy THEN
            _sinaPara.axisNo := BYTE#16#3; // drive object number from hardware configuration
            _sinaPara.paraNo := 8; // read out 8 parameters at once
            _sinaPara.readWrite := SinaParaMode#Read; // setup read mode

            _readParameters[1].siParaNo := UINT#210;
            _readParameters[1].siIndex := UINT#0;
            _readParameters[2].siParaNo := UINT#304;
            _readParameters[2].siIndex := UINT#0;
            _readParameters[3].siParaNo := UINT#305;
            _readParameters[3].siIndex := UINT#0;
            _readParameters[4].siParaNo := UINT#307;
            _readParameters[4].siIndex := UINT#0;
            _readParameters[5].siParaNo := UINT#311;
            _readParameters[5].siIndex := UINT#0;
            _readParameters[6].siParaNo := UINT#312;
            _readParameters[6].siIndex := UINT#0;
            _readParameters[7].siParaNo := UINT#338;
            _readParameters[7].siIndex := UINT#0;
            _readParameters[8].siParaNo := UINT#341;
            _readParameters[8].siIndex := UINT#0;

            _sinaPara.start := TRUE;
        END_IF;
    END_IF;
    _executeOld := _execute;

    IF _sinaPara.done AND NOT _sinaPara.error THEN
      _sinaPara.start := FALSE;
    END_IF;

    // call SinaPara cyclically
    _sinaPara(hardwareID := HW_ID_DRIVE, parameter := _readParameters);
END_PROGRAM
```
