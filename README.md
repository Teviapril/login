<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tevi Hotspot - Selamat Datang</title>
    <!-- Memuat Tailwind CSS melalui CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap" rel="stylesheet">
    <style>
        /* Gaya kustom untuk memastikan latar belakang penuh dan font Inter */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #e5e7eb; /* Abu-abu terang untuk latar */
        }
        .card-shadow {
            box-shadow: 0 10px 25px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
    </style>
</head>
<body class="min-h-screen flex items-center justify-center p-4">

    <!-- Kontainer Utama dan Card Login -->
    <div class="w-full max-w-md">

        <!-- Logo/Nama Hotspot -->
        <header class="text-center mb-8">
            <h1 class="text-4xl font-extrabold text-blue-700">Tevi <span class="text-gray-800">Hotspot</span></h1>
            <p class="mt-2 text-gray-600 text-lg">Akses Internet Anda Sudah Dekat!</p>
        </header>

        <!-- Form Login -->
        <!-- Action form harus merujuk ke variabel $(action) Mikrotik -->
        <form name="login" action="$(action)" method="post"
              class="bg-white p-6 sm:p-8 rounded-xl card-shadow border border-gray-100">

            <!-- Pesan Kesalahan (Disembunyikan secara default, hanya muncul jika ada error dari Mikrotik) -->
            <!-- Variabel $(error) akan berisi pesan error jika login gagal -->
            <!-- Script di bawah akan menampilkan pesan ini jika $(error) tidak kosong -->
            <div id="error-message" class="mb-4 p-3 bg-red-100 border border-red-400 text-red-700 rounded-lg hidden">
                <span class="font-semibold">Kesalahan Login:</span> $(error)
            </div>

            <!-- Input Username -->
            <div class="mb-5">
                <label for="username" class="block text-sm font-medium text-gray-700 mb-2">Kode Voucher / Username</label>
                <input name="username" type="text" id="username" autofocus autocomplete="username"
                       class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:ring-blue-500 focus:border-blue-500 text-base"
                       placeholder="Masukkan kode voucher Anda" required>
            </div>

            <!-- Input Password (Jika menggunakan mode User/Password) -->
            <div class="mb-6">
                <label for="password" class="block text-sm font-medium text-gray-700 mb-2">Password (Kosongkan jika menggunakan Voucher)</label>
                <!-- Nama field harus "password" -->
                <input name="password" type="password" id="password" autocomplete="current-password"
                       class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:ring-blue-500 focus:border-blue-500 text-base"
                       placeholder="Masukkan password Anda (Opsional)">
            </div>

            <!-- Input tersembunyi untuk variabel Mikrotik -->
            <input type="hidden" name="dst" value="$(dst)">
            <input type="hidden" name="popup" value="true">

            <!-- Tombol Login -->
            <button type="submit"
                    class="w-full flex justify-center py-3 px-4 border border-transparent rounded-lg shadow-md text-lg font-semibold text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 transition duration-150 ease-in-out">
                Hubungkan ke Internet
            </button>
        </form>

        <!-- Informasi Status dan Footer -->
        <footer class="mt-8 text-center text-gray-500 text-sm">
            <p>Status Koneksi Anda: <span class="font-semibold text-red-500">Belum Terhubung</span></p>

            <!-- Variabel Mikrotik yang menampilkan detail koneksi -->
            <div class="mt-2 text-xs space-y-1">
                <p>IP Anda: $(ip)</p>
                <!-- Link Logout (jika sudah login, Mikrotik akan mengarahkan ke halaman ini lagi) -->
                <p>Jika sudah punya akun, silakan <a href="$(link-login)" class="text-blue-500 hover:underline font-medium">coba login di sini</a>.</p>
            </div>

            <p class="mt-4 text-gray-400">&copy; 2025 Tevi Hotspot. Ditenagai oleh Mikrotik.</p>
        </footer>

    </div>

    <!-- Script untuk penanganan status dan error -->
    <script>
        // Skrip ini akan dijalankan saat halaman dimuat oleh browser
        document.addEventListener('DOMContentLoaded', function() {
            const errorMessageDiv = document.getElementById('error-message');
            const errorText = "$(error)";

            // 1. Menampilkan Pesan Error
            // Cek apakah variabel Mikrotik $(error) memiliki isi
            if (errorText && errorText.trim().length > 0) {
                // Tampilkan div pesan error
                errorMessageDiv.classList.remove('hidden');
                // Fokuskan ke bidang username agar pengguna mudah mencoba lagi
                document.getElementById('username').focus();
            }

            // 2. Fungsi untuk Redireksi Otomatis Jika Sudah Login
            // Variabel $(if-login) dan $(link-orig) disuntikkan oleh Mikrotik
            const isLoggedIn = "$(if-login)" === "yes";
            const originalLink = "$(link-orig)";

            if (isLoggedIn) {
                // Jika pengguna sudah login, arahkan dia ke halaman aslinya
                if (originalLink && originalLink !== "http://$(server-address)/cgi-bin/redirect") {
                    window.location.href = originalLink;
                } else {
                    // Jika link asli tidak ada, arahkan ke status/info halaman Mikrotik
                    window.location.href = "$(link-status)";
                }
            }
        });
    </script>

</body>
</html>

