# Food Rescue
## Sustainable Food & Diet / Waste Management

## Kolaborator
1. 2506534245 - FIQHI DESKI ISMAIL
2. 2506593613 - NABILA OKTAVIA RAMADHANI
3. 2506539082 - EVAN ANDRIAN
4. 2506625470 - KAYSAN NAVID MUSYAFFA

## Deskripsi Aplikasi
Platform untuk meredistribusikan dua jenis _resource_ pangan sebelum menjadi _waste_: 
(1) Packaged Food Surplus, yaitu makanan siap konsumsi yang masih layak dan belum dikonsumsi seperti _rice box_, _snack box_, _bakery packs_, _catering portions_, dan _unopened event food_
(2) Compostable Food Waste, yaitu sisa pangan organik yang masih dapat dimanfaatkan untuk _composting_, BSF/_maggot_, _animal feed_ tertentu, atau _upcycling_. User dapat membuat _supply listing_ untuk DONATE atau SELL, user lain dapat menemukan _supply_ atau membuat _request resource_ yang dibutuhkan, melakukan _reservation_, lalu mengatur _pickup_. Komunikasi lanjutan diarahkan ke platform pihak ke-3. Pembayaran dan negosiasi final dilakukan di luar platform.

## Problem & Solution
Problem: _Packaged food surplus_ dan _compostable food waste_ sering tidak tersalurkan karena pemilik tidak mengetahui pihak yang dapat memanfaatkannya. Akibatnya, makanan layak konsumsi maupun sisa pangan yang masih berguna berakhir menjadi _waste_.

Solution: _Platform matching_ yang mempertemukan pihak yang memiliki _resource_ dengan pihak yang dapat memanfaatkannya melalui Supply Listing, Request Board, Reservation, dan Pickup. Listing dapat berupa DONATE atau SELL. Untuk SELL, harga ditentukan owner dan pembayaran dilakukan di luar platform.

## Daftar Modul
### Module 1 — Supply Listing (ResourceListing)
User membuat dan mengelola resource yang dimiliki. Registered User dapat melakukan CRUD, sedangkan public hanya dapat browse/search/filter listing.
Data utama:
- resource_type: PACKAGED_FOOD_SURPLUS / COMPOSTABLE_FOOD_WASTE
- title, category, description
- quantity, unit
- mode: DONATE / SELL
- price (jika SELL)
- condition
- suitable_for / intended_use: HUMAN_CONSUMPTION / COMPOSTING / BSF / ANIMAL_FEED / UPCYCLING sesuai resource
- pickup_location
- available_until/deadline
- storage notes, allergen notes, packaging status bila relevan
- status

### Module 2 — Request Board (ResourceRequest)
User membuat dan mengelola request resource yang sedang dicari/dibutuhkan. Modul ini memungkinkan user untuk browse active requests, search/filter request, dan provider dapat menawarkan resource terhadap request tertentu.
Data utama:
- resource_type/category
- quantity_needed, unit
- location/area
- donate_only/max_price
- intended_use
- valid_until
- status

### Module 3 — Match/Reservation (Reservation)
Mengelola kesepakatan antara provider dan receiver. Reservation dapat dibuat ketika receiver claim Supply Listing, atau ketika provider menawarkan supply terhadap Resource Request.
Fokus modul: siapa mengambil resource apa, berapa quantity, harga yang disepakati, serta approval.
Data utama:
- provider
- receiver
- listing/request
- quantity
- agreed_price (jika ada)
- status: PENDING / ACCEPTED / REJECTED / CANCELLED
Setelah reservation diterima, stock pada listing dapat diperbarui.

### Module 4 — Pickup & Fulfillment (Pickup)
Mengelola pelaksanaan pengambilan setelah reservation diterima.
Fokus modul: kapan dan di mana handoff dilakukan serta apakah fulfillment berhasil.
Data utama:
- reservation
- pickup_date
- pickup_time
- final pickup_location
- pickup instructions / notes
- status: SCHEDULED / COMPLETED / NO_SHOW / CANCELLED

