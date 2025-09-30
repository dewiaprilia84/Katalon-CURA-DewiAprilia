# Katalon-CURA – Dewi Aprillia

Project **Katalon Studio** untuk otomatisasi pengujian pada situs demo **CURA Healthcare Service**.

- **Repo name**: `Katalon-CURA-DewiAprillia`
- **AUT/URL**: `https://katalon-demo-cura.herokuapp.com/`
- **Runner utama**: **Test Suite** `Test Suites/TS_CURA_Demo`
- **Project file**: `testQA.prj`

> Repo ini mengikuti struktur standar Katalon dan siap dijalankan di Katalon Studio atau Katalon Runtime Engine (CLI).

---

## Isi Repository

```
Include/config/            # (opsional) custom keyword settings, hooks, global configs
Object Repository/...      # Kumpulan TestObject (locator) untuk halaman CURA
Profiles/                  # Execution Profile (GlobalVariable, mis. BASE_URL/USERNAME/PASSWORD)
Scripts/                   # Script hasil rekaman/keyword dari Test Cases
Test Cases/                # Test case utama (login/appointment/logout.)
Test Suites/TS_CURA_Demo/  # Test suite yang mengorkestrasi test cases
settings/                  # Pengaturan proyek Katalon (execution preferences, dll.)
.gitignore
build.gradle               # (opsional) untuk integrasi CI/gradle wrapper
console.properties         # Contoh properti eksekusi via CLI
testQA.prj                 # File project Katalon
```

---

## Prasyarat

- **Katalon Studio 8.x+** (disarankan terbaru) atau **Katalon Runtime Engine (KRE)** untuk eksekusi headless/CI.
- **Google Chrome** dan **ChromeDriver** yang sesuai versi.
- Akses internet ke situs demo `katalon-demo-cura.herokuapp.com`.

> Perbarui WebDriver: **Tools → Update WebDrivers → Chrome** di Katalon Studio.

---

## Konfigurasi Cepat

1. **Buka proyek** di Katalon Studio: `File → Open Project → pilih file testQA.prj`.
2. **Profile**: buka `Profiles/default` dan sesuaikan **GlobalVariable** jika diperlukan:
   - `BASE_URL` – default: `https://katalon-demo-cura.herokuapp.com/`
   - `USERNAME` – default demo: `John Doe`
   - `PASSWORD` – default demo: `ThisIsNotAPassword`
3. (Opsional) Aktifkan plugin **Basic Report** dari Katalon Store untuk output HTML/PDF/CSV.

---

## Menjalankan Test

### 1) Via Katalon Studio (GUI)

- Buka **Test Suites → TS_CURA_Demo** lalu klik **Run** (pilih browser **Chrome**).
- Hasil eksekusi ada di folder:
  ```
  Reports/<timestamp>/TS_CURA_Demo/
  ```

### 2) Via CLI (Katalon Runtime Engine)

Contoh perintah Windows (sesuaikan path):

```bash
katalonc -noSplash -runMode=console ^
  -projectPath="C:\path\to\testQA.prj" ^
  -retry=0 -testSuitePath="Test Suites/TS_CURA_Demo" ^
  -executionProfile="default" -browserType="Chrome" ^
  -apiKey=<YOUR_KATALON_API_KEY>
```

Contoh macOS/Linux:

```bash
./katalonc -noSplash -runMode=console   -projectPath="/path/to/testQA.prj"   -retry=0 -testSuitePath="Test Suites/TS_CURA_Demo"   -executionProfile="default" -browserType="Chrome"   -apiKey=<YOUR_KATALON_API_KEY>
```

> `console.properties` dapat digunakan untuk menyimpan pengaturan default eksekusi di lingkungan CI.

---

## Skenario Uji (Umum)

Polanya biasanya mencakup:
1. **Login** – klik **Make Appointment** → isi username (`#txt-username`) & password (`#txt-password`) → klik **Login**.
2. **Make Appointment** – pilih **Facility**, opsi **Readmission**, **Healthcare Program**, isi **Visit Date** & **Comment** → **Book Appointment** → verifikasi **Appointment Confirmation**.
3. **Logout** – buka menu (☰) → **Logout** → verifikasi kembali ke halaman awal.

Lihat implementasi di folder **Test Cases** dan **Scripts**.

---

## Lokasi & Praktik Locator

Gunakan locator yang stabil pada halaman login **(hindari field demo yang read-only)**:
- Username: `//input[@id='txt-username']`
- Password: `//input[@id='txt-password']`
- Tombol Login: `//button[@id='btn-login']`
- Judul Make Appointment: `//h2[normalize-space()='Make Appointment']`

> Hindari `//input[@value='John Doe'|'ThisIsNotAPassword']`—itu **panel demo** yang read-only dan memicu `InvalidElementStateException` saat di-`setText`.

---

## Laporan (Reports)

- Hasil eksekusi tersimpan di `Reports/<timestamp>/...`.
- Jika **Basic Report** terpasang, tersedia **HTML/PDF/CSV**.
- Untuk **screenshot saat gagal**, aktifkan di **Project → Settings → Report** atau gunakan `WebUI.takeScreenshot()` pada listener/teardown.

---

## Troubleshooting Singkat

- **InvalidElementStateException saat mengisi password**  
  Pastikan TestObject menunjuk ke `#txt-password` (bukan field demo read-only). Tambahkan `waitForElementClickable` sebelum `setText/setEncryptedText`.

- **Versi Chrome/Driver tidak cocok**  
  Perbarui via **Tools → Update WebDrivers → Chrome**.

- **Elemen tertutup overlay/menu**  
  Gunakan `WebUI.waitForPageLoad(10)` dan `waitForElementClickable(...)` sebelum aksi klik/ketik.

---

## CI/CD (opsional)

- `build.gradle` tersedia untuk integrasi pipeline. Panggil KRE dari job CI (GitHub Actions/Jenkins/GitLab CI) dan arsipkan folder `Reports/` sebagai artifact.

---

## Lisensi & Kredit

- Situs **CURA Healthcare Service** adalah demo resmi Katalon untuk pembelajaran & pengujian.
- Kredit pada pemilik repo **Dewi Aprillia**.
