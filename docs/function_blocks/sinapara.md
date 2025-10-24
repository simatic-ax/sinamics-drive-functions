# SinaPara

The function block `SinaPara` is used to acyclically read or write up to 16 parameters from/to a SINAMICS S/G drive. Connect the hardware identifier of the drive object to the input `hardwareId`.

## Parameters

| Name                  | Section | Type                   | Description                                                                            |
| --------------------- | ------- | ---------------------- | -------------------------------------------------------------------------------------- |
| start                 | Input   | `BOOL`                 | Start of the job (0 = no job or cancel the actual job; 1 = start and perform the job)  |
| readWrite             | Input   | `SinaParaMode`         | Type of job: SinaParaMode#Read (FALSE), SinaParaMode#Write (TRUE)                      |
| paraNo                | Input   | `INT`                  | Number of parameters to read or write (1..16)                                          |
| axisNo                | Input   | `BYTE`                 | Axis Number / Object ID of the drive                                                   |
| hardwareId            | Input   | `UINT`                 | Hardware ID for WriteRecord and ReadRecord                                             |
| ready                 | Output  | `BOOL`                 | Read or write parameter order ready                                                    |
| busy                  | Output  | `BOOL`                 | Job in progress                                                                        |
| done                  | Output  | `BOOL`                 | Operation finished successfully                                                        |
| error                 | Output  | `BOOL`                 | Error during operation                                                                 |
| errorId               | Output  | `DWORD`                | Error information                                                                      |
| diagId                | Output  | `WORD`                 | Additional communication information                                                   |
| parameter             | In/Out  | `ARRAY[1..16] OF SinaParameter` | List of parameters (max. 16 parameter)                                        |

## Data structure of the `parameter` field

The InOut variable `parameter` acts as a container for the individual parameters that are read or written with the `SinaPara` function block.
The following values have to be supplied always by the user:

- `parameter[i].siParaNo`: parameter number
- `parameter[i].siIndex`: parameter index

The following values havt to be supplied for writing operation:

- `parameter[i].srValue`: parameter value for `REAL` type fields
- `parameter[i].sdValue`: parameter value for `DWORD`/`DINT` type fields

## Writing parameters

The write action results in the parameter value and the format of the set parameter first being read from the SINAMICS drive and written into the parameter structure. After successfully reading, the parameter value specified by the user is sent to the SINAMICS drive.<br><br>
During this operation, the `busy` output is set to `TRUE`. <br><br>
If the parameter to be written is erroneous, the assiociated parameter error number is also read and entered into the parameter structure. Additionally, the corresponding error bit in the first `WORD` of the `errorId(DWORD)` is set. <br><br>
A successful write operation ends with a falling edge of the `busy` output and a rising edge of the `done` output.

## Reading parameters

The read action results in the parameter value and the format of the set parameter first being read from the SINAMICS drive and written into the parameter structure. Then, the value to be read is saved in the structure.<br><br>
During this operation, the `busy` output is set to `TRUE`. <br><br>
If the parameter to be written is erroneous, the assiociated parameter error number is also read and entered into the parameter structure. Additionally, the corresponding error bit in the first `WORD` of the `errorId(DWORD)` is set. <br><br>
A successful write operation ends with a falling edge of the `busy` output and a rising edge of the `done` output.

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
