_____________________________________________________________________________________________________________________________
Base:-
 Address=00007FF7F5A20000
Size=0000000000001000
Party=User
Page Information=msedge.exe
Allocation Type=IMG
Current Protection=-R---
Allocation Protection=ERWC-
-----------------------------------------------------------------------
7ff7f5bc8af7	ff 25			JMP qword ptr [->_guard_dispatch_icall] -Ghydra
------------------------------------------------------------------------------
00007FF7F5BC8AF6 | 48:FF25 9B5E1600         | jmp qword ptr ds:[7FF7F5D2E998]         
00007FF7F5D2E998 | 4030B423FD7F0000         | dq 7FFD23B43040                         |
00007FFD23B43040 | 49:BB 000043D2F57D0000   | mov r11,7DF5D2430000                    |
----------------------------------------------------------------------------
000001FA65FD0000 | 0000                     | add byte ptr ds:[rax],al                | User-allocated memory
_________________________________________________________________________________________________________

## 🧪 CFG Trampoline Patching Experiment — `ntdll.dll`

### 📌 Objective

To test whether a Control Flow Guard (CFG) trampoline inside `ntdll.dll` (e.g., `mov r11, target; jmp r11`) can be patched at runtime using standard or native Windows API calls.

---

### 🔍 Tools Used

* **Ghidra** — for disassembly and CFG trampoline identification
* **x64dbg** — for runtime inspection and memory analysis
* **Custom C++ Injector** using:

  * `VirtualProtectEx`
  * `NtProtectVirtualMemory`
  * `WriteProcessMemory`

---

### 🧠 Procedure

1. **Loaded Microsoft Edge executable in Ghidra**

   * Searched for indirect control flow instructions (e.g., `ff 25`, `49 bb ... 41 ff e3`)
   * Located a trampoline at:

     ```
     0x7FFD23B43040 (inside ntdll.dll)
     mov r11, <target>
     jmp r11
     ```

2. **Confirmed control flow path**:

   ```
   jmp qword ptr [0x7FF7F5D2E998] → 0x7FFD23B43040 → mov r11, 0x7DF5D2430000 → jmp r11
   ```

3. **Attempted patching using C++**:

   * Tried to overwrite `mov r11, ...` with shellcode redirection
   * Used `VirtualProtectEx` and `NtProtectVirtualMemory` to change memory protection
   * All write attempts failed

4. **x64dbg analysis**:

   * Verified that the target address lies in a section:

     * Marked as `SEC_IMAGE`
     * Type: `ImageExtension`
     * Protection: `PAGE_EXECUTE_READ`
   * Region belongs to `ntdll.dll` (immutable by design)

---

### ❌ Result

All user-mode attempts to modify the trampoline at `0x7FFD23B43040` **failed**, including native API calls.

---

### ✅ Conclusion

CFG trampolines in `ntdll.dll` are **backed by immutable image sections** and **cannot be patched from userland**, even using `NtProtectVirtualMemory`. This behavior is enforced by Windows for security reasons.

---

### ✅ Recommended Bypass (Working Alternative)

* **Allocate your own trampoline** in RWX memory
* **Inject shellcode** into remote process
* **Patch guarded call pointer** to point to your custom trampoline instead of `ntdll.dll`

This method successfully bypasses CFG without modifying protected system DLLs.

