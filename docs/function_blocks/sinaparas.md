# SinaParaS

The function block `SinaParaS` is used to acyclically read or write a single parameter from/to a SINAMICS S/G drive. Connect the hardware identifier of the drive object to the input `hardwareId`.

## Parameters

| Name                  | Section | Type                   | Description                                                                            |
| --------------------- | ------- | ---------------------- | -------------------------------------------------------------------------------------- |
| start                 | Input   | `BOOL`                 | Start of the job (0 = no job or cancel the actual job; 1 = start and perform the job) |
| readWrite             | Input   | `SinaParaMode`         | Type of job: SinaParaMode#Read (FALSE), SinaParaMode#Write (TRUE)                      |
| parameter             | Input   | `UINT`                 | Parameter number (1..65535)                                                           |
| index                 | Input   | `UINT`                 | Subindex (0..65535)                                                                   |
| valueWrite1           | Input   | `REAL`                 | Value of the write parameter (BYTE, WORD, REAL)                                       |
| valueWrite2           | Input   | `DINT`                 | Value of the write parameter (DINT)                                                   |
| valueWrite3           | Input   | `LREAL`                | Value of the write parameter (LREAL)                                                  |
| axisNo                | Input   | `BYTE`                 | Axis Number / Object ID of the drive                                                   |
| hardwareId            | Input   | `UINT`                 | Hardware ID for WriteRecord and ReadRecord                                             |
| ready                 | Output  | `BOOL`                 | Read or write parameter order ready                                                    |
| busy                  | Output  | `BOOL`                 | Job in progress                                                                        |
| done                  | Output  | `BOOL`                 | Operation finished successfully                                                        |
| valueRead1            | Output  | `REAL`                 | Value of the read parameter (BYTE, WORD, REAL)                                        |
| valueRead2            | Output  | `DINT`                 | Value of the read parameter (DINT)                                                    |
| valueRead3            | Output  | `LREAL`                | Value of the read parameter (LREAL)                                                   |
| format                | Output  | `BYTE`                 | Format of the value (Format 0x40..0x44)                                              |
| errorNo               | Output  | `WORD`                 | Error number                                                                          |
| error                 | Output  | `BOOL`                 | Error during operation                                                                 |
| errorId               | Output  | `DWORD`                | Error information                                                                      |
| diagId                | Output  | `WORD`                 | Additional communication information                                                    |

## Writing a parameter

The write action validates the parameter value at the inputs and reads the parameter format from the drive. Aftewards, the value is transferred to the SINAMICS drive.<br><br>
During this operation, the `busy` output is set to `TRUE`. <br><br>
If the parameter to be written is erroneous, the assiociated parameter error number is also read and entered into the parameter structure. Additionally, the corresponding error bit in the first `WORD` of the `errorId(DWORD)` is set. <br><br>
A successful write operation ends with a falling edge of the `busy` output and a rising edge of the `done` output.

## Reading a parameter

The read action reads out the value specified by the `parameter` and `index` inputs and displays the associated value at the output `valueRead1`. `valueRead2` or `valueRead3` (depending on the data type).<br><br>
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
        _readValue : REAL;
        _sinaParaS: SinaParaS;
    END_VAR

    IF _execute AND NOT _executeOld THEN
        // setup parameters for read access
        IF NOT _sinaParaS.busy THEN
            _sinaParaS.axisNo := BYTE#16#1; // drive object number from hardware configuration
            _sinaParaS.readWrite := SinaParaMode#Read; // setup read mode
            _sinaParaS.parameter := UINT#210;
            _sinaParaS.index := UINT#0;

            _sinaParaS.start := TRUE;
        END_IF;
    END_IF;

    _executeOld := _execute;

    IF _sinaParaS.done AND NOT _sinaParaS.error THEN
        _readValue := _sinaParaS.valueRead1;
        _sinaParaS.start := FALSE;
    END_IF;
    // call SinaPara cyclically
    _sinaParaS(hardwareID := HW_ID_DRIVE);
END_PROGRAM
```
