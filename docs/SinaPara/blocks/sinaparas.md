# SinaParaS

## Principle of Operation

The function block `SinaParaS` is used to acyclically read or write a single parameter from/to a SINAMICS S/G drive. Connect the hardware identifier of the drive object to the input `hardwareId`.

### Writing a parameter

The write action validates the parameter value at the inputs and reads the parameter format from the drive. Aftewards, the value is transferred to the SINAMICS drive.
During this operation, the `busy` output is set to `TRUE`.
If the parameter to be written is erroneous, the assiociated parameter error number is also read and entered into the parameter structure. Additionally, the corresponding error bit in the first `WORD` of the `errorId(DWORD)` is set.
A successful write operation ends with a falling edge of the `busy` output and a rising edge of the `done` output.

### Reading a parameter

The read action reads out the value specified by the `parameter` and `index` inputs and displays the associated value at the output `valueRead1`. `valueRead2` or `valueRead3` (depending on the data type).
During this operation, the `busy` output is set to `TRUE`.
If the parameter to be written is erroneous, the assiociated parameter error number is also read and entered into the parameter structure. Additionally, the corresponding error bit in the first `WORD` of the `errorId(DWORD)` is set.
A successful write operation ends with a falling edge of the `busy` output and a rising edge of the `done` output.

## Interface

### Input Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| start | `BOOL` | Start of the job (0 = no job or cancel the actual job; 1 = start and perform the job) |
| readWrite | [`SinaParaMode`](../enums/SinaParaMode.md) | Type of job: SinaParaMode#Read (FALSE), SinaParaMode#Write (TRUE) |
| parameter | `UINT` | Parameter number (1..65535) |
| index | `UINT` | Subindex (0..65535) |
| valueWrite1 | `REAL` | Value of the write parameter (BYTE, WORD, REAL) |
| valueWrite2 | `DINT` | Value of the write parameter (DINT) |
| valueWrite3 | `LREAL` | Value of the write parameter (LREAL) |
| axisNo | `BYTE` | Axis Number / Object ID of the drive |
| hardwareId | `UINT` | Hardware ID for WriteRecord and ReadRecord |

## Output Parameters

| Parameter | Type | Description |
| --------- | ---- | ----------- |
| ready | `BOOL` | Read or write parameter order ready |
| busy | `BOOL` | Job in progress |
| done | `BOOL` | Operation finished successfully |
| valueRead1 | `REAL` | Value of the read parameter (BYTE, WORD, REAL) |
| valueRead2 | `DINT` | Value of the read parameter (DINT) |
| valueRead3 | `LREAL` | Value of the read parameter (LREAL) |
| format | `BYTE` | Format of the value (Format 0x40..0x44) |
| errorNo | `WORD` | Error number |
| error | `BOOL` | Error during operation |
| errorId | `DWORD` | Error information |
| diagId | `WORD` | Additional communication information |

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
