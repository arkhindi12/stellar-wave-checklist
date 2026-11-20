# Stellar Wave — checklist sebelum kamu kirim lamaran

Catatan lapangan dari mengerjakan Wave 11 (18–25 Nov 2026). Isinya hal-hal yang
bikin lamaran hangus atau poin tidak turun, bukan teori.

## Urutan yang benar

```
apply → di-assign maintainer → baru tulis kode → PR → merge → maintainer tandai Resolved
```

**Menulis kode sebelum di-assign adalah pekerjaan yang bisa hilang.** Kalau orang
lain yang di-assign, PR-mu tidak bernilai poin. Ini kesalahan paling mahal dan
paling sering terjadi.

## Cek dulu: apakah repo-nya masih di program Wave

Label `Stellar Wave` di GitHub **tidak** berarti repo masih ikut program. Label
tertinggal setelah repo keluar. Cara memastikan:

```bash
curl -s "https://wave-api.drips.network/api/issues?limit=100&page=1" \
  | jq -r '.data[].repo.gitHubRepoFullName' | sort -u
```

Kalau repo-mu tidak muncul di sana, label di GitHub-nya menipu — PR-mu tidak akan
menghasilkan poin, apa pun yang terjadi.

## Batas kuota yang sebenarnya

| Batas | Angka |
| --- | --- |
| Lamaran aktif per wave | **15** |
| Lamaran per repo | 5 |
| Lamaran per organisasi | 5 |

Banyak panduan menyebut "15 per repo" — itu salah. Kuota 15 itu **total**, dan bisa
habis di satu repo. Kalau sudah 15/15, sistem menolak dengan:

```
Application quota exceeded. You can have at most 15 active applications
```

Slot bisa dibuka lagi dengan **Withdraw** dari dashboard kontributor.

## Cara membaca issue sebelum melamar

1. **Lihat jumlah pelamar.** `pendingApplicationsCount` di API. Nol pelamar jauh
   lebih baik daripada sepuluh.
2. **Cek apakah sudah dikerjakan.** Judul issue bisa basi — fitur yang diminta
   mungkin sudah ada di `main`. Baca log file yang disebut, bukan cuma judulnya.
3. **Cari PR yang menutup issue lain tapi memenuhi issue ini.** Wave sering
   menerbitkan issue dalam batch dengan arah desain yang sama, jadi satu PR lebar
   bisa menutup beberapa issue sekaligus — dan issue yang "kosong" itu sebenarnya
   sudah selesai.
4. **Hindari yang ada assignee.** Sudah jelas.

## Poin: kapan benar-benar turun

Poin hanya turun saat maintainer menandai **Resolved** di Drips. Merge saja tidak
cukup — itu dua tindakan terpisah.

```
Poin = points × pointsMultiplier
```

Multiplier biasanya seragam per repo, bukan premium. Kalau semua issue di satu repo
×4, itu baseline repo tersebut.

## Distribusi hadiah

Hadiah dibagi **pro-rata** dari pool, bukan angka tetap per issue. Nilai dolar satu
issue tidak bisa diketahui sampai wave tutup dan jendela review 7 hari selesai.
Jangan pernah menjanjikan "issue ini bernilai $X".

## Hal yang sering bikin gagal

- **Kode satu baris, PR besar.** Maintainer menolak PR yang mengubah banyak hal
  yang tidak diminta.
- **Tidak menjalankan test.** Kirim hasil test yang benar-benar kamu jalankan,
  termasuk kalau ada yang gagal sebelum perubahanmu.
- **Tidak menyebut issue.** PR tanpa `Closes #N` tidak terhubung ke issue, jadi
  tidak terhitung.
- **Komentar "assign me please".** Tidak menambah peluang; yang menambah peluang
  adalah PR yang sudah jadi atau rencana konkret yang menyebut file dan fungsi.

## Menulis lamaran yang dibaca

Yang bekerja: sebut apa yang sudah kamu baca, apa yang akan kamu ubah, dan apa
yang akan kamu verifikasi.

Yang tidak bekerja: template panjang tentang pengalaman dan motivasi.

Contoh yang pernah dapat respons:

> Saya sudah baca `builder.ts` dan `schedule.ts`. Validasi cliff ada di
> `validateStreamSchedule` dengan aturan `cliffTime >= startTime` dan
> `endTime > cliffTime`. Saya akan menambah `withCliffDate()` yang mendelegasikan
> ke `startTime()` supaya validasi yang ada tetap satu jalur, plus test untuk
> string angka murni yang kalau diserahkan ke `new Date()` terbaca sebagai tahun.

## Lisensi

MIT. Silakan pakai, ubah, atau gabungkan ke panduanmu sendiri.
