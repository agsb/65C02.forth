
# 6502 code snipets

    
## BRA, always branch +/- 127, 4 cc
```
* = $0000
0000   18                   CLC                ;(2)
0001   90 00                BCC $00            ;(2)  
.END
```

## always direct jump, 3 cc
```
* = $0000
0000   4C 0000              JMP $0000          ;(3)
.END
```

## always indirect jump, 5 cc
```
* = $0000
0000   6C 0000              JMP ($0000)        ;(5)
.END
```

## push a zp word, 12 cc
```
* = $0000
0000   A5 00                LDA $00            ;(3)
0002   48                   PHA                ;(3)
0003   A5 01                LDA $01            ;(3)
0005   48                   PHA                ;(3)
.END
```

## pull a zp word, 14 cc
```
* = $0000
0000   68                   PLA                ;(4)
0001   85 01                STA $01            ;(3)
0003   68                   PLA                ;(4)
0004   85 00                STA $00            ;(3)
.END
```

## push a indexed word, 18 cc
```
* = $0000
0000   B5 00                LDA $00,X          ;(4)
0002   48                   PHA                ;(3)
0004   B5 00                LDA $01,X          ;(4)
0006   48                   PHA                ;(3)
0004   CA                   DEX                ;(2)
0004   CA                   DEX                ;(2)
.END
```

## pull a indexed word, 20 cc
```
* = $0000
0003   E8                   INX                ;(2)
0007   E8                   INX                ;(2)
0001   68                   PLA                ;(4)
0002   95 00                STA $01,X          ;(4)
0005   68                   PLA                ;(4)
0006   95 00                STA $00,X          ;(4)
.END
```

## pull word indexed by X, into Y and A, 12 cc
```
* = $0000
0000   B4 00                LDY $01,X          ;(4)
0003   B5 00                LDA $00,X          ;(4)
0003   E8                   INX                ;(2)
0003   E8                   INX                ;(2)
.END
```

## push a word indexed by X, from Y and A, 12 cc
```
* = $0000
0004   CA                   DEX                ;(2)
0004   CA                   DEX                ;(2)
0000   B4 00                STY $01,X          ;(4)
0003   B5 00                STA $00,X          ;(4)
.END
```

## copy from a word indexed, 18 cc
```
* = $0000
0000   B5 00                LDA $00,X          ;(4)
0002   85 02                STA $00            ;(3)
0005   B5 00                LDA $01,X          ;(4)
0007   85 01                STA $01            ;(3)
0004   E8                   INX                ;(2)
0009   E8                   INX                ;(2)
.END
```

## copy into a word indexed, 18 cc
```
* = $0000
0002   CA                   DEX                ;(2)
0007   CA                   DEX                ;(2)
0000   A5 00                LDA $00            ;(3)
0003   95 00                STA $00,X          ;(4)
0005   A5 01                LDA $01            ;(3)
0008   95 00                STA $01,X          ;(4)
.END
```

## increase a word in zp, 12 cc
```
* = $0000
0000   E6 00                INC $00            ;(5)
0002   D0 02                BNE $0006          ;(2)
0004   E6 01                INC $01            ;(5)
0006   ; any
.END
```

## increase a word indexd in zp, 14 cc
```
* = $0000
0000   F6 00                INC $00,X          ;(6)
0002   D0 02                BNE $0006          ;(2)
0004   F6 01                INC $01,X          ;(6)
0006   ; any
.END
```

## add a byte to a word in zp, 17 cc
```
* = $0000
0000   18                   CLC                ;(2)
0001   A9 02                LDA #$02           ;(2)
0003   65 00                ADC $00            ;(3)
0005   85 00                STA $00            ;(3)
0007   D0 02                BCC $000B          ;(2)
0009   E6 01                INC $01            ;(5)
.END
```

## add a byte to a word indexed in zp, 22 cc
```
* = $0000
0000   18                   CLC                ;(2)
0001   A9 02                LDA #$02           ;(2)
0003   75 00                ADC $00,X          ;(4)
0005   95 00                STA $00,X          ;(4)
0007   D0 02                BCC $000B          ;(2)
0009   E6 01                INC $01,X          ;(5)
000B   ; any
.END
```

## copy into memory indexed Y, 22 cc
```
* = $0000
0000   A0 00                LDY #$00           ;(2)
0002   A5 02                LDA $02            ;(3)
0004   91 00                STA ($00),Y        ;(6)
0006   C8                   INY                ;(2)
0007   A5 03                LDA $03            ;(3)
0009   91 00                STA ($00),Y        ;(6)
.END
```

## copy from memory indexed Y, 20 cc
```
* = $0000
0000   A0 00                LDY #$00           ;(2)
0002   B1 00                LDA ($00),Y        ;(5)
0004   85 02                STA $02            ;(3)
0006   C8                   INY                ;(2)
0007   B1 00                LDA ($00),Y        ;(5)
0009   85 03                STA $03            ;(3)
.END
```

## copy from memory indirect X, 20 + 12 cc
```
* = $0000
0000   A2 00                LDX #$00           ;(2)
0002   A1 00                LDA ($00,X)        ;(6)
0004   85 02                STA $02            ;(3)
0006   E6 00                INC $00            ;(5)
0008   D0 02                BNE L000C          ;(2)
000A   E6 01                INC $01            ;(5)
000C   A1 00      L000C     LDA ($00,X)        ;(6)
000E   85 03                STA $03            ;(3)
.END
```

## copy into memory indirect X, 20 + 12 cc
```
* = $0000
0000   A2 00                LDX #$00           ;(2)
0002   A5 02                LDA $02            ;(3)
0004   81 00                STA ($00,X)        ;(6)
0006   E6 00                INC $00            ;(5)
0008   D0 02                BNE L000C          ;(2)
000A   E6 01                INC $01            ;(5)
000C   A5 03      L000C     LDA $03            ;(3)
000E   81 00                STA ($00,X)        ;(6)
.END
```

## change +/- 2 (INC or DEC) to SP, 14 cc 
```
* = $0000
0000   86 00                STX $00            ;(3)
0002   BA                   TSX                ;(2)
0003   E8                   INX                ;(2)
0004   E8                   INX                ;(2)
0005   9A                   TXS                ;(2)
0006   A6 00                LDX $00            ;(3)
.END
```

## interrupt cascate counter, 42 cc, ~49.7 days in milliseconds
## could be at any memory also
```
* = $0000
0000   78                   SEI                ;(2)
0001   D8                   CLD                ;(2)
0002   2C 00 00             BIT $0000          ;(4)
0005   E6 00                INC $00            ;(5)
0007   D0 0A                BNE L0013          ;(2)
0009   E6 01                INC $01            ;(5)
000B   D0 06                BNE L0013          ;(2)
000D   E6 02                INC $02            ;(5)
000F   D0 02                BNE L0013          ;(2)
0011   E6 03                INC $03            ;(5)
; add resolution and extention $04, $05 etc
0013   58         L0013     CLI                ;(2)
0014   40                   RTI                ;(6)
.END
```
