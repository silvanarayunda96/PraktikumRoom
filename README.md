# Praktikum Room - Aplikasi To-Do List

Aplikasi Android sederhana untuk mengelola daftar tugas harian menggunakan Room Database, Kotlin Coroutines, dan Jetpack Compose.

## 📋 Deskripsi

Aplikasi ini adalah implementasi dari praktikum Room Database yang mencakup operasi CRUD (Create, Read, Update, Delete) dengan fitur:
- ✅ Menambahkan tugas baru
- ✅ Menampilkan daftar tugas
- ✅ Menandai tugas sebagai selesai
- ✅ Menghapus tugas
- ✅ Data persisten (tetap tersimpan setelah aplikasi ditutup)

## 🛠️ Teknologi yang Digunakan

- **Kotlin** - Bahasa pemrograman utama
- **Jetpack Compose** - Modern UI toolkit untuk Android
- **Room Database** - Abstraction layer untuk SQLite
- **Kotlin Coroutines** - Untuk operasi asinkron
- **Flow** - Untuk observasi data secara real-time
- **MVVM Architecture** - Model-View-ViewModel pattern

## 📦 Dependencies

```gradle
// Room Database
implementation("androidx.room:room-runtime:2.8.2")
implementation("androidx.room:room-ktx:2.8.2")
kapt("androidx.room:room-compiler:2.8.2")
```

## 🏗️ Struktur Proyek

```
com.example.praktikumroom/
├── Task.kt                 # Entity - Model data tugas
├── TaskDao.kt              # DAO - Interface untuk operasi database
├── TaskDatabase.kt         # Database Class - Singleton instance
├── TaskRepository.kt       # Repository - Abstraksi data layer
├── TaskViewModel.kt        # ViewModel - Business logic
└── MainActivity.kt         # UI Layer - Jetpack Compose
```

## 🎯 Arsitektur

Aplikasi ini menggunakan **MVVM Architecture** dengan komponen:

```
┌─────────────┐
│   Activity  │ (View - Jetpack Compose)
└──────┬──────┘
       │ observes
┌──────▼──────┐
│  ViewModel  │ (Business Logic)
└──────┬──────┘
       │ uses
┌──────▼──────┐
│ Repository  │ (Data Abstraction)
└──────┬──────┘
       │ accesses
┌──────▼──────┐
│    Room     │ (Local Database)
└─────────────┘
```

## 📱 Fitur Aplikasi

### 1. CREATE (Tambah Tugas)
Pengguna dapat menambahkan tugas baru dengan mengetik judul tugas dan menekan tombol (+).

### 2. READ (Lihat Tugas)
Semua tugas ditampilkan dalam daftar yang dapat di-scroll. Data diobservasi secara real-time menggunakan Flow.

### 3. UPDATE (Update Status)
Pengguna dapat mencentang checkbox untuk menandai tugas sebagai selesai. Tugas yang selesai akan ditampilkan dengan warna berbeda.

### 4. DELETE (Hapus Tugas)
Pengguna dapat menghapus tugas dengan menekan tombol ikon tempat sampah (🗑️).

### 5. EDIT (Edit Tugas)
Pengguna dapat mengedit dan mengganti nama tugas yang sebelumnya.

## 🚀 Cara Menjalankan

1. **Clone atau download proyek**
2. **Buka dengan Android Studio**
3. **Sync Gradle** - Tunggu hingga dependencies terunduh
4. **Run aplikasi** - Tekan tombol Run (▶️) atau Shift + F10
5. **Pilih emulator atau device fisik**

## 📝 Komponen Room

### 1. Entity (Task.kt)
```kotlin
@Entity(tableName = "task_table")
data class Task(
    @PrimaryKey(autoGenerate = true)
    val id: Int = 0,
    val title: String,
    val isCompleted: Boolean = false
)
```

### 2. DAO (TaskDao.kt)
Interface yang mendefinisikan operasi database:
- `@Insert` - Menambah data baru
- `@Query` - Membaca data
- `@Update` - Mengupdate data
- `@Delete` - Menghapus data

### 3. Database (TaskDatabase.kt)
Abstract class yang mengimplementasikan Singleton Pattern untuk menghindari multiple instance.

## 🔄 Flow Data

1. User melakukan aksi di UI (Compose)
2. UI memanggil fungsi di ViewModel
3. ViewModel menjalankan operasi di Repository menggunakan `viewModelScope.launch`
4. Repository memanggil fungsi DAO
5. DAO melakukan operasi di Room Database
6. Perubahan data dipancarkan melalui Flow
7. UI secara otomatis ter-update
