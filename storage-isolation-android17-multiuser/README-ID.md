# Storage Isolation — perbaikan multi-user Android 17

Modul ini mendukung Storage Isolation `8.8.1` dengan Enhanced Mode/Zygisk `v29.0.1` pada Android 17 arm64-v8a, untuk user personal dan work profile terkelola.

## Yang diperbaiki

- Kompatibilitas `PackageInfoList` Android 17 pada hook daftar paket Enhanced Mode.
- Baris **App rule management** terpisah untuk setiap user/profile yang memiliki aplikasi.
- Label `User 10` dan badge work profile yang benar.
- Status On/Off mengikuti nilai enabled rule sebenarnya, bukan keberadaan cache.
- Pembacaan rule menggunakan identitas user/profile dari UID setiap baris.

APK Storage Isolation resmi tidak dimodifikasi atau di-sign ulang. Ini satu modul Zygisk; tidak memerlukan LSPosed maupun modul kedua.

## Instalasi

1. Pastikan core service Storage Isolation versinya `355` dan ABI perangkat adalah `arm64-v8a`.
2. Di KernelSU atau Magisk, pasang `storage-isolation-enhanced-multiuser-v29.0.1.5-candidate.zip`. ZIP ini menggantikan modul Enhanced Mode Storage Isolation yang ada.
3. Reboot.
4. Buka Storage Isolation dan pastikan tertulis **Using Enhanced mode v29.0.1 (Zygisk)**.
5. Buka **App rule management**. Baris profile menampilkan suffix user dan badge work profile.

## Cara kerja

Modul mempertahankan hook kompatibilitas daftar paket Android 17 dan membawa identitas user dari UID paket ke respons daftar daemon. Service boot modul memasang kembali daemon patched setelah Storage Isolation mengekstrak daemon resmi, sehingga perbaikan bertahan setelah reboot.

## Cakupan dan batasan

- Dibuat hanya untuk core service Storage Isolation `355`, Enhanced Mode `v29.0.1`, dan Android 17 `arm64-v8a`.
- Tidak menghapus atau mengubah rule yang tersimpan di `/data/adb/storage-isolation/configuration.json`.
- Modul melewati patch daemon bila versi core service bukan `355`; jangan gunakan dengan versi lain.
- Diuji dengan user personal `0` dan work profile `10`.

## Status pengujian v29.0.1-multiuser.5

Pre-release, diuji pada satu perangkat. Uji ON→OFF dan buka ulang halaman berhasil pada profile personal dan work profile. Perbaikan dilaporkan tetap bekerja setelah reboot. Isolasi filesystem aktual belum diuji ulang.
