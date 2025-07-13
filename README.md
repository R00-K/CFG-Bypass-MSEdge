# 🚩 CFG Bypass via Dispatch Table Patch – Microsoft Edge

## 🔒 Project Goal

Bypass **Control Flow Guard (CFG)** in `msedge.exe` by redirecting a dispatch table entry (`dq`) to custom **trampoline+shellcode** memory, while ensuring execution passes CFG validation.

---

## 🧪 Demonstration Summary

* Target: `msedge.exe`
* Patched Offset: `0x1A8AF7`
* Original DQ at: `msedge_base + 0x30E998`
* Shellcode: Reverse shell payload injected into `VirtualAllocEx` memory
* CFG-compliant: Executed via copied trampoline structure from `ntdll.dll`

---

## ⚙️ How It Works

This project:

1. Locates the PID of `msedge.exe`
2. Gets its base address and calculates `jmp` and `dq` offsets
3. Allocates memory in the remote process
4. Writes shellcode into allocated memory
5. Patches the dispatch table (`dq`) pointer to the new memory
6. Execution is redirected from a `jmp qword ptr ds:[...+30E998]` to shellcode
7. Shellcode executes with **CFG validation passed**

---

## 💎 Files Included

```
CFG-Bypass-MSEdge/
├── injector.cpp                 
├── README.md             
├── notes.md             
└── screenshots/       
```

---

## 🧐 injector.cpp – Overview

The injector does the following:

* 🌿 Locates `msedge.exe` PID using `Toolhelp32Snapshot`
* 📍 Retrieves base address of `msedge.exe`
* 🧠 Allocates RWX memory inside the target process with `VirtualAllocEx`
* 💣 Writes raw shellcode into that memory
* �� Patches the DQ pointer (dispatch table) at `base + 0x30E998` to point to shellcode
* 🔓 Temporarily changes memory protection to allow patching
* ✅ Successfully redirects execution when `jmp qword ptr [base + 0x1A8AF7]` is hit

### 🔧 Build Instructions

Use `g++` or `cl.exe` with Windows SDK:

```bash
g++ injector.cpp -o injector.exe
```

---

## ✅ Example Output

```
🧬 PID of msedge.exe: 6496
🏠 Base address of msedge.exe: 0x7FF69E750000
Jmp Address   : 0x7FF69E8F8AF7
🎯 DQ address to patch: 0x7FF69EA5E998
🧠 Allocated shellcode memory at: 0x1CF63DE00000
✅ Successfully patched dq to point to shellcode.
```

---

## 🧪 Verification

Once patched:

* `jmp qword ptr [0x7FF69EA5E998]` jumps to your memory
* `RIP` is redirected to the shellcode start
* **No CFG violations**
* ✅ Reverse shell (or MessageBox, test payload) executed successfully

---

## 📈 Memory Map Example

```
Address=00007FF69E750000
Size=1000
Module=msedge.exe
Offset Patched=0x1A8AF7
Patched dq     = 0x1CF63DE00000
```

---

## 🔐 Disclaimer

> This project is for **educational research** only. Do not use on systems you do not own or have explicit permission to test.

---

## 👤 Author

**Godwin Jose**
Cybersecurity Researcher
Windows Internals | Red Teaming | Exploit Dev
📬 GitHub: \[R00-K]
