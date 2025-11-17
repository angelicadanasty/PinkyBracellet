# PinkyBracellet

<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pinky Bracelet | Aksesori Remaja</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Charm:wght@400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* Warna Pastel Khas Pinky Bracelet */
        :root {
            --color-pink-pastel: #FADADD; /* Pink Pastel */
            --color-purple-pastel: #E6E6FA; /* Purple Pastel (Lavender) */
            --color-dark-accent: #5B21B6; /* Deep Purple for text/accents */
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: #FFF7F9; /* Off-white / Very light pink background */
            color: #333;
        }
        .section-title {
            font-family: 'Charm', cursive; /* Font yang lebih manis/dekoratif untuk judul */
            color: var(--color-dark-accent);
        }
        .coquette-button {
            background-color: var(--color-pink-pastel);
            color: var(--color-dark-accent);
            border: 2px solid var(--color-dark-accent);
            transition: all 0.2s ease-in-out;
        }
        .coquette-button:hover {
            background-color: var(--color-dark-accent);
            color: var(--color-pink-pastel);
            transform: scale(1.05);
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.1);
        }
        .product-card {
            background-color: white;
            border: 1px solid var(--color-pink-pastel);
            transition: transform 0.3s;
        }
        .product-card:hover {
            transform: translateY(-5px);
            /* Bayangan lebih manis, meniru warna pink pastel */
            box-shadow: 0 10px 15px -3px rgba(240, 150, 168, 0.5), 0 4px 6px -4px rgba(240, 150, 168, 0.5);
        }
        .flower-decoration {
            color: var(--color-purple-pastel);
        }
        .coquette-border {
            border: 4px dashed var(--color-pink-pastel);
            padding: 1rem;
        }
        .modal {
            transition: opacity 0.3s ease-out;
        }
    </style>
