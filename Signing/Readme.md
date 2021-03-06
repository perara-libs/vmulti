# How to production-sign for Windows 7 to 10

**Note:** the scripts and commands listed here are customized for my needs, adjust accordingly and use with care.

1. Run `SIGN4PORTAL.CMD`
2. Upload the resulting `*.cab` files (one for each architecture) to the [Microsoft portal](https://developer.microsoft.com/en-us/dashboard/hardware/Driver/)
3. Download signed files from portal
4. Extract contents and move to folder containing the files
5. Append EV-Signature to binaries with `signtool sign /v /ac "C:\Program Files (x86)\Windows Kits\10\CrossCertificates\GlobalSign Root CA.crt" /n "Wohlfeil.IT e.U." /tr http://sha256timestamp.ws.symantec.com/sha256/timestamp /fd sha256 /td sha256 /as *.sys *.dll`
6. Remove catalog files provided by Microsoft
7. Create new catalog files for both architectures:
   * x64: `Inf2Cat.exe /driver:"." /uselocaltime /os:7_X64,Server2008R2_X64,8_X64,Server8_X64,6_3_X64,Server6_3_X64,10_X64,Server10_X64,10_AU_X64,Server2016_X64,10_RS2_X64,ServerRS2_X64`
   * x86: `Inf2Cat.exe /driver:"." /uselocaltime /os:7_X86,8_X86,6_3_X86,10_X86,10_AU_X86,10_RS2_X86`
8. Sign new catalog files with `signtool sign /v /ac "C:\Program Files (x86)\Windows Kits\10\CrossCertificates\GlobalSign Root CA.crt" /n "Wohlfeil.IT e.U." /tr http://sha256timestamp.ws.symantec.com/sha256/timestamp /fd sha256 /td sha256 *.cat`
