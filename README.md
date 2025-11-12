# SINAMICS Drive Functions

This package contains functionality for handling drive functions such as moving an axis with a given speed or to a target position (EPOS). Furthermore, function blocks for reading or writing parameters from/to a SINAMICS device are implemented.

## Getting started

Install with Apax:

> If not yet done login to the GitHub registry first.
> More information you'll find [here](https://github.com/simatic-ax/.github/blob/main/docs/personalaccesstoken.md)

```cli
apax add @simatic-ax/sinamics-drive-functions
```

Add the namespace in your ST code:

```iec-st
USING Simatic.Ax.Sinamics
```

## Functions

| Function Blocks                             | Description                                                            |
| ------------------------------------------- | ---------------------------------------------------------------------- |
| [SinaSpeed](./docs/function_blocks/sinaspeed.md) | Moves an axis with a given speed via telegram 1                        |
| [SinaPos](./docs/function_blocks/sinapos.md)     | Controls an axis via the basic positioner technology from SINAMICS S/G |
| [SinaPara](./docs/function_blocks/sinapara.md)   | Read/write up to 16 parameters from/to a SINAMICS S/G drive            |
| [SinaParaS](./docs/function_blocks/sinaparas.md) | Read/write a single parameter from/to a SINAMICS S/G drive             |

Please refer to the documentation from SIOS [Blocks for activating SINAMICS drive functions with SIMATIC S7-1200/1500](https://support.industry.siemens.com/cs/ar/en/view/109475044), for further information about description and functionality of these blocks. Additionally for the drive functions and used telegrams also refer to the respective manual for each drive system [Landing page for SINAMICS drive functions](https://support.industry.siemens.com/cs/do/en/view/109781535).

## Additional information

> NOTE
>
> This implementation of SinaSpeed and SinaPos for SIMATIC AX has been only validated on a SINAMICS S120 drive system. Other SINAMICS drive system from the G and S series have not been tested, but should be compatible in general. The implementation of SinaPara and SinaParaS has been validated with both the SINAMICS S120 as well as the SINAMICS S210 device.