</head>
<body class="min-h-screen">

    <!-- Modal Keranjang (Cart) -->
    <div id="cart-modal" class="modal fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50 hidden opacity-0" onclick="closeModal('cart-modal')">
        <div class="bg-white p-6 rounded-xl shadow-2xl w-11/12 max-w-lg transform scale-100 transition-all duration-300" onclick="event.stopPropagation()">
            <div class="flex justify-between items-center mb-4 border-b pb-2">
                <h3 class="text-2xl font-bold text-gray-800 section-title">Keranjang Belanja <i class="fas fa-shopping-bag text-pink-500"></i></h3>
                <button onclick="closeModal('cart-modal')" class="text-gray-500 hover:text-gray-800 text-3xl font-light leading-none">&times;</button>
            </div>
            <div id="cart-items" class="space-y-3 max-h-80 overflow-y-auto">
                <!-- Item keranjang akan dimasukkan di sini oleh JS -->
                <p id="empty-cart-message" class="text-gray-500 text-center">Keranjangmu masih kosong, yuk belanja!</p>
            </div>
            <div id="cart-summary" class="mt-6 pt-4 border-t border-purple-200">
                <div class="flex justify-between font-bold text-xl text-dark-accent">
                    <span>Total:</span>
                    <span id="cart-total" class="text-pink-600">Rp 0</span>
                </div>
                <button onclick="window.location.hash='#form-pemesanan'; closeModal('cart-modal');" class="coquette-button w-full mt-4 py-3 rounded-full font-semibold hover:shadow-lg">
                    Lanjut ke Pemesanan
                </button>
            </div>
        </div>
        <!-- Custom Notification Message Box -->
        <div id="custom-notification" class="fixed top-4 right-4 bg-pink-500 text-white p-3 rounded-lg shadow-xl z-50 transition-all duration-500 transform translate-x-full"></div>
    </div>

    <!-- Header & Navigasi -->
    <header class="sticky top-0 z-40 bg-white shadow-md">
        <div class="max-w-7xl mx-auto p-4 flex justify-between items-center">
            <a href="#beranda" class="flex flex-col items-start leading-tight">
                <span class="text-4xl font-bold section-title">Pinky Bracelet</span>
                <span class="text-sm font-light text-gray-500">12 H | Kelompok 11</span>
                <span class="text-xs font-medium text-purple-600">Asty dan Angelica</span>
            </a>

            <!-- Menu Navigasi (Hanya untuk desktop/tablet) -->
            <nav class="hidden md:flex space-x-6 text-lg font-medium">
                <a href="#beranda" class="text-gray-600 hover:text-pink-500 transition duration-150">Beranda</a>
                <a href="#tentang-kami" class="text-gray-600 hover:text-pink-500 transition duration-150">Tentang Kami</a>
                <a href="#produk" class="text-gray-600 hover:text-pink-500 transition duration-150">Produk</a>
                <a href="#testimoni" class="text-gray-600 hover:text-pink-500 transition duration-150">Testimoni</a>
                <a href="#form-pemesanan" class="text-gray-600 hover:text-pink-500 transition duration-150">Pesan</a>
            </nav>

            <!-- Container untuk Tombol Keranjang dan Hamburger -->
            <div class="flex items-center space-x-3">
                <!-- Ikon Keranjang -->
                <button onclick="openModal('cart-modal')" class="relative p-2 rounded-full coquette-button">
                    <i class="fas fa-shopping-bag text-xl"></i>
                    <span id="cart-count" class="absolute -top-1 -right-1 bg-pink-600 text-white text-xs font-bold w-5 h-5 flex items-center justify-center rounded-full">0</span>
                </button>
                
                <!-- Hamburger Menu Button (Mobile Only) -->
                <button id="menu-toggle-btn" onclick="toggleMobileMenu()" class="p-2 rounded-full coquette-button md:hidden">
                    <i id="menu-icon" class="fas fa-bars text-xl"></i>
                </button>
            </div>
        </div>

        <!-- Mobile Nav Overlay (Added for responsiveness) -->
        <nav id="mobile-menu" class="absolute top-full left-0 w-full bg-white shadow-xl md:hidden z-30 transition-all duration-300 ease-in-out transform -translate-y-full opacity-0">
            <div class="flex flex-col p-4 space-y-2 font-medium">
                <a href="#beranda" onclick="toggleMobileMenu()" class="text-gray-700 hover:bg-pink-100 p-2 rounded-md transition duration-150">Beranda</a>
                <a href="#tentang-kami" onclick="toggleMobileMenu()" class="text-gray-700 hover:bg-pink-100 p-2 rounded-md transition duration-150">Tentang Kami</a>
                <a href="#produk" onclick="toggleMobileMenu()" class="text-gray-700 hover:bg-pink-100 p-2 rounded-md transition duration-150">Produk</a>
                <a href="#testimoni" onclick="toggleMobileMenu()" class="text-gray-700 hover:bg-pink-100 p-2 rounded-md transition duration-150">Testimoni</a>
                <a href="#form-pemesanan" onclick="toggleMobileMenu()" class="text-gray-700 hover:bg-pink-100 p-2 rounded-md transition duration-150">Pesan</a>
            </div>
        </nav>
    </header>

    <main class="max-w-7xl mx-auto py-8 px-4 sm:px-6 lg:px-8">
        
        <!-- Bagian Autentikasi dan ID Pengguna -->
        <div class="text-center text-xs text-gray-500 mb-4 p-2 bg-yellow-100 rounded-lg shadow-sm">
            <p>Status Database: <span id="db-status" class="font-bold text-red-500">Menghubungkan...</span></p>
            <!-- Menampilkan user ID agar bisa digunakan untuk kolaborasi, sesuai instruksi -->
            <p>ID Pengguna (Share ini untuk kolaborasi): <span id="user-id-display" class="font-mono text-xs break-all">N/A</span></p>
        </div>
        
        <!-- Bagian 1: Beranda (Home) -->
        <section id="beranda" class="min-h-[50vh] flex flex-col justify-center items-center text-center py-20 bg-purple-100 rounded-3xl shadow-xl mb-12 relative" style="background-color: var(--color-purple-pastel);">
            <i class="fas fa-heart text-5xl text-pink-500 flower-decoration mb-4"></i>
            <h1 class="text-5xl md:text-7xl font-extrabold mb-4 section-title text-dark-accent">
                Selamat Datang di Pinky Bracelet!
            </h1>
            <p class="text-xl md:text-2xl max-w-2xl text-dark-accent/80 font-medium">
                Jelajahi koleksi gelang manis & *trendy* yang akan membuat harimu lebih ceria!
            </p>
            <a href="#produk" class="coquette-button mt-8 px-8 py-3 rounded-full font-semibold text-lg hover:shadow-lg">
                Lihat Semua Produk <i class="fas fa-arrow-right ml-2"></i>
            </a>
            <!-- Dekorasi Bunga Ringan -->
            <div class="absolute top-4 right-4 text-pink-400 opacity-50 text-2xl hidden sm:block">🌸</div>
            <div class="absolute bottom-4 left-4 text-purple-400 opacity-50 text-3xl hidden sm:block">🎀</div>
        </section>

        <!-- Bagian 2: Tentang Kami (About Us) -->
        <section id="tentang-kami" class="py-12 md:py-16 text-center coquette-border rounded-xl mb-12 bg-white">
            <h2 class="text-4xl font-bold mb-6 section-title">Tentang Pinky Bracelet</h2>
            <div class="flex justify-center">
                <p class="text-lg max-w-3xl px-4 font-medium text-gray-700">
                    Halo! Kami sebagai Owner dari <span class="font-bold text-pink-500">Pinky Bracelet</span> dengan nama Asty dan Angelica. Kami menciptakan gelang-gelang ini dengan penuh cinta, terinspirasi dari gaya *coquette* dan keceriaan remaja. Semoga kalian puas dengan produk kami dan aksesoris kami bisa menjadi bagian dari gaya unikmu!
                </p>
            </div>
            <div class="mt-6 flex justify-center space-x-4 text-2xl text-dark-accent">
                <i class="fas fa-gem"></i>
                <i class="fas fa-star"></i>
                <i class="fas fa-crown"></i>
            </div>
        </section>

        <!-- Bagian 3: Daftar Produk -->
        <section id="produk" class="py-12 md:py-16">
            <h2 class="text-4xl font-bold text-center mb-10 section-title">
                Daftar Layanan Pembelian Produk <i class="fas fa-tags text-pink-500"></i>
            </h2>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">

                <!-- Produk 1: Gelang Warna Warni -->
                <div id="product-1" class="product-card rounded-xl overflow-hidden shadow-lg p-6 flex flex-col items-center">
                    <!-- Menggunakan h-72 agar semua card memiliki tinggi yang konsisten -->
                    <img src="https://media.karousell.com/media/photos/products/2025/6/20/beads_bracelet__gelang_manik___1750427608_2409c04d_progressive.jpg" alt="Gelang Warna-warni" class="w-full h-72 rounded-lg mb-4 shadow-md object-cover">
                    <h3 class="text-3xl font-bold section-title mb-2">Gelang Warna Warni</h3>
                    <p class="text-xl font-semibold text-pink-600 mb-4">Rp 10.000</p>
                    <p class="text-center text-gray-600 mb-6">Gelang manik-manik penuh warna yang cocok untuk tampilan sehari-hari yang ceria. Cocok dipadukan dengan semua *outfit*!</p>
                    <button onclick="addToCart({id: 1, name: 'Gelang Warna Warni', price: 10000})" class="coquette-button w-full py-3 rounded-full font-semibold">
                        Tambah ke Keranjang <i class="fas fa-cart-plus ml-2"></i>
                    </button>
                </div>

                <!-- Produk 2: Gelang Y2K -->
                <div id="product-2" class="product-card rounded-xl overflow-hidden shadow-lg p-6 flex flex-col items-center">
                    <!-- Menggunakan h-72 agar semua card memiliki tinggi yang konsisten -->
                    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRVMsy88qks5m7h7i2SFeY04eMNI4UKThyTmnSKsTOmhw&s" alt="Gelang Y2K" class="w-full h-72 rounded-lg mb-4 shadow-md object-cover">
                    <h3 class="text-3xl font-bold section-title mb-2">Gelang Y2K</h3>
                    <p class="text-xl font-semibold text-pink-600 mb-4">Rp 10.000</p>
                    <p class="text-center text-gray-600 mb-6">Gelang dengan desain manik-manik unik, terinspirasi dari gaya populer tahun 2000-an. *A must-have*!</p>
                    <button onclick="addToCart({id: 2, name: 'Gelang Y2K', price: 10000})" class="coquette-button w-full py-3 rounded-full font-semibold">
                        Tambah ke Keranjang <i class="fas fa-cart-plus ml-2"></i>
                    </button>
                </div>
            </div>
        </section>

        <!-- Bagian 4: Testimoni Pelanggan -->
        <section id="testimoni" class="py-12 md:py-16 bg-pink-100 rounded-3xl shadow-xl mb-12" style="background-color: var(--color-pink-pastel);">
            <h2 class="text-4xl font-bold text-center mb-10 section-title text-dark-accent">
                Bagikan Manisnya Pengalamanmu <i class="fas fa-comment-dots text-purple-600"></i>
            </h2>
            
            <!-- Daftar Testimoni dari Firestore -->
            <div id="testimonials-list" class="max-w-2xl mx-auto space-y-4 mb-8">
                <p class="text-center text-gray-500 italic" id="no-testimonials">Memuat testimoni...</p>
            </div>

            <div class="max-w-xl mx-auto p-6 bg-white rounded-xl shadow-lg">
                <form id="testimonial-form" class="space-y-4">
                    <div>
                        <label for="testi-name" class="block text-sm font-medium text-gray-700">Nama (Opsional)</label>
                        <input type="text" id="testi-name" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 focus:border-pink-500 focus:ring focus:ring-pink-200" placeholder="Contoh: Bunga P.">
                    </div>
                    <div>
                        <label for="testi-text" class="block text-sm font-medium text-gray-700">Testimoni Anda *</label>
                        <textarea id="testi-text" required rows="4" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm p-3 focus:border-pink-500 focus:ring focus:ring-pink-200" placeholder="Tuliskan pendapatmu tentang produk kami..."></textarea>
                    </div>
                    <button type="submit" id="submit-testi-btn" class="coquette-button w-full py-3 rounded-full font-semibold hover:shadow-lg">
                        Kirim Testimoni
                    </button>
                    <div id="testi-message" class="text-center mt-3 text-sm font-medium text-green-600 hidden">Terima kasih atas testimonimu yang manis!</div>
                </form>
            </div>
        </section>

        <!-- Bagian 5: Formulir Pemesanan -->
        <section id="form-pemesanan" class="py-12 md:py-16">
            <h2 class="text-4xl font-bold text-center mb-10 section-title">
                Selesaikan Pesananmu! <i class="fas fa-truck-moving text-pink-500"></i>
            </h2>

            <div class="max-w-3xl mx-auto p-6 bg-purple-100 rounded-xl shadow-xl coquette-border">
                <form id="order-form" class="space-y-6">
                    <!-- Detail Pemesanan -->
                    <div>
                        <label for="order-name" class="block text-sm font-bold mb-1 text-dark-accent">Nama Lengkap *</label>
                        <input type="text" id="order-name" required class="block w-full rounded-md border-gray-300 shadow-sm p-3 focus:border-pink-500 focus:ring focus:ring-pink-200" placeholder="Nama sesuai KTP/Kartu Pelajar">
                    </div>
                    <div>
                        <label for="order-address" class="block text-sm font-bold mb-1 text-dark-accent">Alamat Lengkap *</label>
                        <textarea id="order-address" required rows="3" class="block w-full rounded-md border-gray-300 shadow-sm p-3 focus:border-pink-500 focus:ring focus:ring-pink-200" placeholder="Nama Jalan, Nomor Rumah, RT/RW, Kelurahan, Kecamatan, Kota/Kabupaten, Kode Pos"></textarea>
                    </div>
                    <div>
                        <label for="order-phone" class="block text-sm font-bold mb-1 text-dark-accent">Nomor Telepon (WA Aktif) *</label>
                        <input type="tel" id="order-phone" required class="block w-full rounded-md border-gray-300 shadow-sm p-3 focus:border-pink-500 focus:ring focus:ring-pink-200" placeholder="Contoh: 081234567890">
                    </div>

                    <!-- Ringkasan Pesanan -->
                    <div class="border-t pt-4 border-pink-300">
                        <h3 class="text-xl font-bold section-title mb-3">Ringkasan Pesanan</h3>
                        <div id="order-summary-list" class="space-y-2 text-gray-700">
                            <p id="empty-order-message" class="text-center italic text-red-500">Keranjang masih kosong. Silakan pilih produk terlebih dahulu.</p>
                        </div>
                        <div class="flex justify-between font-bold text-2xl mt-4 border-t pt-2 border-pink-300">
                            <span>Total Pembayaran:</span>
                            <span id="order-total" class="text-pink-600">Rp 0</span>
                        </div>
                    </div>

                    <!-- Informasi Pembayaran -->
                    <div class="border-t pt-4 border-pink-300 space-y-4">
                        <h3 class="text-xl font-bold section-title mb-3">Metode Pembayaran (Transfer DANA)</h3>
                        <div class="p-4 bg-white rounded-lg shadow-inner text-center">
                            <p class="text-gray-700 mb-2 font-medium">Anda dapat melakukan pembayaran langsung melalui DANA:</p>
                            <a href="https://link.dana.id/minta?full_url=https://qr.dana.id/v1/281012012023052082211598" target="_blank" class="text-lg font-bold text-blue-600 hover:text-blue-800 underline transition duration-150 block mb-2">
                                Klik untuk Bayar via Link DANA
                            </a>
                            <p class="text-sm font-medium text-gray-500">Atau transfer manual ke nomor DANA:</p>
                            <p class="text-2xl font-extrabold text-dark-accent">081211234744</p>
                            <p class="text-xs mt-1 text-red-500">Pastikan jumlah yang ditransfer sesuai dengan Total Pembayaran di atas.</p>
                        </div>
                    </div>

                    <button type="submit" class="coquette-button w-full py-4 rounded-full font-extrabold text-xl hover:shadow-xl">
                        Konfirmasi Pesanan Selesai
                    </button>
                    <div id="order-message" class="text-center mt-3 text-lg font-bold text-green-600 hidden p-3 bg-green-50 rounded-lg">Pesananmu telah dikonfirmasi! Silakan lakukan pembayaran dan tunggu informasi pengiriman lebih lanjut. Terima kasih!</div>
                </form>
            </div>
        </section>

    </main>

    <footer class="text-center py-6 bg-purple-200 text-dark-accent mt-12">
        <p>&copy; 2025 Pinky Bracelet. Dibuat dengan cinta oleh Asty dan Angelica.</p>
    </footer>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { 
            getAuth, 
            signInAnonymously, 
            signInWithCustomToken, 
            onAuthStateChanged 
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { 
            getFirestore, 
            doc, 
            setDoc, 
            onSnapshot, 
            collection, 
            addDoc, 
            serverTimestamp,
            query
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        // --- MANDATORY GLOBAL VARIABLES ---
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
        
        let app;
        let db;
        let auth;
        let userId = null;
        let cart = []; // Local representation of the cart
        let isAuthReady = false;

        // --- FIREBASE INITIALIZATION ---
        try {
            app = initializeApp(firebaseConfig);
            db = getFirestore(app);
            auth = getAuth(app);

            // 1. Authentication Listener
            onAuthStateChanged(auth, async (user) => {
                isAuthReady = true;
                if (user) {
                    userId = user.uid;
                    document.getElementById('db-status').textContent = 'Tersambung';
                    document.getElementById('db-status').classList.remove('text-red-500');
                    document.getElementById('db-status').classList.add('text-green-600');
                    document.getElementById('user-id-display').textContent = userId;

                    // 2. Start Data Listeners AFTER Auth is Ready
                    loadCartFromFirestore();
                    loadTestimonials();
                } else {
                    // Sign in anonymously if no user is found
                    if (initialAuthToken) {
                        await signInWithCustomToken(auth, initialAuthToken);
                    } else {
                        await signInAnonymously(auth);
                    }
                }
            });

            // 3. Initial Sign-In Attempt (Triggered if not already signed in)
            // This is primarily for the first load to ensure we get a user/token ASAP
            if (!auth.currentUser) {
                if (initialAuthToken) {
                    // Note: We don't await here as the onAuthStateChanged listener handles the rest
                    signInWithCustomToken(auth, initialAuthToken).catch(e => console.error("Initial Token Sign-In Failed:", e));
                } else {
                    signInAnonymously(auth).catch(e => console.error("Initial Anonymous Sign-In Failed:", e));
                }
            }

        } catch (error) {
            console.error("Firebase/Authentication Error:", error);
            document.getElementById('db-status').textContent = 'GAGAL (Lihat konsol)';
            document.getElementById('db-status').classList.add('text-red-500');
        }

        // --- FIRESTORE REFERENCES ---
        function getCartDocRef() {
            if (!db || !userId) return null;
            // Path: /artifacts/{appId}/users/{userId}/pinky_cart/myCart (Private)
            return doc(db, 'artifacts', appId, 'users', userId, 'pinky_cart', 'myCart');
        }

        function getTestimonialsCollectionRef() {
            if (!db) return null;
            // Path: /artifacts/{appId}/public/data/pinky_testimonials (Public)
            return collection(db, 'artifacts', appId, 'public', 'data', 'pinky_testimonials');
        }

        function getOrdersCollectionRef() {
            if (!db) return null;
            // Path: /artifacts/{appId}/public/data/pinky_orders (Public: For shop owners to view orders)
            return collection(db, 'artifacts', appId, 'public', 'data', 'pinky_orders');
        }

        // --- CART LOGIC (Firestore Integration) ---

        // Pushes the local 'cart' array to Firestore
        async function updateCartInFirestore() {
            const cartRef = getCartDocRef();
            if (cartRef) {
                try {
                    // Ensure 'items' array is saved correctly
                    await setDoc(cartRef, { items: cart, lastUpdate: serverTimestamp() }, { merge: true });
                } catch (e) {
                    console.error("Error updating cart:", e);
                    showNotification("Gagal menyimpan keranjang ke database.");
                }
            }
        }

        // Sets up the real-time listener for the user's cart
        function loadCartFromFirestore() {
            const cartRef = getCartDocRef();
            if (cartRef) {
                onSnapshot(cartRef, (docSnap) => {
                    if (docSnap.exists() && docSnap.data().items) {
                        cart = docSnap.data().items;
                    } else {
                        cart = []; // Empty cart if doc doesn't exist
                    }
                    updateCartUI(); // Update the local UI with the data from Firestore
                }, (error) => {
                    console.error("Error loading cart:", error);
                });
            }
        }

        // --- TESTIMONIALS LOGIC (Firestore Integration) ---

        function loadTestimonials() {
            const testiRef = getTestimonialsCollectionRef();
            if (testiRef) {
                // Query: Ambil semua testimoni. Gunakan query() tanpa orderBy untuk menghindari index error.
                const q = query(testiRef); 
                
                onSnapshot(q, (snapshot) => {
                    const testimonialsList = document.getElementById('testimonials-list');
                    testimonialsList.innerHTML = '';
                    let testimonials = [];
                    
                    snapshot.forEach(doc => {
                        testimonials.push(doc.data());
                    });
                    
                    // Sortir secara lokal berdasarkan timestamp (terbaru di atas)
                    testimonials.sort((a, b) => {
                        // Safely access timestamp and convert to date object for comparison
                        const dateA = a.timestamp?.toDate ? a.timestamp.toDate().getTime() : 0;
                        const dateB = b.timestamp?.toDate ? b.timestamp.toDate().getTime() : 0;
                        return dateB - dateA; // Descending (Newest first)
                    });
                    
                    const noTestimonialsMessage = document.getElementById('no-testimonials');

                    if (testimonials.length === 0) {
                        if (!noTestimonialsMessage) {
                             const p = document.createElement('p');
                             p.id = 'no-testimonials';
                             p.className = 'text-center text-gray-500 italic';
                             p.textContent = 'Belum ada testimoni. Jadilah yang pertama!';
                             testimonialsList.appendChild(p);
                        } else {
                            noTestimonialsMessage.textContent = 'Belum ada testimoni. Jadilah yang pertama!';
                            testimonialsList.appendChild(noTestimonialsMessage);
                        }
                        return;
                    }
                    
                    // Render Testimonials
                    testimonials.forEach(t => {
                        // Use try-catch for date conversion safety
                        let dateString = 'Tanggal tidak diketahui';
                        try {
                             const date = t.timestamp?.toDate ? t.timestamp.toDate() : new Date(0); // Default to a very old date if timestamp is missing
                             dateString = date.toLocaleDateString('id-ID', { year: 'numeric', month: 'short', day: 'numeric' });
                        } catch (e) {
                             console.warn("Could not parse timestamp:", e);
                        }
                        
                        const testiDiv = document.createElement('div');
                        testiDiv.className = 'bg-white p-4 rounded-lg shadow-sm border border-pink-200';
                        testiDiv.innerHTML = `
                            <p class="italic text-gray-700">"${t.text}"</p>
                            <div class="mt-2 text-sm text-right font-medium text-dark-accent">
                                - ${t.name || 'Anonim'} <span class="text-xs text-gray-400">(${dateString})</span>
                            </div>
                        `;
                        testimonialsList.appendChild(testiDiv);
                    });
                }, (error) => {
                    console.error("Error loading testimonials:", error);
                });
            }
        }


        // --- UI & HELPER FUNCTIONS ---

        // Fungsi Helper: Format Rupiah
        function formatRupiah(number) {
            return new Intl.NumberFormat('id-ID', {
                style: 'currency',
                currency: 'IDR',
                minimumFractionDigits: 0
            }).format(number);
        }

        // Fungsi Utama: Memperbarui tampilan keranjang dan order form
        function updateCartUI() {
            const cartCountElement = document.getElementById('cart-count');
            const cartItemsContainer = document.getElementById('cart-items');
            const cartTotalElement = document.getElementById('cart-total');
            const emptyMessage = document.getElementById('empty-cart-message');

            const orderSummaryList = document.getElementById('order-summary-list');
            const orderTotalElement = document.getElementById('order-total');
            const emptyOrderMessage = document.getElementById('empty-order-message');

            // Hitung total dan jumlah item
            const total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);
            const count = cart.reduce((sum, item) => sum + item.quantity, 0);

            // 1. Update Header Cart Icon
            cartCountElement.textContent = count;

            // 2. Update Cart Modal
            cartItemsContainer.innerHTML = '';
            if (cart.length === 0) {
                emptyMessage.classList.remove('hidden');
                cartTotalElement.textContent = formatRupiah(0);
                cartItemsContainer.appendChild(emptyMessage);
            } else {
                emptyMessage.classList.add('hidden');
                cart.forEach(item => {
                    const itemElement = document.createElement('div');
                    itemElement.className = 'flex justify-between items-center text-gray-700 border-b pb-2';
                    itemElement.innerHTML = `
                        <div>
                            <span class="font-semibold">${item.name}</span>
                            <span class="text-sm text-gray-500"> (${formatRupiah(item.price)})</span>
                        </div>
                        <div class="flex items-center space-x-3">
                            <span class="font-bold text-pink-500">${item.quantity}x</span>
                            <button onclick="window.removeFromCart(${item.id})" class="text-red-500 hover:text-red-700 text-lg">
                                <i class="fas fa-trash"></i>
                            </button>
                        </div>
                    `;
                    cartItemsContainer.appendChild(itemElement);
                });
                cartTotalElement.textContent = formatRupiah(total);
            }

            // 3. Update Order Form Summary
            orderSummaryList.innerHTML = '';
            if (cart.length === 0) {
                emptyOrderMessage.classList.remove('hidden');
                orderTotalElement.textContent = formatRupiah(0);
                orderSummaryList.appendChild(emptyOrderMessage);
            } else {
                emptyOrderMessage.classList.add('hidden');
                cart.forEach(item => {
                    const listItem = document.createElement('p');
                    listItem.className = 'flex justify-between';
                    listItem.innerHTML = `
                        <span class="font-medium">${item.name} (${item.quantity}x)</span>
                        <span class="font-semibold text-dark-accent">${formatRupiah(item.price * item.quantity)}</span>
                    `;
                    orderSummaryList.appendChild(listItem);
                });
                orderTotalElement.textContent = formatRupiah(total);
            }
        }

        // Fungsi: Tambah Produk ke Keranjang (Updates local cart then syncs to Firestore)
        window.addToCart = function(product) {
            const existingItem = cart.find(item => item.id === product.id);
            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                cart.push({ ...product, quantity: 1 });
            }
            updateCartInFirestore();
            showNotification(`"${product.name}" ditambahkan ke keranjang!`);
        }

        // Fungsi: Hapus Item dari Keranjang (Updates local cart then syncs to Firestore)
        window.removeFromCart = function(productId) {
            const itemIndex = cart.findIndex(item => item.id === productId);
            if (itemIndex > -1) {
                // Hapus item dari keranjang lokal
                cart.splice(itemIndex, 1);
            }
            updateCartInFirestore();
            showNotification(`Item berhasil dihapus dari keranjang.`);
        }

        // Fungsi: Toggle Menu Mobile
        window.toggleMobileMenu = function() {
            const menu = document.getElementById('mobile-menu');
            const icon = document.getElementById('menu-icon');
            const isOpen = menu.classList.toggle('translate-y-0');

            if (isOpen) {
                 // Open state
                menu.classList.remove('-translate-y-full', 'opacity-0');
                menu.classList.add('opacity-100');
                icon.classList.remove('fa-bars');
                icon.classList.add('fa-times'); // Change to close icon
            } else {
                // Close state
                menu.classList.add('-translate-y-full', 'opacity-0');
                menu.classList.remove('opacity-100');
                icon.classList.remove('fa-times');
                icon.classList.add('fa-bars'); // Change back to hamburger icon
            }
        }

        // Fungsi: Modal Logic (Open/Close)
        window.openModal = function(id) {
            const modal = document.getElementById(id);
            if (modal) {
                modal.classList.remove('hidden');
                setTimeout(() => modal.classList.remove('opacity-0'), 10);
                updateCartUI(); // Pastikan keranjang di-update saat dibuka
            }
        }
        window.closeModal = function(id) {
            const modal = document.getElementById(id);
            if (modal) {
                modal.classList.add('opacity-0');
                setTimeout(() => modal.classList.add('hidden'), 300);
            }
        }

        // Fungsi: Notifikasi (Pengganti alert())
        function showNotification(message) {
            let notification = document.getElementById('custom-notification');
            if (!notification) return; // Should not happen as it's defined in HTML

            notification.textContent = message;
            notification.classList.remove('translate-x-full');
            notification.classList.add('translate-x-0');
            
            clearTimeout(notification.timer);
            notification.timer = setTimeout(() => {
                notification.classList.remove('translate-x-0');
                notification.classList.add('translate-x-full');
            }, 3000);
        }

        // Event Listener untuk Formulir Testimoni
        document.getElementById('testimonial-form').addEventListener('submit', async function(e) {
            e.preventDefault();
            const nameInput = document.getElementById('testi-name').value.trim();
            const textInput = document.getElementById('testi-text').value.trim();
            const messageElement = document.getElementById('testi-message');
            const submitBtn = document.getElementById('submit-testi-btn');

            if (!textInput) {
                showNotification("Testimoni tidak boleh kosong.");
                return;
            }

            if (!isAuthReady) {
                 showNotification("Database belum siap. Coba lagi sebentar.");
                 return;
            }

            submitBtn.disabled = true;
            submitBtn.textContent = 'Mengirim...';

            const testiCollectionRef = getTestimonialsCollectionRef();
            if (testiCollectionRef) {
                try {
                    await addDoc(testiCollectionRef, {
                        name: nameInput || 'Anonim',
                        text: textInput,
                        timestamp: serverTimestamp(),
                        userId: userId
                    });
                    
                    messageElement.classList.remove('hidden');
                    document.getElementById('testi-name').value = '';
                    document.getElementById('testi-text').value = '';

                } catch (error) {
                    console.error("Error adding testimonial:", error);
                    showNotification("Gagal mengirim testimoni. Coba lagi.");
                    messageElement.classList.add('hidden');
                } finally {
                    submitBtn.disabled = false;
                    submitBtn.textContent = 'Kirim Testimoni';
                    // Hide success message after a few seconds
                    setTimeout(() => messageElement.classList.add('hidden'), 5000); 
                }
            } else {
                showNotification("Database tidak siap. Coba lagi sebentar.");
                submitBtn.disabled = false;
                submitBtn.textContent = 'Kirim Testimoni';
            }
        });

        // Event Listener untuk Formulir Pemesanan
        document.getElementById('order-form').addEventListener('submit', async function(e) {
            e.preventDefault();
            
            if (cart.length === 0) {
                showNotification("Keranjangmu kosong, tidak bisa membuat pesanan!");
                return;
            }

            const name = document.getElementById('order-name').value.trim();
            const address = document.getElementById('order-address').value.trim();
            const phone = document.getElementById('order-phone').value.trim();
            const orderMessage = document.getElementById('order-message');
            const submitBtn = e.target.querySelector('button[type="submit"]');

            if (!name || !address || !phone) {
                showNotification("Mohon lengkapi semua data pemesanan (Nama, Alamat, Telepon).");
                return;
            }

            if (!isAuthReady) {
                showNotification("Database belum siap. Coba lagi sebentar.");
                return;
            }

            submitBtn.disabled = true;
            submitBtn.textContent = 'Memproses Pesanan...';

            const ordersCollectionRef = getOrdersCollectionRef();
            const total = cart.reduce((sum, item) => sum + item.price * item.quantity, 0);

            if (ordersCollectionRef) {
                try {
                    const orderData = {
                        customerName: name,
                        customerAddress: address,
                        customerPhone: phone,
                        items: cart,
                        totalAmount: total,
                        orderDate: serverTimestamp(),
                        userId: userId,
                        status: 'Pending Payment'
                    };
                    
                    // 1. Simpan pesanan ke koleksi public/pinky_orders
                    await addDoc(ordersCollectionRef, orderData);

                    // 2. Hapus keranjang setelah pesanan berhasil
                    cart = [];
                    await updateCartInFirestore(); // Ini akan mengosongkan keranjang di Firestore dan memicu updateCartUI
                    
                    // 3. Tampilkan pesan sukses dan reset form
                    orderMessage.classList.remove('hidden');
                    document.getElementById('order-form').reset();
                    
                    showNotification("Pesanan berhasil dikonfirmasi!");

                } catch (error) {
                    console.error("Error confirming order:", error);
                    showNotification("Gagal menyimpan pesanan. Coba lagi.");
                    orderMessage.classList.add('hidden');
                } finally {
                    submitBtn.disabled = false;
                    submitBtn.textContent = 'Konfirmasi Pesanan Selesai';
                    // Sembunyikan pesan sukses setelah 8 detik
                    setTimeout(() => orderMessage.classList.add('hidden'), 8000); 
                }
            } else {
                showNotification("Database tidak siap. Coba lagi sebentar.");
                submitBtn.disabled = false;
                submitBtn.textContent = 'Konfirmasi Pesanan Selesai';
            }
        });
    </script>
</body>
</html>
