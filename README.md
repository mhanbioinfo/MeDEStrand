# MeDEStrand
R package MeDEStrand (bedpe input available)

## Full .bedpe file structure

```
read1                   read2                                        read1   read2   read1   read2   read1   read2       read1   read2  read1   read2   read1   read2
CHR     STAR    END     CHR     START   END     FRAGMENT ID          MAPQ    MAPQ    STRAND  STRAND  CIGAR   CIGAR       FLAG    FLAG   TLEN    TLEN    NM_TAG  NM_TAG
chr1    10028   10098   chr1    10152   10221   NB551056:129:H7...   0       0       +       -       70M     24M1I45M    99      147    193     -193    3       2
chr1    10035   10105   chr1    10174   10230   NB551056:129:H7...   0       0       +       -       70M     14S56M      99      147     195    -195    1       0
chr1    10175   10245   chr1    10257   10328   NB551056:129:H7...   9       9       +       -       70M     28M1D42M    99      147     153    -153    5       4
...
```

## Lean .bedpe for input into MeDEStrandBEDPE

- lean .bedpe should be pre-filtered from full .bedpe above by CIGAR and FLAG columns
    - only retain fragments where read1 CIGAR has only matches (e.g. 70M, and not 68M2S)
    - isPaired = TRUE ( FLAG 0x1 set )
    - isProperPair = TRUE ( FLAG 0x2 set )
    - hasUnmappedMate = FALSE ( FLAG 0x8 unset )
    - isUnammpedQuery = FALSE ( FLAG 0x4 unset )
    - isFirstMateRead = TRUE ( FLAG 0x40 set )
    - isSecondMateRead = FALSE ( FLAG 0x80 unset )
    - isSecondaryAlignment = FALSE ( FLAG 0x100 unset )
    - ( all above are consistent with original MeDEStrand parameters )
- lean .bedpe should then be transformed into tab delimited document with columns shown below
- QWIDTH is not used in MeDESTrand, keeping it just for consistency

```
read1   read1   read1   FAKE    read2   abs
CHR     STRAND  START   QWIDTH  START   TLEN
chr1    +       10029   1       10153   193
chr1    +       10036   1       10175   195
chr1    +       10176   1       10258   153
...
```