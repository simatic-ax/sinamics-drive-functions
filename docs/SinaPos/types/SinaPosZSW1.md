# SinaPosZSW1

## Description

Defines a structure that contains the bits from ZSW1 of telegram 110/111.

## Structure

| Field | Type | Description |
| ----- | ---- | ----------- |
| ActiveTraversingBlockBit0 | `BOOL` | TRUE = traversing block bit 0 active |
| ActiveTraversingBlockBit1 | `BOOL` | TRUE = traversing block bit 1 active |
| ActiveTraversingBlockBit2 | `BOOL` | TRUE = traversing block bit 2 active |
| ActiveTraversingBlockBit3 | `BOOL` | TRUE = traversing block bit 3 active |
| ActiveTraversingBlockBit4 | `BOOL` | TRUE = traversing block bit 4 active |
| ActiveTraversingBlockBit5 | `BOOL` | TRUE = traversing block bit 5 active |
| StopCamMinusActive | `BOOL` | TRUE = STOP cam minus active |
| StopCamPlusActive | `BOOL` | TRUE = STOP cam plus active |
| JoggingActive | `BOOL` | TRUE = jogging active |
| ReferencePointApproachActive | `BOOL` | TRUE = reference point approach active |
| FlyingReferencingActive | `BOOL` | TRUE = flying referencing active |
| TraversingBlockActive | `BOOL` | TRUE = traversing block active |
| SetUpActive | `BOOL` | TRUE = set-up mode active |
| MDI | `BOOL` | TRUE = MDI active |

## Usage

The type is used as a diagnostics structure for the function block [SinaPos](../blocks/sinapos.md).