Fitur: reschedule pickup, update fulfillment status, dan opsi contact redirect ke pihak ke-3. Payment dan negosiasi final dilakukan di luar platform.

### Shared Foundation — Account & Authentication
Modul ini bukan business module utama. Modul ini bertanggung jawab untuk menangani autentikasi dan otorisasi pengguna.
Mencakup:
- register, login, logout
- basic profile dan contact/WhatsApp number
- authentication
- ownership/authorization rules agar user hanya dapat mengelola data miliknya

### Core flow:
Supply Listing / Resource Request → Reservation → Pickup → Completed

## Pembagian Modul
[Module 1](#module-1): [Fiqhi](#kolaborator)

[Module 2](#module-2): [Kaysan](#kolaborator)

[Module 3](#module-3): [Evan](#kolaborator)

[Module 4](#module-4): [Nabila](#kolaborator)

[Shared Foundation](#shared-foundation): [Fiqhi](#kolaborator)

## Public API
OpenStreetMap/Nominatim digunakan untuk pencarian dan geocoding lokasi pada Supply Listing, Resource Request, dan Pickup.
Penggunaan:
- User mencari alamat/lokasi ketika membuat supply atau request;
- Nominatim mengubah pencarian lokasi menjadi data terstruktur seperti display name dan koordinat;
- Lokasi final pickup dapat dicari/dikonfirmasi dengan data lokasi tersebut;
- Data lokasi dari API dapat di filter berdasarkan area/kota sebelum dipilih atau ditampilkan.
Untuk scope saat ini, platform tidak menjanjikan nearest-listing search, radius search, route optimization, ETA, atau distance calculation.

## Peran Pengguna
### Potential Users:
Packaged Food Surplus providers: event organizer, catering, bakery, restoran/kafe, hotel, kantin, komunitas/organisasi yang mengadakan acara.
Compostable Food Waste providers: kafe, restoran, juice shop, bakery, pasar, food-processing business, dan rumah tangga.
Receivers: individu, komunitas/NGO, composter/composting community, urban farmer/community garden, BSF/maggot farm, peternak untuk jenis waste yang sesuai, dan upcycling business.
### User Roles:
Guest: browse Supply Listing dan Request Board yang tersedia.
Registered User: dapat bertindak sebagai provider maupun receiver tergantung aktivitasnya; membuat supply/request, melakukan reservation, dan mengatur pickup.
Admin: moderasi user, listing, request, reservation, dan pickup.

Provider/receiver adalah behavioral role, bukan account role terpisah.

## Scope & Safety Notes
### Scope:
- Packaged Food Surplus yang masih layak konsumsi dan belum dikonsumsi, misalnya rice box, snack box, bakery packs, catering portions, dan unopened event food.
- Compostable Food Waste yang masih dapat dimanfaatkan, misalnya coffee grounds, fruit/vegetable scraps, eggshells, dan kategori organik lain yang sesuai.
- Donate/sell, request, reservation, local pickup, dan contact via WhatsApp.

### Safety/Platform Limitation:
- Provider wajib mencantumkan condition, deadline/available-until, dan catatan penyimpanan bila relevan.
- Platform hanya memfasilitasi listing dan matching; platform tidak melakukan automatic food safety verification.
- Food waste untuk animal feed hanya boleh ditandai jika jenis resource memang sesuai; platform tidak menganggap semua food waste aman untuk pakan.

### Out of Scope:
- payment gateway
- delivery/logistics
- AI/CV identification
- in-app chat/call
- route optimization
- pengolahan sampah
- automatic safety verification

## AI Disclosure
Dalam proses pengerjaan proyek ini, tim menggunakan bantuan AI sebagai alat pendukung untuk _brainstorming_ ide, penyusunan awal Product Requirements Document (PRD), perumusan _user flow_, serta penyusunan struktur modul aplikasi. AI juga digunakan untuk membantu mengevaluasi _scope_ fitur agar tetap sesuai dengan tema Sustainable Living dan kebutuhan tugas. 
