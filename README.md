# Gelud

## Deskripsi

Gelud adalah game aksi di mana pemain 1 dan pemain 2 bertarung untuk mengalahkan satu sama lain dengan mengurangi energi satu sama lain. Terinspirasi oleh banyak game pertarungan, logika dasar game akan sangat mirip dengan game yang lebih populer dan terkenal.

Pengguna akan mengendalikan sebuah avatar dengan gerakan dasar (kiri dan kanan) melompat serta memiliki kemampuan untuk menyerang musuh. Model hitbox akan berubah tergantung pada posisi avatar, dan setiap kali serangan mendarat (tabrakan sederhana tidak dihitung), kerusakan terjadi.

## Fitur

Mengingat kendala waktu, hanya fitur dasar yang telah diterapkan. Layar mulai/ulang telah diterapkan dengan kontrol untuk mengaktifkan/menonaktifkan soundtrack latar belakang, dan tampilan bantuan modal untuk menampilkan kontrol dan kredit. Setelah mengklik, pengguna memulai permainan dengan dua avatar berhadapan satu sama lain dengan batas waktu 120 detik untuk mengalahkan satu sama lain. Jika tidak, permainan akan habis waktu dan menghasilkan seri.

![start](https://github.com/fitrailyasa/gelud/blob/main/docs/images/thumb_gelud.png)

Gameplay sebenarnya terbatas pada meninju saja, dan kedua sprite dibuat menggunakan karakter diri sendiri. Kontrol untuk 2 pemain, secara lokal, pada keyboard berukuran penuh dengan numpad. Melompat, menganggur, berjalan, dan menyerang memiliki animasi yang unik.

![game](https://github.com/fitrailyasa/gelud/blob/main/docs/gifs/gif-gelud.gif)

## Timer Circle (Moving "slice")

```Javascript
// frontend/level.js
drawTimerDisplay() {
   this.ctx.beginPath();
   this.ctx.fillStyle = COLOR_PALETTE.QUATERNARY;
   this.ctx.arc(
     this.dimensions.width / 2,
     LEVEL_CONSTANTS.TIMER_TEXT_HEIGHT - 5,
     LEVEL_CONSTANTS.TIMER_RADIUS,
     - 0.5 * Math.PI,
     (LEVEL_CONSTANTS.MAX_TIME - this.time)
     * 2 * Math.PI / LEVEL_CONSTANTS.MAX_TIME
     - 0.5 * Math.PI
   );
   this.ctx.lineTo(this.dimensions.width / 2,
     LEVEL_CONSTANTS.TIMER_TEXT_HEIGHT - 5
   );
   this.ctx.fill();
 }
```

Sebuah fitur yang tampak sederhana, saya ingin menerapkan lingkaran pengatur waktu secara manual yang mewakili waktu yang tersisa melalui proporsi lingkaran penuh diambil setiap beberapa detik. Tanpa merujuk ke perpustakaan, desain saat ini menggunakan beberapa matematika untuk mencari tahu di mana garis harus digambar agar tampilan berfungsi dengan benar. Awalnya, interpretasi busur dipahami secara berbeda, membuat segmen lingkaran alih-alih busur yang sebenarnya, namun semuanya menjadi lebih sederhana ketika ctx.arc digunakan dengan benar. Di samping bar kesehatan dengan ujung miring, kedua metode ini terbukti paling menantang, bukan karena kesulitan matematika, atau kompleksitas bahasa pengkodean, melainkan implementasi keduanya.

## Karakter

```Javascript
// frontend/avatar.js
this.state = {
  health: AVATAR_CONSTANTS.MAX_HEALTH,
  basicAttacking: false,
  damageDone: false,
  basicAttackHitbox: {
    w: AVATAR_CONSTANTS.AVATAR_DIMENSIONS.width / 2 + 40,
    h: 10
  },
  basicAttackDamage: 10,
  facing: playerNum === 1 ? 1 : -1,
  basicAttackKeycode: playerNum === 1 ? 74 : 97,
  movement: 'idle'
}
```

Terinspirasi dari React/Redux, mengimplementasikan POJO yang mewakili keadaan umum instance avatar terbukti sangat membantu karena terlalu banyak variabel instance terpisah yang harus dilacak. Ini diatur sebagai inisialisasi, mirip dengan keadaan awal yang diatur dalam komponen React. Penekanan tombol direkam dan ditempatkan di POJO lain untuk dilakukan dan ditampilkan sesegera mungkin, serta untuk mencegah tumpang tindih dalam beberapa penekanan tombol dari tombol yang sama. Fitur penting lainnya adalah variabel "facing", yang diatur berdasarkan pemain mana yang diwakili oleh instance avatar, mis. Fitra selalu menghadap ke kanan, dan mulai dari kiri, sedangkan Ilyasa selalu menghadap ke kiri, dan mulai dari kanan.
