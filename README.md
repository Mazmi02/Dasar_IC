<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Komponen Elektronik Terpadu Dalam Satu Chip." },
  { "en": "Siapa Penemu IC (Integrated Circuit)?", "id": "Jack Kilby Dan Robert Noyce." },
  { "en": "Apa Bahan Utama Pembuatan IC (Integrated Circuit)?", "id": "Semikonduktor Seperti Silikon Murni." },
  { "en": "Apa Komponen Utama Dalam IC (Integrated Circuit)?", "id": "Transistor, Resistor, Kapasitor, Dioda." },
  { "en": "Apa Fungsi Transistor Di IC (Integrated Circuit)?", "id": "Sebagai Saklar Dan Penguat Sinyal." },
  { "en": "Apa Itu Wafer Produksi IC (Integrated Circuit)?", "id": "Lempengan Tipis Bahan Semikonduktor." },
  { "en": "Apa Itu SSI (Small Scale Integration)?", "id": "Integrasi Skala Kecil." },
  { "en": "Berapa Gerbang Logika SSI (Small Scale Integration)?", "id": "Kurang Dari Sepuluh Gerbang Logika." },
  { "en": "Apa Itu MSI (Medium Scale Integration)?", "id": "Integrasi Skala Menengah." },
  { "en": "Berapa Gerbang Logika MSI (Medium Scale Integration)?", "id": "Sepuluh Hingga Seratus Gerbang Logika." },
  { "en": "Apa Itu LSI (Large Scale Integration)?", "id": "Integrasi Skala Besar." },
  { "en": "Berapa Gerbang Logika LSI (Large Scale Integration)?", "id": "Seratus Hingga Ribuan Gerbang Logika." },
  { "en": "Apa Itu VLSI (Very Large Scale Integration)?", "id": "Integrasi Skala Sangat Besar." },
  { "en": "Berapa Gerbang Logika VLSI (Very Large Scale Integration)?", "id": "Ribuan Hingga Jutaan Gerbang Logika." },
  { "en": "Apa Itu ULSI (Ultra Large Scale Integration)?", "id": "Integrasi Skala Ultra Besar." },
  { "en": "Apa Keuntungan Utama IC (Integrated Circuit)?", "id": "Ukuran Kecil, Biaya Rendah, Keandalan Tinggi." },
  { "en": "Apa Itu IC (Integrated Circuit) Monolitik?", "id": "Seluruh Rangkaian Dibuat Pada Satu Chip." },
  { "en": "Apa Itu IC (Integrated Circuit) Hibrida?", "id": "Kombinasi Komponen Diskrit Dan Monolitik." },
  { "en": "Apa Fungsi Kemasan IC (Integrated Circuit)?", "id": "Melindungi Chip Dan Menghubungkan Kaki." },
  { "en": "Apa Kemasan IC (Integrated Circuit) Paling Umum?", "id": "DIP (Dual In-line Package)." },
  { "en": "Apa Itu DIP (Dual In-line Package)?", "id": "Kemasan (Package) Dua Baris." },
  { "en": "Apa Itu Fabrikasi IC (Integrated Circuit)?", "id": "Proses Pembuatan Rangkaian Terpadu." },
  { "en": "Apa Itu Fotolitografi Fabrikasi IC (Integrated Circuit)?", "id": "Proses Pencetakan Pola Rangkaian." },
  { "en": "Apa Itu Doping Dalam Semikonduktor?", "id": "Penambahan Atom Asing Mengubah Sifat." },
  { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Semikonduktor Dengan Kelebihan Lubang (Hole)." },
  { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Semikonduktor Dengan Kelebihan Elektron." },
  { "en": "Apa Itu Dioda Pada IC (Integrated Circuit)?", "id": "Komponen Pengalir Arus Satu Arah." },
  { "en": "Apa Itu Resistor Pada IC (Integrated Circuit)?", "id": "Komponen Penghambat Aliran Arus Listrik." },
  { "en": "Apa Itu Kapasitor Pada IC (Integrated Circuit)?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Apa Itu IC (Integrated Circuit) Digital?", "id": "IC Yang Bekerja Berdasarkan Sinyal Digital." },
  { "en": "Apa Itu IC (Integrated Circuit) Analog?", "id": "IC Yang Bekerja Berdasarkan Sinyal Analog." },
  { "en": "Apa Contoh IC (Integrated Circuit) Digital?", "id": "Mikroprosesor, Memori, Gerbang Logika." },
  { "en": "Apa Contoh IC (Integrated Circuit) Analog?", "id": "Op-Amp, Regulator Tegangan." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "Penguat Operasional." },
  { "en": "Fungsi IC (Integrated Circuit) Regulator Tegangan?", "id": "Menjaga Tegangan Output Tetap Stabil." },
  { "en": "Apa Itu IC (Integrated Circuit) Linear?", "id": "Nama Lain Untuk IC Analog." },
  { "en": "Apa Itu Substrat IC (Integrated Circuit)?", "id": "Bahan Dasar Tempat Komponen Dibuat." },
  { "en": "Apa Itu Die IC (Integrated Circuit)?", "id": "Potongan Kecil Wafer Berisi Rangkaian." },
  { "en": "Apa Itu Bonding Kawat (Wire Bonding)?", "id": "Proses Menghubungkan Die Ke Kaki Kemasan." },
  { "en": "Apa Itu Yield Produksi IC (Integrated Circuit)?", "id": "Persentase Chip Fungsional Per Wafer." },
  { "en": "Apa Itu Cleanroom (Ruang Bersih)?", "id": "Ruangan Produksi IC Bebas Debu." },
  { "en": "Mengapa Produksi IC (Integrated Circuit) Perlu Cleanroom?", "id": "Debu Dapat Merusak Rangkaian Sangat Kecil." },
  { "en": "Apa Itu Skala Integrasi IC (Integrated Circuit)?", "id": "Ukuran Kepadatan Komponen Dalam Satu Chip." },
  { "en": "Apa Itu Hukum Moore?", "id": "Jumlah Transistor Berlipat Ganda Setiap Dua Tahun." },
  { "en": "Siapa Penggagas Hukum Moore?", "id": "Gordon Moore, Salah Satu Pendiri Intel." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Sebutkan Tiga Gerbang Logika Dasar?", "id": "AND, OR, Dan NOT." },
  { "en": "Apa Itu IC TTL (Transistor-Transistor Logic)?", "id": "Logika (Logic) Transistor-Transistor." },
  { "en": "Apa Itu IC CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Semikonduktor Oksida Logam Komplementer." },
  { "en": "Kelebihan IC CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Konsumsi Daya Jauh Lebih Rendah." },
  { "en": "Apa Itu Mikroprosesor?", "id": "IC (Integrated Circuit) Sebagai CPU Komputer." },
  { "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Unit Pemroses Sentral." },
  { "en": "Apa Itu Mikrokontroler?", "id": "IC (Integrated Circuit) Berisi CPU, Memori, I/O." },
  { "en": "Apa Itu I/O (Input/Output)?", "id": "Masukan (Input) Dan Keluaran (Output)." },
  { "en": "Perbedaan Mikroprosesor Dan Mikrokontroler?", "id": "Mikrokontroler Adalah Sistem Komputer Lengkap." },
  { "en": "Apa Itu IC (Integrated Circuit) Memori?", "id": "IC Yang Digunakan Untuk Menyimpan Data." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Akses Acak." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Hanya Baca." },
  { "en": "Apa Sifat Memori RAM (Random Access Memory)?", "id": "Volatile, Data Hilang Saat Listrik Mati." },
  { "en": "Apa Sifat Memori ROM (Read Only Memory)?", "id": "Non-Volatile, Data Tetap Ada." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis." },
  { "en": "Mengapa ESD (Electrostatic Discharge) Berbahaya Bagi IC?", "id": "Dapat Merusak Komponen Internal Sensitif." },
  { "en": "Apa Itu Heatsink Pada IC (Integrated Circuit)?", "id": "Komponen Logam Untuk Membuang Panas." },
  { "en": "Mengapa IC (Integrated Circuit) Perlu Heatsink?", "id": "Karena Menghasilkan Panas Berlebih Saat Bekerja." },
  { "en": "Apa Itu Pin IC (Integrated Circuit)?", "id": "Kaki-Kaki Logam Untuk Koneksi Eksternal." },
  { "en": "Apa Itu Datasheet IC (Integrated Circuit)?", "id": "Dokumen Spesifikasi Teknis Lengkap IC." },
  { "en": "Informasi Apa Di Datasheet IC (Integrated Circuit)?", "id": "Fungsi Pin, Tegangan Kerja, Karakteristik." },
  { "en": "Apa Itu IC (Integrated Circuit) Timer 555?", "id": "IC Populer Untuk Pewaktu Dan Osilator." },
  { "en": "Apa Itu Osilator?", "id": "Rangkaian Yang Menghasilkan Sinyal Berulang." },
  { "en": "Apa Itu PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Tercetak." },
  { "en": "Fungsi PCB (Printed Circuit Board)?", "id": "Papan Tempat Komponen Elektronik Disolder." },
  { "en": "Bagaimana IC (Integrated Circuit) Dipasang Di PCB?", "id": "Melalui Lubang (Through-Hole) Atau Permukaan (SMD)." },
  { "en": "Apa Itu SMD (Surface Mount Device)?", "id": "Perangkat Pemasangan Permukaan." },
  { "en": "Keuntungan Teknologi SMD (Surface Mount Device)?", "id": "Ukuran Lebih Kecil, Kepadatan Komponen Tinggi." },
  { "en": "Apa Itu Etsa (Etching) Pada PCB (Printed Circuit Board)?", "id": "Proses Menghilangkan Tembaga Yang Tidak Diinginkan." },
  { "en": "Apa Itu Solder?", "id": "Logam Campuran Untuk Menyatukan Komponen." },
  { "en": "Apa Itu Desoldering?", "id": "Proses Melepas Komponen Yang Telah Disolder." },
  { "en": "Apa Itu PMIC (Power Management Integrated Circuit)?", "id": "IC (Integrated Circuit) Manajemen Daya." },
  { "en": "Nama Lain PMIC (Power Management Integrated Circuit)?", "id": "IC (Integrated Circuit) Power Management." },
  { "en": "Di Mana PMIC (Power Management Integrated Circuit) Ditemukan?", "id": "Di Ponsel Pintar, Laptop, Perangkat Portabel." },
  { "en": "Apa Itu ASIC (Application-Specific Integrated Circuit)?", "id": "IC (Integrated Circuit) Aplikasi Spesifik." },
  { "en": "Apa Ciri Khas ASIC (Application-Specific Integrated Circuit)?", "id": "Didesain Khusus Untuk Satu Fungsi Saja." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "Larik Gerbang Terprogram Lapangan." },
  { "en": "Kelebihan FPGA (Field-Programmable Gate Array) Dibanding ASIC?", "id": "Dapat Diprogram Ulang Oleh Pengguna." },
  { "en": "Apa Itu SoC (System on a Chip)?", "id": "Sistem Dalam Satu Chip." },
  { "en": "Apa Maksud SoC (System on a Chip)?", "id": "Seluruh Komponen Sistem Terintegrasi Satu Chip." },
  { "en": "Apa Contoh SoC (System on a Chip)?", "id": "Prosesor Ponsel Pintar Modern." },
  { "en": "Apa Itu Nanometer Di IC (Integrated Circuit)?", "id": "Satuan Ukuran Proses Fabrikasi Transistor." },
  { "en": "Arti Proses IC (Integrated Circuit) 7nm?", "id": "Ukuran Fitur Transistor 7 Nanometer." },
  { "en": "Mengapa Ukuran Nanometer Semakin Kecil?", "id": "Agar Lebih Banyak Transistor Muat." },
  { "en": "Apa Itu Silikon Dioksida (SiO2)?", "id": "Bahan Isolator Penting Dalam IC." },
  { "en": "Apa Fungsi Silikon Dioksida (SiO2)?", "id": "Sebagai Isolator Gerbang (Gate) Transistor." },
  { "en": "Apa Itu Amplifier?", "id": "Rangkaian Untuk Menguatkan Sinyal Listrik." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Dengan Dua Nilai, Nol Atau Satu." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Yang Berubah Seiring Waktu." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Fungsi ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Dunia Nyata Ke Digital." },
  { "en": "Apa Fungsi DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Menjadi Sinyal Analog." },
  { "en": "Apa Itu Kemasan IC (Integrated Circuit) Plastik?", "id": "Kemasan Paling Umum Dan Murah." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Transistor Pengendali Tegangan Medan Listrik." },
  { "en": "Sebutkan Tiga Terminal MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor)?", "id": "Gate (Gerbang), Drain (Saluran), Source (Sumber)." },
  { "en": "Apa Fungsi Gate (Gerbang) Pada MOSFET?", "id": "Mengontrol Aliran Arus Saluran (Channel)." },
  { "en": "Apa Itu Saluran (Channel) MOSFET?", "id": "Area Aliran Arus Antara Drain Dan Source." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Tipe N-Channel?", "id": "Saluran (Channel) Terbuat Dari Elektron." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Tipe P-Channel?", "id": "Saluran (Channel) Terbuat Dari Lubang (Hole)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Mode Enhancement?", "id": "Transistor Mati (Off) Tanpa Tegangan Gate." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Mode Depletion?", "id": "Transistor Nyala (On) Tanpa Tegangan Gate." },
  { "en": "Apa Itu Photomask (Masker Foto)?", "id": "Pola Rangkaian Untuk Proses Litografi." },
  { "en": "Apa Itu Deposisi (Deposition) Dalam Fabrikasi?", "id": "Proses Penambahan Lapisan Material Tipis." },
  { "en": "Apa Itu CVD (Chemical Vapor Deposition)?", "id": "Deposisi (Deposition) Menggunakan Reaksi Kimia Gas." },
  { "en": "Apa Itu PVD (Physical Vapor Deposition)?", "id": "Deposisi (Deposition) Menggunakan Metode Fisik." },
  { "en": "Apa Itu Etsa Basah (Wet Etching)?", "id": "Proses Etsa Menggunakan Bahan Kimia Cair." },
  { "en": "Apa Itu Etsa Kering (Dry Etching)?", "id": "Proses Etsa Menggunakan Gas Plasma." },
  { "en": "Apa Itu Implantasi Ion (Ion Implantation)?", "id": "Proses Doping Menggunakan Tembakan Ion." },
  { "en": "Apa Itu Annealing (Pelianinan) Fabrikasi IC?", "id": "Proses Pemanasan Untuk Memperbaiki Kristal." },
  { "en": "Apa Itu Metallization (Metalisasi)?", "id": "Proses Pembentukan Jalur Logam Interkoneksi." },
  { "en": "Logam Apa Untuk Metallization (Metalisasi) IC?", "id": "Aluminium Atau Tembaga (Copper)." },
  { "en": "Apa Itu SOP (Small Outline Package)?", "id": "Kemasan (Package) SMD Dengan Kaki Samping." },
  { "en": "Apa Itu QFP (Quad Flat Package)?", "id": "Kemasan (Package) Kaki Rata Empat Sisi." },
  { "en": "Apa Itu BGA (Ball Grid Array)?", "id": "Kemasan (Package) Menggunakan Bola Solder Bawah." },
  { "en": "Apa Itu SIP (Single In-line Package)?", "id": "Kemasan (Package) Kaki Satu Baris." },
  { "en": "Apa Itu PLCC (Plastic Leaded Chip Carrier)?", "id": "Kemasan (Package) Plastik Kaki Huruf J." },
  { "en": "Apa Itu Fan-Out Dalam Logika Digital?", "id": "Jumlah Maksimal Input Yang Dapat Digerakkan." },
  { "en": "Apa Itu Fan-In Dalam Logika Digital?", "id": "Jumlah Maksimal Input Pada Satu Gerbang." },
  { "en": "Apa Itu Waktu Tunda Propagasi (Propagation Delay)?", "id": "Waktu Sinyal Pindah Input Ke Output." },
  { "en": "Apa Itu Noise Margin (Batas Kebisingan)?", "id": "Toleransi Rangkaian Terhadap Gangguan Sinyal (Noise)." },
  { "en": "Apa Itu SRAM (Static Random Access Memory)?", "id": "RAM (Random Access Memory) Yang Menggunakan Flip-Flop." },
  { "en": "Apa Itu DRAM (Dynamic Random Access Memory)?", "id": "RAM (Random Access Memory) Yang Menggunakan Kapasitor." },
  { "en": "Mengapa DRAM (Dynamic RAM) Perlu Refresh?", "id": "Karena Muatan Kapasitor Bocor Seiring Waktu." },
  { "en": "Apa Itu Flip-Flop?", "id": "Rangkaian Penyimpan Data Satu Bit." },
  { "en": "Apa Itu EEPROM (Electrically Erasable Programmable ROM)?", "id": "ROM (Read Only Memory) Yang Dapat Dihapus Listrik." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Tipe EEPROM (Electrically Erasable Programmable ROM) Cepat." },
  { "en": "Di Mana Memori Flash Biasa Digunakan?", "id": "USB (Universal Serial Bus) Drive, SSD (Solid State Drive)." },
  { "en": "Apa Itu SSD (Solid State Drive)?", "id": "Perangkat Penyimpanan Data Berbasis IC (Integrated Circuit)." },
  { "en": "Apa Itu Latch-Up Pada IC (Integrated Circuit) CMOS?", "id": "Kondisi Hubung Singkat Internal Akibat Parasitik." },
  { "en": "Apa Itu Elektromigrasi (Electromigration)?", "id": "Pergerakan Atom Logam Akibat Arus Tinggi." },
  { "en": "Apa Akibat Elektromigrasi (Electromigration)?", "id": "Jalur Logam Interkoneksi Putus Atau Rusak." },
  { "en": "Apa Itu Stres Termal (Thermal Stress)?", "id": "Tekanan Akibat Perubahan Suhu Ekstrem." },
  { "en": "Apa Itu ECL (Emitter-Coupled Logic)?", "id": "Keluarga Logika (Logic Family) Sangat Cepat." },
  { "en": "Apa Kerugian ECL (Emitter-Coupled Logic)?", "id": "Konsumsi Daya Sangat Tinggi Boros." },
  { "en": "Apa Itu BiCMOS (Bipolar CMOS)?", "id": "Kombinasi Teknologi Bipolar (BJT) Dan CMOS." },
  { "en": "Apa Keuntungan BiCMOS (Bipolar CMOS)?", "id": "Kecepatan Tinggi (BJT) Daya Rendah (CMOS)." },
  { "en": "Apa Itu Optocoupler (Penggandeng Optik)?", "id": "IC (Integrated Circuit) Isolasi Sinyal Pakai Cahaya." },
  { "en": "Komponen Apa Dalam Optocoupler?", "id": "LED (Light Emitting Diode) Dan Fototransistor." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya Saat Dialiri Arus." },
  { "en": "Apa Itu Fototransistor?", "id": "Transistor Yang Dikendalikan Oleh Cahaya." },
  { "en": "Apa Itu RFIC (Radio Frequency Integrated Circuit)?", "id": "IC (Integrated Circuit) Untuk Sinyal Frekuensi Radio." },
  { "en": "Di Mana RFIC (Radio Frequency IC) Digunakan?", "id": "Ponsel, Wi-Fi, Bluetooth, GPS (Global Positioning System)." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Navigasi Global Berbasis Satelit." },
  { "en": "Apa Itu IC (Integrated Circuit) Sensor?", "id": "IC (Integrated Circuit) Yang Mendeteksi Besaran Fisik." },
  { "en": "Apa Contoh IC (Integrated Circuit) Sensor?", "id": "Sensor Suhu, Sensor Tekanan, Sensor Cahaya." },
  { "en": "Bagaimana Resistor Dibuat Pada IC (Integrated Circuit)?", "id": "Menggunakan Lapisan Polisilicon Atau Difusi." },
  { "en": "Bagaimana Kapasitor Dibuat Pada IC (Integrated Circuit)?", "id": "Menggunakan Struktur MOS (Metal-Oxide-Semiconductor) Atau Sambungan PN." },
  { "en": "Bagaimana Dioda Dibuat Pada IC (Integrated Circuit)?", "id": "Menggunakan Sambungan (Junction) PN Tipe-P Dan Tipe-N." },
  { "en": "Apa Itu Polisilicon (Polycrystalline Silicon)?", "id": "Silikon Dengan Struktur Kristal Acak." },
  { "en": "Apa Itu Epitaksi (Epitaxy)?", "id": "Pertumbuhan Lapisan Kristal Tunggal Teratur." },
  { "en": "Apa Itu CMP (Chemical Mechanical Polishing)?", "id": "Proses Perataan Permukaan Wafer (Polishing)." },
  { "en": "Mengapa CMP (Chemical Mechanical Polishing) Penting?", "id": "Untuk Membangun Banyak Lapisan (Multilayer) Rata." },
  { "en": "Apa Itu Oksidasi (Oxidation) Fabrikasi IC?", "id": "Proses Penumbuhan Lapisan Silikon Dioksida (SiO2)." },
  { "en": "Apa Itu Sputtering?", "id": "Salah Satu Metode PVD (Physical Vapor Deposition)." },
  { "en": "Apa Itu Doping Tipe-N?", "id": "Menambahkan Dopant Pentavalen (Contoh: Fosfor)." },
  { "en": "Apa Itu Doping Tipe-P?", "id": "Menambahkan Dopant Trivalen (Contoh: Boron)." },
  { "en": "Apa Itu IC Mixed-Signal (Sinyal Campuran)?", "id": "IC (Integrated Circuit) Gabungan Analog Dan Digital." },
  { "en": "Mengapa IC Mixed-Signal (Sinyal Campuran) Dibutuhkan?", "id": "Menghubungkan Sinyal Dunia Nyata Ke Digital." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Rangkaian Sinkronisasi Sinyal (Frekuensi/Fasa)." },
  { "en": "Apa Itu IC (Integrated Circuit) Audio Codec?", "id": "IC (Integrated Circuit) Pengkodean Dan Pendekodean Audio." },
  { "en": "Apa Fungsi Audio Codec?", "id": "Mengubah Audio Analog Ke Digital, Dan Sebaliknya." },
  { "en": "Apa Itu Gerbang NAND?", "id": "Gerbang Logika NOT (Bukan) AND (Dan)." },
  { "en": "Apa Itu Gerbang NOR?", "id": "Gerbang Logika NOT (Bukan) OR (Atau)." },
  { "en": "Apa Itu Gerbang XOR (Exclusive OR)?", "id": "Output Tinggi Jika Input Berbeda." },
  { "en": "Apa Itu Gerbang XNOR (Exclusive NOR)?", "id": "Output Tinggi Jika Input Sama." },
  { "en": "Mengapa NAND Dan NOR Gerbang Universal?", "id": "Dapat Membentuk Semua Gerbang Logika Lainnya." },
  { "en": "Apa Itu Desain Fisik (Physical Design)?", "id": "Proses Menggambar Tata Letak (Layout) Transistor." },
  { "en": "Apa Itu Verifikasi Desain (Design Verification)?", "id": "Proses Memastikan Desain IC (Integrated Circuit) Benar." },
  { "en": "Apa Itu EDA (Electronic Design Automation)?", "id": "Perangkat Lunak (Software) Untuk Mendesain IC." },
  { "en": "Apa Itu Tape-Out?", "id": "Desain IC (Integrated Circuit) Selesai, Siap Produksi." },
  { "en": "Apa Itu ATE (Automated Test Equipment)?", "id": "Mesin Penguji IC (Integrated Circuit) Otomatis." },
  { "en": "Mengapa Pengujian IC (Integrated Circuit) Penting?", "id": "Memastikan Hanya Chip Fungsional Yang Dijual." },
  { "en": "Apa Itu BJT (Bipolar Junction Transistor)?", "id": "Transistor Pengendali Arus (Current-Controlled)." },
  { "en": "Apa Beda BJT (Bipolar) Dan MOSFET?", "id": "BJT (Bipolar) Dikontrol Arus, MOSFET Tegangan." },
  { "en": "Sebutkan Tiga Terminal BJT (Bipolar Junction Transistor)?", "id": "Emitor (Emitter), Basis (Base), Kolektor (Collector)." },
  { "en": "Apa Itu BJT (Bipolar Junction Transistor) Tipe NPN?", "id": "Terdiri Atas Lapisan N-P-N." },
  { "en": "Apa Itu BJT (Bipolar Junction Transistor) Tipe PNP?", "id": "Terdiri Atas Lapisan P-N-P." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit)?", "id": "Padanan Istilah Indonesia Untuk IC (Integrated Circuit)." },
  { "en": "Apa Itu Semikonduktor Intrinsik?", "id": "Semikonduktor Murni Tanpa Doping." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik?", "id": "Semikonduktor Yang Telah Diberi Doping." },
  { "en": "Apa Itu Catu Daya (Power Supply)?", "id": "Sumber Tenaga Listrik Untuk Rangkaian." },
  { "en": "Apa Itu VCC Pada Datasheet IC?", "id": "Pin Catu Daya Positif Untuk Logika TTL." },
  { "en": "Apa Itu VDD Pada Datasheet IC?", "id": "Pin Catu Daya Positif Untuk Logika CMOS." },
  { "en": "Apa Itu VSS Pada Datasheet IC?", "id": "Pin Catu Daya Negatif Untuk Logika CMOS." },
  { "en": "Apa Itu GND (Ground) Pada Datasheet IC?", "id": "Pin Referensi Tegangan Nol (Nol Volt)." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Kecil Saat Transistor Seharusnya Mati." },
  { "en": "Apa Itu Disipasi Daya (Power Dissipation)?", "id": "Energi Listrik Yang Berubah Menjadi Panas." },
  { "en": "Apa Satuan Disipasi Daya (Power Dissipation)?", "id": "Watt (W) Atau Milliwatt (mW)." },
  { "en": "Apa Itu Resistansi On (On-Resistance) Transistor?", "id": "Hambatan Transistor Saat Kondisi Menyala (On)." },
  { "en": "Apa Itu Tegangan Tembus (Breakdown Voltage)?", "id": "Tegangan Maksimal Sebelum Komponen Rusak." },
  { "en": "Apa Itu Kapasitansi Parasitik?", "id": "Kapasitansi Tidak Diinginkan Dalam Rangkaian IC." },
  { "en": "Mengapa Kapasitansi Parasitik Berdampak?", "id": "Dapat Memperlambat Kecepatan Rangkaian (Sirkuit)." },
  { "en": "Apa Itu Level Logika (Logic Level)?", "id": "Rentang Tegangan Untuk Logika 0 Dan 1." },
  { "en": "Apa Itu Logika Positif?", "id": "Logika 1 Tegangan Tinggi, Logika 0 Rendah." },
  { "en": "Apa Itu Semikonduktor?", "id": "Bahan Antara Konduktor Dan Isolator." },
  { "en": "Apa Itu Celah Pita (Band Gap)?", "id": "Energi Pemisah Pita Valensi Konduksi." },
  { "en": "Apa Itu Pita Valensi (Valence Band)?", "id": "Pita Energi Terisi Elektron Valensi." },
  { "en": "Apa Itu Pita Konduksi (Conduction Band)?", "id": "Pita Energi Elektron Bebas Bergerak." },
  { "en": "Apa Itu Pembawa Muatan (Charge Carrier)?", "id": "Elektron (Electron) Dan Lubang (Hole)." },
  { "en": "Apa Itu Lubang (Hole) Semikonduktor?", "id": "Kekosongan Elektron Pada Pita Valensi." },
  { "en": "Apa Itu Rekombinasi (Recombination)?", "id": "Elektron (Electron) Mengisi Lubang (Hole)." },
  { "en": "Apa Itu Arus Difusi (Diffusion Current)?", "id": "Aliran Muatan Akibat Perbedaan Konsentrasi." },
  { "en": "Apa Itu Arus Hanyut (Drift Current)?", "id": "Aliran Muatan Akibat Medan Listrik." },
  { "en": "Apa Itu Sambungan PN (PN Junction)?", "id": "Batas Antara Semikonduktor Tipe-P Tipe-N." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Area Sambungan PN Tanpa Muatan Bebas." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Tegangan Positif Ke P, Negatif Ke N." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Tegangan Negatif Ke P, Positif Ke N." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda Yang Bekerja Pada Bias Mundur." },
  { "en": "Apa Fungsi Dioda Zener?", "id": "Sebagai Referensi Atau Regulator Tegangan." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Sambungan (Junction) Logam-Semikonduktor." },
  { "en": "Apa Kelebihan Dioda Schottky?", "id": "Tegangan Maju Rendah, Switching Cepat." },
  { "en": "Apa Itu Keluarga Logika (Logic Family)?", "id": "Grup Gerbang Logika Teknologi Sama." },
  { "en": "Apa Itu RTL (Resistor-Transistor Logic)?", "id": "Keluarga Logika (Logic) Pakai Resistor Transistor." },
  { "en": "Apa Itu DTL (Diode-Transistor Logic)?", "id": "Keluarga Logika (Logic) Pakai Dioda Transistor." },
  { "en": "Apa Itu HTL (High-Threshold Logic)?", "id": "Logika (Logic) Untuk Lingkungan Kebisingan Tinggi." },
  { "en": "Apa Itu IIL (Integrated Injection Logic) (I2L)?", "id": "Keluarga Logika (Logic) Bipolar Kepadatan Tinggi." },
  { "en": "Apa Itu Level Logika High (Tinggi)?", "id": "Rentang Tegangan Yang Mewakili Logika 1." },
  { "en": "Apa Itu Level Logika Low (Rendah)?", "id": "Rentang Tegangan Yang Mewakili Logika 0." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Sinyal Naik 10% Ke 90%." },
  { "en": "Apa Itu Waktu Turun (Fall Time)?", "id": "Waktu Sinyal Turun 90% Ke 10%." },
  { "en": "Apa Itu Sinyal Jam (Clock Signal)?", "id": "Sinyal Periodik Untuk Sinkronisasi Rangkaian." },
  { "en": "Apa Itu Frekuensi Jam (Clock Frequency)?", "id": "Kecepatan Sinyal Jam (Clock Signal) Berosilasi." },
  { "en": "Apa Satuan Frekuensi Jam (Clock Frequency)?", "id": "Hertz (Hz), Megahertz (MHz), Gigahertz (GHz)." },
  { "en": "Apa Itu Siklus Tugas (Duty Cycle)?", "id": "Persentase Waktu Sinyal Dalam Kondisi Tinggi." },
  { "en": "Apa Itu Tepi Naik (Rising Edge)?", "id": "Transisi Sinyal Dari Rendah Ke Tinggi." },
  { "en": "Apa Itu Tepi Turun (Falling Edge)?", "id": "Transisi Sinyal Dari Tinggi Ke Rendah." },
  { "en": "Apa Itu Rangkaian Sekuensial (Sequential Circuit)?", "id": "Rangkaian Digital Dengan Memori (Memory)." },
  { "en": "Apa Itu Rangkaian Kombinasional (Combinational Circuit)?", "id": "Rangkaian Digital Tanpa Memori (Memory)." },
  { "en": "Contoh Rangkaian Kombinasional?", "id": "Encoder, Decoder, Multiplexer, Adder." },
  { "en": "Contoh Rangkaian Sekuensial?", "id": "Flip-Flop, Register, Counter (Penghitung)." },
  { "en": "Apa Itu Register?", "id": "Grup Flip-Flop Penyimpan Data (Word)." },
  { "en": "Apa Itu Register Geser (Shift Register)?", "id": "Register Yang Dapat Menggeser Data." },
  { "en": "Apa Itu Counter (Penghitung)?", "id": "Rangkaian Sekuensial Penghitung Pulsa Jam." },
  { "en": "Apa Itu Counter Sinkron (Synchronous Counter)?", "id": "Semua Flip-Flop Menerima Jam Bersamaan." },
  { "en": "Apa Itu Counter Asinkron (Asynchronous Counter)?", "id": "Flip-Flop Memicu Flip-Flop Berikutnya." },
  { "en": "Apa Itu Ripple Counter?", "id": "Nama Lain Counter Asinkron (Asynchronous Counter)." },
  { "en": "Apa Itu Adder (Penjumlah)?", "id": "Rangkaian Kombinasional Penjumlah Bilangan Biner." },
  { "en": "Apa Itu Half Adder (Penjumlah Setengah)?", "id": "Menjumlahkan Dua Bit (Bit) Tunggal." },
  { "en": "Apa Itu Full Adder (Penjumlah Penuh)?", "id": "Menjumlahkan Tiga Bit (Bit) Tunggal." },
  { "en": "Apa Itu Multiplexer (MUX)?", "id": "Memilih Satu Dari Banyak Input." },
  { "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Mengarahkan Satu Input Ke Banyak Output." },
  { "en": "Apa Itu Encoder (Penyandi)?", "id": "Mengubah Data Non-Biner Ke Kode Biner." },
  { "en": "Apa Itu Decoder (Pengurai Sandi)?", "id": "Mengubah Kode Biner Ke Data Non-Biner." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Rangkaian Pembanding Dua Nilai Biner." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Unit Aritmatika Dan Logika." },
  { "en": "Fungsi ALU (Arithmetic Logic Unit)?", "id": "Melakukan Operasi Aritmatika Dan Logika." },
  { "en": "Di Mana ALU (Arithmetic Logic Unit) Ditemukan?", "id": "Di Dalam CPU (Central Processing Unit)." },
  { "en": "Apa Itu Tristate Buffer (Penyangga Tiga Keadaan)?", "id": "Gerbang Logika Dengan Tiga Status Output." },
  { "en": "Apa Tiga Status Tristate Buffer?", "id": "Tinggi (High), Rendah (Low), Impedansi Tinggi (Hi-Z)." },
  { "en": "Apa Itu Impedansi Tinggi (High Impedance)?", "id": "Status Output Terputus (Saklar Terbuka)." },
  { "en": "Apa Itu Bus Data (Data Bus)?", "id": "Jalur Komunikasi Bersama Transfer Data." },
  { "en": "Mengapa Bus Data (Data Bus) Perlu Tristate?", "id": "Agar Hanya Satu Perangkat Mengirim Data." },
  { "en": "Apa Itu Wafer Dicing (Pemotongan Wafer)?", "id": "Proses Memotong Wafer Menjadi Die." },
  { "en": "Apa Itu Die Attach (Pemasangan Die)?", "id": "Proses Melekatkan Die (Chip) Ke Kemasan." },
  { "en": "Apa Itu Enkapsulasi (Encapsulation)?", "id": "Proses Pembungkusan Chip Dengan Plastik Keramik." },
  { "en": "Tujuan Enkapsulasi (Encapsulation)?", "id": "Melindungi Die (Chip) Dari Lingkungan." },
  { "en": "Apa Itu Proses Planar (Planar Process)?", "id": "Teknik Fabrikasi IC (Integrated Circuit) Lapisan Datar." },
  { "en": "Siapa Penemu Proses Planar (Planar Process)?", "id": "Jean Hoerni." },
  { "en": "Apa Itu Sensor Gambar CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Sensor Kamera Digital Berbasis Teknologi CMOS." },
  { "en": "Apa Itu Sensor CCD (Charge-Coupled Device)?", "id": "Sensor Gambar (Image Sensor) Tipe Lain." },
  { "en": "Apa Itu Memori Non-Volatile?", "id": "Memori Yang Menyimpan Data Tanpa Listrik." },
  { "en": "Apa Itu Memori Volatile?", "id": "Memori Yang Kehilangan Data Tanpa Listrik." },
  { "en": "Apa Itu MRAM (Magnetoresistive Random Access Memory)?", "id": "RAM (Random Access Memory) Non-Volatile Magnetik." },
  { "en": "Apa Itu FeRAM (Ferroelectric Random Access Memory)?", "id": "RAM (Random Access Memory) Non-Volatile Ferroelektrik." },
  { "en": "Apa Itu Resonator Kristal (Crystal Resonator)?", "id": "Komponen Penentu Frekuensi Jam (Clock) Stabil." },
  { "en": "Apa Itu Osilator Kristal (Crystal Oscillator)?", "id": "Osilator (Oscillator) Yang Menggunakan Resonator Kristal." },
  { "en": "Apa Itu Jitter Sinyal Jam (Clock Jitter)?", "id": "Variasi Waktu Tepi Jam (Clock Edge) Acak." },
  { "en": "Apa Itu Skew Sinyal Jam (Clock Skew)?", "id": "Perbedaan Waktu Tiba Sinyal Jam (Clock)." },
  { "en": "Mengapa Clock Skew (Kemiringan Jam) Buruk?", "id": "Dapat Menyebabkan Kesalahan Waktu (Timing Error)." },
  { "en": "Apa Itu Distribusi Jam (Clock Distribution)?", "id": "Jaringan Pengirim Sinyal Jam (Clock) Chip." },
  { "en": "Apa Itu Pohon Jam (Clock Tree)?", "id": "Struktur Jaringan Distribusi Jam (Clock)." },
  { "en": "Apa Itu Gated Clock (Jam Bergerbang)?", "id": "Teknik Menghentikan Jam (Clock) Hemat Daya." },
  { "en": "Apa Itu Disipasi Daya Statis?", "id": "Konsumsi Daya Saat Rangkaian Diam (Idle)." },
  { "en": "Apa Itu Disipasi Daya Dinamis?", "id": "Konsumsi Daya Saat Rangkaian Beralih (Switching)." },
  { "en": "Apa Itu Power Gating (Gerbang Daya)?", "id": "Teknik Mematikan Daya Bagian IC (Integrated Circuit)." },
  { "en": "Apa Itu Scaling Tegangan (Voltage Scaling)?", "id": "Menurunkan Tegangan Operasi Untuk Hemat Daya." },
  { "en": "Apa Itu Tembok Panas (Thermal Wall)?", "id": "Batas Kinerja IC (Integrated Circuit) Akibat Panas." },
  { "en": "Apa Itu Manajemen Termal (Thermal Management)?", "id": "Teknik Pengelolaan Panas Pada IC (Integrated Circuit)." },
  { "en": "Apa Itu Model Kegagalan (Failure Model)?", "id": "Cara Spesifik IC (Integrated Circuit) Dapat Rusak." },
  { "en": "Apa Itu Kegagalan Stuck-At?", "id": "Output Gerbang Logika Terjebak Tinggi Rendah." },
  { "en": "Apa Itu Kegagalan Jembatan (Bridging Fault)?", "id": "Hubungan Singkat Antara Dua Jalur." },
  { "en": "Apa Itu DFT (Design for Testability)?", "id": "Mendesain IC (Integrated Circuit) Agar Mudah Diuji." },
  { "en": "Apa Itu Scan Chain (Rantai Pindai)?", "id": "Teknik DFT (Design for Testability) Uji Rangkaian Sekuensial." },
  { "en": "Apa Itu BIST (Built-In Self-Test)?", "id": "Kemampuan IC (Integrated Circuit) Menguji Dirinya Sendiri." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Standar Industri Untuk Pengujian Papan Sirkuit." },
  { "en": "Apa Itu Boundary Scan (Pindai Batas)?", "id": "Metode Pengujian JTAG (Joint Test Action Group)." },
  { "en": "Apa Itu IC (Integrated Circuit) Regulator Linear?", "id": "Regulator Penurun Tegangan Efisiensi Rendah." },
  { "en": "Apa Itu IC (Integrated Circuit) Regulator Switching?", "id": "Regulator Pengubah Tegangan Efisiensi Tinggi." },
  { "en": "Apa Itu Buck Converter (Pengubah Buck)?", "id": "Regulator Switching (Switching Regulator) Penurun Tegangan." },
  { "en": "Apa Itu Boost Converter (Pengubah Boost)?", "id": "Regulator Switching (Switching Regulator) Penaik Tegangan." },
  { "en": "Apa Itu LDO (Low-Dropout Regulator)?", "id": "Regulator Linear Beda Tegangan Input-Output Kecil." },
  { "en": "Apa Itu Bandgap Reference (Referensi Celah Pita)?", "id": "Rangkaian Penghasil Tegangan Referensi Stabil." },
  { "en": "Apa Itu Current Mirror (Cermin Arus)?", "id": "Rangkaian Penyalin (Copy) Arus Listrik." },
  { "en": "Apa Itu Differential Pair (Pasangan Diferensial)?", "id": "Blok Bangunan Dasar Penguat (Amplifier) Analog." },
  { "en": "Apa Itu Penguat Operasional (Operational Amplifier) Ideal?", "id": "Penguatan Tak Terhingga, Impedansi Input Tak Terhingga." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Op-Amp?", "id": "Hambatan (Resistansi) Pada Terminal Input." },
  { "en": "Apa Itu Impedansi Output (Output Impedance) Op-Amp?", "id": "Hambatan (Resistansi) Pada Terminal Output." },
  { "en": "Apa Itu Penguatan Loop Terbuka (Open-Loop Gain)?", "id": "Penguatan (Gain) Op-Amp Tanpa Umpan Balik." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Mengurangi Penguatan (Gain), Meningkatkan Stabilitas." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Penguat (Amplifier) Yang Membalik Fasa Sinyal." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Penguat (Amplifier) Yang Tidak Membalik Fasa." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat (Amplifier) Non-Inverting Dengan Penguatan Satu." },
  { "en": "Apa Fungsi Pengikut Tegangan (Voltage Follower)?", "id": "Sebagai Penyangga (Buffer) Impedansi." },
  { "en": "Apa Itu Tegangan Offset Input (Input Offset Voltage)?", "id": "Tegangan Input Agar Output Nol Volt." },
  { "en": "Apa Itu Arus Bias Input (Input Bias Current)?", "id": "Arus DC (Direct Current) Kecil Ke Input." },
  { "en": "Apa Itu Arus Offset Input (Input Offset Current)?", "id": "Perbedaan Arus Bias (Bias Current) Kedua Input." },
  { "en": "Apa Itu CMRR (Common-Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Common-Mode." },
  { "en": "Apa Itu Sinyal Common-Mode?", "id": "Sinyal Yang Sama Pada Kedua Input Op-Amp." },
  { "en": "Apa Itu PSRR (Power Supply Rejection Ratio)?", "id": "Kemampuan Menolak Kebisingan (Noise) Catu Daya." },
  { "en": "Apa Itu Slew Rate (Laju Perubahan)?", "id": "Kecepatan Perubahan Maksimal Tegangan Output." },
  { "en": "Apa Itu GBWP (Gain-Bandwidth Product)?", "id": "Produk Penguatan (Gain) Dan Lebar Pita (Bandwidth)." },
  { "en": "Apa Itu Lebar Pita (Bandwidth) Penguat?", "id": "Rentang Frekuensi Operasi Efektif Penguat." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter (Filter) Yang Menggunakan Komponen Aktif." },
  { "en": "Komponen Aktif Apa Di Filter Aktif?", "id": "Op-Amp (Operational Amplifier) Atau Transistor." },
  { "en": "Apa Itu LPF (Low-Pass Filter)?", "id": "Filter (Filter) Pelolos Frekuensi Rendah." },
  { "en": "Apa Itu HPF (High-Pass Filter)?", "id": "Filter (Filter) Pelolos Frekuensi Tinggi." },
  { "en": "Apa Itu BPF (Band-Pass Filter)?", "id": "Filter (Filter) Pelolos Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu BSF (Band-Stop Filter)?", "id": "Filter (Filter) Penolak Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Notch Filter?", "id": "Nama Lain BSF (Band-Stop Filter) Sempit." },
  { "en": "Apa Itu Osilator (Oscillator) Gelombang Sinus?", "id": "Rangkaian Pembangkit Sinyal Sinusoidal." },
  { "en": "Apa Itu Osilator (Oscillator) Relaksasi?", "id": "Rangkaian Pembangkit Sinyal Non-Sinusoidal." },
  { "en": "Apa Itu Multivibrator Astabil (Astable Multivibrator)?", "id": "Osilator (Oscillator) Tanpa Status Stabil." },
  { "en": "Apa Itu Multivibrator Monostabil (Monostable Multivibrator)?", "id": "Mempunyai Satu Status Stabil Saja." },
  { "en": "Apa Itu Multivibrator Bistabil (Bistable Multivibrator)?", "id": "Mempunyai Dua Status Stabil (Flip-Flop)." },
  { "en": "Apa Itu Komparator (Comparator) Analog?", "id": "Membandingkan Dua Tegangan Analog." },
  { "en": "Apa Output Komparator (Comparator) Analog?", "id": "Sinyal Digital (Tinggi Atau Rendah)." },
  { "en": "Apa Itu Histeresis (Hysteresis)?", "id": "Perbedaan Ambang Batas (Threshold) Naik Turun." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator (Comparator) Yang Menggunakan Histeresis (Hysteresis)." },
  { "en": "Fungsi Schmitt Trigger?", "id": "Mengurangi Sensitivitas Terhadap Kebisingan (Noise)." },
  { "en": "Apa Itu Saturasi (Saturation) Op-Amp?", "id": "Kondisi Output Op-Amp Mencapai Batas Maksimal." },
  { "en": "Apa Itu Rail-to-Rail Op-Amp?", "id": "Op-Amp (Operational Amplifier) Output Hingga VCC VSS." },
  { "en": "Apa Itu Arus Diam (Quiescent Current)?", "id": "Arus Konsumsi IC (Integrated Circuit) Tanpa Sinyal." },
  { "en": "Apa Itu Kebisingan Termal (Thermal Noise)?", "id": "Kebisingan (Noise) Akibat Gerakan Termal Elektron." },
  { "en": "Apa Itu Kebisingan Tembakan (Shot Noise)?", "id": "Kebisingan (Noise) Akibat Sifat Diskrit Muatan." },
  { "en": "Apa Itu Kebisingan Flicker (Flicker Noise) (1/f)?", "id": "Kebisingan (Noise) Frekuensi Rendah Pada Semikonduktor." },
  { "en": "Apa Itu Angka Kebisingan (Noise Figure)?", "id": "Ukuran Degradasi Sinyal Akibat Kebisingan (Noise)." },
  { "en": "Apa Itu Sinyal Diferensial (Differential Signaling)?", "id": "Sinyal Menggunakan Dua Kawat Komplementer." },
  { "en": "Keuntungan Sinyal Diferensial (Differential Signaling)?", "id": "Tahan Terhadap Kebisingan (Noise) Common-Mode." },
  { "en": "Apa Itu Sinyal Single-Ended (Single-Ended Signaling)?", "id": "Sinyal Menggunakan Satu Kawat Dan Ground." },
  { "en": "Apa Itu LVDS (Low-Voltage Differential Signaling)?", "id": "Standar Sinyal Diferensial (Differential Signaling) Tegangan Rendah." },
  { "en": "Apa Itu ESD (Electrostatic Discharge) Protection?", "id": "Perlindungan IC (Integrated Circuit) Dari Listrik Statis." },
  { "en": "Apa Itu Dioda TVS (Transient Voltage Suppressor)?", "id": "Dioda Pelindung Lonjakan Tegangan Transien." },
  { "en": "Apa Itu Sirkuit Penjepit (Clamping Circuit)?", "id": "Rangkaian Pembatas Level Tegangan Sinyal." },
  { "en": "Apa Itu Mask ROM (Read Only Memory)?", "id": "ROM (Read Only Memory) Diprogram Saat Fabrikasi." },
  { "en": "Apa Itu PROM (Programmable Read Only Memory)?", "id": "ROM (Read Only Memory) Dapat Diprogram Sekali." },
  { "en": "Apa Itu EPROM (Erasable Programmable ROM)?", "id": "ROM (Read Only Memory) Dihapus Sinar Ultra-Violet." },
  { "en": "Apa Itu Jendela Kaca EPROM (Erasable Programmable ROM)?", "id": "Jendela Untuk Menghapus Data Pakai Sinar UV." },
  { "en": "Apa Itu SDRAM (Synchronous Dynamic RAM)?", "id": "DRAM (Dynamic RAM) Sinkron Sinyal Jam (Clock)." },
  { "en": "Apa Itu DDR SDRAM (Double Data Rate SDRAM)?", "id": "SDRAM (Synchronous Dynamic RAM) Transfer Data Dua Kali." },
  { "en": "Apa Itu Sel Memori (Memory Cell)?", "id": "Unit Penyimpan Satu Bit (Bit) Data." },
  { "en": "Berapa Transistor Sel SRAM (Static RAM)?", "id": "Biasanya Enam Transistor (6T)." },
  { "en": "Berapa Transistor Sel DRAM (Dynamic RAM)?", "id": "Satu Transistor (1T) Satu Kapasitor (1C)." },
  { "en": "Apa Itu Arsitektur Memori (Memory Architecture)?", "id": "Struktur Organisasi Blok Memori." },
  { "en": "Apa Itu Page Memori (Memory Page)?", "id": "Blok Data Dalam Operasi Memori." },
  { "en": "Apa Itu Bank Memori (Memory Bank)?", "id": "Sub-Array Independen Dalam Chip Memori." },
  { "en": "Apa Itu Latensi CAS (CAS Latency) RAM?", "id": "Tunda Waktu Kolom (Column Address Strobe)." },
  { "en": "Apa Itu MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sistem Mekanis Elektro Mikro Pada Chip." },
  { "en": "Apa Contoh Sensor MEMS (Micro-Electro-Mechanical Systems)?", "id": "Akselerometer (Accelerometer), Giroskop (Gyroscope), Mikrofon (Microphone)." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Sensor (Sensor) Pengukur Percepatan Getaran." },
  { "en": "Apa Itu Giroskop (Gyroscope)?", "id": "Sensor (Sensor) Pengukur Orientasi Kecepatan Sudut." },
  { "en": "Apa Itu Aktuator (Actuator) MEMS?", "id": "Perangkat MEMS (Micro-Electro-Mechanical Systems) Yang Bergerak." },
  { "en": "Apa Itu Cermin Mikro (Micromirror) MEMS?", "id": "Aktuator (Actuator) MEMS Pengendali Pantulan Cahaya." },
  { "en": "Apa Itu IC (Integrated Circuit) Optoelektronik?", "id": "IC (Integrated Circuit) Yang Berinteraksi Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda (Diode) Sensor Cahaya." },
  { "en": "Apa Itu Laser Semikonduktor (Semiconductor Laser)?", "id": "Laser (Laser) Yang Dibuat Dari Bahan Semikonduktor." },
  { "en": "Apa Itu FinFET (Fin Field-Effect Transistor)?", "id": "Transistor MOSFET (Metal-Oxide-Semiconductor FET) Non-Planar Tiga Dimensi." },
  { "en": "Mengapa FinFET (Fin Field-Effect Transistor) Dibuat?", "id": "Mengurangi Arus Bocor (Leakage Current) Ukuran Kecil." },
  { "en": "Apa Itu GAAFET (Gate-All-Around Field-Effect Transistor)?", "id": "Transistor (Transistor) Gerbang Mengelilingi Saluran." },
  { "en": "Apa Itu Foundry (Pengecoran) Semikonduktor?", "id": "Perusahaan Manufaktur Pembuat Chip (Chip) Saja." },
  { "en": "Apa Itu Perusahaan Fabless (Tanpa Fabrikasi)?", "id": "Perusahaan Desain Chip (Chip) Tanpa Pabrik." },
  { "en": "Apa Itu IDM (Integrated Device Manufacturer)?", "id": "Perusahaan Mendesain Dan Memfabrikasi Chip (Chip)." },
  { "en": "Contoh Perusahaan Foundry (Pengecoran)?", "id": "TSMC (Taiwan Semiconductor Manufacturing Company), Samsung." },
  { "en": "Contoh Perusahaan Fabless (Tanpa Fabrikasi)?", "id": "Qualcomm, Nvidia, AMD (Advanced Micro Devices)." },
  { "en": "Contoh Perusahaan IDM (Integrated Device Manufacturer)?", "id": "Intel, Texas Instruments." },
  { "en": "Apa Itu IP (Intellectual Property) Core?", "id": "Blok Desain Rangkaian Siap Pakai." },
  { "en": "Apa Itu IP (Intellectual Property) Keras?", "id": "Blok Desain Fisik (Layout) Tetap." },
  { "en": "Apa Itu IP (Intellectual Property) Lunak?", "id": "Blok Desain Logika (RTL) Fleksibel." },
  { "en": "Apa Itu RTL (Register-Transfer Level)?", "id": "Level Abstraksi Desain Logika Digital." },
  { "en": "Bahasa Apa Untuk Deskripsi RTL (Register-Transfer Level)?", "id": "VHDL (VHSIC Hardware Description Language), Verilog." },
  { "en": "Apa Itu VHDL (VHSIC Hardware Description Language)?", "id": "Bahasa (Language) Deskripsi Perangkat Keras." },
  { "en": "Apa Itu Verilog?", "id": "Bahasa (Language) Deskripsi Perangkat Keras Populer." },
  { "en": "Apa Itu Sintesis Logika (Logic Synthesis)?", "id": "Proses Mengubah RTL (Register-Transfer Level) Menjadi Gerbang." },
  { "en": "Apa Itu Place and Route (P&R)?", "id": "Proses Penempatan Gerbang Pemasangan Jalur." },
  { "en": "Apa Itu Timing Closure (Penutupan Waktu)?", "id": "Memastikan Desain Memenuhi Batasan Waktu (Timing)." },
  { "en": "Apa Itu Verifikasi Formal (Formal Verification)?", "id": "Pembuktian Desain Benar Secara Matematis." },
  { "en": "Apa Itu Simulasi (Simulation) Logika?", "id": "Pengujian Fungsionalitas Desain RTL (Register-Transfer Level)." },
  { "en": "Apa Itu Emulasi (Emulation) Perangkat Keras?", "id": "Simulasi (Simulation) Desain Menggunakan FPGA (Field-Programmable Gate Array)." },
  { "en": "Apa Itu Prototyping (Purwarupa) FPGA?", "id": "Implementasi Awal Desain ASIC (Application-Specific IC) Pada FPGA." },
  { "en": "Apa Itu IC (Integrated Circuit) Analog Kustom?", "id": "Desain (Design) Analog Spesifik Manual." },
  { "en": "Mengapa Desain Analog Sulit?", "id": "Sangat Sensitif Terhadap Noise (Kebisingan) Variasi." },
  { "en": "Apa Itu Pencocokan (Matching) Komponen?", "id": "Membuat Komponen Analog Identik Mungkin." },
  { "en": "Apa Itu Buffer (Penyangga)?", "id": "Gerbang (Gate) Logika Penguat Arus Sinyal." },
  { "en": "Apa Itu Gerbang NOT (Bukan)?", "id": "Gerbang (Gate) Logika Pembalik Sinyal (Inverter)." },
  { "en": "Apa Itu Inverter (Pembalik)?", "id": "Nama Lain Untuk Gerbang NOT (Bukan)." },
  { "en": "Apa Itu Tabel Kebenaran (Truth Table)?", "id": "Tabel (Table) Output Untuk Semua Input Logika." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika (Math) Untuk Menganalisis Sirkuit Logika." },
  { "en": "Apa Itu Teorema De Morgan?", "id": "Aturan (Rule) Penting Dalam Aljabar Boolean." },
  { "en": "Apa Itu Peta Karnaugh (Karnaugh Map)?", "id": "Metode (Method) Grafis Penyederhanaan Logika." },
  { "en": "Apa Itu Logika High (Tinggi) (1)?", "id": "Merepresentasikan Tegangan (Voltage) Tinggi." },
  { "en": "Apa Itu Logika Low (Rendah) (0)?", "id": "Merepresentasikan Tegangan (Voltage) Rendah." },
  { "en": "Apa Itu Rangkaian Aritmatika (Arithmetic Circuit)?", "id": "Rangkaian (Circuit) Untuk Operasi Matematika." },
  { "en": "Apa Itu Subtractor (Pengurang)?", "id": "Rangkaian (Circuit) Pengurang Bilangan Biner." },
  { "en": "Apa Itu Encoder Prioritas (Priority Encoder)?", "id": "Encoder (Encoder) Dengan Level Prioritas Input." },
  { "en": "Apa Itu Paritas (Parity)?", "id": "Metode (Method) Pengecekan Kesalahan Data Sederhana." },
  { "en": "Apa Itu Bit (Bit) Paritas?", "id": "Bit (Bit) Tambahan Untuk Pengecekan Paritas." },
  { "en": "Apa Itu Paritas Ganjil (Odd Parity)?", "id": "Jumlah (Count) Bit Satu Adalah Ganjil." },
  { "en": "Apa Itu Paritas Genap (Even Parity)?", "id": "Jumlah (Count) Bit Satu Adalah Genap." },
  { "en": "Apa Itu Generator Paritas (Parity Generator)?", "id": "Rangkaian (Circuit) Pembuat Bit Paritas." },
  { "en": "Apa Itu Pengecek Paritas (Parity Checker)?", "id": "Rangkaian (Circuit) Pemeriksa Bit Paritas." },
  { "en": "Apa Itu Kode Gray (Gray Code)?", "id": "Urutan (Sequence) Kode Biner Berubah Satu Bit." },
  { "en": "Apa Itu Kode BCD (Binary-Coded Decimal)?", "id": "Representasi (Representation) Biner Untuk Angka Desimal." },
  { "en": "Apa Itu Tampilan Tujuh Segmen (Seven-Segment Display)?", "id": "Komponen (Component) Penampil Angka." },
  { "en": "Apa Itu Dekoder BCD (Binary-Coded Decimal) Ke Tujuh Segmen?", "id": "IC (Integrated Circuit) Pengendali Tampilan Tujuh Segmen." },
  { "en": "Apa Itu Latch (Selak)?", "id": "Elemen (Element) Memori Dasar Sensitif Level." },
  { "en": "Apa Itu Latch SR (Set-Reset Latch)?", "id": "Latch (Latch) Dasar Menggunakan Gerbang NAND NOR." },
  { "en": "Apa Beda Latch (Selak) Dan Flip-Flop?", "id": "Latch (Selak) Sensitif Level, Flip-Flop Tepi." },
  { "en": "Apa Itu Flip-Flop Tipe D (Data)?", "id": "Menyimpan (Store) Nilai Input D Saat Tepi Jam." },
  { "en": "Apa Itu Flip-Flop Tipe T (Toggle)?", "id": "Membalik (Toggle) Output Saat Tepi Jam." },
  { "en": "Apa Itu Flip-Flop Tipe JK?", "id": "Flip-Flop (Flip-Flop) Fleksibel (Set, Reset, Toggle)." },
  { "en": "Apa Itu Kondisi Balapan (Race Condition)?", "id": "Masalah (Problem) Waktu (Timing) Pada Rangkaian Sekuensial." },
  { "en": "Apa Itu Master-Slave Flip-Flop?", "id": "Kombinasi (Combination) Dua Latch Hindari Balapan." },
  { "en": "Apa Itu Input Asinkron (Asynchronous Input)?", "id": "Input (Input) Flip-Flop (Preset, Clear) Independen Jam." },
  { "en": "Apa Itu Preset (Prasetel) Flip-Flop?", "id": "Input (Input) Asinkron Set (Setel) Output Ke Satu." },
  { "en": "Apa Itu Clear (Hapus) Flip-Flop?", "id": "Input (Input) Asinkron Reset (Atur Ulang) Output Ke Nol." },
  { "en": "Apa Itu Registrasi (Register) Memori?", "id": "Sekelompok (Group) Flip-Flop Penyimpan Data." },
  { "en": "Apa Itu PIPO (Parallel In Parallel Out)?", "id": "Register (Register) Geser Input Paralel Output Paralel." },
  { "en": "Apa Itu SIPO (Serial In Parallel Out)?", "id": "Register (Register) Geser Input Serial Output Paralel." },
  { "en": "Apa Itu PISO (Parallel In Serial Out)?", "id": "Register (Register) Geser Input Paralel Output Serial." },
  { "en": "Apa Itu SISO (Serial In Serial Out)?", "id": "Register (Register) Geser Input Serial Output Serial." },
  { "en": "Apa Itu State Machine (Mesin Keadaan)?", "id": "Rangkaian (Circuit) Sekuensial Representasi Diagram Keadaan." },
  { "en": "Apa Itu Mealy Machine (Mesin Mealy)?", "id": "Output (Output) Mesin Keadaan Tergantung Input Keadaan." },
  { "en": "Apa Itu Moore Machine (Mesin Moore)?", "id": "Output (Output) Mesin Keadaan Tergantung Keadaan Saja." },
  { "en": "Apa Itu Diagram Keadaan (State Diagram)?", "id": "Representasi (Representation) Grafis Transisi Mesin Keadaan." },
  { "en": "Apa Itu Tabel Keadaan (State Table)?", "id": "Representasi (Representation) Tabel Transisi Mesin Keadaan." },
  { "en": "Apa Itu Penetapan Keadaan (State Assignment)?", "id": "Memberi (Assign) Kode Biner Ke Setiap Keadaan." },
  { "en": "Apa Itu Pengurangan Keadaan (State Reduction)?", "id": "Menyederhanakan (Simplify) Mesin Keadaan Hapus Keadaan Redundan." },
  { "en": "Apa Itu PLD (Programmable Logic Device)?", "id": "IC (Integrated Circuit) Logika Dapat Diprogram Pengguna." },
  { "en": "Apa Itu PAL (Programmable Array Logic)?", "id": "Jenis (Type) PLD (Programmable Logic Device) Array AND Terprogram." },
  { "en": "Apa Itu GAL (Generic Array Logic)?", "id": "Jenis (Type) PAL (Programmable Array Logic) Dapat Dihapus Diprogram Ulang." },
  { "en": "Apa Itu PLA (Programmable Logic Array)?", "id": "Jenis (Type) PLD (Programmable Logic Device) Array AND OR Terprogram." },
  { "en": "Apa Itu CPLD (Complex Programmable Logic Device)?", "id": "Kumpulan (Collection) Blok PLD (Programmable Logic Device) Terhubung." },
  { "en": "Apa Arsitektur CPLD (Complex Programmable Logic Device)?", "id": "Berbasis (Based On) Makrosel (Macrocell) Dan Interkoneksi." },
  { "en": "Apa Arsitektur FPGA (Field-Programmable Gate Array)?", "id": "Berbasis (Based On) CLB (Configurable Logic Block) Interkoneksi." },
  { "en": "Apa Itu CLB (Configurable Logic Block)?", "id": "Blok (Block) Logika Dasar Dalam FPGA (Field-Programmable Gate Array)." },
  { "en": "Apa Isi CLB (Configurable Logic Block)?", "id": "LUT (Look-Up Table) Dan Flip-Flop." },
  { "en": "Apa Itu LUT (Look-Up Table)?", "id": "Memori (Memory) Kecil Implementasi Fungsi Logika." },
  { "en": "Apa Itu Perutean (Routing) FPGA?", "id": "Jalur (Path) Interkoneksi Yang Dapat Diprogram." },
  { "en": "Apa Itu Bitstream FPGA (Field-Programmable Gate Array)?", "id": "File (File) Konfigurasi Untuk Memprogram FPGA (Field-Programmable Gate Array)." },
  { "en": "Apa Itu JTAG (Joint Test Action Group) Chain?", "id": "Koneksi (Connection) Serial Antar IC (Integrated Circuit) Untuk Tes." },
  { "en": "Apa Itu TCK (Test Clock)?", "id": "Sinyal (Signal) Jam (Clock) Untuk Antarmuka JTAG (Joint Test Action Group)." },
  { "en": "Apa Itu TMS (Test Mode Select)?", "id": "Sinyal (Signal) Kontrol Mode Operasi JTAG (Joint Test Action Group)." },
  { "en": "Apa Itu TDI (Test Data In)?", "id": "Input (Input) Data Serial Untuk JTAG (Joint Test Action Group)." },
  { "en": "Apa Itu TDO (Test Data Out)?", "id": "Output (Output) Data Serial Dari JTAG (Joint Test Action Group)." },
  { "en": "Apa Itu Register Instruksi (Instruction Register) JTAG?", "id": "Menyimpan (Store) Perintah (Command) Tes JTAG (Joint Test Action Group)." },
  { "en": "Apa Itu Register Data (Data Register) JTAG?", "id": "Menyimpan (Store) Data Tes JTAG (Joint Test Action Group)." },
  { "en": "Apa Itu TAP (Test Access Port)?", "id": "Port (Port) Antarmuka JTAG (Joint Test Action Group) Pada IC." },
  { "en": "Apa Itu IDCODE (Identification Code) JTAG?", "id": "Instruksi (Instruction) JTAG (Joint Test Action Group) Identifikasi Perangkat." },
  { "en": "Apa Itu BYPASS (Pintas) JTAG?", "id": "Instruksi (Instruction) JTAG (Joint Test Action Group) Melewati IC (Integrated Circuit)." },
  { "en": "Apa Itu EXTEST (External Test) JTAG?", "id": "Instruksi (Instruction) JTAG (Joint Test Action Group) Tes Koneksi Pin." },
  { "en": "Apa Itu INTEST (Internal Test) JTAG?", "id": "Instruksi (Instruction) JTAG (Joint Test Action Group) Tes Logika Internal." },
  { "en": "Apa Itu Bahasa BSDL (Boundary Scan Description Language)?", "id": "Bahasa (Language) Deskripsi Kemampuan JTAG (Joint Test Action Group) IC." },
  { "en": "Apa Itu IC (Integrated Circuit) Regulator Seri?", "id": "Regulator (Regulator) Linear Elemen Kontrol Seri." },
  { "en": "Apa Itu IC (Integrated Circuit) Regulator Shunt?", "id": "Regulator (Regulator) Linear Elemen Kontrol Paralel." },
  { "en": "Apa Itu Proteksi Arus Lebih (Overcurrent Protection)?", "id": "Fitur (Feature) Keamanan Pembatas Arus Output." },
  { "en": "Apa Itu Proteksi Termal (Thermal Shutdown)?", "id": "Fitur (Feature) Keamanan Mematikan IC (Integrated Circuit) Panas." },
  { "en": "Apa Itu Arus Input (Input Current) Regulator?", "id": "Arus (Current) Yang Ditarik Dari Sumber." },
  { "en": "Apa Itu Tegangan Dropout (Dropout Voltage) LDO?", "id": "Perbedaan (Difference) Tegangan Input Output Minimal." },
  { "en": "Apa Itu Efisiensi (Efficiency) Regulator?", "id": "Rasio (Ratio) Daya Output Terhadap Daya Input." },
  { "en": "Mengapa Efisiensi Regulator Switching (Switching Regulator) Tinggi?", "id": "Transistor (Transistor) Bekerja Mode Saklar (Switch)." },
  { "en": "Apa Itu Induktor (Inductor) Pada Regulator Switching?", "id": "Komponen (Component) Penyimpan Energi Magnetik." },
  { "en": "Apa Itu Kapasitor (Capacitor) Pada Regulator Switching?", "id": "Komponen (Component) Penyimpan Energi Listrik." },
  { "en": "Apa Itu Dioda (Diode) Pada Regulator Switching?", "id": "Komponen (Component) Pengarah Arus (Current) Induktor." },
  { "en": "Apa Itu Mode Kontinu (Continuous Mode) Switching?", "id": "Arus (Current) Induktor Tidak Pernah Nol." },
  { "en": "Apa Itu Mode Discontinu (Discontinuous Mode) Switching?", "id": "Arus (Current) Induktor Mencapai Nol." },
  { "en": "Apa Itu Ripple (Riak) Tegangan Output?", "id": "Variasi (Variation) AC (Alternating Current) Kecil Tegangan DC." },
  { "en": "Apa Itu Respon Transien (Transient Response) Regulator?", "id": "Reaksi (Reaction) Regulator Terhadap Perubahan Beban." },
  { "en": "Apa Itu Regulasi Jalur (Line Regulation)?", "id": "Kemampuan (Ability) Jaga Output Stabil Ubah Input." },
  { "en": "Apa Itu Regulasi Beban (Load Regulation)?", "id": "Kemampuan (Ability) Jaga Output Stabil Ubah Beban." },
  { "en": "Apa Itu Penguat Indera Arus (Current Sense Amplifier)?", "id": "Penguat (Amplifier) Pengukur Arus Listrik." },
  { "en": "Apa Itu Kontrol Mode Tegangan (Voltage Mode Control)?", "id": "Metode (Method) Kontrol Umpan Balik Regulator Switching." },
  { "en": "Apa Itu Kontrol Mode Arus (Current Mode Control)?", "id": "Metode (Method) Kontrol Umpan Balik Regulator Switching." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Modulasi (Modulation) Lebar Pulsa Sinyal Kontrol." },
  { "en": "Apa Itu PFM (Pulse Frequency Modulation)?", "id": "Modulasi (Modulation) Frekuensi Pulsa Sinyal Kontrol." },
  { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Gangguan (Interference) Elektromagnetik Frekuensi Radio." },
  { "en": "Mengapa Regulator Switching (Switching Regulator) Menghasilkan EMI?", "id": "Karena (Because Of) Proses Pensaklaran Cepat." },
  { "en": "Apa Itu Sirkuit Snubber (Snubber Circuit)?", "id": "Rangkaian (Circuit) Peredam Lonjakan Tegangan (Spike)." },
  { "en": "Apa Itu Hot Swap Controller (Pengontrol Hot Swap) IC?", "id": "IC (Integrated Circuit) Manajemen Koneksi Daya Aman." },
  { "en": "Apa Itu Soft Start (Mulai Lunak) Regulator?", "id": "Fitur (Feature) Penaikan Tegangan Output Perlahan." },
  { "en": "Apa Itu UVLO (Under-Voltage Lockout)?", "id": "Mematikan (Shutdown) IC (Integrated Circuit) Jika Tegangan Input Rendah." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Daya?", "id": "MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Arus Tegangan Tinggi." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Kombinasi (Combination) MOSFET (Metal-Oxide-Semiconductor FET) Dan BJT (Bipolar Junction Transistor)." },
  { "en": "Kelebihan IGBT (Insulated Gate Bipolar Transistor)?", "id": "Drive (Kemudi) MOSFET (Metal-Oxide-Semiconductor FET) Kemampuan Arus BJT (Bipolar Junction Transistor)." },
  { "en": "Di Mana IGBT (Insulated Gate Bipolar Transistor) Digunakan?", "id": "Motor (Motor) Drive, Inverter (Inverter), Power (Daya) Switching." },
  { "en": "Apa Itu Thyristor (Tiristor)?", "id": "Saklar (Switch) Semikonduktor Empat Lapis (PNPN)." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Jenis (Type) Thyristor (Tiristor) Paling Umum." },
  { "en": "Bagaimana Cara Memicu (Trigger) SCR (Silicon Controlled Rectifier)?", "id": "Memberi (Give) Pulsa Arus Singkat Ke Gate (Gerbang)." },
  { "en": "Bagaimana Cara Mematikan (Turn Off) SCR (Silicon Controlled Rectifier)?", "id": "Arus (Current) Anoda Turun Dibawah Arus Tahan." },
  { "en": "Apa Itu Arus Tahan (Holding Current) SCR?", "id": "Arus (Current) Anoda Minimum Agar Tetap Menyala." },
  { "en": "Apa Itu Arus Latching (Latching Current) SCR?", "id": "Arus (Current) Anoda Minimum Agar Mulai Menyala." },
  { "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Thyristor (Tiristor) Dua Arah Untuk Arus AC." },
  { "en": "Apa Itu DIAC (Diode for Alternating Current)?", "id": "Dioda (Diode) Pemicu (Trigger) Dua Arah." },
  { "en": "Apa Fungsi DIAC (Diode for Alternating Current)?", "id": "Biasanya (Usually) Digunakan Memicu (Trigger) TRIAC (Triode for AC)." },
  { "en": "Apa Itu IC (Integrated Circuit) Driver Gerbang (Gate Driver)?", "id": "IC (Integrated Circuit) Penguat Sinyal Kontrol MOSFET (Metal-Oxide-Semiconductor FET) IGBT." },
  { "en": "Mengapa Perlu Driver Gerbang (Gate Driver)?", "id": "MOSFET (Metal-Oxide-Semiconductor FET) IGBT (Insulated Gate Bipolar Transistor) Butuh Arus Gate (Gerbang) Besar." },
  { "en": "Apa Itu Driver Sisi Tinggi (High-Side Driver)?", "id": "Driver (Driver) Gerbang Saklar (Switch) Terhubung Sisi Positif." },
  { "en": "Apa Itu Driver Sisi Rendah (Low-Side Driver)?", "id": "Driver (Driver) Gerbang Saklar (Switch) Terhubung Sisi Negatif." },
  { "en": "Apa Itu Topologi Setengah Jembatan (Half-Bridge)?", "id": "Dua (Two) Saklar (Switch) Terhubung Seri." },
  { "en": "Apa Itu Topologi Jembatan Penuh (Full-Bridge)?", "id": "Empat (Four) Saklar (Switch) Topologi H." },
  { "en": "Apa Itu Masalah Tembak Tembus (Shoot-Through)?", "id": "Kedua (Both) Saklar (Switch) Jembatan Menyala Bersamaan." },
  { "en": "Apa Itu Waktu Mati (Dead Time)?", "id": "Tunda (Delay) Waktu Mencegah Tembak Tembus (Shoot-Through)." },
  { "en": "Apa Itu Penggerak Motor (Motor Driver) IC?", "id": "IC (Integrated Circuit) Kontrol Kecepatan Arah Motor." },
  { "en": "Apa Itu Motor DC (Direct Current) Disikat (Brushed)?", "id": "Motor (Motor) DC (Direct Current) Menggunakan Sikat (Brush) Komutasi." },
  { "en": "Apa Itu Motor DC (Direct Current) Tanpa Sikat (Brushless)?", "id": "Motor (Motor) DC (Direct Current) Komutasi Elektronik." },
  { "en": "Apa Itu BLDC (Brushless Direct Current) Motor?", "id": "Motor (Motor) Arus Searah Tanpa Sikat." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor (Motor) Bergerak Dalam Langkah (Step) Diskrit." },
  { "en": "Apa Itu Penggerak Mikrostep (Microstepping Driver)?", "id": "Penggerak (Driver) Motor Stepper (Stepper Motor) Langkah Halus." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Flash?", "id": "Tipe (Type) ADC (Analog-to-Digital Converter) Paralel Sangat Cepat." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) SAR (Successive Approximation Register)?", "id": "Tipe (Type) ADC (Analog-to-Digital Converter) Umum Keseimbangan Baik." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Sigma-Delta?", "id": "Tipe (Type) ADC (Analog-to-Digital Converter) Resolusi Sangat Tinggi." },
  { "en": "Apa Itu Resolusi (Resolution) ADC?", "id": "Jumlah (Number) Bit (Bit) Output Digital." },
  { "en": "Apa Itu Laju Sampel (Sample Rate) ADC?", "id": "Seberapa (How) Cepat ADC (Analog-to-Digital Converter) Mengambil Sampel." },
  { "en": "Apa Itu Teorema Nyquist-Shannon?", "id": "Laju (Rate) Sampel Harus Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Kesalahan (Error) Sinyal Akibat Laju Sampel Rendah." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "LPF (Low-Pass Filter) Sebelum ADC (Analog-to-Digital Converter) Cegah Aliasing." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Proses (Process) Pembulatan Sinyal Analog Ke Level Diskrit." },
  { "en": "Apa Itu Kesalahan Kuantisasi (Quantization Error)?", "id": "Perbedaan (Difference) Sinyal Asli Hasil Kuantisasi." },
  { "en": "Apa Itu SNR (Signal-to-Noise Ratio)?", "id": "Rasio (Ratio) Daya Sinyal Terhadap Daya Kebisingan." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) R-2R Ladder?", "id": "Tipe (Type) DAC (Digital-to-Analog Converter) Pakai Jaringan Resistor." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) String Resistor?", "id": "Tipe (Type) DAC (Digital-to-Digital Converter) Pakai Pembagi Tegangan." },
  { "en": "Apa Itu Filter Rekonstruksi (Reconstruction Filter)?", "id": "LPF (Low-Pass Filter) Setelah DAC (Digital-to-Analog Converter) Haluskan Sinyal." },
  { "en": "Apa Itu DNL (Differential Non-Linearity)?", "id": "Kesalahan (Error) Ukuran Langkah Antar Kode ADC DAC." },
  { "en": "Apa Itu INL (Integral Non-Linearity)?", "id": "Kesalahan (Error) Total Deviasi Dari Garis Lurus Ideal." },
  { "en": "Apa Itu Sirkuit Sampel Tahan (Sample and Hold)?", "id": "Rangkaian (Circuit) Menangkap Menahan Tegangan Sesaat." },
  { "en": "Fungsi Sirkuit Sampel Tahan (Sample and Hold)?", "id": "Menstabilkan (Stabilize) Input Untuk ADC (Analog-to-Digital Converter)." },
  { "en": "Apa Itu Kode Biner Lurus (Straight Binary Code)?", "id": "Format (Format) Kode Biner Standar." },
  { "en": "Apa Itu Kode Komplemen Dua (Two's Complement)?", "id": "Format (Format) Kode Biner Representasi Angka Negatif." },
  { "en": "Apa Itu Kode Offset Biner (Offset Binary Code)?", "id": "Format (Format) Kode Biner Digeser (Offset)." },
  { "en": "Apa Itu Antarmuka (Interface) Serial?", "id": "Komunikasi (Communication) Data Satu Bit (Bit) Sekaligus." },
  { "en": "Apa Itu Antarmuka (Interface) Paralel?", "id": "Komunikasi (Communication) Data Banyak Bit (Bit) Bersamaan." },
  { "en": "Apa Itu UART (Universal Asynchronous Receiver-Transmitter)?", "id": "IC (Integrated Circuit) Komunikasi Serial Asinkron." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Antarmuka (Interface) Bus Serial Sinkron Empat Kawat." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Antarmuka (Interface) Bus Serial Sinkron Dua Kawat." },
  { "en": "Siapa Pengembang I2C (Inter-Integrated Circuit)?", "id": "Philips (Sekarang NXP Semiconductors)." },
  { "en": "Apa Dua Kawat Sinyal I2C?", "id": "SDA (Serial Data) Dan SCL (Serial Clock)." },
  { "en": "Apa Itu Mode Master I2C?", "id": "Perangkat (Device) Yang Memulai Komunikasi Menghasilkan Jam." },
  { "en": "Apa Itu Mode Slave I2C?", "id": "Perangkat (Device) Yang Merespon Master (Master)." },
  { "en": "Apa Itu Alamat (Address) Slave I2C?", "id": "Alamat (Address) Unik Tujuh Bit (Bit) Perangkat Slave." },
  { "en": "Apa Empat Kawat Sinyal SPI?", "id": "MISO (Master In Slave Out), MOSI (Master Out Slave In), SCK (Serial Clock), SS (Slave Select)." },
  { "en": "Apa Itu Sinyal SS (Slave Select) SPI?", "id": "Sinyal (Signal) Master (Master) Pilih Slave (Slave) Aktif." },
  { "en": "Apa Itu Komunikasi Simplex (Simpleks)?", "id": "Komunikasi (Communication) Satu Arah Saja." },
  { "en": "Apa Itu Komunikasi Half-Duplex (Setengah Dupleks)?", "id": "Komunikasi (Communication) Dua Arah Bergantian." },
  { "en": "Apa Itu Komunikasi Full-Duplex (Dupleks Penuh)?", "id": "Komunikasi (Communication) Dua Arah Bersamaan." },
  { "en": "Apa Itu RS-232 (Recommended Standard 232)?", "id": "Standar (Standard) Komunikasi Serial Tegang Tinggi." },
  { "en": "Apa Itu RS-485 (Recommended Standard 485)?", "id": "Standar (Standard) Komunikasi Serial Diferensial Jauh." },
  { "en": "Apa Itu CAN (Controller Area Network) Bus?", "id": "Standar (Standard) Bus Serial Kuat (Robust) Otomotif." },
  { "en": "Apa Itu USB (Universal Serial Bus)?", "id": "Standar (Standard) Antarmuka (Interface) Serial Populer Periferal." },
  { "en": "Apa Itu Ethernet?", "id": "Standar (Standard) Teknologi Jaringan Area Lokal (LAN)." },
  { "en": "Apa Itu PHY (Physical Layer) Ethernet?", "id": "IC (Integrated Circuit) Antarmuka (Interface) Fisik Jaringan Ethernet." },
  { "en": "Apa Itu MAC (Media Access Control) Ethernet?", "id": "Sub-Lapisan (Sub-Layer) Kontrol Akses Media Jaringan." },
  { "en": "Apa Itu HDMI (High-Definition Multimedia Interface)?", "id": "Antarmuka (Interface) Digital Audio Video Definisi Tinggi." },
  { "en": "Apa Itu LVDS (Low-Voltage Differential Signaling) Interface?", "id": "Antarmuka (Interface) Diferensial Kecepatan Tinggi." },
  { "en": "Di Mana LVDS (Low-Voltage Differential Signaling) Digunakan?", "id": "Panel (Panel) LCD (Liquid Crystal Display) Internal, Kamera (Camera)." },
  { "en": "Apa Itu MIPI (Mobile Industry Processor Interface)?", "id": "Standar (Standard) Antarmuka (Interface) Industri Seluler." },
  { "en": "Apa Itu D-PHY (MIPI Alliance specification for a physical layer)?", "id": "Lapisan (Layer) Fisik MIPI (Mobile Industry Processor Interface) Kecepatan Tinggi." },
  { "en": "Apa Itu GPIO (General Purpose Input/Output)?", "id": "Pin (Pin) IC (Integrated Circuit) Fungsi Input Output Umum." },
  { "en": "Apa Itu Mode Input (Input Mode) GPIO?", "id": "Mengkonfigurasi (Configure) Pin (Pin) Membaca Sinyal Digital." },
  { "en": "Apa Itu Mode Output (Output Mode) GPIO?", "id": "Mengkonfigurasi (Configure) Pin (Pin) Mengirim Sinyal Digital." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Resistor (Resistor) Menarik Sinyal Ke Tegangan Tinggi." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Resistor (Resistor) Menarik Sinyal Ke Tegangan Rendah." },
  { "en": "Apa Itu Konfigurasi Open-Drain (Kuras Terbuka)?", "id": "Output (Output) Hanya Menarik Sinyal Ke Rendah." },
  { "en": "Mengapa Perlu Resistor Pull-Up Eksternal?", "id": "Untuk (For) Output Open-Drain (Kuras Terbuka)." },
  { "en": "Apa Itu Konfigurasi Push-Pull (Dorong-Tarik)?", "id": "Output (Output) Aktif Mendorong Tinggi Menarik Rendah." },
  { "en": "Apa Itu Debouncing (Anti-Pantul)?", "id": "Teknik (Technique) Mengabaikan Pantulan Sinyal Saklar." },
  { "en": "Apa Itu Interupsi (Interrupt)?", "id": "Sinyal (Signal) Perangkat Eksternal Minta Perhatian CPU." },
  { "en": "Apa Itu ISR (Interrupt Service Routine)?", "id": "Fungsi (Function) Kode Penanganan Interupsi (Interrupt)." },
  { "en": "Apa Itu NMI (Non-Maskable Interrupt)?", "id": "Interupsi (Interrupt) Prioritas Tinggi Tidak Dapat Diabaikan." },
  { "en": "Apa Itu DMA (Direct Memory Access)?", "id": "Transfer (Transfer) Data Langsung Antara Memori Periferal." },
  { "en": "Mengapa DMA (Direct Memory Access) Efisien?", "id": "Membebaskan (Frees Up) CPU (Central Processing Unit) Tugas Lain." },
  { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Pewaktu (Timer) Reset (Reset) Sistem Jika Terjadi Hang." },
  { "en": "Bagaimana Watchdog Timer (WDT) Bekerja?", "id": "Perangkat Lunak (Software) Harus Reset (Reset) Timer Periodik." },
  { "en": "Apa Itu RTC (Real-Time Clock)?", "id": "IC (Integrated Circuit) Pewaktu (Timer) Pelacak Waktu Nyata." },
  { "en": "Mengapa RTC (Real-Time Clock) Perlu Baterai?", "id": "Agar (So That) Tetap Berjalan Saat Daya Utama Mati." },
  { "en": "Apa Itu Brown-Out Reset (BOR)?", "id": "Reset (Reset) IC (Integrated Circuit) Jika Tegangan Turun." },
  { "en": "Apa Itu Power-On Reset (POR)?", "id": "Reset (Reset) IC (Integrated Circuit) Saat Dinyalakan." },
  { "en": "Apa Itu Siklus Ambil-Dekode-Eksekusi (Fetch-Decode-Execute)?", "id": "Siklus (Cycle) Operasi Dasar CPU (Central Processing Unit)." },
  { "en": "Apa Itu Set Instruksi (Instruction Set)?", "id": "Kumpulan (Set) Perintah Yang Dipahami CPU (Central Processing Unit)." },
  { "en": "Apa Itu Arsitektur Von Neumann?", "id": "Memori (Memory) Tunggal Untuk Instruksi (Instruction) Dan Data." },
  { "en": "Apa Itu Arsitektur Harvard?", "id": "Memori (Memory) Terpisah Untuk Instruksi (Instruction) Dan Data." },
  { "en": "Apa Itu CISC (Complex Instruction Set Computer)?", "id": "Komputer (Computer) Set Instruksi (Instruction) Kompleks." },
  { "en": "Apa Itu RISC (Reduced Instruction Set Computer)?", "id": "Komputer (Computer) Set Instruksi (Instruction) Sederhana." },
  { "en": "Apa Itu Pipeline (Pipa) CPU (Central Processing Unit)?", "id": "Teknik (Technique) Paralel Eksekusi Instruksi." },
  { "en": "Apa Tahapan Pipeline (Pipa) CPU (Central Processing Unit) Sederhana?", "id": "Ambil (Fetch), Dekode (Decode), Eksekusi (Execute)." },
  { "en": "Apa Itu Hazard (Bahaya) Pipeline?", "id": "Kondisi (Condition) Yang Menghentikan Pipeline (Pipa)." },
  { "en": "Apa Itu Cache (Singgahan) Memori CPU (Central Processing Unit)?", "id": "Memori (Memory) Cepat Kecil Dekat CPU (Central Processing Unit)." },
  { "en": "Apa Itu Cache (Singgahan) Level 1 (L1)?", "id": "Cache (Singgahan) Terkecil Tercepat Dalam Inti CPU." },
  { "en": "Apa Itu Cache (Singgahan) Level 2 (L2)?", "id": "Cache (Singgahan) Lebih Besar Lambat Dari L1." },
  { "en": "Apa Itu Cache (Singgahan) Level 3 (L3)?", "id": "Cache (Singgahan) Terbesar Dibagi Antar Inti CPU." },
  { "en": "Apa Itu Cache Hit (Trengginas Singgahan)?", "id": "Data (Data) Yang Dicari Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Miss (Gagal Singgahan)?", "id": "Data (Data) Yang Dicari Tidak Ditemukan Cache." },
  { "en": "Apa Itu Kebijakan Penggantian (Replacement Policy) Cache?", "id": "Aturan (Rule) Membuang Data Lama Dari Cache." },
  { "en": "Apa Itu LRU (Least Recently Used)?", "id": "Kebijakan (Policy) Ganti Data Paling Lama Digunakan." },
  { "en": "Apa Itu MMU (Memory Management Unit)?", "id": "Unit (Unit) Pengelola Alamat Memori (Memory)." },
  { "en": "Apa Fungsi MMU (Memory Management Unit)?", "id": "Menerjemahkan (Translate) Alamat Virtual Ke Fisik." },
  { "en": "Apa Itu Memori Virtual (Virtual Memory)?", "id": "Teknik (Technique) Gunakan Disk (Disk) Sebagai RAM (Random Access Memory) Tambahan." },
  { "en": "Apa Itu Page Fault (Kesalahan Halaman)?", "id": "Data (Data) Virtual Diminta Tidak Ada RAM (Random Access Memory)." },
  { "en": "Apa Itu TLB (Translation Lookaside Buffer)?", "id": "Cache (Singgahan) Cepat Untuk Translasi Alamat MMU." },
  { "en": "Apa Itu Inti (Core) CPU (Central Processing Unit)?", "id": "Unit (Unit) Pemroses Independen Dalam CPU (Central Processing Unit)." },
  { "en": "Apa Itu Multicore (Multi-Inti) Prosesor?", "id": "Prosesor (Processor) Dengan Dua (Two) Atau Lebih Inti." },
  { "en": "Apa Itu Multithreading (Multi-Umpan) Simultan?", "id": "Satu (One) Inti CPU (Central Processing Unit) Eksekusi Banyak Thread." },
  { "en": "Apa Itu ECC (Error-Correcting Code) Memori?", "id": "Kode (Code) Deteksi Koreksi Kesalahan Data Memori." },
  { "en": "Di Mana Memori ECC (Error-Correcting Code) Digunakan?", "id": "Server (Server), Workstation (Workstation), Aplikasi Kritis." },
  { "en": "Apa Itu Wear Leveling (Perataan Keausan)?", "id": "Teknik (Technique) Distribusi Penulisan Data Memori Flash." },
  { "en": "Mengapa Wear Leveling (Perataan Keausan) Penting?", "id": "Memperpanjang (Extend) Umur (Lifetime) Sel Memori Flash." },
  { "en": "Apa Itu Over-Provisioning SSD (Solid State Drive)?", "id": "Area (Area) Cadangan SSD (Solid State Drive) Manajemen Flash." },
  { "en": "Apa Itu Garbage Collection (Pengumpulan Sampah) SSD?", "id": "Proses (Process) Mengoptimalkan Blok (Block) Memori Flash." },
  { "en": "Apa Itu Leadframe (Rangka Kaki) Kemasan IC?", "id": "Rangka (Frame) Logam Internal Kaki (Pin) IC." },
  { "en": "Bahan Apa Untuk Kawat Bond (Bond Wire)?", "id": "Emas (Gold), Tembaga (Copper), Atau Aluminium (Aluminum)." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance) IC?", "id": "Ukuran (Measure) Hambatan Aliran Panas Dari Chip." },
  { "en": "Apa Satuan Resistansi Termal (Thermal Resistance)?", "id": "Derajat (Degrees) Celcius Per Watt (°C/W)." },
  { "en": "Apa Itu Junction Temperature (Suhu Sambungan) (Tj)?", "id": "Suhu (Temperature) Operasi Internal Inti Silikon." },
  { "en": "Apa Itu Ambient Temperature (Suhu Sekitar) (Ta)?", "id": "Suhu (Temperature) Udara Sekitar Perangkat." },
  { "en": "Apa Itu Sinyal Integritas (Signal Integrity)?", "id": "Kualitas (Quality) Sinyal Listrik Pada Jalur PCB." },
  { "en": "Apa Itu Crosstalk (Bicara Silang) Sinyal?", "id": "Interferensi (Interference) Sinyal Antar Jalur Berdekatan." },
  { "en": "Apa Itu Refleksi (Reflection) Sinyal?", "id": "Sinyal (Signal) Memantul Balik Akibat Mismatch Impedansi." },
  { "en": "Apa Itu Mismatch (Ketidakcocokan) Impedansi?", "id": "Perbedaan (Difference) Impedansi Jalur (Trace) Dan Beban (Load)." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance)?", "id": "Impedansi (Impedance) Jalur Transmisi (Misal: 50 Ohm)." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Menyamakan (Matching) Impedansi Sumber (Source) Jalur Beban." },
  { "en": "Apa Itu Resistor Terminator (Termination Resistor)?", "id": "Resistor (Resistor) Pencegah Refleksi (Reflection) Ujung Jalur." },
  { "en": "Apa Itu Sensor Efek Hall (Hall Effect Sensor)?", "id": "Sensor (Sensor) Pendeteksi Medan Magnet." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor) IC?", "id": "IC (Integrated Circuit) Pengukur Suhu (Temperature) Lingkungan." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor (Sensor) Suhu (Temperature) Dua (Two) Logam Berbeda." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor (Sensor) Suhu (Temperature) Berbasis Resistansi Logam." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor (Resistor) Yang Nilainya Sensitif Suhu." },
  { "en": "Apa Itu NTC (Negative Temperature Coefficient) Thermistor?", "id": "Resistansi (Resistance) Turun Saat Suhu (Temperature) Naik." },
  { "en": "Apa Itu PTC (Positive Temperature Coefficient) Thermistor?", "id": "Resistansi (Resistance) Naik Saat Suhu (Temperature) Naik." },
  { "en": "Apa Itu Sensor Tekanan (Pressure Sensor) MEMS?", "id": "Sensor (Sensor) MEMS (Micro-Electro-Mechanical Systems) Pengukur Tekanan Fluida." },
  { "en": "Apa Itu Piezoresistif (Piezoresistive)?", "id": "Perubahan (Change) Resistansi Akibat Tekanan Mekanis." },
  { "en": "Apa Itu IC (Integrated Circuit) Penguat Audio?", "id": "IC (Integrated Circuit) Penguat Sinyal Audio Daya Rendah." },
  { "en": "Apa Itu Penguat Audio Kelas A?", "id": "Transistor (Transistor) Output Selalu Aktif (On)." },
  { "en": "Apa Itu Penguat Audio Kelas B?", "id": "Dua (Two) Transistor (Transistor) Output Bekerja Bergantian." },
  { "en": "Apa Itu Penguat Audio Kelas AB?", "id": "Kombinasi (Combination) Kelas (Class) A Dan Kelas B." },
  { "en": "Apa Itu Penguat Audio Kelas D?", "id": "Penguat (Amplifier) Switching (Switching) Efisiensi Tinggi." },
  { "en": "Apa Itu THD (Total Harmonic Distortion)?", "id": "Ukuran (Measure) Distorsi (Distortion) Sinyal Audio." },
  { "en": "Apa Itu IC (Integrated Circuit) Codec Audio?", "id": "Kombinasi (Combination) ADC (Analog-to-Digital Converter) DAC (Digital-to-Analog Converter) Audio." },
  { "en": "Apa Itu DSP (Digital Signal Processor)?", "id": "Mikroprosesor (Microprocessor) Khusus Pemrosesan Sinyal Digital." },
  { "en": "Apa Fungsi DSP (Digital Signal Processor)?", "id": "Filtering (Filtering), Kompresi (Compression), Efek (Effect) Audio." },
  { "en": "Apa Itu Reticle (Retikel) Fabrikasi IC?", "id": "Masker (Mask) Kaca Ukuran Besar Pola Rangkaian." },
  { "en": "Apa Itu Stepper (Pemindah) Litografi?", "id": "Mesin (Machine) Proyeksi Pola Reticle (Reticle) Ke Wafer." },
  { "en": "Apa Itu DUV (Deep Ultraviolet) Lithography?", "id": "Litografi (Lithography) Menggunakan Sinar Ultra-Violet Jauh." },
  { "en": "Apa Itu EUV (Extreme Ultraviolet) Lithography?", "id": "Litografi (Lithography) Menggunakan Sinar Ultra-Violet Ekstrem." },
  { "en": "Mengapa Perlu EUV (Extreme Ultraviolet) Lithography?", "id": "Untuk (For) Membuat Transistor (Transistor) Sangat Kecil." },
  { "en": "Apa Itu Analisis Kegagalan (Failure Analysis) (FA)?", "id": "Proses (Process) Investigasi Penyebab Kerusakan IC (IC)." },
  { "en": "Apa Itu SEM (Scanning Electron Microscope)?", "id": "Mikroskop (Microscope) Elektron Pindai Gambar Resolusi Tinggi." },
  { "en": "Apa Itu FIB (Focused Ion Beam)?", "id": "Mesin (Machine) Pengguna Ion (Ion) Memotong Melihat IC." },
  { "en": "Apa Itu Decapsulation (Dekapsulasi) IC?", "id": "Proses (Process) Membuka Kemasan (Package) IC (IC) Analisis." },
  { "en": "Apa Itu Testbench (Bangku Uji) Verifikasi?", "id": "Kode (Code) Simulasi (Simulation) Penguji Desain RTL (Register-Transfer Level)." },
  { "en": "Apa Itu Cakupan Kode (Code Coverage)?", "id": "Metrik (Metric) Seberapa Banyak Kode (Code) RTL (Register-Transfer Level) Diuji." },
  { "en": "Apa Itu Cakupan Fungsional (Functional Coverage)?", "id": "Metrik (Metric) Seberapa Banyak Fitur (Feature) Desain Diuji." },
  { "en": "Apa Itu Assertion (Pernyataan) Verifikasi?", "id": "Pemeriksaan (Check) Aturan (Rule) Perilaku Desain Selama Simulasi." },
  { "en": "Apa Itu UVM (Universal Verification Methodology)?", "id": "Metodologi (Methodology) Standar (Standard) Verifikasi Desain IC (IC)." },
  { "en": "Apa Itu Transaksi (Transaction) Verifikasi?", "id": "Objek (Object) Data Level Tinggi (High-Level) Pengujian." },
  { "en": "Apa Itu Sekuenser (Sequencer) UVM?", "id": "Komponen (Component) UVM (Universal Verification Methodology) Pembangkit Stimulus." },
  { "en": "Apa Itu Driver (Pengemudi) UVM?", "id": "Komponen (Component) UVM (Universal Verification Methodology) Penggerak Sinyal Desain." },
  { "en": "Apa Itu Monitor (Pemantau) UVM?", "id": "Komponen (Component) UVM (Universal Verification Methodology) Pengamat Sinyal Desain." },
  { "en": "Apa Itu Papan Skor (Scoreboard) UVM?", "id": "Komponen (Component) UVM (Universal Verification Methodology) Pemeriksa Kebenaran Output." },
  { "en": "Apa Itu Agen (Agent) UVM?", "id": "Kumpulan (Collection) Driver (Driver), Monitor (Monitor), Sekuenser (Sequencer)." },
  { "en": "Apa Itu Lingkungan (Environment) UVM?", "id": "Komponen (Component) UVM (Universal Verification Methodology) Level Atas Pengujian." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface) Flash?", "id": "Memori (Memory) Flash (Flash) Antarmuka (Interface) SPI (Serial Peripheral Interface)." },
  { "en": "Apa Itu QSPI (Quad SPI)?", "id": "SPI (Serial Peripheral Interface) Empat (Four) Jalur Data Cepat." },
  { "en": "Apa Itu OSPI (Octal SPI)?", "id": "SPI (Serial Peripheral Interface) Delapan (Eight) Jalur Data." },
  { "en": "Apa Itu HyperBus (Bus Hiper)?", "id": "Antarmuka (Interface) Memori (Memory) Paralel Kecepatan Tinggi." },
  { "en": "Apa Itu PSRAM (Pseudo-Static Random Access Memory)?", "id": "DRAM (Dynamic RAM) Internal Refresh (Refresh) Mirip SRAM (Static RAM)." },
  { "en": "Apa Itu MPU (Memory Protection Unit)?", "id": "Unit (Unit) Pelindung Memori (Memory) Sederhana Mikrokontroler." },
  { "en": "Apa Beda MPU (Memory Protection Unit) Dan MMU (Memory Management Unit)?", "id": "MPU (Memory Protection Unit) Proteksi, MMU (Memory Management Unit) Translasi Virtual." },
  { "en": "Apa Itu TrustZone (Zona Terpercaya)?", "id": "Teknologi (Technology) Keamanan Perangkat Keras (Hardware) ARM (Advanced RISC Machine)." },
  { "en": "Apa Itu Secure Boot (Boot Aman)?", "id": "Proses (Process) Verifikasi Keaslian Perangkat Lunak (Software) Saat Boot." },
  { "en": "Apa Itu Kriptografi (Cryptography) Perangkat Keras?", "id": "Akselerator (Accelerator) IC (IC) Operasi Enkripsi Dekripsi." },
  { "en": "Apa Itu AES (Advanced Encryption Standard)?", "id": "Standar (Standard) Enkripsi (Encryption) Simetris Populer." },
  { "en": "Apa Itu SHA (Secure Hash Algorithm)?", "id": "Algoritma (Algorithm) Fungsi Hash (Hash) Kriptografi." },
  { "en": "Apa Itu RSA (Rivest–Shamir–Adleman)?", "id": "Algoritma (Algorithm) Kriptografi (Cryptography) Kunci Publik." },
  { "en": "Apa Itu TRNG (True Random Number Generator)?", "id": "Pembangkit (Generator) Angka Acak Sejati Perangkat Keras." },
  { "en": "Apa Itu PUF (Physical Unclonable Function)?", "id": "Fungsi (Function) Fisik Unik IC (IC) Kunci Keamanan." },
  { "en": "Apa Itu Efek Saluran Pendek (Short-Channel Effect)?", "id": "Masalah (Problem) Fisika Transistor (Transistor) Sangat Kecil." },
  { "en": "Apa Itu DIBL (Drain-Induced Barrier Lowering)?", "id": "Efek (Effect) Saluran PendekTegangan Drain (Drain) Pengaruhi Gate." },
  { "en": "Apa Itu Hot Carrier Injection (HCI)?", "id": "Elektron (Electron) Energi Tinggi Rusak Gerbang Oksida." },
  { "en": "Apa Itu NBTI (Negative Bias Temperature Instability)?", "id": "Degradasi (Degradation) Transistor (Transistor) PMOS (P-channel MOS) Seiring Waktu." },
  { "en": "Apa Itu PBTI (Positive Bias Temperature Instability)?", "id": "Degradasi (Degradation) Transistor (Transistor) NMOS (N-channel MOS) Seiring Waktu." },
  { "en": "Apa Itu Time-Dependent Dielectric Breakdown (TDDB)?", "id": "Kerusakan (Breakdown) Isolator (Isolator) Oksida Gerbang." },
  { "en": "Apa Itu Reliability (Keandalan) IC?", "id": "Kemampuan (Ability) IC (Integrated Circuit) Berfungsi Waktu Lama." },
  { "en": "Apa Itu Bathtub Curve (Kurva Bak Mandi)?", "id": "Grafik (Graph) Tingkat Kegagalan IC (Integrated Circuit) Seiring Waktu." },
  { "en": "Apa Itu Infant Mortality (Kematian Bayi) IC?", "id": "Kegagalan (Failure) Dini IC (Integrated Circuit) Akibat Cacat Produksi." },
  { "en": "Apa Itu Burn-In Test (Uji Bakar)?", "id": "Pengujian (Test) IC (Integrated Circuit) Suhu Tinggi Singkirkan Kegagalan Dini." },
  { "en": "Apa Itu Wear-Out (Keausan) IC?", "id": "Kegagalan (Failure) IC (Integrated Circuit) Akhir Umur (Lifetime) Pakai." },
  { "en": "Apa Itu MTTF (Mean Time To Failure)?", "id": "Rata-Rata (Average) Waktu Operasi Sebelum Gagal." },
  { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Rata-Rata (Average) Waktu Antara Kegagalan (Failure)." },
  { "en": "Apa Itu FIT (Failure in Time)?", "id": "Satuan (Unit) Tingkat Kegagalan (Failure) IC (Integrated Circuit)." },
  { "en": "Apa Nilai Satu (1) FIT (Failure in Time)?", "id": "Satu (One) Kegagalan (Failure) Per Miliar Jam Operasi." },
  { "en": "Apa Itu Akselerasi Uji (Test Acceleration)?", "id": "Mempercepat (Speed Up) Uji Keandalan (Reliability) Suhu Tegangan." },
  { "en": "Apa Itu Model Arrhenius?", "id": "Model (Model) Matematis Hubungan Suhu (Temperature) Kegagalan (Failure)." },
  { "en": "Apa Itu Koefisien Ekspansi Termal (CTE)?", "id": "Ukuran (Measure) Perubahan Ukuran Bahan Akibat Suhu." },
  { "en": "Mengapa Perbedaan CTE (Coefficient of Thermal Expansion) Penting?", "id": "Dapat (Can) Menyebabkan Stres (Stress) Mekanis Pada IC (IC)." },
  { "en": "Apa Itu Kemasan (Package) Keramik IC?", "id": "Kemasan (Package) Kinerja Tinggi (High Performance) Tahan Suhu." },
  { "en": "Apa Itu Kemasan (Package) Hermetik?", "id": "Kemasan (Package) IC (Integrated Circuit) Kedap Udara Lingkungan." },
  { "en": "Apa Itu Kemasan (Package) Non-Hermetik?", "id": "Kemasan (Package) IC (Integrated Circuit) Plastik (Plastic) Umum." },
  { "en": "Apa Itu Papan Sirkuit Fleksibel (Flex Circuit)?", "id": "PCB (Printed Circuit Board) Fleksibel (Flexible) Yang Bisa Ditekuk." },
  { "en": "Apa Itu Chip-on-Board (COB)?", "id": "Die (Die) IC (Integrated Circuit) Dipasang Langsung PCB (Printed Circuit Board)." },
  { "en": "Apa Itu Glob-Top (Gumpalan Atas)?", "id": "Enkapsulasi (Encapsulation) Epoksi (Epoxy) Pelindung COB (Chip-on-Board)." },
  { "en": "Apa Itu Flip-Chip BGA (Ball Grid Array)?", "id": "Die (Die) IC (Integrated Circuit) Dibalik Terhubung Bola Solder." },
  { "en": "Apa Itu Underfill (Pengisi Bawah)?", "id": "Epoksi (Epoxy) Pengisi Celah Antara Flip-Chip PCB (PCB)." },
  { "en": "Apa Itu Through-Silicon Via (TSV)?", "id": "Koneksi (Connection) Vertikal Tembus Wafer Silikon." },
  { "en": "Apa Itu IC (Integrated Circuit) Tiga Dimensi (3D)?", "id": "Beberapa (Multiple) Die (Die) Ditumpuk (Stacked) Vertikal." },
  { "en": "Keuntungan IC (Integrated Circuit) Tiga Dimensi (3D)?", "id": "Kepadatan (Density) Tinggi, Jalur (Path) Sinyal Pendek." },
  { "en": "Apa Itu System-in-Package (SiP)?", "id": "Banyak (Multiple) Chip (Chip) Berbeda Dalam Satu Kemasan." },
  { "en": "Apa Beda SiP (System-in-Package) Dan SoC (System on a Chip)?", "id": "SiP (System-in-Package) Banyak Chip, SoC (System on a Chip) Satu Chip." },
  { "en": "Apa Itu Interposer (Penyela) Silikon?", "id": "Lapisan (Layer) Silikon Koneksi Antar Chip (Chip) SiP (SiP)." },
  { "en": "Apa Itu IC (Integrated Circuit) RF (Radio Frequency) Front-End?", "id": "Bagian (Part) Rangkaian RF (Radio Frequency) Antara Antena Prosesor." },
  { "en": "Apa Itu LNA (Low-Noise Amplifier)?", "id": "Penguat (Amplifier) Sinyal RF (Radio Frequency) Lemah Kebisingan Rendah." },
  { "en": "Apa Itu PA (Power Amplifier) RF?", "id": "Penguat (Amplifier) Sinyal RF (Radio Frequency) Daya Tinggi Transmit." },
  { "en": "Apa Itu Mixer (Pencampur) RF (Radio Frequency)?", "id": "Mengubah (Convert) Sinyal (Signal) Antar Frekuensi Berbeda." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator (Oscillator) Frekuensi (Frequency) Diatur Tegangan (Voltage)." },
  { "en": "Apa Itu Sintesis Frekuensi (Frequency Synthesizer)?", "id": "Rangkaian (Circuit) Pembangkit (Generator) Frekuensi (Frequency) Stabil Tepat." },
  { "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?", "id": "Filter (Filter) RF (Radio Frequency) Gelombang Akustik Permukaan." },
  { "en": "Apa Itu Filter BAW (Bulk Acoustic Wave)?", "id": "Filter (Filter) RF (Radio Frequency) Kinerja Tinggi (High Performance)." },
  { "en": "Apa Itu Duplekser (Duplexer)?", "id": "Filter (Filter) Pemisah Sinyal Transmit (Kirim) Receive (Terima)." },
  { "en": "Apa Itu Sirkulator (Circulator) RF (Radio Frequency)?", "id": "Perangkat (Device) Tiga (Three) Port (Port) Pengarah Sinyal." },
  { "en": "Apa Itu Isolator (Isolator) RF (Radio Frequency)?", "id": "Perangkat (Device) Dua (Two) Port (Port) Sinyal Satu Arah." },
  { "en": "Apa Itu Parameter S (S-Parameter)?", "id": "Metrik (Metric) Karakterisasi Jaringan RF (Radio Frequency)." },
  { "en": "Apa Itu Gain (Penguatan) RF (Radio Frequency)?", "id": "Peningkatan (Increase) Daya (Power) Sinyal (Signal) RF (Radio Frequency)." },
  { "en": "Apa Itu Titik Kompresi P1dB?", "id": "Titik (Point) Penguat (Amplifier) Mulai Non-Linear (Non-Linear)." },
  { "en": "Apa Itu Titik Intersep Orde Tiga (IP3)?", "id": "Metrik (Metric) Ukuran Linearitas (Linearity) Rangkaian RF (RF)." },
  { "en": "Apa Itu Kebisingan Fasa (Phase Noise)?", "id": "Ketidakstabilan (Instability) Fasa (Phase) Jangka Pendek Osilator (Oscillator)." },
  { "en": "Apa Itu Papan Evaluasi (Evaluation Board) (EVM)?", "id": "Papan (Board) PCB (Printed Circuit Board) Uji (Test) IC (IC) Tertentu." },
  { "en": "Apa Itu Desain Referensi (Reference Design)?", "id": "Contoh (Example) Desain Sirkuit (Circuit) Lengkap Pakai IC (IC)." },
  { "en": "Apa Itu Catatan Aplikasi (Application Note)?", "id": "Dokumen (Document) Panduan (Guidance) Penggunaan IC (IC) Aplikasi." },
  { "en": "Apa Itu Errata (Ralat) Datasheet?", "id": "Dokumen (Document) Koreksi (Correction) Kesalahan (Error) Pada Datasheet (Datasheet)." },
  { "en": "Apa Itu Peringkat Suhu (Temperature Rating) IC?", "id": "Rentang (Range) Suhu (Temperature) Operasi Aman IC (IC)." },
  { "en": "Apa Itu Peringkat (Rating) Komersial?", "id": "Rentang (Range) Suhu (Temperature) Nol (0) Hingga 70°C." },
  { "en": "Apa Itu Peringkat (Rating) Industri?", "id": "Rentang (Range) Suhu (Temperature) -40 Hingga 85°C." },
  { "en": "Apa Itu Peringkat (Rating) Otomotif?", "id": "Rentang (Range) Suhu (Temperature) -40 Hingga 125°C." },
  { "en": "Apa Itu Peringkat (Rating) Militer?", "id": "Rentang (Range) Suhu (Temperature) -55 Hingga 125°C." },
  { "en": "Apa Itu Kualifikasi AEC-Q100?", "id": "Standar (Standard) Keandalan (Reliability) Komponen (Component) Otomotif." },
  { "en": "Apa Itu Kualifikasi AEC-Q200?", "id": "Standar (Standard) Keandalan (Reliability) Komponen (Component) Pasif Otomotif." },
  { "en": "Apa Itu RoHS (Restriction of Hazardous Substances)?", "id": "Direktif (Directive) Pembatasan Bahan (Material) Berbahaya Elektronik." },
  { "en": "Apa Itu Bahan (Material) Bebas Timbal (Lead-Free)?", "id": "Solder (Solder) Komponen (Component) Tanpa Timbal (Lead)." },
  { "en": "Apa Itu WEEE (Waste Electrical and Electronic Equipment)?", "id": "Direktif (Directive) Penanganan Limbah (Waste) Elektronik Eropa." },
  { "en": "Apa Itu UL (Underwriters Laboratories)?", "id": "Organisasi (Organization) Sertifikasi (Certification) Keamanan (Safety) Produk." },
  { "en": "Apa Itu CE (Conformité Européenne) Marking?", "id": "Tanda (Mark) Kepatuhan (Compliance) Produk (Product) Standar (Standard) Eropa." },
  { "en": "Apa Itu FCC (Federal Communications Commission)?", "id": "Badan (Agency) Regulasi (Regulation) Komunikasi (Communication) Amerika Serikat." },
  { "en": "Apa Itu Sertifikasi (Certification) FCC (Federal Communications Commission)?", "id": "Persetujuan (Approval) Perangkat (Device) Frekuensi (Frequency) Radio (Radio) AS (USA)." },
  { "en": "Apa Itu Simbol Sensitivitas ESD (Electrostatic Discharge)?", "id": "Tanda (Symbol) Peringatan (Warning) Penanganan (Handling) Komponen (Component) Sensitif." },
  { "en": "Apa Itu Level MSL (Moisture Sensitivity Level)?", "id": "Peringkat (Rating) Sensitivitas Komponen (Component) Terhadap Kelembaban." },
  { "en": "Mengapa MSL (Moisture Sensitivity Level) Penting?", "id": "Cegah (Prevent) Kerusakan (Damage) IC (IC) Saat Penyolderan Reflow." },
  { "en": "Apa Itu Pemanggangan (Baking) IC?", "id": "Proses (Process) Pemanasan (Heating) IC (IC) Hilangkan Kelembaban (Moisture)." },
  { "en": "Apa Itu Dry Pack (Kemasan Kering)?", "id": "Kantong (Bag) Vakum (Vakum) Kedap (Airtight) Lindungi IC (IC) Lembab." },
  { "en": "Apa Itu Indikator Kelembaban (Humidity Indicator) Card?", "id": "Kartu (Card) Dalam Dry Pack (Kemasan Kering) Tunjukkan Kelembaban." },
  { "en": "Apa Itu Penyolderan Reflow (Reflow Soldering)?", "id": "Proses (Process) Penyolderan SMD (Surface Mount Device) Oven (Oven) Panas." },
  { "en": "Apa Itu Penyolderan Gelombang (Wave Soldering)?", "id": "Proses (Process) Penyolderan Komponen (Component) Through-Hole (Tembus Lubang)." },
  { "en": "Apa Itu Pasta Solder (Solder Paste)?", "id": "Campuran (Mixture) Bola (Ball) Solder (Solder) Fluks (Flux)." },
  { "en": "Apa Itu Fluks (Flux) Solder?", "id": "Bahan (Material) Kimia (Chemical) Pembersih Oksida (Oxide) Saat Solder." },
  { "en": "Apa Itu Stensil (Stencil) SMT (Surface Mount Technology)?", "id": "Lembaran (Sheet) Logam (Metal) Cetak (Print) Pasta Solder (Solder Paste) PCB." },
  { "en": "Apa Itu Mesin Pick-and-Place (Ambil-dan-Tempat)?", "id": "Mesin (Machine) Otomatis (Automatic) Penempatan Komponen (Component) SMD (SMD)." },
  { "en": "Apa Itu Pemeriksaan Optik Otomatis (AOI)?", "id": "Inspeksi (Inspection) Kualitas (Quality) Solder (Solder) PCB (PCB) Pakai Kamera." },
  { "en": "Apa Itu Pemeriksaan Sinar-X Otomatis (AXI)?", "id": "Inspeksi (Inspection) Solder (Solder) BGA (Ball Grid Array) Pakai Sinar-X." },
  { "en": "Apa Itu Pengujian In-Circuit (ICT)?", "id": "Pengujian (Test) Fungsional (Functional) PCB (PCB) Rakitan (Assembly) Jarum (Needle)." },
  { "en": "Apa Itu Pengujian Flying Probe (Probe Terbang)?", "id": "Pengujian (Test) PCB (PCB) Tanpa (Without) Fixture (Fixture) Pakai Probe (Probe) Bergerak." },
  { "en": "Apa Itu Bed of Nails (Ranjang Paku) ICT?", "id": "Fixture (Fixture) Uji (Test) ICT (In-Circuit Test) Kustom (Custom) Banyak Pin (Pin)." },
  { "en": "Apa Itu Pengujian Fungsional (Functional Test) (FCT)?", "id": "Pengujian (Test) Fungsi (Function) Akhir (Final) Papan (Board) Rakitan (Assembly)." },
  { "en": "Apa Itu Firmware (Perangkat Tegar)?", "id": "Perangkat (Software) Lunak (Software) Tertanam (Embedded) Dalam IC (IC) Mikrokontroler." },
  { "en": "Apa Itu Bootloader (Pemuat Boot)?", "id": "Program (Program) Kecil (Small) Pemuat (Loader) Firmware (Firmware) Utama." },
  { "en": "Apa Itu Pemrograman ICSP (In-Circuit Serial Programming)?", "id": "Memprogram (Programming) Mikrokontroler (Microcontroller) Terpasang (Installed) Sirkuit (Circuit)." },
  { "en": "Apa Itu Debugger (Penyahpepijat) Perangkat Keras?", "id": "Alat (Tool) Bantu (Help) Analisis (Analyze) Eksekusi (Execution) Kode (Code) Mikrokontroler." },
  { "en": "Apa Itu Antarmuka (Interface) SWD (Serial Wire Debug)?", "id": "Antarmuka (Interface) Debug (Debug) Dua (Two) Kawat (Wire) ARM (Advanced RISC Machine)." },
  { "en": "Apa Itu Titik Henti (Breakpoint)?", "id": "Titik (Point) Henti (Stop) Program (Program) Debugger (Debugger) Analisis." },
  { "en": "Apa Itu Titik Pantau (Watchpoint)?", "id": "Titik (Point) Henti (Stop) Program (Program) Saat Data (Data) Berubah." },
  { "en": "Apa Itu Analisis Jejak (Trace Analysis)?", "id": "Merekam (Record) Histori (History) Eksekusi (Execution) Program (Program) Real-Time (Real-Time)." },
  { "en": "Apa Itu Simulator (Simulator) Instruksi?", "id": "Perangkat (Software) Lunak (Software) Simulasi (Simulation) Eksekusi (Execution) Kode (Code) Mikrokontroler." },
  { "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Perangkat (Software) Lunak (Software) Penulis (Writer) Kode (Code) Kompilasi (Compile) Debug (Debug)." },
  { "en": "Apa Itu Kompilator (Compiler)?", "id": "Penerjemah (Translator) Kode (Code) Bahasa (Language) Tinggi (High) Mesin (Machine)." },
  { "en": "Apa Itu Assembler (Perakit)?", "id": "Penerjemah (Translator) Kode (Code) Assembly (Assembly) Mesin (Machine)." },
  { "en": "Apa Itu Linker (Penaut)?", "id": "Penggabung (Combiner) File (File) Objek (Object) Jadi (Become) Program (Program) Eksekutabel." },
  { "en": "Apa Itu File (Berkas) HEX?", "id": "Format (Format) File (File) Biner (Binary) Pemrograman (Programming) Mikrokontroler." },
  { "en": "Apa Itu S-Record (Catatan S) Motorola?", "id": "Format (Format) File (File) Biner (Binary) Teks (Text) ASCII (ASCII)." },
  { "en": "Apa Itu Endianness (Endianitas)?", "id": "Urutan (Order) Byte (Byte) Penyimpanan Data (Data) Memori (Memory)." },
  { "en": "Apa Itu Big-Endian (Endian Besar)?", "id": "Byte (Byte) Paling (Most) Signifikan (Significant) Disimpan (Stored) Pertama (First)." },
  { "en": "Apa Itu Little-Endian (Endian Kecil)?", "id": "Byte (Byte) Paling (Most) Tidak (Least) Signifikan (Significant) Disimpan (Stored) Pertama (First)." },
  { "en": "Apa Itu Mobilitas (Mobility) Pembawa Muatan?", "id": "Kemudahan (Ease) Pembawa (Carrier) Muatan (Charge) Bergerak." },
  { "en": "Mana Lebih Cepat, Elektron (Electron) Atau Lubang (Hole)?", "id": "Elektron (Electron) Bergerak (Move) Lebih Cepat." },
  { "en": "Apa Itu Resistivitas (Resistivity) Semikonduktor?", "id": "Hambatan (Resistance) Bahan (Material) Terhadap Arus (Current) Listrik." },
  { "en": "Bagaimana Doping (Doping) Mempengaruhi Resistivitas (Resistivity)?", "id": "Doping (Doping) Menurunkan (Decrease) Resistivitas (Resistivity)." },
  { "en": "Apa Itu Difusi (Diffusion) Dalam Fabrikasi?", "id": "Proses (Process) Doping (Doping) Menggunakan Suhu (Temperature) Tinggi." },
  { "en": "Apa Itu Oksidasi Basah (Wet Oxidation)?", "id": "Oksidasi (Oxidation) Silikon (Silicon) Menggunakan Uap (Steam) Air." },
  { "en": "Apa Itu Oksidasi Kering (Dry Oxidation)?", "id": "Oksidasi (Oxidation) Silikon (Silicon) Menggunakan Oksigen (Oxygen) Murni." },
  { "en": "Mana Lebih Cepat, Oksidasi Basah (Wet) Kering (Dry)?", "id": "Oksidasi Basah (Wet Oxidation) Tumbuh (Grow) Lebih Cepat." },
  { "en": "Mana Kualitas Oksida (Oxide) Lebih Baik?", "id": "Oksidasi Kering (Dry Oxidation) Kualitas (Quality) Lebih Baik." },
  { "en": "Apa Itu Sel Standar (Standard Cell) Desain?", "id": "Blok (Block) Bangunan (Building) Logika (Logic) Desain (Design) Digital." },
  { "en": "Contoh (Example) Sel Standar (Standard Cell)?", "id": "Gerbang (Gate) NAND (NAND), Gerbang (Gate) NOR (NOR), Flip-Flop (Flip-Flop)." },
  { "en": "Apa Itu Desain (Design) Full-Custom (Kustom Penuh)?", "id": "Desain (Design) Tata Letak (Layout) Manual (Manual) Setiap Transistor." },
  { "en": "Kapan Desain (Design) Full-Custom (Kustom Penuh) Digunakan?", "id": "Untuk (For) Kinerja (Performance) Tinggi (High) Rangkaian (Circuit) Analog." },
  { "en": "Apa Itu DRC (Design Rule Check)?", "id": "Pemeriksaan (Check) Aturan (Rule) Manufaktur (Manufacturing) Tata Letak (Layout)." },
  { "en": "Apa Itu LVS (Layout Versus Schematic)?", "id": "Pemeriksaan (Check) Kesesuaian (Compliance) Tata Letak (Layout) Skematik." },
  { "en": "Apa Itu Ekstraksi Parasitik (Parasitic Extraction)?", "id": "Menghitung (Calculate) Resistor (Resistor) Kapasitor (Capacitor) Parasitik (Parasitic)." },
  { "en": "Apa Itu Analisis Waktu (Timing Analysis) Statis?", "id": "Memeriksa (Check) Waktu (Timing) Tunda (Delay) Rangkaian (Circuit) Digital." },
  { "en": "Apa Itu Jalur Kritis (Critical Path)?", "id": "Jalur (Path) Sinyal (Signal) Terpanjang (Longest) Dalam Desain (Design)." },
  { "en": "Apa Itu Setup Time (Waktu Pengaturan)?", "id": "Data (Data) Harus Stabil (Stable) Sebelum (Before) Tepi Jam." },
  { "en": "Apa Itu Hold Time (Waktu Tahan)?", "id": "Data (Data) Harus Stabil (Stable) Setelah (After) Tepi Jam." },
  { "en": "Apa Itu Pelanggaran (Violation) Setup Time?", "id": "Data (Data) Datang (Come) Terlalu (Too) Lambat (Late)." },
  { "en": "Apa Itu Pelanggaran (Violation) Hold Time?", "id": "Data (Data) Berubah (Change) Terlalu (Too) Cepat (Fast)." },
  { "en": "Apa Itu Resistansi Lembar (Sheet Resistance)?", "id": "Resistansi (Resistance) Lapisan (Layer) Tipis (Thin) Bahan (Material)." },
  { "en": "Apa Satuan (Unit) Resistansi Lembar (Sheet Resistance)?", "id": "Ohm (Ohm) Per Persegi (Per Square) (Ω/sq)." },
  { "en": "Apa Itu Kontak (Contact) Fabrikasi IC?", "id": "Lubang (Hole) Koneksi (Connection) Logam (Metal) Ke Silikon (Silicon)." },
  { "en": "Apa Itu Via (Jalur Vertikal)?", "id": "Lubang (Hole) Koneksi (Connection) Antar (Between) Lapisan (Layer) Logam (Metal)." },
  { "en": "Apa Itu Logam (Metal) Interkoneksi?", "id": "Jalur (Trace) Logam (Metal) Penghubung (Connector) Komponen (Component) IC (IC)." },
  { "en": "Bahan (Material) Apa Interkoneksi (Interconnect) Modern?", "id": "Tembaga (Copper, Cu)." },
  { "en": "Mengapa Tembaga (Copper) Menggantikan Aluminium (Aluminum)?", "id": "Resistivitas (Resistivity) Rendah (Low) Tahan (Resist) Elektromigrasi (Electromigration)." },
  { "en": "Apa Itu Bahan (Material) Dielektrik (Dielectric) Low-K?", "id": "Isolator (Isolator) Mengurangi (Reduce) Kapasitansi (Capacitance) Parasitik (Parasitic)." },
  { "en": "Apa Itu Penyangga (Buffer) Jam (Clock)?", "id": "Penguat (Amplifier) Sinyal (Signal) Jam (Clock) Jaringan (Network) Distribusi." },
  { "en": "Apa Itu Metastabilitas (Metastability)?", "id": "Kondisi (Condition) Tidak Stabil (Unstable) Flip-Flop (Flip-Flop) Transien." },
  { "en": "Kapan Metastabilitas (Metastability) Terjadi?", "id": "Saat (When) Pelanggaran (Violation) Setup (Setup) Hold (Hold) Time (Time)." },
  { "en": "Apa Itu Sinkroniser (Synchronizer)?", "id": "Rangkaian (Circuit) Pengurang (Reducer) Risiko (Risk) Metastabilitas (Metastability)." },
  { "en": "Bagaimana (How) Desain (Design) Sinkroniser (Synchronizer)?", "id": "Menggunakan (Using) Dua (Two) Atau Lebih Flip-Flop (Flip-Flop) Seri." },
  { "en": "Apa Itu FIFO (First-In First-Out)?", "id": "Memori (Memory) Antrian (Queue), Data (Data) Masuk (In) Pertama (First) Keluar (Out) Pertama." },
  { "en": "Apa Itu LIFO (Last-In First-Out)?", "id": "Memori (Memory) Tumpukan (Stack), Data (Data) Masuk (In) Terakhir (Last) Keluar (Out) Pertama." },
  { "en": "Apa Itu Penunjuk (Pointer) Baca (Read) FIFO?", "id": "Menunjuk (Point) Alamat (Address) Data (Data) Akan (Will Be) Dibaca (Read)." },
  { "en": "Apa Itu Penunjuk (Pointer) Tulis (Write) FIFO?", "id": "Menunjuk (Point) Alamat (Address) Data (Data) Akan (Will Be) Ditulis (Written)." },
  { "en": "Apa Itu Kondisi (Condition) FIFO (First-In First-Out) Penuh?", "id": "Penunjuk (Pointer) Tulis (Write) Menyusul (Catch Up) Penunjuk (Pointer) Baca (Read)." },
  { "en": "Apa Itu Kondisi (Condition) FIFO (First-In First-Out) Kosong?", "id": "Penunjuk (Pointer) Baca (Read) Menyusul (Catch Up) Penunjuk (Pointer) Tulis (Write)." },
  { "en": "Apa Itu Penyeberangan Domain Jam (Clock Domain Crossing)?", "id": "Transfer (Transfer) Data (Data) Antar (Between) Domain (Domain) Jam (Clock) Berbeda." },
  { "en": "Apa Masalah (Problem) CDC (Clock Domain Crossing)?", "id": "Risiko (Risk) Tinggi (High) Metastabilitas (Metastability)." },
  { "en": "Apa Itu Kode (Code) Gray (Gray) CDC (Clock Domain Crossing)?", "id": "Menggunakan (Using) Kode (Code) Gray (Gray) Penunjuk (Pointer) FIFO (FIFO)." },
  { "en": "Mengapa Pakai Kode (Code) Gray (Gray) CDC (Clock Domain Crossing)?", "id": "Hanya (Only) Satu (One) Bit (Bit) Berubah (Change) Setiap Waktu." },
  { "en": "Apa Itu Handshake (Jabat Tangan) Dua Arah?", "id": "Protokol (Protocol) Sinyal (Signal) Req (Request) Ack (Acknowledge) Transfer (Transfer) Data." },
  { "en": "Apa Itu IC (Integrated Circuit) Penstabil (Regulator) Arus?", "id": "IC (Integrated Circuit) Menjaga (Maintain) Arus (Current) Output (Output) Konstan." },
  { "en": "Di Mana Penstabil (Regulator) Arus Digunakan?", "id": "Penggerak (Driver) LED (Light Emitting Diode), Pengisi (Charger) Baterai." },
  { "en": "Apa Itu Penguat (Amplifier) Audio Jembatan (Bridge)?", "id": "BTL (Bridge-Tied Load), Menggerakkan (Drive) Beban (Load) Diferensial." },
  { "en": "Kelebihan (Advantage) Penguat (Amplifier) BTL (Bridge-Tied Load)?", "id": "Daya (Power) Output (Output) Empat (Four) Kali Lipat (Times)." },
  { "en": "Apa Itu Osilator (Oscillator) RC (Resistor-Capacitor)?", "id": "Osilator (Oscillator) Menggunakan (Using) Jaringan (Network) Resistor (Resistor) Kapasitor (Capacitor)." },
  { "en": "Apa Itu Osilator (Oscillator) Pergeseran Fasa (Phase-Shift)?", "id": "Tipe (Type) Osilator (Oscillator) RC (Resistor-Capacitor) Menggunakan (Using) Umpan Balik (Feedback)." },
  { "en": "Apa Itu Osilator (Oscillator) Jembatan Wien (Wien Bridge)?", "id": "Tipe (Type) Osilator (Oscillator) RC (Resistor-Capacitor) Gelombang (Wave) Sinus (Sine)." },
  { "en": "Apa Itu Osilator (Oscillator) Cincin (Ring Oscillator)?", "id": "Rantai (Chain) Inverter (Inverter) Ganjil (Odd) Umpan Balik (Feedback)." },
  { "en": "Apa Fungsi Osilator (Oscillator) Cincin (Ring)?", "id": "Pengujian (Test) Chip (Chip), Pembangkit (Generator) Jam (Clock) Sederhana." },
  { "en": "Apa Itu Penjepit (Clamp) Dioda (Diode)?", "id": "Rangkaian (Circuit) Pembatas (Limiter) Tegangan (Voltage) Sinyal (Signal)." },
  { "en": "Apa Itu Pengganda Tegangan (Voltage Doubler)?", "id": "Rangkaian (Circuit) Dioda (Diode) Kapasitor (Capacitor) Lipat (Double) Tegangan (Voltage)." },
  { "en": "Apa Itu Pompa Muatan (Charge Pump)?", "id": "IC (Integrated Circuit) Penaik (Boost) Tegangan (Voltage) DC (Direct Current) Kapasitor." },
  { "en": "Di Mana Pompa Muatan (Charge Pump) Digunakan?", "id": "Memori (Memory) Flash (Flash), Driver (Driver) LCD (Liquid Crystal Display)." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Jalur (Trace) PCB (Printed Circuit Board) Frekuensi (Frequency) Tinggi (High)." },
  { "en": "Kapan Jalur (Trace) Dianggap Saluran Transmisi (Transmission Line)?", "id": "Saat (When) Waktu (Time) Tunda (Delay) Jalur (Trace) Signifikan." },
  { "en": "Apa Itu Microstrip (Mikrostrip)?", "id": "Jalur (Trace) Saluran Transmisi (Transmission Line) Di Atas (Top) Ground Plane." },
  { "en": "Apa Itu Stripline (Strip Jalur)?", "id": "Jalur (Trace) Saluran Transmisi (Transmission Line) Di Antara (Between) Dua (Two) Ground Plane." },
  { "en": "Apa Itu Ground Plane (Bidang Tanah)?", "id": "Lapisan (Layer) Tembaga (Copper) Luas (Large) PCB (PCB) Referensi (Reference) Nol (0) Volt." },
  { "en": "Manfaat (Benefit) Ground Plane (Bidang Tanah)?", "id": "Mengurangi (Reduce) Kebisingan (Noise), Kontrol (Control) Impedansi (Impedance)." },
  { "en": "Apa Itu Ground Bounce (Pantulan Tanah)?", "id": "Lonjakan (Spike) Tegangan (Voltage) Jalur (Path) Ground (Ground) Tidak Sempurna." },
  { "en": "Bagaimana (How) Mencegah (Prevent) Ground Bounce (Pantulan Tanah)?", "id": "Desain (Design) Ground (Ground) Baik (Good), Kapasitor (Capacitor) Bypass (Bypass)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Bypass (Pintas)?", "id": "Kapasitor (Capacitor) Kecil (Small) Dekat (Near) Pin (Pin) Daya (Power) IC (IC)." },
  { "en": "Fungsi (Function) Kapasitor (Capacitor) Bypass (Pintas)?", "id": "Menyaring (Filter) Kebisingan (Noise) Frekuensi (Frequency) Tinggi (High) Daya (Power)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Decoupling (Pemisah)?", "id": "Istilah (Term) Lain (Other) Kapasitor (Capacitor) Bypass (Pintas)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Bulk (Grosir)?", "id": "Kapasitor (Capacitor) Besar (Large) Penyimpan (Storage) Energi (Energy) Catu (Power) Daya (Supply)." },
  { "en": "Apa Itu Ferrite Bead (Manik Ferit)?", "id": "Komponen (Component) Pasif (Passive) Penekan (Suppressor) Kebisingan (Noise) EMI (EMI)." },
  { "en": "Bagaimana (How) Ferrite Bead (Manik Ferit) Bekerja?", "id": "Bekerja (Work) Seperti (Like) Induktor (Inductor) Frekuensi (Frequency) Tinggi (High)." },
  { "en": "Apa Itu Common-Mode Choke (Cekik Mode-Umum)?", "id": "Filter (Filter) Penekan (Suppressor) Kebisingan (Noise) Common-Mode (Mode-Umum)." },
  { "en": "Apa Itu Kebisingan (Noise) Common-Mode (Mode-Umum)?", "id": "Kebisingan (Noise) Sama (Same) Pada (On) Dua (Two) Jalur (Line) Sinyal (Signal)." },
  { "en": "Apa Itu Kebisingan (Noise) Differential-Mode (Mode-Diferensial)?", "id": "Kebisingan (Noise) Berlawanan (Opposite) Pada (On) Dua (Two) Jalur (Line) Sinyal (Signal)." },
  { "en": "Apa Itu Analisis (Analysis) Integritas Daya (Power Integrity)?", "id": "Analisis (Analysis) Kualitas (Quality) Jaringan (Network) Distribusi (Distribution) Daya (Power)." },
  { "en": "Apa Itu PDN (Power Distribution Network)?", "id": "Jaringan (Network) Distribusi (Distribution) Daya (Power) PCB (PCB) IC (IC)." },
  { "en": "Apa Itu Penurunan (Drop) IR (IR Drop)?", "id": "Kehilangan (Loss) Tegangan (Voltage) Akibat (Due To) Resistansi (Resistance) Jalur (Trace) Daya (Power)." },
  { "en": "Bagaimana (How) Mengurangi (Reduce) Penurunan (Drop) IR (IR Drop)?", "id": "Memperlebar (Widen) Jalur (Trace) Daya (Power), Gunakan (Use) Plane (Plane)." },
  { "en": "Apa Itu Layout (Tata Letak) Termal?", "id": "Desain (Design) PCB (Printed Circuit Board) Manajemen (Management) Panas (Heat) Efektif." },
  { "en": "Apa Itu Via (Jalur Vertikal) Termal?", "id": "Via (Via) Pemindah (Transfer) Panas (Heat) Komponen (Component) Ke Plane (Plane)." },
  { "en": "Apa Itu Poligon (Polygon) Tembaga (Copper)?", "id": "Area (Area) Tembaga (Copper) Besar (Large) PCB (PCB) (Power/Ground)." },
  { "en": "Apa Itu Solder Mask (Masker Solder)?", "id": "Lapisan (Layer) Pelindung (Protective) PCB (PCB) Cegah (Prevent) Solder (Solder) Hubung Singkat." },
  { "en": "Apa Itu Silkscreen (Sablon Sutra)?", "id": "Label (Label) Teks (Text) Putih (White) PCB (PCB) Penanda (Marker) Komponen." },
  { "en": "Apa Itu Panelisasi (Panelization) PCB (Printed Circuit Board)?", "id": "Menggabungkan (Combine) Banyak (Multiple) PCB (PCB) Kecil (Small) Panel (Panel) Besar." },
  { "en": "Apa Itu V-Score (Skor V) Panelisasi?", "id": "Potongan (Cut) V (V) Pemisah (Separator) PCB (PCB) Pada (On) Panel (Panel)." },
  { "en": "Apa Itu Tab-Route (Rute Tab) Panelisasi?", "id": "PCB (PCB) Ditahan (Held) Tab (Tab) Kecil (Small) Bisa (Can Be) Dipatahkan (Broken)." },
  { "en": "Apa Itu File (Berkas) Gerber?", "id": "Format (Format) File (File) Standar (Standard) Industri (Industry) Desain (Design) PCB (PCB)." },
  { "en": "Apa Itu File (Berkas) Bor (Drill File)?", "id": "File (File) Penentu (Determiner) Lokasi (Location) Ukuran (Size) Lubang (Hole) Bor (Drill) PCB." },
  { "en": "Apa Itu Bill of Materials (BOM)?", "id": "Daftar (List) Semua (All) Komponen (Component) Diperlukan (Needed) Rakit (Assemble) PCB." },
  { "en": "Apa Itu Centroid (Sentroid) File (Berkas)?", "id": "File (File) Lokasi (Location) Orientasi (Orientation) Komponen (Component) Pick-and-Place (Ambil-dan-Tempat)." },
  { "en": "Apa Itu Pad (Bantalan) Solder?", "id": "Area (Area) Tembaga (Copper) Terbuka (Open) Tempat (Place) Komponen (Component) Disolder." },
  { "en": "Apa Itu Pad (Bantalan) Non-Solder Mask Defined (NSMD)?", "id": "Bukaan (Opening) Solder Mask (Masker Solder) Lebih (More) Besar (Large) Pad (Pad) Tembaga (Copper)." },
  { "en": "Apa Itu Pad (Bantalan) Solder Mask Defined (SMD)?", "id": "Bukaan (Opening) Solder Mask (Masker Solder) Lebih (More) Kecil (Small) Pad (Pad) Tembaga (Copper)." },
  { "en": "Apa Itu Annular Ring (Cincin Annular)?", "id": "Area (Area) Tembaga (Copper) Sekitar (Around) Lubang (Hole) Bor (Drill) Via (Via)." },
  { "en": "Apa Itu Teardrop (Tetesan Air Mata) PCB?", "id": "Bentuk (Shape) Tambahan (Additional) Tembaga (Copper) Koneksi (Connection) Jalur (Trace) Pad (Pad)." },
  { "en": "Fungsi (Function) Teardrop (Tetesan Air Mata) PCB?", "id": "Meningkatkan (Increase) Kekuatan (Strength) Mekanis (Mechanical) Cegah (Prevent) Retak (Crack)." },
  { "en": "Apa Itu SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Perangkat (Software) Lunak (Software) Simulasi (Simulation) Rangkaian (Circuit) Elektronik." },
  { "en": "Apa Itu Model (Model) SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Model (Model) Matematis (Mathematical) Komponen (Component) Simulasi (Simulation)." },
  { "en": "Apa Itu Guard Ring (Cincin Pelindung)?", "id": "Struktur (Structure) Difusi (Diffusion) Isolasi (Isolation) Komponen (Component)." },
  { "en": "Fungsi (Function) Guard Ring (Cincin Pelindung) CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Mencegah (Prevent) Terjadinya (Occurrence) Latch-Up (Kait-Atas)." },
  { "en": "Apa Itu Wafer Probing (Penyelidikan Wafer)?", "id": "Pengujian (Test) Listrik (Electrical) Chip (Chip) Pada (On) Wafer (Wafer)." },
  { "en": "Apa Itu Probe Card (Kartu Penyelidik)?", "id": "Alat (Tool) Jarum (Needle) Kontak (Contact) Pad (Pad) Wafer (Wafer)." },
  { "en": "Apa Itu Die (Mata Dadu) Sort (Pilah)?", "id": "Proses (Process) Memilah (Sort) Die (Die) Fungsional (Functional) Gagal (Failed)." },
  { "en": "Apa Itu Penandaan (Inking) Die (Mata Dadu) Gagal?", "id": "Menandai (Mark) Die (Die) Gagal (Failed) Pada (On) Wafer (Wafer)." },
  { "en": "Apa Itu Gallium Nitride (GaN)?", "id": "Bahan (Material) Semikonduktor (Semiconductor) Celah (Gap) Pita (Band) Lebar (Wide)." },
  { "en": "Kelebihan (Advantage) IC (Integrated Circuit) GaN (Gallium Nitride)?", "id": "Efisiensi (Efficiency) Tinggi (High) Frekuensi (Frequency) Tinggi (High)." },
  { "en": "Apa Itu Silicon Carbide (SiC)?", "id": "Semikonduktor (Semiconductor) Celah (Gap) Pita (Band) Lebar (Wide) Lainnya (Other)." },
  { "en": "Kelebihan (Advantage) IC (Integrated Circuit) SiC (Silicon Carbide)?", "id": "Tegangan (Voltage) Tinggi (High) Suhu (Temperature) Tinggi (High)." },
  { "en": "Di Mana (Where) GaN (Gallium Nitride) SiC (Silicon Carbide) Digunakan?", "id": "Catu (Power) Daya (Supply), Inverter (Inverter), RF (Radio Frequency)." },
  { "en": "Apa Itu Chiplet (Anak Chip)?", "id": "Die (Die) Fungsional (Functional) Kecil (Small) Independen (Independent)." },
  { "en": "Apa Itu Teknologi (Technology) Chiplet (Anak Chip)?", "id": "Menggabungkan (Combine) Chiplet (Anak Chip) Dalam (In) Satu (One) Kemasan." },
  { "en": "Kelebihan (Advantage) Chiplet (Anak Chip)?", "id": "Desain (Design) Modular (Modular) Yield (Yield) Lebih (More) Baik (Good)." },
  { "en": "Apa Itu Interkoneksi (Interconnect) Chiplet (Anak Chip)?", "id": "Jalur (Path) Komunikasi (Communication) Cepat (Fast) Antar (Between) Chiplet (Chiplet)." },
  { "en": "Apa Itu Sensor (Sensor) Gas (Gas) MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sensor (Sensor) MEMS (Micro-Electro-Mechanical Systems) Deteksi (Detect) Gas (Gas) Kimia (Chemical)." },
  { "en": "Apa Itu Sensor (Sensor) Kelembaban (Humidity) IC?", "id": "IC (Integrated Circuit) Pengukur (Measure) Kadar (Level) Air (Water) Udara (Air)." },
  { "en": "Apa Itu IC (Integrated Circuit) Driver (Penggerak) LED (Light Emitting Diode)?", "id": "IC (Integrated Circuit) Kontrol (Control) Arus (Current) LED (Light Emitting Diode)." },
  { "en": "Mengapa (Why) LED (Light Emitting Diode) Perlu (Need) Driver (Penggerak) Arus (Current)?", "id": "Butuh (Need) Arus (Current) Konstan (Constant) Kecerahan (Brightness) Stabil." },
  { "en": "Apa Itu Driver (Penggerak) LED (Light Emitting Diode) Linear (Linear)?", "id": "Driver (Penggerak) LED (Light Emitting Diode) Efisiensi (Efficiency) Rendah (Low) Sederhana (Simple)." },
  { "en": "Apa Itu Driver (Penggerak) LED (Light Emitting Diode) Switching (Pensaklaran)?", "id": "Driver (Penggerak) LED (Light Emitting Diode) Efisiensi (Efficiency) Tinggi (High)." },
  { "en": "Apa Itu Dimming (Peredupan) PWM (Pulse Width Modulation) LED (Light Emitting Diode)?", "id": "Mengatur (Adjust) Kecerahan (Brightness) LED (Light Emitting Diode) PWM (PWM)." },
  { "en": "Apa Itu IC (Integrated Circuit) Driver (Penggerak) Layar (Display)?", "id": "IC (Integrated Circuit) Pengontrol (Controller) Piksel (Pixel) Layar (Screen) (LCD/OLED)." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Tampilan (Display) Kristal (Crystal) Cair (Liquid)." },
  { "en": "Apa Itu OLED (Organic Light Emitting Diode)?", "id": "Dioda (Diode) Pemancar (Emitter) Cahaya (Light) Organik (Organic)." },
  { "en": "Apa Itu IC (Integrated Circuit) Kontroler (Kontroler) Sentuh (Touch)?", "id": "IC (Integrated Circuit) Pendeteksi (Detector) Input (Input) Layar (Screen) Sentuh (Touch)." },
  { "en": "Apa Itu Layar (Screen) Sentuh (Touch) Kapasitif (Capacitive)?", "id": "Layar (Screen) Sentuh (Touch) Deteksi (Detect) Gangguan (Disturbance) Medan (Field) Listrik." },
  { "en": "Apa Itu Layar (Screen) Sentuh (Touch) Resistif (Resistive)?", "id": "Layar (Screen) Sentuh (Touch) Deteksi (Detect) Tekanan (Pressure) Fisik (Physical)." },
  { "en": "Apa Itu IC (Integrated Circuit) Pengisi (Charger) Baterai?", "id": "IC (Integrated Circuit) Manajemen (Management) Proses (Process) Pengisian (Charging) Baterai." },
  { "en": "Tahapan (Phase) Pengisian (Charging) Baterai (Battery) Li-Ion (Lithium-Ion)?", "id": "Arus (Current) Konstan (Constant) Tegangan (Voltage) Konstan (Constant)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem (System) Manajemen (Management) Baterai (Battery)." },
  { "en": "Fungsi (Function) BMS (Battery Management System)?", "id": "Proteksi (Protection) Overcharge (Isi Berlebih) Over-Discharge (Kuras Berlebih)." },
  { "en": "Apa Itu Pengukur (Gauge) Bahan Bakar (Fuel) Baterai?", "id": "IC (Integrated Circuit) Estimasi (Estimation) Sisa (Remaining) Daya (Capacity) Baterai." },
  { "en": "Apa Itu System-on-Module (SOM)?", "id": "Papan (Board) Sirkuit (Circuit) Berisi (Contains) SoC (System on a Chip) Memori." },
  { "en": "Beda (Difference) SOM (System-on-Module) Dan Mikrokontroler (Microcontroller)?", "id": "SOM (System-on-Module) Sistem (System) Lengkap (Complete) Butuh (Need) Papan (Board) Induk." },
  { "en": "Apa Itu Papan (Board) Induk (Carrier Board) SOM?", "id": "Papan (Board) PCB (Printed Circuit Board) Antarmuka (Interface) Konektor (Connector) SOM (SOM)." },
  { "en": "Apa Itu Modulator (Modulator) RF (Radio Frequency)?", "id": "Rangkaian (Circuit) Tumpangkan (Superimpose) Sinyal (Signal) Informasi (Information) Pembawa (Carrier)." },
  { "en": "Apa Itu Demodulator (Demodulator) RF (Radio Frequency)?", "id": "Rangkaian (Circuit) Ekstraksi (Extraction) Sinyal (Signal) Informasi (Information) Pembawa (Carrier)." },
  { "en": "Apa Itu Modulasi (Modulation) Amplitudo (Amplitude) (AM)?", "id": "Modulasi (Modulation) Amplitudo (Amplitude) Sinyal (Signal) Pembawa (Carrier) Berubah." },
  { "en": "Apa Itu Modulasi (Modulation) Frekuensi (Frequency) (FM)?", "id": "Modulasi (Modulation) Frekuensi (Frequency) Sinyal (Signal) Pembawa (Carrier) Berubah." },
  { "en": "Apa Itu Modulasi (Modulation) Fasa (Phase) (PM)?", "id": "Modulasi (Modulation) Fasa (Phase) Sinyal (Signal) Pembawa (Carrier) Berubah." },
  { "en": "Apa Itu QAM (Quadrature Amplitude Modulation)?", "id": "Modulasi (Modulation) Digital (Digital) Gabungan (Combine) Amplitudo (Amplitude) Fasa (Phase)." },
  { "en": "Apa Itu QPSK (Quadrature Phase-Shift Keying)?", "id": "Modulasi (Modulation) Fasa (Phase) Digital (Digital) Empat (Four) Keadaan (State)." },
  { "en": "Apa Itu Transceiver (Transiver) RF (Radio Frequency)?", "id": "IC (Integrated Circuit) Gabungan (Combine) Transmitter (Pemancar) Receiver (Penerima)." },
  { "en": "Apa Itu IF (Intermediate Frequency)?", "id": "Frekuensi (Frequency) Menengah (Intermediate) Rangkaian (Circuit) Radio (Radio)." },
  { "en": "Apa Itu Arsitektur (Architecture) Superheterodyne (Superheterodin)?", "id": "Arsitektur (Architecture) Penerima (Receiver) Radio (Radio) Pakai (Use) Frekuensi (Frequency) IF (IF)." },
  { "en": "Apa Itu Arsitektur (Architecture) Direct Conversion (Konversi Langsung)?", "id": "Arsitektur (Architecture) Penerima (Receiver) RF (RF) Langsung (Direct) Baseband (Baseband)." },
  { "en": "Apa Itu Baseband (Pita Basis) Prosesor?", "id": "IC (Integrated Circuit) Pemroses (Processor) Sinyal (Signal) Digital (Digital) Komunikasi (Communication)." },
  { "en": "Apa Itu DCR (Direct Current Resistance)?", "id": "Resistansi (Resistance) Arus (Current) Searah (Direct) Komponen (Component)." },
  { "en": "Apa Itu ESR (Equivalent Series Resistance)?", "id": "Resistansi (Resistance) Seri (Series) Ekuivalen (Equivalent) Kapasitor (Capacitor) Induktor (Induktor)." },
  { "en": "Mengapa (Why) ESR (Equivalent Series Resistance) Kapasitor (Capacitor) Penting?", "id": "ESR (Equivalent Series Resistance) Rendah (Low) Baik (Good) Aplikasi (Application) Daya (Power)." },
  { "en": "Apa Itu ESL (Equivalent Series Inductance)?", "id": "Induktansi (Inductance) Seri (Series) Ekuivalen (Equivalent) Kapasitor (Capacitor)." },
  { "en": "Apa Itu Faktor (Factor) Q (Kualitas) Induktor?", "id": "Ukuran (Measure) Kualitas (Quality) Efisiensi (Efficiency) Induktor (Inductor)." },
  { "en": "Apa Itu Frekuensi (Frequency) Resonansi (Resonance) Sendiri (Self)?", "id": "Frekuensi (Frequency) Komponen (Component) Resonansi (Resonate) Parasitik (Parasitic)." },
  { "en": "Apa Itu Solder Ball (Bola Solder)?", "id": "Bola (Ball) Solder (Solder) Kecil (Small) Kemasan (Package) BGA (Ball Grid Array)." },
  { "en": "Apa Itu Solder Bump (Benjolan Solder)?", "id": "Benjolan (Bump) Solder (Solder) Koneksi (Connection) Flip-Chip (Flip-Chip)." },
  { "en": "Apa Itu Pad (Bantalan) Termal (Thermal)?", "id": "Pad (Pad) Tembaga (Copper) Bawah (Bottom) IC (IC) Pembuang (Heat) Panas." },
  { "en": "Apa Itu Pitch (Jarak) Kaki (Lead) IC?", "id": "Jarak (Distance) Antar (Between) Pusat (Center) Kaki (Pin) IC (IC)." },
  { "en": "Apa Itu Coplanarity (Kesejajaran) Kaki (Lead)?", "id": "Tingkat (Level) Kesejajaran (Parallelism) Kaki (Lead) IC (IC) Permukaan (Surface)." },
  { "en": "Apa Itu J-Lead (Kaki J)?", "id": "Bentuk (Shape) Kaki (Lead) IC (IC) Tipe (Type) J (J)." },
  { "en": "Apa Itu Gull-Wing Lead (Kaki Sayap Camar)?", "id": "Bentuk (Shape) Kaki (Lead) IC (IC) Sayap (Wing) Camar (Gull)." },
  { "en": "Apa Itu QFN (Quad Flat No-Lead)?", "id": "Kemasan (Package) Datar (Flat) Empat (Four) Sisi (Side) Tanpa (No) Kaki (Lead)." },
  { "en": "Apa Itu DFN (Dual Flat No-Lead)?", "id": "Kemasan (Package) Datar (Flat) Dua (Two) Sisi (Side) Tanpa (No) Kaki (Lead)." },
  { "en": "Apa Itu CSP (Chip Scale Package)?", "id": "Kemasan (Package) Ukuran (Size) Hampir (Almost) Sama (Same) Die (Die) Silikon." },
  { "en": "Apa Itu Wafer-Level Packaging (WLP)?", "id": "Kemasan (Package) IC (IC) Dibuat (Made) Level (Level) Wafer (Wafer)." },
  { "en": "Apa Itu Fan-In WLP (Wafer-Level Packaging)?", "id": "Koneksi (Connection) BGA (Ball Grid Array) Dalam (Inside) Area (Area) Die (Die)." },
  { "en": "Apa Itu Fan-Out WLP (Wafer-Level Packaging)?", "id": "Koneksi (Connection) BGA (Ball Grid Array) Luar (Outside) Area (Area) Die (Die)." },
  { "en": "Apa Itu Delaminasi (Delamination) IC?", "id": "Pemisahan (Separation) Lapisan (Layer) Material (Material) Kemasan (Package) IC (IC)." },
  { "en": "Apa Itu Wire Bond (Ikatan Kawat) Sweep (Sapu)?", "id": "Pergeseran (Shift) Kawat (Wire) Bond (Bond) Proses (Process) Enkapsulasi." },
  { "en": "Apa Itu Popcorning (Letupan Jagung) IC?", "id": "Kerusakan (Damage) IC (IC) Akibat (Due To) Kelembaban (Moisture) Saat (During) Reflow." },
  { "en": "Apa Itu Tomografi (Tomography) Komputer (Computer) Sinar-X IC?", "id": "Pemindaian (Scan) Tiga (Three) Dimensi (Dimension) (3D) Internal (Internal) IC (IC)." },
  { "en": "Apa Itu Analisis (Analysis) Akustik (Acoustic) Pindai (Scan) (SAM)?", "id": "Metode (Method) Deteksi (Detection) Celah (Void) Delaminasi (Delamination) IC (IC)." },
  { "en": "Apa Itu Getaran (Vibration) Ultrasonik (Ultrasonic) Wire Bonding?", "id": "Energi (Energy) Ultrasonik (Ultrasonic) Bantu (Help) Proses (Process) Wire (Wire) Bonding (Bonding)." },
  { "en": "Apa Itu Thermosonic (Termosonik) Bonding (Ikatan)?", "id": "Wire (Wire) Bonding (Bonding) Gabungan (Combine) Panas (Heat) Tekanan (Pressure) Ultrasonik." },
  { "en": "Apa Itu Ball (Bola) Bonding (Ikatan)?", "id": "Metode (Method) Wire (Wire) Bonding (Bonding) Bentuk (Form) Bola (Ball) Ujung (End) Kawat." },
  { "en": "Apa Itu Wedge (Baji) Bonding (Ikatan)?", "id": "Metode (Method) Wire (Wire) Bonding (Bonding) Bentuk (Form) Baji (Wedge) Datar." },
  { "en": "Apa Itu Pad (Bantalan) Bonding (Ikatan)?", "id": "Area (Area) Logam (Metal) Terekspos (Exposed) Die (Die) Koneksi (Connection) Kawat." },
  { "en": "Apa Itu Arus (Current) Inrush (Serbu)?", "id": "Arus (Current) Puncak (Peak) Tinggi (High) Awal (Start) Catu (Power) Daya (Supply)." },
  { "en": "Apa Itu Pembatas (Limiter) Arus (Current) Inrush (Serbu)?", "id": "Rangkaian (Circuit) Pembatas (Limiter) Arus (Current) Puncak (Peak) Awal (Initial)." },
  { "en": "Apa Itu NTC (Negative Temperature Coefficient) Thermistor (Termistor) Inrush (Serbu)?", "id": "Resistor (Resistor) NTC (Negative Temperature Coefficient) Pembatas (Limiter) Arus (Current) Inrush (Serbu)." },
  { "en": "Apa Itu Penyearah (Rectifier) Jembatan (Bridge)?", "id": "Rangkaian (Circuit) Dioda (Diode) Ubah (Convert) AC (Alternating Current) DC (Direct Current)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Penghalus (Smoothing)?", "id": "Kapasitor (Capacitor) Perata (Smoother) Riak (Ripple) Tegangan (Voltage) DC (DC)." },
  { "en": "Apa Itu Inverter (Inverter) Daya (Power)?", "id": "Rangkaian (Circuit) Pengubah (Converter) DC (Direct Current) Menjadi (To) AC (Alternating Current)." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Catu (Power) Daya (Supply) Cadangan (Backup) Baterai (Battery) Tidak (No) Terputus." },
  { "en": "Apa Itu Crowbar (Linggis) Circuit?", "id": "Rangkaian (Circuit) Proteksi (Protection) Hubung (Short) Singkat (Circuit) Catu (Power) Daya (Supply)." },
  { "en": "Apa Itu Foldback (Lipat Balik) Current Limiting?", "id": "Proteksi (Protection) Arus (Current) Turunkan (Decrease) Tegangan (Voltage) Arus (Current)." },
  { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Koreksi (Correction) Faktor (Factor) Daya (Power)." },
  { "en": "Mengapa (Why) PFC (Power Factor Correction) Penting?", "id": "Meningkatkan (Improve) Efisiensi (Efficiency) Daya (Power) Jaringan (Grid) Listrik." },
  { "en": "Apa Itu PFC (Power Factor Correction) Aktif (Active)?", "id": "Rangkaian (Circuit) PFC (Power Factor Correction) Elektronik (Electronic) Kompleks (Complex)." },
  { "en": "Apa Itu PFC (Power Factor Correction) Pasif (Passive)?", "id": "Rangkaian (Circuit) PFC (Power Factor Correction) Pakai (Use) Induktor (Inductor) Kapasitor (Capacitor)." },
  { "en": "Apa Itu IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Institut (Institute) Insinyur (Engineer) Listrik (Electrical) Elektronik (Electronics)." },
  { "en": "Apa Itu JEDEC (Joint Electron Device Engineering Council)?", "id": "Badan (Body) Standar (Standard) Industri (Industry) Semikonduktor (Semiconductor)." },
  { "en": "Apa Itu Standar (Standard) Memori (Memory) JEDEC (Joint Electron Device Engineering Council)?", "id": "Standar (Standard) Industri (Industry) Modul (Module) Memori (Memory) RAM (RAM)." },
  { "en": "Apa Itu IPC (Association Connecting Electronics Industries)?", "id": "Asosiasi (Association) Industri (Industry) Standar (Standard) Desain (Design) PCB (PCB)." },
  { "en": "Apa Itu Standar (Standard) IPC (Association Connecting Electronics Industries) PCB (Printed Circuit Board)?", "id": "Aturan (Rule) Desain (Design) Manufaktur (Manufacturing) PCB (PCB)." },
  { "en": "Apa Itu Rangkaian Integrator Op-Amp?", "id": "Rangkaian Op-Amp (Operational Amplifier) Menghitung Integral." },
  { "en": "Apa Itu Rangkaian Diferensiator Op-Amp?", "id": "Rangkaian Op-Amp (Operational Amplifier) Menghitung Turunan." },
  { "en": "Apa Itu Penguat Penjumlah Op-Amp?", "id": "Rangkaian Op-Amp (Operational Amplifier) Menjumlahkan Sinyal." },
  { "en": "Apa Itu Osilator Colpitts?", "id": "Osilator LC (Induktor-Kapasitor) Pembagi Kapasitif." },
  { "en": "Apa Itu Osilator Hartley?", "id": "Osilator LC (Induktor-Kapasitor) Pembagi Induktif." },
  { "en": "Apa Itu Osilator Clapp?", "id": "Modifikasi Osilator Colpitts Kapasitor Seri." },
  { "en": "Apa Itu DCO (Digitally Controlled Oscillator)?", "id": "Osilator Frekuensi Diatur Secara Digital." },
  { "en": "Apa Itu Germanium (Ge)?", "id": "Bahan Semikonduktor Selain Silikon." },
  { "en": "Apa Itu Gallium Arsenide (GaAs)?", "id": "Semikonduktor Majemuk Kecepatan Tinggi." },
  { "en": "Kelebihan GaAs (Gallium Arsenide) Dari Silikon?", "id": "Mobilitas Elektron Tinggi Frekuensi Radio." },
  { "en": "Apa Itu Hukum Komutatif Boolean?", "id": "Urutan Operasi Logika Tidak Penting." },
  { "en": "Apa Itu Hukum Asosiatif Boolean?", "id": "Pengelompokan Operasi Logika Tidak Penting." },
  { "en": "Apa Itu Hukum Distributif Boolean?", "id": "Aturan Distribusi Operasi AND OR." },
  { "en": "Apa Itu Hazard Statis-0?", "id": "Output Salah Berdenyut Satu Singkat." },
  { "en": "Apa Itu Hazard Statis-1?", "id": "Output Salah Berdenyut Nol Singkat." },
  { "en": "Apa Itu Hazard Dinamis?", "id": "Output Berubah Nilai Beberapa Kali." },
  { "en": "Apa Itu Konsentrasi Intrinsik Pembawa?", "id": "Jumlah Elektron Lubang Semikonduktor Murni." },
  { "en": "Apa Itu Kemasan TO (Transistor Outline)?", "id": "Kemasan Logam Tabung Transistor Lama." },
  { "en": "Apa Itu Kemasan SOIC (Small Outline Integrated Circuit)?", "id": "Kemasan SMD (Surface Mount Device) Populer Versi Kecil DIP." },
  { "en": "Apa Itu Kemasan TSSOP (Thin-Shrink Small Outline Package)?", "id": "Kemasan SOIC (Small Outline Integrated Circuit) Sangat Tipis Rapat." },
  { "en": "Apa Itu Kemasan MSOP (Miniature Small Outline Package)?", "id": "Kemasan Lebih Kecil Dari TSSOP (Thin-Shrink Small Outline Package)." },
  { "en": "Apa Itu Dioda Penjepit ESD (Electrostatic Discharge)?", "id": "Dioda Proteksi ESD (Electrostatic Discharge) Salurkan Lonjakan." },
  { "en": "Apa Itu Struktur SCR (Silicon Controlled Rectifier) ESD?", "id": "Struktur Proteksi ESD (Electrostatic Discharge) Efektif Kuat." },
  { "en": "Apa Itu Bus Alamat?", "id": "Jalur Sinyal CPU (Central Processing Unit) Pilih Lokasi Memori." },
  { "en": "Apa Itu Bus Data?", "id": "Jalur Sinyal Transfer Data Antar CPU (CPU) Memori." },
  { "en": "Apa Itu Bus Kontrol?", "id": "Jalur Sinyal Kontrol Operasi Baca Tulis." },
  { "en": "Apa Itu Lebar Bus Alamat?", "id": "Tentukan Jumlah Memori Maksimal Dialamati." },
  { "en": "Apa Itu Lebar Bus Data?", "id": "Tentukan Jumlah Data Ditransfer Sekali." },
  { "en": "Apa Itu Tahap Spesifikasi Desain IC?", "id": "Tahap Awal Tentukan Fitur Fungsi IC (IC)." },
  { "en": "Apa Itu Tahap Desain RTL (Register-Transfer Level)?", "id": "Tahap Penulisan Kode VHDL (VHSIC Hardware Description Language) Verilog." },
  { "en": "Apa Itu Tahap Sintesis Desain IC?", "id": "Proses Ubah Kode RTL (Register-Transfer Level) Gerbang Logika." },
  { "en": "Apa Itu Tahap P&R (Place and Route)?", "id": "Tahap Penempatan Perutean Gerbang Layout." },
  { "en": "Apa Itu Tahap Verifikasi Fisik?", "id": "Tahap Pemeriksaan DRC (Design Rule Check) LVS (Layout Versus Schematic)." },
  { "en": "Apa Itu Frekuensi Cutoff Filter?", "id": "Frekuensi Transisi Antara Passband Stopband." },
  { "en": "Apa Itu Passband Filter?", "id": "Rentang Frekuensi Yang Dilewatkan Filter." },
  { "en": "Apa Itu Stopband Filter?", "id": "Rentang Frekuensi Yang Ditolak Filter." },
  { "en": "Apa Itu Orde Filter?", "id": "Ukuran Kompleksitas Kemiringan Roll-Off Filter." },
  { "en": "Apa Itu Roll-Off Filter?", "id": "Seberapa Cepat Filter Meredam Sinyal Stopband." },
  { "en": "Apa Itu Filter Butterworth?", "id": "Filter Respon Datar Maksimal Passband." },
  { "en": "Apa Itu Filter Chebyshev?", "id": "Filter Roll-Off Tajam Riak Passband." },
  { "en": "Apa Itu Filter Bessel?", "id": "Filter Respon Fasa Linear Tunda Grup Konstan." },
  { "en": "Apa Itu Filter Celah Aktif?", "id": "Filter Aktif Tolak Satu Frekuensi Spesifik." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Nilai Resistansi Berubah Cahaya." },
  { "en": "Nama Lain LDR (Light Dependent Resistor)?", "id": "Fotoresistor Atau Sel Fotokonduktif." },
  { "en": "Bahan Apa Pembuat LDR (Light Dependent Resistor)?", "id": "Kadmium Sulfida (CdS)." },
  { "en": "Apa Itu Fototransistor?", "id": "Transistor BJT (Bipolar Junction Transistor) Sensitif Cahaya." },
  { "en": "Apa Beda Fotodioda Fototransistor?", "id": "Fototransistor Punya Penguatan Arus Internal." },
  { "en": "Apa Itu Sel Surya?", "id": "Dioda PN (PN Junction) Besar Ubah Cahaya Listrik." },
  { "en": "Nama Lain Sel Surya?", "id": "Sel Fotovoltaik (PV)." },
  { "en": "Apa Itu Efek Fotovoltaik?", "id": "Pembangkitan Tegangan Akibat Penyerapan Cahaya." },
  { "en": "Apa Itu MPPT (Maximum Power Point Tracking)?", "id": "Teknik Maksimalkan Panen Daya Sel Surya." },
  { "en": "Apa Itu IC Kontroler MPPT (Maximum Power Point Tracking)?", "id": "IC (Integrated Circuit) Implementasi Algoritma MPPT." },
  { "en": "Apa Itu Bus Antarmuka I3C (Improved Inter Integrated Circuit)?", "id": "Evolusi I2C (Inter-Integrated Circuit) Cepat Efisien." },
  { "en": "Apa Itu RFFE (RF Front-End) MIPI (Mobile Industry Processor Interface)?", "id": "Standar Antarmuka Kontrol Komponen RF (Radio Frequency) Seluler." },
  { "en": "Apa Itu SoundWire MIPI (Mobile Industry Processor Interface)?", "id": "Antarmuka Audio Digital Efisien Daya Rendah." },
  { "en": "Apa Itu UFS (Universal Flash Storage)?", "id": "Standar Memori Flash Kinerja Tinggi Seluler." },
  { "en": "Apa Itu eMMC (Embedded MultiMediaCard)?", "id": "Standar Memori Flash Tertanam Populer Lama." },
  { "en": "Apa Itu C-PHY (MIPI Alliance specification for a physical layer)?", "id": "Lapisan Fisik MIPI (Mobile Industry Processor Interface) Efisien Kamera Layar." },
  { "en": "Apa Itu Tegangan Tembus Dioda?", "id": "Tegangan Bias Mundur Dioda Rusak." },
  { "en": "Apa Itu Tembus Zener?", "id": "Tembus Dioda Doping Berat Medan Listrik." },
  { "en": "Apa Itu Tembus Avalanche?", "id": "Tembus Dioda Doping Ringan Tabrakan Ionisasi." },
  { "en": "Apa Itu Waktu Pemulihan Mundur Dioda?", "id": "Waktu Dioda Mati Kondisi Maju." },
  { "en": "Apa Itu Dioda Pemulihan Cepat?", "id": "Dioda Waktu Pemulihan Mundur Sangat Singkat." },
  { "en": "Apa Itu Dioda Pemulihan Lunak?", "id": "Dioda Transisi Pemulihan Mundur Halus." },
  { "en": "Apa Itu Dioda Varactor?", "id": "Dioda Kapasitansi Berubah Tegangan Bias." },
  { "en": "Nama Lain Dioda Varactor?", "id": "Dioda Varicap Atau Dioda Penyetel." },
  { "en": "Di Mana Dioda Varactor Digunakan?", "id": "VCO (Voltage-Controlled Oscillator), PLL (Phase-Locked Loop), Filter Tunable." },
  { "en": "Apa Itu Dioda PIN (PIN Diode)?", "id": "Dioda Lapisan Intrinsik (I) Lebar." },
  { "en": "Fungsi Dioda PIN (PIN Diode) RF?", "id": "Sebagai Saklar RF (Radio Frequency) Atau Attenuator." },
  { "en": "Apa Itu Dioda Terowongan?", "id": "Dioda Tunjukkan Resistansi Diferensial Negatif." },
  { "en": "Apa Itu Dioda Gunn?", "id": "Perangkat Semikonduktor Resistansi Negatif Frekuensi Microwave." },
  { "en": "Apa Itu Dioda IMPATT (IMPATT Diode)?", "id": "Dioda Pembangkit Daya Microwave Tinggi." },
  { "en": "Apa Itu Dioda Penekan Transien?", "id": "Dioda (TVS) Pelindung Lonjakan Tegangan Cepat." },
  { "en": "Apa Itu TVS (Transient Voltage Suppressor) Uni-Directional?", "id": "TVS (Transient Voltage Suppressor) Lindungi Satu Polaritas Tegangan." },
  { "en": "Apa Itu TVS (Transient Voltage Suppressor) Bi-Directional?", "id": "TVS (Transient Voltage Suppressor) Lindungi Kedua Polaritas Tegangan." },
  { "en": "Apa Itu Varistor Oksida Logam (MOV)?", "id": "Resistor Dependen Tegangan Pelindung Lonjakan." },
  { "en": "Apa Beda TVS (Transient Voltage Suppressor) MOV (Metal Oxide Varistor)?", "id": "TVS (Transient Voltage Suppressor) Respon Cepat, MOV (MOV) Kapasitas Tinggi." },
  { "en": "Apa Itu Tabung Pelepasan Gas (GDT)?", "id": "Pelindung Lonjakan Tegangan Sangat Tinggi." },
  { "en": "Apa Itu Polimer ESD (Electrostatic Discharge) Suppressor?", "id": "Bahan Polimer Pelindung ESD (Electrostatic Discharge) Kapasitansi Rendah." },
  { "en": "Apa Itu Proses Czochralski?", "id": "Metode Penumbuhan Kristal Silikon Tunggal." },
  { "en": "Apa Itu Boule Silikon?", "id": "Kristal Silikon Silinder Tunggal Besar." },
  { "en": "Apa Itu Pemotongan Wafer?", "id": "Proses Memotong Boule Menjadi Wafer Tipis." },
  { "en": "Apa Itu Lapping Wafer?", "id": "Proses Penghalusan Permukaan Wafer Awal." },
  { "en": "Apa Itu Pemolesan Wafer?", "id": "Proses Akhir Buat Permukaan Wafer Cermin." },
  { "en": "Apa Itu Wafer Prime?", "id": "Wafer Kualitas Tertinggi Produksi IC (IC) Canggih." },
  { "en": "Apa Itu Wafer Uji?", "id": "Wafer Kualitas Rendah Pemantauan Proses." },
  { "en": "Apa Itu Wafer Datar Notch?", "id": "Penanda Orientasi Kristal Pada Wafer." },
  { "en": "Apa Itu Doping Gas?", "id": "Doping Menggunakan Gas Dopant Suhu Tinggi." },
  { "en": "Apa Itu Photoresist Positif?", "id": "Area Terekspos Cahaya Larut Pengembang." },
  { "en": "Apa Itu Photoresist Negatif?", "id": "Area Terekspos Cahaya Mengeras Tidak Larut." },
  { "en": "Proses Apa Hasilkan Resolusi Lebih Baik?", "id": "Photoresist Positif Resolusi Lebih Baik." },
  { "en": "Apa Itu Hard Bake Resist?", "id": "Pemanasan Resist Setelah Pengembangan Kuatkan Pola." },
  { "en": "Apa Itu Soft Bake Resist?", "id": "Pemanasan Resist Setelah Pelapisan Hilangkan Pelarut." },
  { "en": "Apa Itu Resist Stripping?", "id": "Proses Menghilangkan Photoresist Setelah Etsa." },
  { "en": "Apa Itu Plasma Oksigen?", "id": "Gas Oksigen Terionisasi Pengupasan Resist Kering." },
  { "en": "Apa Itu Etsa Anisotropik?", "id": "Etsa Satu Arah Saja (Vertikal)." },
  { "en": "Apa Itu Etsa Isotropik?", "id": "Etsa Semua Arah Sama (Horizontal Vertikal)." },
  { "en": "Etsa Apa Lebih Diinginkan IC (IC) Modern?", "id": "Etsa Anisotropik Untuk Fitur Kecil." },
  { "en": "Apa Itu RIE (Reactive Ion Etching)?", "id": "Etsa Kering Anisotropik Gabungan Kimia Fisik." },
  { "en": "Apa Itu Trench (Parit) MOSFET?", "id": "MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Struktur Saluran Vertikal." },
  { "en": "Apa Itu LDMOS (Laterally Diffused Metal Oxide Semiconductor)?", "id": "MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Daya (Power) Struktur Lateral." },
  { "en": "Di Mana LDMOS (Laterally Diffused Metal Oxide Semiconductor) Digunakan?", "id": "Penguat (Amplifier) Daya (Power) RF (Radio Frequency) Base Station." },
  { "en": "Apa Itu HEMT (High-Electron-Mobility Transistor)?", "id": "Transistor (Transistor) Kecepatan (Speed) Sangat (Very) Tinggi (High)." },
  { "en": "Bahan (Material) Apa (What) HEMT (High-Electron-Mobility Transistor) Gunakan?", "id": "Semikonduktor (Semiconductor) Majemuk (Compound) Seperti (Like) GaAs (Gallium Arsenide)." },
  { "en": "Apa Itu 2DEG (Two-Dimensional Electron Gas)?", "id": "Gas (Gas) Elektron (Electron) Dua (Two) Dimensi (Dimension) HEMT (HEMT)." },
  { "en": "Apa Itu Fotolitografi (Photolithography) Tanpa (Without) Masker (Mask)?", "id": "Litografi (Lithography) Paparan (Exposure) Sinar (Beam) Laser (Laser) Langsung." },
  { "en": "Apa Itu Nanoimprint Lithography (NIL)?", "id": "Litografi (Lithography) Cetak (Imprint) Pola (Pattern) Nano (Nano) Stempel (Stamp)." },
  { "en": "Apa Itu ALD (Atomic Layer Deposition)?", "id": "Deposisi (Deposition) Material (Material) Satu (One) Lapisan (Layer) Atom (Atom)." },
  { "en": "Kelebihan (Advantage) ALD (Atomic Layer Deposition)?", "id": "Ketebalan (Thickness) Sangat (Very) Tipis (Thin) Tepat (Precise) Seragam (Uniform)." },
  { "en": "Apa Itu Bahan (Material) Dielektrik (Dielectric) High-K?", "id": "Isolator (Isolator) Gerbang (Gate) Konstanta (Constant) Dielektrik (Dielectric) Tinggi (High)." },
  { "en": "Contoh (Example) Dielektrik (Dielectric) High-K?", "id": "Hafnium (Hafnium) Dioksida (Dioxide) (HfO2)." },
  { "en": "Mengapa (Why) Perlu (Need) Dielektrik (Dielectric) High-K?", "id": "Mengurangi (Reduce) Arus (Current) Bocor (Leakage) Gerbang (Gate) Tipis." },
  { "en": "Apa Itu Gerbang (Gate) Logam (Metal)?", "id": "Material (Material) Gerbang (Gate) Transistor (Transistor) Menggunakan (Using) Logam (Metal)." },
  { "en": "Apa Itu HKMG (High-K Metal Gate)?", "id": "Kombinasi (Combination) Dielektrik (Dielectric) High-K (High-K) Gerbang (Gate) Logam (Metal)." },
  { "en": "Apa Itu Silikon (Silicon) Tegang (Strained)?", "id": "Silikon (Silicon) Regangan (Strain) Mekanis (Mechanical) Tingkatkan (Improve) Mobilitas (Mobility)." },
  { "en": "Apa Itu SOI (Silicon on Insulator)?", "id": "Lapisan (Layer) Silikon (Silicon) Tipis (Thin) Di Atas (On) Isolator (Isolator)." },
  { "en": "Kelebihan (Advantage) SOI (Silicon on Insulator)?", "id": "Kapasitansi (Capacitance) Parasitik (Parasitic) Rendah (Low) Radiasi (Radiation) Kuat (Hard)." },
  { "en": "Apa Itu IC (Integrated Circuit) Tahan (Hardened) Radiasi?", "id": "IC (Integrated Circuit) Desain (Design) Khusus (Special) Tahan (Resist) Radiasi (Radiation)." },
  { "en": "Di Mana (Where) IC (Integrated Circuit) Tahan (Hardened) Radiasi Digunakan?", "id": "Aplikasi (Application) Antariksa (Space) Militer (Military) Nuklir (Nuclear)." },
  { "en": "Apa Itu SEE (Single-Event Effect)?", "id": "Efek (Effect) Partikel (Particle) Radiasi (Radiation) Tunggal (Single) IC (IC)." },
  { "en": "Apa Itu SEU (Single-Event Upset)?", "id": "SEE (Single-Event Effect) Balikkan (Flip) Bit (Bit) Memori (Memory) Flip-Flop (Flip-Flop)." },
  { "en": "Apa Itu SEL (Single-Event Latch-up)?", "id": "SEE (Single-Event Effect) Picu (Trigger) Kondisi (Condition) Latch-Up (Latch-Up) CMOS (CMOS)." },
  { "en": "Apa Itu SEB (Single-Event Burnout)?", "id": "SEE (Single-Event Effect) Rusak (Damage) Total (Total) MOSFET (MOSFET) Daya (Power)." },
  { "en": "Apa Itu SEGR (Single-Event Gate Rupture)?", "id": "SEE (Single-Event Effect) Rusak (Damage) Gerbang (Gate) Oksida (Oxide) MOSFET (MOSFET)." },
  { "en": "Apa Itu Redundansi (Redundancy) Tiga (Three) Modular (Modular) (TMR)?", "id": "Teknik (Technique) Tahan (Tolerant) Kesalahan (Fault) Tiga (Three) Modul (Module) Voting (Voting)." },
  { "en": "Apa Itu Voting (Pemungutan Suara) Logika (Logic)?", "id": "Logika (Logic) Tentukan (Determine) Output (Output) Mayoritas (Majority) TMR (TMR)." },
  { "en": "Apa Itu Hamming Code (Kode Hamming)?", "id": "Tipe (Type) Kode (Code) ECC (Error-Correcting Code) Sederhana (Simple)." },
  { "en": "Apa Itu Kode (Code) Reed-Solomon?", "id": "Tipe (Type) Kode (Code) ECC (Error-Correcting Code) Kuat (Powerful) Blok (Block)." },
  { "en": "Apa Itu Interleaving (Interlasi) Memori?", "id": "Teknik (Technique) Sebar (Spread) Data (Data) Kurangi (Reduce) Efek (Effect) Burst Error." },
  { "en": "Apa Itu Burst Error (Kesalahan Ledakan)?", "id": "Beberapa (Multiple) Kesalahan (Error) Bit (Bit) Berurutan (Consecutive)." },
  { "en": "Apa Itu Penggosokan (Scrubbing) Memori?", "id": "Proses (Process) Periodik (Periodic) Baca (Read) Koreksi (Correct) Kesalahan (Error) Memori." },
  { "en": "Apa Itu Sensor (Sensor) Suhu (Temperature) Celah (Gap) Pita (Bandgap)?", "id": "Sensor (Sensor) Suhu (Temperature) Akurat (Accurate) Berbasis (Based) Referensi (Reference) Bandgap." },
  { "en": "Apa Itu PTAT (Proportional to Absolute Temperature)?", "id": "Proporsional (Proportional) Terhadap (To) Suhu (Temperature) Absolut (Absolute)." },
  { "en": "Apa Itu CTAT (Complementary to Absolute Temperature)?", "id": "Komplementer (Complementary) Terhadap (To) Suhu (Temperature) Absolut (Absolute)." },
  { "en": "Bagaimana (How) Sensor (Sensor) Bandgap (Bandgap) Bekerja?", "id": "Menggabungkan (Combine) Sinyal (Signal) PTAT (PTAT) CTAT (CTAT)." },
  { "en": "Apa Itu FBAR (Film Bulk Acoustic Resonator)?", "id": "Tipe (Type) Resonator (Resonator) BAW (Bulk Acoustic Wave) Kinerja (Performance) Tinggi (High)." },
  { "en": "Apa Itu Penguat (Amplifier) Doherty?", "id": "Arsitektur (Architecture) Penguat (Amplifier) Daya (Power) RF (RF) Efisiensi (Efficiency) Tinggi (High)." },
  { "en": "Apa Itu Pre-Distorsi (Pre-Distortion) Digital (Digital) (DPD)?", "id": "Teknik (Technique) Linearitasi (Linearization) Penguat (Amplifier) Daya (Power) RF (RF)." },
  { "en": "Apa Itu Crest Factor Reduction (CFR)?", "id": "Teknik (Technique) Kurangi (Reduce) Rasio (Ratio) Puncak (Peak) Rata-Rata (Average) Sinyal (Signal)." },
  { "en": "Apa Itu Sinyal (Signal) OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Sinyal (Signal) Komunikasi (Communication) Digital (Digital) Multi-Carrier (Multi-Carrier)." },
  { "en": "Mengapa (Why) Sinyal (Signal) OFDM (Orthogonal Frequency-Division Multiplexing) Punya (Have) PAPR (Peak-to-Average Power Ratio) Tinggi?", "id": "Karena (Because) Penjumlahan (Sum) Banyak (Many) Sub-Carrier (Sub-Carrier)." },
  { "en": "Apa Itu PAPR (Peak-to-Average Power Ratio)?", "id": "Rasio (Ratio) Daya (Power) Puncak (Peak) Rata-Rata (Average) Sinyal (Signal)." },
  { "en": "Apa Itu IC (Integrated Circuit) Transponder (Transponder) RFID (Radio-Frequency Identification)?", "id": "IC (IC) Tag (Tag) Identifikasi (Identification) Frekuensi (Frequency) Radio (Radio)." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification) Pasif (Passive)?", "id": "Tag (Tag) RFID (Radio-Frequency Identification) Daya (Power) Dari (From) Pembaca (Reader)." },
  { "en": "Apa Itu RFID (Radio-Frequency Identification) Aktif (Active)?", "id": "Tag (Tag) RFID (Radio-Frequency Identification) Punya (Have) Baterai (Battery) Sendiri." },
  { "en": "Apa Itu Backscatter (Hamburan Balik) RFID?", "id": "Metode (Method) Komunikasi (Communication) Tag (Tag) Pasif (Passive) Pantulkan (Reflect) Sinyal (Signal)." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Komunikasi (Communication) Nirkabel (Wireless) Jarak (Range) Sangat (Very) Dekat (Short)." },
  { "en": "NFC (Near Field Communication) Berbasis (Based On) Standar (Standard) Apa (What)?", "id": "Berbasis (Based On) Standar (Standard) RFID (Radio-Frequency Identification)." },
  { "en": "Apa Itu Elemen (Element) Aman (Secure) (SE)?", "id": "Chip (Chip) Khusus (Special) Penyimpanan (Storage) Data (Data) Sensitif (Sensitive) Aman." },
  { "en": "Di Mana (Where) Elemen (Element) Aman (Secure) (SE) Digunakan?", "id": "Kartu (Card) SIM (SIM), Kartu (Card) Kredit (Credit), NFC (NFC) Aman." },
  { "en": "Apa Itu IC (Integrated Circuit) Antarmuka (Interface) Sensor (Sensor)?", "id": "IC (IC) Akuisisi (Acquisition) Kondisi (Conditioning) Sinyal (Signal) Sensor (Sensor)." },
  { "en": "Apa Itu Pengkondisian (Conditioning) Sinyal (Signal)?", "id": "Proses (Process) Penguatan (Amplification) Penyaringan (Filtering) Sinyal (Signal) Sensor (Sensor)." },
  { "en": "Apa Itu Penguat (Amplifier) Instrumentasi (Instrumentation) (In-Amp)?", "id": "Penguat (Amplifier) Diferensial (Differential) CMRR (Common-Mode Rejection Ratio) Tinggi (High)." },
  { "en": "Kapan (When) Penguat (Amplifier) Instrumentasi (Instrumentation) Digunakan?", "id": "Pengukuran (Measurement) Sinyal (Signal) Kecil (Small) Kebisingan (Noise) Tinggi (High)." },
  { "en": "Apa Itu Jembatan (Bridge) Wheatstone (Wheatstone)?", "id": "Rangkaian (Circuit) Pengukuran (Measurement) Perubahan (Change) Resistansi (Resistance) Kecil." },
  { "en": "Sensor (Sensor) Apa (What) Pakai (Use) Jembatan (Bridge) Wheatstone (Wheatstone)?", "id": "Strain (Strain) Gauge (Gauge), Sensor (Sensor) Tekanan (Pressure) Piezoresistif." },
  { "en": "Apa Itu Strain (Strain) Gauge (Gauge)?", "id": "Sensor (Sensor) Resistansi (Resistance) Berubah (Change) Bentuk (Deformation) Mekanis." },
  { "en": "Apa Itu Faktor (Factor) Gauge (Gauge)?", "id": "Ukuran (Measure) Sensitivitas (Sensitivity) Strain (Strain) Gauge (Gauge)." },
  { "en": "Apa Itu Load Cell (Sel Beban)?", "id": "Sensor (Sensor) Pengukur (Measure) Berat (Weight) Gaya (Force) Pakai (Use) Strain (Strain) Gauge." },
  { "en": "Apa Itu IC (Integrated Circuit) Penguat (Amplifier) Isolasi (Isolation)?", "id": "Penguat (Amplifier) Isolasi (Isolate) Listrik (Electrical) Input (Input) Output (Output)." },
  { "en": "Kapan (When) Penguat (Amplifier) Isolasi (Isolation) Digunakan?", "id": "Pengukuran (Measurement) Medis (Medical) Tegangan (Voltage) Tinggi (High) Aman." },
  { "en": "Metode (Method) Isolasi (Isolation) Apa (What) Yang (That) Ada?", "id": "Optik (Optical) (Optocoupler), Kapasitif (Capacitive), Magnetik (Magnetic)." },
  { "en": "Apa Itu Isolator (Isolator) Digital (Digital)?", "id": "IC (Integrated Circuit) Isolasi (Isolate) Sinyal (Signal) Digital (Digital) (Contoh: I2C, SPI)." },
  { "en": "Apa Itu Barrier (Penghalang) Isolasi (Isolation)?", "id": "Batas (Boundary) Fisik (Physical) Pemisah (Separator) Rangkaian (Circuit) Terisolasi." },
  { "en": "Apa Itu Jarak (Distance) Creepage (Rambat)?", "id": "Jarak (Distance) Terpendek (Shortest) Permukaan (Surface) Antara (Between) Konduktor (Conductor) Terisolasi." },
  { "en": "Apa Itu Jarak (Distance) Clearance (Celah)?", "id": "Jarak (Distance) Terpendek (Shortest) Udara (Air) Antara (Between) Konduktor (Conductor) Terisolasi." },
  { "en": "Apa Itu Tegangan (Voltage) Tahan (Withstand) Isolasi (Isolation)?", "id": "Tegangan (Voltage) Maksimal (Maximum) Tahan (Withstand) Barrier (Penghalang) Isolasi (Isolation)." },
  { "en": "Apa Itu Standar (Standard) Keamanan (Safety) IEC 60601?", "id": "Standar (Standard) Keamanan (Safety) Peralatan (Equipment) Listrik (Electrical) Medis (Medical)." },
  { "en": "Apa Itu Standar (Standard) Keamanan (Safety) UL 1577?", "id": "Standar (Standard) Keamanan (Safety) Isolator (Isolator) Optik (Optical) (Optocoupler)." },
  { "en": "Apa Itu Desain (Design) Tiga (Three) Dimensi (Dimension) (3D) IC (IC) Heterogen?", "id": "Integrasi (Integration) Tumpuk (Stack) Chiplet (Chiplet) Teknologi (Technology) Berbeda." },
  { "en": "Apa Itu Silicon Photonics (Fotonika Silikon)?", "id": "Teknologi (Technology) Transmisi (Transmission) Data (Data) Cahaya (Light) Chip (Chip) Silikon." },
  { "en": "Kelebihan (Advantage) Silicon Photonics (Fotonika Silikon)?", "id": "Kecepatan (Speed) Data (Data) Tinggi (High) Konsumsi (Consumption) Daya (Power) Rendah." },
  { "en": "Apa Itu Modulator (Modulator) Fotonik (Photonic)?", "id": "Perangkat (Device) Ubah (Convert) Sinyal (Signal) Listrik (Electrical) Optik (Optical)." },
  { "en": "Apa Itu Detektor (Detector) Foto (Photo) Fotonik?", "id": "Perangkat (Device) Ubah (Convert) Sinyal (Signal) Optik (Optical) Listrik (Electrical)." },
  { "en": "Apa Itu Waveguide (Pemandu Gelombang) Fotonik?", "id": "Struktur (Structure) Pemandu (Guide) Cahaya (Light) Pada (On) Chip (Chip) Silikon." },
  { "en": "Apa Itu Komputasi (Computing) Neuromorfik (Neuromorphic)?", "id": "Desain (Design) Chip (Chip) Meniru (Mimic) Otak (Brain) Manusia (Human) (Neuron)." },
  { "en": "Apa Itu Memristor (Memristor)?", "id": "Komponen (Component) Pasif (Passive) Keempat (Fourth) (Resistansi Memori)." },
  { "en": "Potensi (Potential) Penggunaan (Use) Memristor (Memristor)?", "id": "Memori (Memory) Non-Volatile (Non-Volatile), Komputasi (Computing) Neuromorfik (Neuromorphic)." },
  { "en": "Apa Itu Komputasi (Computing) Kuantum (Quantum)?", "id": "Komputasi (Computing) Menggunakan (Using) Prinsip (Principle) Mekanika (Mechanics) Kuantum (Quantum)." },
  { "en": "Apa Itu Qubit (Quantum Bit)?", "id": "Unit (Unit) Dasar (Basic) Informasi (Information) Komputasi (Computing) Kuantum (Quantum)." },
  { "en": "Apa Itu Superposisi (Superposition) Kuantum?", "id": "Qubit (Quantum Bit) Bisa (Can Be) Nol (0) Satu (1) Bersamaan (Simultaneously)." },
  { "en": "Apa Itu Keterikatan (Entanglement) Kuantum?", "id": "Koneksi (Connection) Kuantum (Quantum) Antara (Between) Qubit (Qubit) Terpisah (Separated)." },
  { "en": "Apa Itu IC (Integrated Circuit) Kontrol (Control) Kriogenik (Cryogenic)?", "id": "IC (IC) CMOS (Complementary Metal-Oxide-Semiconductor) Operasi (Operation) Suhu (Temperature) Sangat (Very) Rendah." },
  { "en": "Mengapa (Why) Perlu (Need) IC (Integrated Circuit) Kriogenik (Cryogenic)?", "id": "Mengontrol (Control) Qubit (Qubit) Komputer (Computer) Kuantum (Quantum) Dingin (Cold)." },
  { "en": "Apa Itu Silikon (Silicon) Oksida (Oxide) Hitam (Black)?", "id": "Material (Material) Nano (Nano) Silikon (Silicon) Penyerap (Absorber) Cahaya (Light) Kuat (Strong)." },
  { "en": "Apa Itu Metamaterial (Metamaterial)?", "id": "Material (Material) Rekayasa (Engineered) Properti (Property) Unik (Unique) (Contoh: Indeks Refraksi Negatif)." },
  { "en": "Apa Itu Graphene (Grafena)?", "id": "Lapisan (Layer) Tunggal (Single) Atom (Atom) Karbon (Carbon) Struktur (Structure) Honeycomb." },
  { "en": "Potensi (Potential) Graphene (Grafena) Elektronik?", "id": "Transistor (Transistor) Cepat (Fast) Konduktor (Conductor) Transparan (Transparent)." },
  { "en": "Apa Itu Carbon Nanotube (CNT)?", "id": "Tabung (Tube) Nano (Nano) Silinder (Cylinder) Atom (Atom) Karbon (Carbon)." },
  { "en": "Apa Itu CNTFET (Carbon Nanotube Field-Effect Transistor)?", "id": "Transistor (Transistor) FET (Field-Effect Transistor) Menggunakan (Using) CNT (Carbon Nanotube) Saluran (Channel)." },
  { "en": "Apa Itu Pendinginan (Cooling) Mikrofluida (Microfluidic) IC?", "id": "Pendinginan (Cooling) Chip (Chip) Cairan (Liquid) Saluran (Channel) Mikro (Micro) Internal." },
  { "en": "Apa Itu Panen (Harvesting) Energi (Energy)?", "id": "Mengumpulkan (Collect) Energi (Energy) Lingkungan (Environment) (Cahaya, Getaran, Panas)." },
  { "en": "Apa Itu IC (Integrated Circuit) Panen (Harvesting) Energi (Energy)?", "id": "IC (IC) PMIC (Power Management IC) Kelola (Manage) Energi (Energy) Panen (Harvested)." },
  { "en": "Apa Itu Efek (Effect) Termoelektrik (Thermoelectric)?", "id": "Pembangkitan (Generation) Tegangan (Voltage) Akibat (Due To) Perbedaan (Difference) Suhu (Temperature)." },
  { "en": "Apa Itu TEG (Thermoelectric Generator)?", "id": "Perangkat (Device) Ubah (Convert) Panas (Heat) Langsung (Directly) Listrik (Electricity)." },
  { "en": "Apa Itu Efek (Effect) Piezoelektrik (Piezoelectric)?", "id": "Pembangkitan (Generation) Tegangan (Voltage) Akibat (Due To) Tekanan (Pressure) Mekanis." },
  { "en": "Apa Itu PZEH (Piezoelectric Energy Harvester)?", "id": "Pemanen (Harvester) Energi (Energy) Berbasis (Based) Getaran (Vibration) Piezoelektrik." },
  { "en": "Apa Itu Metastabilitas Flip-Flop?", "id": "Kondisi Transien Tidak Stabil Antara Nol Satu." },
  { "en": "Apa Itu Jendela Metastabilitas?", "id": "Jendela Waktu Rentan Pelanggaran Setup Hold." },
  { "en": "Apa Itu MTBF (Mean Time Between Failures) Metastabilitas?", "id": "Waktu Rata-Rata Antara Kegagalan Metastabil." },
  { "en": "Apa Itu Pengurangan Hazard Logika?", "id": "Teknik Desain Eliminasi Glitch Hazard." },
  { "en": "Bagaimana Peta Karnaugh Kurangi Hazard?", "id": "Dengan Menambahkan Term Redundan (Consensus)." },
  { "en": "Apa Itu Desain Tahan Hazard?", "id": "Desain Logika Yang Bebas Glitch Hazard." },
  { "en": "Apa Itu FSM (Finite State Machine)?", "id": "Mesin Keadaan Terbatas (Finite State Machine)." },
  { "en": "Apa Itu Transisi Keadaan?", "id": "Perpindahan Dari Satu Keadaan Ke Keadaan Lain." },
  { "en": "Apa Itu Keadaan Tereduksi?", "id": "Jumlah Keadaan Minimum FSM (Finite State Machine)." },
  { "en": "Apa Itu Pengkodean One-Hot?", "id": "Satu Flip-Flop Aktif Per Keadaan (State)." },
  { "en": "Keuntungan Pengkodean One-Hot?", "id": "Logika Dekode Sederhana, Potensi Cepat." },
  { "en": "Apa Itu Pengkodean Biner?", "id": "Jumlah Flip-Flop Minimum (Log2 N)." },
  { "en": "Apa Itu Sel Memori 6T SRAM?", "id": "Enam Transistor Penyimpan Satu Bit." },
  { "en": "Apa Fungsi Dua Inverter SRAM?", "id": "Membentuk Latch (Selak) Penyimpan Data." },
  { "en": "Apa Fungsi Transistor Akses SRAM?", "id": "Menghubungkan Latch (Selak) Ke Jalur Bit." },
  { "en": "Apa Itu Jalur Kata SRAM?", "id": "Sinyal Kontrol Aktivasi Transistor Akses." },
  { "en": "Apa Itu Jalur Bit SRAM?", "id": "Jalur Transfer Data Baca Tulis." },
  { "en": "Apa Itu Operasi Baca SRAM?", "id": "Merasakan Perbedaan Tegangan Jalur Bit." },
  { "en": "Apa Itu Operasi Tulis SRAM?", "id": "Memaksa Tegangan Latch (Selak) Nilai Baru." },
  { "en": "Apa Itu Sel Memori 1T1C DRAM?", "id": "Satu Transistor Satu Kapasitor." },
  { "en": "Bagaimana DRAM (Dynamic Random Access Memory) Simpan Data?", "id": "Menyimpan Muatan Listrik Dalam Kapasitor." },
  { "en": "Apa Itu Operasi Refresh DRAM?", "id": "Membaca Menulis Ulang Data Periodik." },
  { "en": "Mengapa DRAM (Dynamic Random Access Memory) Perlu Refresh?", "id": "Muatan Kapasitor Bocor Seiring Waktu." },
  { "en": "Apa Itu Kapasitor Parit DRAM?", "id": "Kapasitor Dibuat Vertikal Dalam Parit Silikon." },
  { "en": "Apa Itu Kapasitor Tumpuk DRAM?", "id": "Kapasitor Dibuat Di Atas Transistor Akses." },
  { "en": "Apa Itu Penguat Indra DRAM?", "id": "Penguat Diferensial Deteksi Muatan Kecil." },
  { "en": "Apa Itu Operasi Baca Destruktif DRAM?", "id": "Proses Baca Hancurkan Muatan Sel." },
  { "en": "Apa Itu Kontroler Memori?", "id": "IC (Integrated Circuit) Manajemen Akses Baca Tulis Memori." },
  { "en": "Apa Itu RAS (Row Address Strobe)?", "id": "Sinyal Kontrol DRAM (Dynamic RAM) Pilih Alamat Baris." },
  { "en": "Apa Itu CAS (Column Address Strobe)?", "id": "Sinyal Kontrol DRAM (Dynamic RAM) Pilih Alamat Kolom." },
  { "en": "Apa Itu WE (Write Enable)?", "id": "Sinyal Kontrol Tentukan Operasi Baca Tulis." },
  { "en": "Apa Itu Precharge DRAM (Dynamic RAM)?", "id": "Operasi Tutup Baris Siap Buka Baris Baru." },
  { "en": "Apa Itu Skema Alamat Multiplexing DRAM?", "id": "Kirim Alamat Baris Kolom Bergantian." },
  { "en": "Apa Itu Paging Cepat DRAM (Dynamic RAM)?", "id": "Akses Kolom Cepat Baris Sama Terbuka." },
  { "en": "Apa Itu EDO (Extended Data Out) DRAM?", "id": "Tipe DRAM (Dynamic RAM) Paging Cepat Ditingkatkan." },
  { "en": "Apa Itu Pipeline Burst SRAM?", "id": "SRAM (Static RAM) Cepat Transfer Blok Data Sinkron." },
  { "en": "Apa Itu Sel NOR (Bukan OR) Flash?", "id": "Arsitektur Memori Flash Akses Acak Cepat." },
  { "en": "Apa Itu Sel NAND (Bukan DAN) Flash?", "id": "Arsitektur Memori Flash Kepadatan Tinggi Serial." },
  { "en": "Di Mana NOR (Bukan OR) Flash Digunakan?", "id": "Penyimpanan Kode (Contoh: BIOS, Firmware)." },
  { "en": "Di Mana NAND (Bukan DAN) Flash Digunakan?", "id": "Penyimpanan Data Massal (Contoh: SSD, USB Drive)." },
  { "en": "Apa Itu Hot-Electron Injection?", "id": "Mekanisme Pemrograman Memori Flash (NOR)." },
  { "en": "Apa Itu Fowler-Nordheim (FN) Tunneling?", "id": "Mekanisme Pemrograman Hapus Memori Flash." },
  { "en": "Apa Itu Gerbang Apung Flash?", "id": "Gerbang Logam Terisolasi Penyimpan Muatan Data." },
  { "en": "Apa Itu Gerbang Kontrol Flash?", "id": "Gerbang Eksternal Kontrol Pemrograman Gerbang Apung." },
  { "en": "Apa Itu Blok Hapus Flash?", "id": "Memori Flash Dihapus Dalam Blok Besar." },
  { "en": "Apa Itu Halaman Tulis Flash?", "id": "Memori Flash Ditulis Dalam Halaman Kecil." },
  { "en": "Apa Itu SLC (Single-Level Cell) Flash?", "id": "Satu Bit Data Disimpan Per Sel." },
  { "en": "Apa Itu MLC (Multi-Level Cell) Flash?", "id": "Dua Bit Data Disimpan Per Sel." },
  { "en": "Apa Itu TLC (Triple-Level Cell) Flash?", "id": "Tiga Bit Data Disimpan Per Sel." },
  { "en": "Apa Itu QLC (Quad-Level Cell) Flash?", "id": "Empat Bit Data Disimpan Per Sel." },
  { "en": "Mana Paling Tahan Lama?", "id": "SLC (Single-Level Cell) Flash Paling Tahan Lama." },
  { "en": "Mana Paling Murah Per Bit?", "id": "QLC (Quad-Level Cell) Flash Paling Murah." },
  { "en": "Apa Itu Gangguan Baca Flash?", "id": "Operasi Baca Perlahan Ubah Sel Tetangga." },
  { "en": "Apa Itu Gangguan Program Flash?", "id": "Operasi Program Perlahan Ubah Sel Tetangga." },
  { "en": "Apa Itu IC Antarmuka I2S (Inter-IC Sound)?", "id": "Antarmuka Bus Serial Transfer Audio Digital." },
  { "en": "Sinyal Apa Saja I2S (Inter-IC Sound)?", "id": "Jam Serial, Jam Word Select, Data Serial." },
  { "en": "Apa Itu Bus LIN (Local Interconnect Network)?", "id": "Bus Serial Biaya Rendah Otomotif." },
  { "en": "Apa Beda CAN (Controller Area Network) LIN (Local Interconnect Network)?", "id": "LIN (Local Interconnect Network) Lebih Lambat Murah (Single-Master)." },
  { "en": "Apa Itu IC Transceiver LIN (Local Interconnect Network)?", "id": "IC (Integrated Circuit) Antarmuka Fisik Bus LIN." },
  { "en": "Apa Itu IC Transceiver CAN (Controller Area Network)?", "id": "IC (Integrated Circuit) Antarmuka Fisik Bus CAN." },
  { "en": "Apa Itu CAN (Controller Area Network) FD (Flexible Data-Rate)?", "id": "Versi CAN (Controller Area Network) Kecepatan Data Tinggi." },
  { "en": "Apa Itu Resistor Terminator Bus CAN?", "id": "Dua Resistor 120 Ohm Ujung Bus." },
  { "en": "Mengapa Bus CAN (Controller Area Network) Perlu Terminator?", "id": "Mencegah Refleksi Sinyal Pencocokan Impedansi." },
  { "en": "Apa Itu Ethernet Single-Pair?", "id": "Ethernet Kecepatan Tinggi Satu Pasang Kabel." },
  { "en": "Apa Itu Automotive Ethernet?", "id": "Standar Ethernet Untuk Aplikasi Kendaraan." },
  { "en": "Apa Itu Osilator Pierce?", "id": "Osilator Kristal Umum Pakai Inverter." },
  { "en": "Apa Itu Detektor Fasa (PD)?", "id": "Rangkaian Pembanding Fasa Dua Sinyal." },
  { "en": "Apa Itu Detektor Frekuensi Fasa (PFD)?", "id": "Rangkaian Pembanding Fasa Dan Frekuensi." },
  { "en": "Apa Itu Filter Loop PLL (Phase-Locked Loop)?", "id": "LPF (Low-Pass Filter) Integrator Output PFD (PFD)." },
  { "en": "Apa Itu Rentang Tangkapan PLL (Phase-Locked Loop)?", "id": "Rentang Frekuensi PLL (PLL) Dapat Mengunci." },
  { "en": "Apa Itu Rentang Kunci PLL (Phase-Locked Loop)?", "id": "Rentang Frekuensi PLL (PLL) Tetap Terkunci." },
  { "en": "Apa Itu Sintesis Frekuensi Integer-N?", "id": "PLL (Phase-Locked Loop) Faktor Pembagi Bilangan Bulat." },
  { "en": "Apa Itu Sintesis Frekuensi Fractional-N?", "id": "PLL (Phase-Locked Loop) Faktor Pembagi Bilangan Pecahan." },
  { "en": "Kelebihan Fractional-N PLL (Phase-Locked Loop)?", "id": "Resolusi Frekuensi Tinggi Kunci Cepat." },
  { "en": "Apa Itu Modulator Sigma-Delta PLL (Phase-Locked Loop)?", "id": "Rangkaian Hasilkan Pembagian Pecahan Efektif." },
  { "en": "Apa Itu DLL (Delay-Locked Loop)?", "id": "Rangkaian Serupa PLL (PLL) Kunci Tunda Fasa." },
  { "en": "Apa Beda PLL (Phase-Locked Loop) DLL (Delay-Locked Loop)?", "id": "PLL (PLL) Hasilkan Frekuensi, DLL (DLL) Tunda Jam." },
  { "en": "Di Mana DLL (Delay-Locked Loop) Digunakan?", "id": "Manajemen Clock Skew Antarmuka Memori." },
  { "en": "Apa Itu Pengganda Jam?", "id": "Rangkaian (PLL) Hasilkan Jam Frekuensi Tinggi." },
  { "en": "Apa Itu Pembagi Jam?", "id": "Rangkaian (Counter) Hasilkan Jam Frekuensi Rendah." },
  { "en": "Apa Itu De-Skew Jam?", "id": "Teknik (DLL/PLL) Minimalkan Clock Skew." },
  { "en": "Apa Itu Analisis Monte Carlo?", "id": "Simulasi Statistik Perhitungkan Variasi Proses." },
  { "en": "Apa Itu Variasi PVT (Process, Voltage, Temperature)?", "id": "Variasi Kinerja IC Akibat Proses Tegangan Suhu." },
  { "en": "Apa Itu Sudut Proses (SS, FF, SF, FS)?", "id": "Model Kinerja Ekstrem (Lambat, Cepat) Transistor." },
  { "en": "Apa Itu Sudut FF (Fast-Fast)?", "id": "Transistor NMOS (N-channel MOS) PMOS (P-channel MOS) Keduanya Cepat." },
  { "en": "Apa Itu Sudut SS (Slow-Slow)?", "id": "Transistor NMOS (N-channel MOS) PMOS (P-channel MOS) Keduanya Lambat." },
  { "en": "Apa Itu Penyangga Daya Rendah?", "id": "Gerbang Logika Desain Konsumsi Daya Statis Rendah." },
  { "en": "Apa Itu Sel Multi-Vth?", "id": "Sel Standar Tegangan Ambang Berbeda." },
  { "en": "Kapan Pakai Sel Vth (Threshold Voltage) Tinggi?", "id": "Jalur Non-Kritis Kurangi Arus Bocor." },
  { "en": "Kapan Pakai Sel Vth (Threshold Voltage) Rendah?", "id": "Jalur Kritis Penuhi Tuntutan Waktu." },
  { "en": "Apa Itu DVS (Dynamic Voltage Scaling)?", "id": "Mengatur Tegangan Operasi IC Real-Time." },
  { "en": "Apa Itu DFS (Dynamic Frequency Scaling)?", "id": "Mengatur Frekuensi Jam Operasi IC Real-Time." },
  { "en": "Apa Itu DVFS (Dynamic Voltage and Frequency Scaling)?", "id": "Kombinasi Skala Tegangan Frekuensi Dinamis." },
  { "en": "Tujuan DVFS (Dynamic Voltage and Frequency Scaling)?", "id": "Mengoptimalkan Kinerja Konsumsi Daya Dinamis." },
  { "en": "Apa Itu Bias Tubuh Balik?", "id": "Menerapkan Tegangan Balik Substrat Transistor." },
  { "en": "Fungsi Bias Tubuh Balik?", "id": "Meningkatkan Vth (Threshold Voltage) Kurangi Arus Bocor." },
  { "en": "Apa Itu Bias Tubuh Maju?", "id": "Menerapkan Tegangan Maju Substrat Transistor." },
  { "en": "Fungsi Bias Tubuh Maju?", "id": "Menurunkan Vth (Threshold Voltage) Tingkatkan Kinerja." },
  { "en": "Apa Itu Teknologi FD-SOI (Fully Depleted Silicon on Insulator)?", "id": "SOI (Silicon on Insulator) Saluran Sangat Tipis Habis Penuh." },
  { "en": "Kelebihan FD-SOI (Fully Depleted Silicon on Insulator)?", "id": "Kontrol Gerbang Baik Bias Tubuh Efektif." },
  { "en": "Apa Itu Sputtering (Peludahan) Fabrikasi?", "id": "Deposisi Material Metode PVD (Physical Vapor Deposition)." },
  { "en": "Apa Itu PVD (Physical Vapor Deposition)?", "id": "Deposisi Uap Fisik." },
  { "en": "Apa Itu Evaporasi (Penguapan) Termal?", "id": "Metode PVD (Physical Vapor Deposition) Pakai Panas." },
  { "en": "Apa Itu E-Beam (Electron Beam) Evaporation?", "id": "Metode PVD (Physical Vapor Deposition) Pakai Tembakan Elektron." },
  { "en": "Apa Itu Step (Langkah) Coverage (Cakupan)?", "id": "Kemampuan Lapisan Tutupi Topografi Curam." },
  { "en": "Mengapa Step (Langkah) Coverage (Cakupan) Penting?", "id": "Mencegah Jalur Terbuka Atau Tipis." },
  { "en": "Apa Itu Planarisasi (Planarization)?", "id": "Proses Membuat Permukaan Wafer Datar." },
  { "en": "Apa Itu CMP (Chemical Mechanical Polishing)?", "id": "Metode Planarisasi Kimia Mekanis." },
  { "en": "Apa Itu Bubur (Slurry) CMP (Chemical Mechanical Polishing)?", "id": "Cairan Kimia Partikel Abrasif Poles." },
  { "en": "Apa Itu Proses Damascene (Damasin) Tembaga?", "id": "Proses Fabrikasi Interkoneksi Tembaga." },
  { "en": "Apa Itu ECP (Electrochemical Plating)?", "id": "Pelapisan Tembaga Elektrokimia Proses Damascene." },
  { "en": "Apa Itu Lapisan Penghalang Tembaga?", "id": "Lapisan Cegah Difusi Tembaga Silikon." },
  { "en": "Bahan Apa Lapisan Penghalang Tembaga?", "id": "Tantalum (Ta) Tantalum Nitrida (TaN)." },
  { "en": "Apa Itu Lapisan Benih Tembaga?", "id": "Lapisan Tembaga Tipis Awal ECP (Electrochemical Plating)." },
  { "en": "Apa Itu Silisida (Silicide)?", "id": "Senyawa Logam Silikon Kontak Resistansi Rendah." },
  { "en": "Logam Apa Pembuat Silisida?", "id": "Titanium, Kobalt, Nikel." },
  { "en": "Apa Itu Salisida (Salicide)?", "id": "Self-Aligned Silicide (Silisida Sejajar Sendiri)." },
  { "en": "Apa Itu Gerbang Pengganti HKMG?", "id": "Proses Fabrikasi HKMG (High-K Metal Gate) Akhir." },
  { "en": "Apa Itu Gerbang Awal HKMG?", "id": "Proses Fabrikasi HKMG (High-K Metal Gate) Awal." },
  { "en": "Apa Itu DUV (Deep Ultraviolet) Immersion Lithography?", "id": "Litografi DUV (Deep Ultraviolet) Gunakan Cairan Imersi." },
  { "en": "Mengapa Pakai Imersi Cairan?", "id": "Tingkatkan Resolusi Efektif Lensa." },
  { "en": "Cairan Apa Imersi Litografi?", "id": "Air Murni (Ultra-Pure Water)." },
  { "en": "Apa Itu OPC (Optical Proximity Correction)?", "id": "Koreksi Optik Proksimitas." },
  { "en": "Fungsi OPC (Optical Proximity Correction)?", "id": "Modifikasi Pola Masker Kompensasi Distorsi." },
  { "en": "Apa Itu PSM (Phase-Shift Mask)?", "id": "Masker Pergeseran Fasa." },
  { "en": "Fungsi PSM (Phase-Shift Mask)?", "id": "Masker Ubah Fasa Cahaya Tingkatkan Kontras." },
  { "en": "Apa Itu Litografi Paparan Ganda?", "id": "Teknik Cetak Pola Dua Langkah Terpisah." },
  { "en": "Apa Itu Penuaan Sirkuit?", "id": "Degradasi Kinerja IC (Integrated Circuit) Seiring Waktu." },
  { "en": "Efek Apa Sebabkan Penuaan Sirkuit?", "id": "NBTI (Negative Bias Temperature Instability), PBTI (Positive Bias Temperature Instability), HCI (Hot Carrier Injection)." },
  { "en": "Apa Itu NBTI (Negative Bias Temperature Instability)?", "id": "Degradasi PMOS (P-channel MOS) Bias Gerbang Negatif." },
  { "en": "Apa Itu PBTI (Positive Bias Temperature Instability)?", "id": "Degradasi NMOS (N-channel MOS) Bias Gerbang Positif." },
  { "en": "Apa Itu HCI (Hot Carrier Injection)?", "id": "Elektron Lubang \"Panas\" Rusak Oksida Gerbang." },
  { "en": "Apa Itu TDDB (Time-Dependent Dielectric Breakdown)?", "id": "Kerusakan Isolator Oksida Gerbang Seiring Waktu." },
  { "en": "Apa Itu Analisis Penuaan Sirkuit?", "id": "Simulasi Prediksi Degradasi Kinerja IC (Integrated Circuit)." },
  { "en": "Apa Itu Margin Desain Penuaan?", "id": "Margin Kinerja Ekstra Kompensasi Efek Penuaan." },
  { "en": "Apa Itu Model BJT (Bipolar Junction Transistor) Ebers-Moll?", "id": "Model Fisik Sederhana BJT (Bipolar Junction Transistor)." },
  { "en": "Apa Itu Model BJT (Bipolar Junction Transistor) Gummel-Poon?", "id": "Model BJT (Bipolar Junction Transistor) Lebih Akurat." },
  { "en": "Apa Model MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Level 1 SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Model Shichman-Hodges Sederhana." },
  { "en": "Apa Model MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Level 3 SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Model Semi-Empiris Lebih Baik." },
  { "en": "Apa Itu Model BSIM (Berkeley Short-channel IGFET Model)?", "id": "Model Standar Industri MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Modern." },
  { "en": "Apa Itu BSIM3 (Berkeley Short-channel IGFET Model 3)?", "id": "Versi Model BSIM (Berkeley Short-channel IGFET Model) Populer." },
  { "en": "Apa Itu BSIM4 (Berkeley Short-channel IGFET Model 4)?", "id": "Versi BSIM (Berkeley Short-channel IGFET Model) Ditingkatkan Efek Skala Nano." },
  { "en": "Apa Itu BSIM-CMG (Berkeley Short-channel IGFET Model Common Multi-Gate)?", "id": "Model BSIM (Berkeley Short-channel IGFET Model) Transistor FinFET (Fin Field-Effect Transistor)." },
  { "en": "Apa Netlist SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Deskripsi Teks Rangkaian Komponen Koneksi." },
  { "en": "Apa Analisis DC SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Simulasi Temukan Titik Operasi DC (Direct Current)." },
  { "en": "Apa Analisis AC SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Simulasi Respon Frekuensi Sinyal Kecil." },
  { "en": "Apa Analisis Transien SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Simulasi Respon Rangkaian Terhadap Waktu." },
  { "en": "Apa Perintah .OP SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Perintah Lakukan Analisis Titik Operasi DC (Direct Current)." },
  { "en": "Apa Perintah .DC SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Perintah Lakukan Sapuan Tegangan Arus DC (Direct Current)." },
  { "en": "Apa Perintah .AC SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Perintah Lakukan Sapuan Frekuensi AC (Alternating Current)." },
  { "en": "Apa Perintah .TRAN SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Perintah Lakukan Analisis Transien Waktu." },
  { "en": "Apa Perintah .PLOT SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Perintah Cetak Hasil Simulasi Bentuk Teks Grafis." },
  { "en": "Apa Perintah .PRINT SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Perintah Cetak Hasil Simulasi Bentuk Tabel." },
  { "en": "Apa Perintah .MODEL SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Perintah Definisikan Parameter Model Komponen." },
  { "en": "Apa Itu Analisis Kebisingan SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Simulasi Hitung Kebisingan Rangkaian." },
  { "en": "Apa Itu Analisis Distorsi SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Simulasi Hitung Distorsi Harmonik Rangkaian." },
  { "en": "Apa Analisis Sensitivitas SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Simulasi Ukur Dampak Perubahan Komponen." },
  { "en": "Apa Itu HSPICE?", "id": "Versi Komersial SPICE (Simulation Program with Integrated Circuit Emphasis) Populer." },
  { "en": "Apa Itu PSpice?", "id": "Versi SPICE (Simulation Program with Integrated Circuit Emphasis) PC (Personal Computer) Populer." },
  { "en": "Apa Itu LTspice?", "id": "Versi SPICE (Simulation Program with Integrated Circuit Emphasis) Gratis Kinerja Tinggi." },
  { "en": "Apa Itu Verilog-A?", "id": "Bahasa Deskripsi Model Rangkaian Analog." },
  { "en": "Apa Itu Verilog-AMS?", "id": "Bahasa Deskripsi Rangkaian Sinyal Campuran." },
  { "en": "Apa Itu VHDL-AMS (VHDL-Analog Mixed-Signal)?", "id": "Bahasa Deskripsi VHDL (VHSIC Hardware Description Language) Sinyal Campuran." },
  { "en": "Apa Itu Simulasi Sinyal Campuran?", "id": "Simulasi Gabungan Rangkaian Analog Digital Bersamaan." },
  { "en": "Apa Itu Co-Simulasi?", "id": "Menghubungkan Dua Simulator Berbeda (Analog Digital)." },
  { "en": "Apa Itu Rangkaian POR (Power-On Reset)?", "id": "Rangkaian Hasilkan Sinyal Reset Saat Daya Nyala." },
  { "en": "Mengapa Rangkaian POR (Power-On Reset) Penting?", "id": "Memastikan IC (Integrated Circuit) Mulai Kondisi Terdefinisi." },
  { "en": "Apa Itu Rangkaian BOR (Brown-Out Reset)?", "id": "Rangkaian Reset IC (Integrated Circuit) Tegangan Turun Bahaya." },
  { "en": "Apa Itu UVLO (Under-Voltage Lockout)?", "id": "Fitur Matikan IC (Integrated Circuit) Tegangan Input Rendah." },
  { "en": "Apa Itu OVLO (Over-Voltage Lockout)?", "id": "Fitur Matikan IC (Integrated Circuit) Tegangan Input Tinggi." },
  { "en": "Apa Itu OCP (Over-Current Protection)?", "id": "Proteksi Arus Lebih." },
  { "en": "Apa Itu OTP (Over-Temperature Protection)?", "id": "Proteksi Suhu Lebih (Thermal Shutdown)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge) HBM (Human Body Model)?", "id": "Model Tes ESD (Electrostatic Discharge) Simulasi Tubuh Manusia." },
  { "en": "Apa Itu ESD (Electrostatic Discharge) MM (Machine Model)?", "id": "Model Tes ESD (Electrostatic Discharge) Simulasi Mesin Manufaktur." },
  { "en": "Apa Itu ESD (Electrostatic Discharge) CDM (Charged Device Model)?", "id": "Model Tes ESD (Electrostatic Discharge) Simulasi IC (IC) Terisi Muatan." },
  { "en": "Apa Itu Level Latch-Up JEDEC?", "id": "Standar Pengujian Kekebalan Latch-Up (Kait-Atas) IC (IC)." },
  { "en": "Apa Itu Efek Antena Fabrikasi?", "id": "Kerusakan Gerbang Oksida Akibat Pengumpulan Muatan." },
  { "en": "Kapan Efek Antena Terjadi?", "id": "Selama Proses Etsa Plasma Interkoneksi." },
  { "en": "Bagaimana Mencegah Efek Antena?", "id": "Gunakan Jumper Logam Dioda Proteksi." },
  { "en": "Apa Itu Rasio Antena?", "id": "Rasio Area Logam Terhubung Area Gerbang." },
  { "en": "Apa Itu Dioda Jumper Antena?", "id": "Dioda Tambahan Buang Muatan Akibat Efek Antena." },
  { "en": "Apa Itu Pengisian Wafer?", "id": "Penumpukan Muatan Statis Permukaan Wafer Fabrikasi." },
  { "en": "Apa Itu Ionizer (Pengion)?", "id": "Alat Penetral Muatan Statis Cleanroom (Ruang Bersih)." },
  { "en": "Apa Itu Kelas Cleanroom (Ruang Bersih)?", "id": "Standar Kebersihan Udara (Jumlah Partikel)." },
  { "en": "Apa Itu Kelas 1 Cleanroom (Ruang Bersih)?", "id": "Sangat Bersih (Satu Partikel Per Kaki Kubik)." },
  { "en": "Apa Itu FFU (Fan Filter Unit)?", "id": "Unit Kipas Filter HEPA (HEPA) ULPA (ULPA) Cleanroom." },
  { "en": "Apa Itu Filter HEPA (High-Efficiency Particulate Air)?", "id": "Filter Udara Efisiensi Partikulat Tinggi." },
  { "en": "Apa Itu Filter ULPA (Ultra-Low Particulate Air)?", "id": "Filter Udara Partikulat Sangat Rendah." },
  { "en": "Apa Itu Pakaian Cleanroom (Ruang Bersih)?", "id": "Pakaian Khusus Cegah Kontaminasi Partikel Manusia." },
  { "en": "Apa Itu FOUP (Front Opening Unified Pod)?", "id": "Kontainer Standar Pembawa Wafer Otomatis." },
  { "en": "Apa Itu SMIF (Standard Mechanical Interface)?", "id": "Sistem Isolasi Wafer Mini-Environment (Lingkungan Mini)." },
  { "en": "Apa Itu OHT (Overhead Hoist Transport)?", "id": "Sistem Transportasi Otomatis FOUP (Front Opening Unified Pod) Langit-Langit." },
  { "en": "Apa Itu AGV (Automated Guided Vehicle)?", "id": "Kendaraan Pemandu Otomatis Transportasi Material Pabrik." },
  { "en": "Apa Itu Masker Pellicle?", "id": "Membran Tipis Pelindung Reticle (Retikel) Debu." },
  { "en": "Apa Itu Inspeksi Masker?", "id": "Proses Pemeriksaan Cacat Reticle (Retikel) Litografi." },
  { "en": "Apa Itu Perbaikan Masker?", "id": "Proses Perbaikan Cacat Reticle (Retikel) (Contoh: FIB, Laser)." },
  { "en": "Apa Itu OPC (Optical Proximity Correction) Berbasis Model?", "id": "OPC (Optical Proximity Correction) Gunakan Simulasi Model Fisik Litografi." },
  { "en": "Apa Itu OPC (Optical Proximity Correction) Berbasis Aturan?", "id": "OPC (Optical Proximity Correction) Gunakan Aturan Geometris Sederhana." },
  { "en": "Apa Itu Metrologi Wafer?", "id": "Ilmu Pengukuran Dimensi Kritis Wafer." },
  { "en": "Apa Itu CD-SEM (Critical Dimension Scanning Electron Microscope)?", "id": "SEM (Scanning Electron Microscope) Pengukur Dimensi Kritis Pola." },
  { "en": "Apa Itu Elipsometri?", "id": "Teknik Optik Ukur Ketebalan Lapisan Tipis." },
  { "en": "Apa Itu Profilometri?", "id": "Teknik Ukur Topografi Permukaan Wafer." },
  { "en": "Apa Itu Penguat (Amplifier) Diferensial?", "id": "Menguatkan (Amplify) Perbedaan (Difference) Dua (Two) Sinyal (Signal) Input (Input)." },
  { "en": "Apa Itu Cermin (Mirror) Arus (Current)?", "id": "Rangkaian (Circuit) Menyalin (Copy) Arus (Current) Referensi (Reference)." },
  { "en": "Apa Itu Beban (Load) Aktif (Active)?", "id": "Beban (Load) Rangkaian (Circuit) Menggunakan (Using) Transistor (Transistor)." },
  { "en": "Apa Itu Penguat (Amplifier) Cascode (Cascode)?", "id": "Kombinasi (Combination) Penguat (Amplifier) Tingkatkan (Improve) Lebar (Width) Pita (Band)." },
  { "en": "Apa Itu Respon (Response) Frekuensi (Frequency)?", "id": "Perilaku (Behavior) Rangkaian (Circuit) Terhadap (To) Frekuensi (Frequency) Sinyal (Signal)." },
  { "en": "Apa Itu Plot (Plot) Bode (Bode)?", "id": "Grafik (Graph) Respon (Response) Frekuensi (Frequency) (Penguatan Fasa)." },
  { "en": "Apa Itu Kutub (Pole) Rangkaian?", "id": "Frekuensi (Frequency) Penguatan (Gain) Mulai (Start) Turun (Drop)." },
  { "en": "Apa Itu Nol (Zero) Rangkaian?", "id": "Frekuensi (Frequency) Penguatan (Gain) Mulai (Start) Naik (Increase)." },
  { "en": "Apa Itu Margin (Margin) Fasa (Phase)?", "id": "Ukuran (Measure) Stabilitas (Stability) Rangkaian (Circuit) Umpan (Feedback) Balik (Negative)." },
  { "en": "Apa Itu Margin (Margin) Penguatan (Gain)?", "id": "Ukuran (Measure) Stabilitas (Stability) Lainnya (Other) Rangkaian (Circuit)." },
  { "en": "Apa Itu Osilasi (Oscillation) Rangkaian?", "id": "Kondisi (Condition) Tidak (Not) Stabil (Stable) Hasilkan (Generate) Sinyal (Signal) Sendiri." },
  { "en": "Apa Itu Kompensasi (Compensation) Frekuensi (Frequency)?", "id": "Teknik (Technique) Stabilkan (Stabilize) Penguat (Amplifier) Umpan (Feedback) Balik (Negative)." },
  { "en": "Apa Itu Umpan (Feedback) Balik (Negative) Positif (Positive)?", "id": "Umpan (Feedback) Balik (Negative) Penguat (Amplifier) Sinyal (Signal) (Osilasi)." },
  { "en": "Apa Itu Kriteria (Criteria) Barkhausen?", "id": "Syarat (Condition) Terjadinya (Occurrence) Osilasi (Oscillation) Stabil (Stable)." },
  { "en": "Apa Itu Arus (Current) Jenuh (Saturation) Transistor?", "id": "Arus (Current) Maksimal (Maximum) Transistor (Transistor) (Kondisi Aktif)." },
  { "en": "Apa Itu Daerah (Region) Cutoff (Sumbat) Transistor?", "id": "Kondisi (Condition) Transistor (Transistor) Mati (Off) (Saklar Terbuka)." },
  { "en": "Apa Itu Daerah (Region) Triode (Trioda) MOSFET?", "id": "Kondisi (Condition) MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Bekerja (Work) Resistor (Resistor) Variabel." },
  { "en": "Apa Itu Daerah (Region) Saturasi (Jenuh) MOSFET?", "id": "Kondisi (Condition) MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Bekerja (Work) Penguat (Amplifier)." },
  { "en": "Apa Itu Daerah (Region) Aktif (Active) BJT?", "id": "Kondisi (Condition) BJT (Bipolar Junction Transistor) Bekerja (Work) Penguat (Amplifier)." },
  { "en": "Apa Itu Daerah (Region) Saturasi (Jenuh) BJT?", "id": "Kondisi (Condition) BJT (Bipolar Junction Transistor) Bekerja (Work) Saklar (Switch) Tertutup (Closed)." },
  { "en": "Apa Itu Pemetaan (Mapping) Logika (Logic)?", "id": "Proses (Process) Sintesis (Synthesis) Ubah (Convert) Gerbang (Gate) Sel (Cell) Standar (Standard)." },
  { "en": "Apa Itu Penggerak (Driver) Bus (Bus)?", "id": "IC (Integrated Circuit) Penguat (Amplifier) Sinyal (Signal) Bus (Bus) (Contoh: Tristate Buffer)." },
  { "en": "Apa Itu Transceiver (Transiver) Bus (Bus)?", "id": "IC (Integrated Circuit) Kirim (Send) Terima (Receive) Data (Data) Bus (Bus) (Dua Arah)." },
  { "en": "Apa Itu Arbitrasi (Arbitration) Bus (Bus)?", "id": "Proses (Process) Penentuan (Determination) Master (Master) Bus (Bus) Berikutnya (Next)." },
  { "en": "Apa Itu Master (Master) Bus (Bus)?", "id": "Perangkat (Device) Pengendali (Controller) Bus (Bus) Inisiasi (Initiate) Transfer (Transfer)." },
  { "en": "Apa Itu Slave (Slave) Bus (Bus)?", "id": "Perangkat (Device) Merespon (Respond) Permintaan (Request) Master (Master) Bus (Bus)." },
  { "en": "Apa Itu Protokol (Protocol) Bus (Bus)?", "id": "Aturan (Rule) Komunikasi (Communication) Transfer (Transfer) Data (Data) Bus (Bus)." },
  { "en": "Apa Itu Alokasi (Allocation) Alamat (Address)?", "id": "Pemberian (Assignment) Rentang (Range) Alamat (Address) Unik (Unique) Perangkat (Device)." },
  { "en": "Apa Itu Peta (Map) Memori (Memory)?", "id": "Diagram (Diagram) Alokasi (Allocation) Alamat (Address) Memori (Memory) Periferal (Peripheral)." },
  { "en": "Apa Itu MMIO (Memory-Mapped I/O)?", "id": "Akses (Access) Periferal (Peripheral) I/O (Input/Output) Seperti (Like) Alamat (Address) Memori." },
  { "en": "Apa Itu Port-Mapped I/O (PMIO)?", "id": "Akses (Access) Periferal (Peripheral) I/O (Input/Output) Instruksi (Instruction) Khusus (Special)." },
  { "en": "Apa Itu Buffer (Penyangga) FIFO (First-In First-Out) Perangkat Keras?", "id": "Memori (Memory) Perangkat (Hardware) Keras (Hardware) Antrian (Queue) Data (Data)." },
  { "en": "Apa Itu Watermark (Tanda Air) FIFO (First-In First-Out)?", "id": "Ambang (Threshold) Batas (Limit) Picu (Trigger) Interupsi (Interrupt) (Contoh: Penuh, Kosong)." },
  { "en": "Apa Itu Overflow (Limpahan) FIFO (First-In First-Out)?", "id": "Kondisi (Condition) Menulis (Write) Data (Data) FIFO (FIFO) Penuh (Full)." },
  { "en": "Apa Itu Underflow (Kurang Alir) FIFO (First-In First-Out)?", "id": "Kondisi (Condition) Membaca (Read) Data (Data) FIFO (FIFO) Kosong (Empty)." },
  { "en": "Apa Itu Pengganda (Multiplier) Logika (Logic)?", "id": "Rangkaian (Circuit) Kombinasional (Combinational) Perkalian (Multiplication) Bilangan (Number) Biner (Binary)." },
  { "en": "Apa Itu Barrel Shifter (Penggeser Barel)?", "id": "Rangkaian (Circuit) Logika (Logic) Geser (Shift) Data (Data) Banyak (Multiple) Bit (Bit)." },
  { "en": "Apa Itu Logika (Logic) Tiga (Three) Keadaan (State)?", "id": "Logika (Logic) Output (Output) (Tinggi, Rendah, Impedansi Tinggi)." },
  { "en": "Apa Itu Gerbang (Gate) Transmisi (Transmission)?", "id": "Saklar (Switch) Elektronik (Electronic) Dua (Two) Arah (Direction) CMOS (CMOS)." },
  { "en": "Komponen (Component) Apa (What) Gerbang (Gate) Transmisi (Transmission)?", "id": "Satu (One) NMOS (N-channel MOS) Satu (One) PMOS (P-channel MOS) Paralel (Parallel)." },
  { "en": "Apa Itu Logika (Logic) Pass-Transistor (Transistor Lolos)?", "id": "Desain (Design) Logika (Logic) Gunakan (Use) Transistor (Transistor) Saklar (Switch) Sinyal (Signal)." },
  { "en": "Apa Itu Logika (Logic) Dinamis (Dynamic) CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Logika (Logic) Gunakan (Use) Muatan (Charge) Kapasitor (Capacitor) Evaluasi (Evaluation) Jam (Clock)." },
  { "en": "Kelebihan (Advantage) Logika (Logic) Dinamis (Dynamic)?", "id": "Cepat (Fast) Jumlah (Count) Transistor (Transistor) Sedikit (Few)." },
  { "en": "Kekurangan (Disadvantage) Logika (Logic) Dinamis (Dynamic)?", "id": "Kompleks (Complex) Butuh (Need) Jam (Clock) Rawan (Prone) Kebocoran (Leakage)." },
  { "en": "Apa Itu Logika (Logic) Domino (Domino)?", "id": "Tipe (Type) Logika (Logic) Dinamis (Dynamic) Rangkaian (Circuit) Bertingkat (Cascaded)." },
  { "en": "Apa Itu Sel (Cell) SRAM (Static Random Access Memory) 4T (Empat Transistor)?", "id": "Sel (Cell) SRAM (Static Random Access Memory) Empat (Four) Transistor (Transistor) Dua (Two) Resistor (Resistor)." },
  { "en": "Apa Itu Sel (Cell) SRAM (Static Random Access Memory) 2T (Dua Transistor)?", "id": "Sel (Cell) SRAM (Static Random Access Memory) Sangat (Very) Padat (Dense) (Kurang Umum)." },
  { "en": "Apa Itu Teknologi (Technology) BiCMOS (Bipolar CMOS)?", "id": "Gabungkan (Combine) Transistor (Transistor) BJT (Bipolar Junction Transistor) CMOS (CMOS) Chip (Chip)." },
  { "en": "Kelebihan (Advantage) BiCMOS (Bipolar CMOS)?", "id": "Kecepatan (Speed) BJT (Bipolar Junction Transistor) Daya (Power) Rendah (Low) CMOS (CMOS)." },
  { "en": "Apa Itu Epitaksi (Epitaxy)?", "id": "Proses (Process) Penumbuhan (Growth) Lapisan (Layer) Kristal (Crystal) Tunggal (Single) Substrat (Substrate)." },
  { "en": "Apa Itu MBE (Molecular Beam Epitaxy)?", "id": "Epitaksi (Epitaxy) Presisi (Precision) Tinggi (High) Ultra-Vacuum (Vakum Ultra)." },
  { "en": "Apa Itu MOCVD (Metalorganic Chemical Vapor Deposition)?", "id": "Deposisi (Deposition) Uap (Vapor) Kimia (Chemical) Organologam (Organometallic)." },
  { "en": "Di Mana (Where) MBE (Molecular Beam Epitaxy) MOCVD (Metalorganic Chemical Vapor Deposition) Digunakan?", "id": "Fabrikasi (Fabrication) Semikonduktor (Semiconductor) Majemuk (Compound) (Contoh: GaAs)." },
  { "en": "Apa Itu Gettering (Pemerangkapan) Wafer?", "id": "Proses (Process) Hilangkan (Remove) Kontaminan (Contaminant) Logam (Metal) Wafer (Wafer)." },
  { "en": "Apa Itu Gettering (Pemerangkapan) Intrinsik (Intrinsic)?", "id": "Gunakan (Use) Cacat (Defect) Oksigen (Oxygen) Dalam (Bulk) Wafer (Wafer)." },
  { "en": "Apa Itu Gettering (Pemerangkapan) Ekstrinsik (Extrinsic)?", "id": "Gunakan (Use) Lapisan (Layer) Belakang (Backside) Wafer (Wafer) Perangkap (Trap) Kontaminan." },
  { "en": "Apa Itu LSA (Laser Spike Annealing)?", "id": "Proses (Process) Annealing (Pelianinan) Cepat (Rapid) Pakai (Use) Laser (Laser)." },
  { "en": "Apa Itu RTP (Rapid Thermal Processing)?", "id": "Proses (Process) Pemanasan (Heating) Cepat (Rapid) Pakai (Use) Lampu (Lamp) Daya (Power) Tinggi (High)." },
  { "en": "Apa Itu Millisecond (Mili Detik) Annealing?", "id": "Proses (Process) Annealing (Pelianinan) Sangat (Very) Singkat (Short) (Flash/Laser)." },
  { "en": "Apa Itu Tahanan (Resistor) Film (Film) Tipis (Thin)?", "id": "Resistor (Resistor) Presisi (Precision) Tinggi (High) Dibuat (Made) Deposisi (Deposition)." },
  { "en": "Apa Itu Tahanan (Resistor) Poly (Polisilicon)?", "id": "Resistor (Resistor) Dibuat (Made) Lapisan (Layer) Polisilicon (Polycrystalline Silicon) Doping (Doping)." },
  { "en": "Apa Itu Tahanan (Resistor) Difusi (Diffused)?", "id": "Resistor (Resistor) Dibuat (Made) Daerah (Region) Difusi (Diffusion) Substrat (Substrate)." },
  { "en": "Apa Itu Koefisien (Coefficient) Suhu (Temperature) Resistor (TCR)?", "id": "Ukuran (Measure) Perubahan (Change) Resistansi (Resistance) Terhadap (To) Suhu (Temperature)." },
  { "en": "Apa Itu Koefisien (Coefficient) Tegangan (Voltage) Resistor (VCR)?", "id": "Ukuran (Measure) Perubahan (Change) Resistansi (Resistance) Terhadap (To) Tegangan (Voltage)." },
  { "en": "Apa Itu Kapasitor (Capacitor) MOS (Metal-Oxide-Semiconductor)?", "id": "Kapasitor (Capacitor) Dibuat (Made) Struktur (Structure) MOS (Metal-Oxide-Semiconductor)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Sambungan (Junction)?", "id": "Kapasitor (Capacitor) Dibuat (Made) Sambungan (Junction) PN (PN Junction) Bias (Bias) Mundur (Reverse)." },
  { "en": "Apa Itu Kapasitor (Capacitor) MIM (Metal-Insulator-Metal)?", "id": "Kapasitor (Capacitor) Presisi (Precision) Tinggi (High) (Logam-Isolator-Logam)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Fringe (Pinggiran)?", "id": "Kapasitor (Capacitor) Dibuat (Made) Struktur (Structure) Jari-Jari (Finger) Logam (Metal)." },
  { "en": "Apa Itu Kepadatan (Density) Kapasitansi (Capacitansi)?", "id": "Kapasitansi (Capacitansi) Per (Per) Satuan (Unit) Luas (Area) Chip (Chip)." },
  { "en": "Apa Itu Pencocokan (Matching) Komponen (Component) Analog?", "id": "Membuat (Make) Dua (Two) Komponen (Component) Identik (Identical) Mungkin (Possible)." },
  { "en": "Teknik (Technique) Apa (What) Pencocokan (Matching) Komponen (Component)?", "id": "Tata (Layout) Letak (Layout) Common-Centroid (Sentroid Umum), Interdigitasi (Interdigitation)." },
  { "en": "Apa Itu Tata (Layout) Letak (Layout) Common-Centroid?", "id": "Penempatan (Placement) Komponen (Component) Simetris (Symmetrical) Sekitar (Around) Titik (Point) Pusat (Center)." },
  { "en": "Apa Itu Interdigitasi (Interdigitation) Tata (Layout) Letak?", "id": "Penempatan (Placement) Komponen (Component) Pola (Pattern) Jari-Jari (Finger) Berselang-Seling (Alternating)." },
  { "en": "Apa Itu Efek (Effect) Tepi (Edge) Layout?", "id": "Variasi (Variation) Komponen (Component) Dekat (Near) Tepi (Edge) Struktur (Structure)." },
  { "en": "Apa Itu Sel (Cell) Dummy (Tiruan) Layout?", "id": "Sel (Cell) Tambahan (Additional) Tepi (Edge) Array (Array) Seragamkan (Uniform) Lingkungan." },
  { "en": "Apa Itu Efek (Effect) LOD (Length of Diffusion)?", "id": "Efek (Effect) Stres (Stress) Mekanis (Mechanical) Performa (Performance) Transistor (Transistor)." },
  { "en": "Apa Itu Well (Sumbu) Proximity (Proksimitas) Effect?", "id": "Efek (Effect) Tepi (Edge) Sumur (Well) Karakteristik (Characteristic) Transistor (Transistor)." },
  { "en": "Apa Itu Analisis (Analysis) IR (IR Drop) Statis?", "id": "Analisis (Analysis) Penurunan (Drop) Tegangan (Voltage) Arus (Current) DC (Direct Current) Rata-Rata (Average)." },
  { "en": "Apa Itu Analisis (Analysis) IR (IR Drop) Dinamis?", "id": "Analisis (Analysis) Penurunan (Drop) Tegangan (Voltage) Arus (Current) Puncak (Peak) Transien." },
  { "en": "Apa Itu Rumpun (Cluster) Tes (Test)?", "id": "Struktur (Structure) Tes (Test) Khusus (Special) Wafer (Wafer) Pantau (Monitor) Proses (Process)." },
  { "en": "Apa Itu Kurva (Curve) C-V (Kapasitansi-Tegangan)?", "id": "Grafik (Graph) Karakteristik (Characteristic) Kapasitansi (Capacitansi) Struktur (Structure) MOS (MOS)." },
  { "en": "Apa Itu Tegangan (Voltage) Flat-Band (Pita Datar)?", "id": "Tegangan (Voltage) Gerbang (Gate) Tidak (No) Ada (No) Pembengkokan (Bending) Pita (Band) Energi." },
  { "en": "Apa Itu Tegangan (Voltage) Ambang (Threshold) (Vt)?", "id": "Tegangan (Voltage) Gerbang (Gate) Mulai (Start) Bentuk (Form) Saluran (Channel) Inversi." },
  { "en": "Apa Itu Modulasi (Modulation) Panjang (Length) Saluran (Channel)?", "id": "Efek (Effect) Perubahan (Change) Panjang (Length) Saluran (Channel) Efektif (Effective) Tegangan (Voltage) Drain." },
  { "en": "Apa Itu Parameter (Parameter) Early (Awal) (VA)?", "id": "Parameter (Parameter) BJT (Bipolar Junction Transistor) Terkait (Related) Modulasi (Modulation) Lebar (Width) Basis (Base)." },
  { "en": "Apa Itu Efek (Effect) Kirk (Kirk)?", "id": "Perluasan (Expansion) Daerah (Region) Basis (Base) Arus (Current) Kolektor (Collector) Tinggi (High)." },
  { "en": "Apa Itu Quasi-Saturasi (Saturasi Semu)?", "id": "Kondisi (Condition) Operasi (Operation) BJT (Bipolar Junction Transistor) Daya (Power) Arus (Current) Tinggi (High)." },
  { "en": "Apa Itu SOAR (Safe Operating Area)?", "id": "Area (Area) Operasi (Operation) Aman (Safe) Transistor (Transistor) (Tegangan Arus)." },
  { "en": "Apa Itu SOA (Safe Operating Area) Maju (Forward) Bias (Bias)?", "id": "SOAR (Safe Operating Area) Transistor (Transistor) Kondisi (Condition) Aktif (Active) Maju (Forward)." },
  { "en": "Apa Itu SOA (Safe Operating Area) Mundur (Reverse) Bias (Bias)?", "id": "SOAR (Safe Operating Area) Transistor (Transistor) Kondisi (Condition) Mati (Off) Cepat (Fast)." },
  { "en": "Apa Itu Tembus (Breakdown) Kedua (Second)?", "id": "Kegagalan (Failure) Termal (Thermal) Destruktif (Destructive) BJT (Bipolar Junction Transistor) Daya (Power)." },
  { "en": "Apa Itu Balasting (Ballasting) Resistor (Resistor) BJT?", "id": "Resistor (Resistor) Kecil (Small) Emitor (Emitter) Cegah (Prevent) Thermal (Thermal) Runaway." },
  { "en": "Apa Itu Thermal (Termal) Runaway (Pelarian)?", "id": "Kondisi (Condition) Panas (Heat) Hasilkan (Generate) Arus (Current) Lebih (More) (Siklus Positif)." },
  { "en": "Apa Itu Sirkuit (Circuit) Baker (Baker) Clamp?", "id": "Rangkaian (Circuit) Cegah (Prevent) Saturasi (Saturation) Keras (Hard) BJT (Bipolar Junction Transistor)." },
  { "en": "Mengapa (Why) Saturasi (Saturation) Keras (Hard) Buruk (Bad)?", "id": "Memperlambat (Slow Down) Waktu (Time) Mati (Off) Transistor (Transistor) (Storage Time)." },
  { "en": "Apa Itu Waktu (Time) Penyimpanan (Storage) BJT (Bipolar Junction Transistor)?", "id": "Tunda (Delay) Waktu (Time) Matikan (Turn Off) BJT (BJT) Saturasi (Saturation)." },
  { "en": "Apa Itu Penggerak (Driver) Gerbang (Gate) Anti-Saturasi?", "id": "Rangkaian (Circuit) Kontrol (Control) Jaga (Keep) Transistor (Transistor) Keluar (Out) Saturasi (Saturation)." },
  { "en": "Apa Itu Filter Sallen-Key?", "id": "Topologi Filter Aktif Populer." },
  { "en": "Apa Itu Filter MFB (Multiple Feedback)?", "id": "Topologi Filter Aktif Umpan Balik Ganda." },
  { "en": "Apa Itu THD (Total Harmonic Distortion)?", "id": "Ukuran Distorsi Harmonik Total Sinyal." },
  { "en": "Apa Itu IMD (Intermodulation Distortion)?", "id": "Distorsi Akibat Interaksi Dua Sinyal." },
  { "en": "Apa Itu SINAD (Signal-to-Noise and Distortion Ratio)?", "id": "Rasio Sinyal Kebisingan Dan Distorsi." },
  { "en": "Apa Itu ENOB (Effective Number of Bits)?", "id": "Jumlah Bit Efektif ADC (Analog-to-Digital Converter)." },
  { "en": "Apa Itu Jitter Sinyal?", "id": "Variasi Waktu Tepi Sinyal Acak." },
  { "en": "Apa Itu Jitter Periodik?", "id": "Jitter Pola Berulang Spesifik." },
  { "en": "Apa Itu Jitter Acak?", "id": "Jitter Tak Terduga (Gaussian)." },
  { "en": "Apa Itu Diagram Mata?", "id": "Visualisasi Kualitas Sinyal Digital Serial." },
  { "en": "Apa Itu Bukaan Mata Horizontal?", "id": "Menunjukkan Toleransi Jitter Waktu." },
  { "en": "Apa Itu Bukaan Mata Vertikal?", "id": "Menunjukkan Toleransi Kebisingan Amplitudo." },
  { "en": "Apa Itu Topologi Catu Daya?", "id": "Desain Arsitektur Rangkaian Catu Daya." },
  { "en": "Apa Itu Cuk Mode Diferensial?", "id": "Induktor Redam Kebisingan Mode Diferensial." },
  { "en": "Apa Itu Kapasitor X?", "id": "Kapasitor Keamanan Antara Jalur Netral." },
  { "en": "Apa Itu Kapasitor Y?", "id": "Kapasitor Keamanan Jalur Ke Ground." },
  { "en": "Apa Itu Kontrol Sisi Primer?", "id": "Regulasi Catu Daya Tanpa Umpan Balik Optik." },
  { "en": "Apa Itu Kontrol Sisi Sekunder?", "id": "Regulasi Catu Daya Pakai Umpan Balik Optik." },
  { "en": "Apa Itu Trafo Flyback?", "id": "Trafo Penyimpan Energi Catu Daya Flyback." },
  { "en": "Apa Itu Trafo Forward?", "id": "Trafo Transfer Energi Catu Daya Forward." },
  { "en": "Apa Itu Penyearah Sinkron?", "id": "Penyearah Gunakan MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Ganti Dioda." },
  { "en": "Kelebihan Penyearah Sinkron?", "id": "Efisiensi Jauh Lebih Tinggi." },
  { "en": "Apa Itu Zero Crossing Detection?", "id": "Deteksi Titik Sinyal AC (Alternating Current) Lintasi Nol Volt." },
  { "en": "Apa Itu Kontrol Sudut Fasa?", "id": "Metode Kontrol Daya AC (Alternating Current) (Contoh: Dimmer)." },
  { "en": "Apa Itu Kontrol Burst Fire?", "id": "Metode Kontrol Daya Nyala Mati Siklus." },
  { "en": "Apa Itu WLCSP (Wafer-Level Chip Scale Package)?", "id": "Kemasan Skala Chip Level Wafer." },
  { "en": "Apa Itu COF (Chip on Flex)?", "id": "Chip IC (Integrated Circuit) Dipasang Papan Sirkuit Fleksibel." },
  { "en": "Apa Itu COB (Chip on Board)?", "id": "Chip IC (Integrated Circuit) Dipasang Langsung PCB (Printed Circuit Board)." },
  { "en": "Apa Itu COG (Chip on Glass)?", "id": "Chip IC (Integrated Circuit) Dipasang Langsung Kaca (Layar)." },
  { "en": "Apa Itu Yield Manufaktur?", "id": "Persentase Chip Fungsional Dari Wafer." },
  { "en": "Apa Itu E-Test (Electrical Test) Wafer?", "id": "Pengujian Listrik Fungsional Level Wafer." },
  { "en": "Apa Itu Binning Chip?", "id": "Proses Sortir Chip Berbasis Kinerja." },
  { "en": "Contoh Binning Chip?", "id": "CPU (Central Processing Unit) Beda Kecepatan Jam." },
  { "en": "Apa Itu Uji Bakar (Burn-In)?", "id": "Pengujian Suhu Tinggi Deteksi Kegagalan Dini." },
  { "en": "Apa Itu Uji HAST (Highly Accelerated Stress Test)?", "id": "Uji Stres Kelembaban Suhu Tekanan." },
  { "en": "Apa Itu Uji HTOL (High Temperature Operating Life)?", "id": "Uji Keandalan Operasi Suhu Tinggi." },
  { "en": "Apa Itu Uji TC (Thermal Cycling)?", "id": "Uji Siklus Suhu Ekstrem (Panas Dingin)." },
  { "en": "Apa Itu Uji THB (Temperature Humidity Bias)?", "id": "Uji Keandalan Bias Tegangan Lembab Panas." },
  { "en": "Apa Itu Tumpukan PCB (Printed Circuit Board)?", "id": "Susunan Lapisan Tembaga Isolator PCB (Printed Circuit Board)." },
  { "en": "Bahan Apa Inti PCB (Printed Circuit Board)?", "id": "FR-4 (Flame Retardant 4) (Fiberglass Epoksi)." },
  { "en": "Apa Itu Prepreg PCB (Printed Circuit Board)?", "id": "Bahan FR-4 (Flame Retardant 4) Setengah Kering Perekat Lapisan." },
  { "en": "Apa Itu Via Tembus (Through-Hole)?", "id": "Lubang Tembus Semua Lapisan PCB (Printed Circuit Board)." },
  { "en": "Apa Itu Via Buta (Blind)?", "id": "Lubang Hubungkan Lapisan Luar Lapisan Dalam." },
  { "en": "Apa Itu Via Terkubur (Buried)?", "id": "Lubang Hubungkan Antar Lapisan Dalam Saja." },
  { "en": "Apa Itu Via-in-Pad?", "id": "Via Ditempatkan Langsung Pad Solder SMD (Surface Mount Device)." },
  { "en": "Mengapa Pakai Via-in-Pad?", "id": "Hemat Ruang Perutean BGA (Ball Grid Array) Rapat." },
  { "en": "Apa Itu Aspect Ratio Via?", "id": "Rasio Kedalaman Lubang Terhadap Diameter Lubang." },
  { "en": "Apa Itu Pengisian Via?", "id": "Mengisi Lubang Via Epoksi Tembaga." },
  { "en": "Apa Itu Penutupan (Tenting) Via?", "id": "Menutup Lubang Via Solder Mask (Masker Solder)." },
  { "en": "Apa Itu Overshoot Sinyal?", "id": "Sinyal Melampaui Level Tegangan Target (Tinggi)." },
  { "en": "Apa Itu Undershoot Sinyal?", "id": "Sinyal Turun Bawah Level Tegangan Target (Rendah)." },
  { "en": "Apa Itu Ringing Sinyal?", "id": "Osilasi Sinyal Sekitar Level Tegangan Stabil." },
  { "en": "Penyebab Ringing Overshoot?", "id": "Mismatch Impedansi Refleksi Sinyal." },
  { "en": "Apa Itu Terminator Seri?", "id": "Resistor Dekat Sumber Sinyal Cocokkan Impedansi." },
  { "en": "Apa Itu Terminator Paralel?", "id": "Resistor Dekat Beban Sinyal Cocokkan Impedansi." },
  { "en": "Apa Itu Terminator Thevenin?", "id": "Terminator Paralel Pembagi Tegangan." },
  { "en": "Apa Itu Terminator AC (Alternating Current)?", "id": "Terminator Paralel Resistor Kapasitor Seri." },
  { "en": "Apa Itu Jalur Kopling?", "id": "Dua Jalur PCB (Printed Circuit Board) Berdekatan (Crosstalk)." },
  { "en": "Apa Itu Crosstalk Ujung Dekat (NEXT)?", "id": "Crosstalk (Bicara Silang) Diukur Ujung Sumber Pengganggu." },
  { "en": "Apa Itu Crosstalk Ujung Jauh (FEXT)?", "id": "Crosstalk (Bicara Silang) Diukur Ujung Beban Pengganggu." },
  { "en": "Cara Kurangi Crosstalk?", "id": "Jauhkan Jalur, Gunakan Ground Plane." },
  { "en": "Apa Itu Perutean Diferensial?", "id": "Merutekan Dua Jalur Sinyal Paralel Rapat." },
  { "en": "Mengapa Perutean Diferensial Rapat?", "id": "Maksimalkan Penolakan Kebisingan Common-Mode." },
  { "en": "Apa Itu Impedansi Diferensial?", "id": "Impedansi Total Pasangan Diferensial (Contoh: 100 Ohm)." },
  { "en": "Apa Itu Impedansi Common-Mode?", "id": "Impedansi Satu Jalur Pasangan Ke Ground." },
  { "en": "Apa Itu Skew Pasangan Diferensial?", "id": "Perbedaan Waktu Tiba Sinyal Pasangan Diferensial." },
  { "en": "Cara Perbaiki Skew Pasangan?", "id": "Tambah Pola Berkelok Jalur Lebih Pendek." },
  { "en": "Apa Itu Pola Berkelok (Serpentine) Routing?", "id": "Pola Jalur PCB (Printed Circuit Board) Samakan Panjang." },
  { "en": "Apa Itu Penjaga Ground (Guard Trace)?", "id": "Jalur Ground Sekitar Sinyal Sensitif." },
  { "en": "Fungsi Penjaga Ground?", "id": "Blokir Kebisingan Crosstalk Sinyal Analog." },
  { "en": "Apa Itu Moat Ground?", "id": "Pemisahan Area Ground PCB (Printed Circuit Board) (Analog Digital)." },
  { "en": "Apa Itu Koneksi Star Ground?", "id": "Hubungkan Semua Ground Ke Satu Titik Pusat." },
  { "en": "Apa Itu Pergeseran Level Logika?", "id": "Mengubah Sinyal Logika Antar Tegangan Berbeda." },
  { "en": "Apa Itu Translator Tegangan Dua Arah?", "id": "IC (Integrated Circuit) Geser Level Logika Dua Arah." },
  { "en": "Apa Itu IC Pengawas Tegangan?", "id": "IC (Integrated Circuit) Monitor Catu Daya Hasilkan Reset." },
  { "en": "Apa Itu Detektor Jendela Tegangan?", "id": "Monitor Tegangan Dalam Rentang (Atas Bawah)." },
  { "en": "Apa Itu Sequencer Daya?", "id": "IC (Integrated Circuit) Kontrol Urutan Nyala Mati Catu Daya." },
  { "en": "Mengapa Perlu Urutan Daya?", "id": "Cegah Kerusakan Latch-Up (Kait-Atas) FPGA (FPGA) CPU (CPU)." },
  { "en": "Apa Itu Saklar Beban Daya?", "id": "IC (Integrated Circuit) MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Kontrol Distribusi Daya." },
  { "en": "Fitur Apa Saklar Beban?", "id": "Soft Start, Proteksi Arus, Discharge Cepat." },
  { "en": "Apa Itu Rangkaian ORing Daya?", "id": "Rangkaian Pilih Satu Dari Banyak Sumber Daya." },
  { "en": "Apa Itu Dioda Ideal?", "id": "IC (Integrated Circuit) Kontroler MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) Perilaku Dioda Sempurna." },
  { "en": "Kelebihan Dioda Ideal?", "id": "Penurunan Tegangan Maju Sangat Rendah." },
  { "en": "Apa Itu Proteksi Polaritas Terbalik?", "id": "Rangkaian Cegah Kerusakan Koneksi Daya Terbalik." },
  { "en": "Cara Sederhana Proteksi Polaritas Terbalik?", "id": "Gunakan Satu Dioda Seri." },
  { "en": "Cara Efisien Proteksi Polaritas Terbalik?", "id": "Gunakan MOSFET (Metal-Oxide-Semiconductor Field-Effect Transistor) P-Channel." },
  { "en": "Apa Itu IC Otentikasi?", "id": "IC (Integrated Circuit) Keamanan Verifikasi Keaslian Produk." },
  { "en": "Apa Itu Koproprosesor Kriptografi?", "id": "IC (Integrated Circuit) Akselerasi Perangkat Keras Operasi Enkripsi." },
  { "en": "Apa Itu TPM (Trusted Platform Module)?", "id": "Standar Kriptoprosesor Aman Simpan Kunci." },
  { "en": "Apa Itu HSM (Hardware Security Module)?", "id": "Modul Keamanan Perangkat Keras." },
  { "en": "Apa Itu Firmware Over-The-Air (OTA)?", "id": "Pembaruan Firmware Nirkabel Jarak Jauh." },
  { "en": "Apa Itu Bootloader Aman?", "id": "Bootloader Verifikasi Tanda Tangan Digital Firmware." },
  { "en": "Apa Itu Jaringan Syaraf Tiruan (ANN)?", "id": "Model Komputasi Terinspirasi Otak Biologis." },
  { "en": "Apa Itu Pembelajaran Mesin (ML)?", "id": "Sub-Bidang AI (Artificial Intelligence) Sistem Belajar Data." },
  { "en": "Apa Itu Akselerator AI (Artificial Intelligence)?", "id": "IC (Integrated Circuit) Perangkat Keras Khusus Komputasi AI (AI)." },
  { "en": "Apa Itu TPU (Tensor Processing Unit)?", "id": "ASIC (Application-Specific Integrated Circuit) Google Pembelajaran Mesin." },
  { "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Pemrosesan Data Dekat Sumber Data." },
  { "en": "Apa Itu AI (Artificial Intelligence) Tepi?", "id": "Menjalankan Model AI (Artificial Intelligence) Perangkat Tepi Lokal." },
  { "en": "Apa Itu TinyML (Tiny Machine Learning)?", "id": "Pembelajaran Mesin Perangkat Mikrokontroler Daya Rendah." },
  { "en": "Apa Itu ASIC (Application-Specific Integrated Circuit) Kripto?", "id": "ASIC (Application-Specific Integrated Circuit) Khusus Penambangan Mata Uang Kripto." },
  { "en": "Apa Itu Atenuasi Sinyal?", "id": "Pelemahan Kekuatan Sinyal." },
  { "en": "Apa Itu Dispersi Sinyal?", "id": "Penyebaran Pulsa Sinyal Seiring Waktu." },
  { "en": "Apa Itu Konstanta Dielektrik (Er)?", "id": "Kemampuan Bahan Simpan Energi Listrik." },
  { "en": "Mengapa Konstanta Dielektrik Rendah Baik?", "id": "Mengurangi Kapasitansi Tunda Sinyal." },
  { "en": "Apa Itu Rugi Tangen Dielektrik?", "id": "Ukuran Energi Hilang Dielektrik (Panas)." },
  { "en": "Apa Itu Efek Kulit?", "id": "Arus AC Mengalir Permukaan Konduktor." },
  { "en": "Kapan Efek Kulit Signifikan?", "id": "Pada Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Pita Lebar Sinyal?", "id": "Rentang Frekuensi Sinyal Digital." },
  { "en": "Apa Itu ISI (Inter-Symbol Interference)?", "id": "Interferensi Antar Simbol (Bit) Berurutan." },
  { "en": "Penyebab ISI (Inter-Symbol Interference)?", "id": "Dispersi Sinyal Saluran Terbatas." },
  { "en": "Apa Itu Pre-Emphasis Sinyal?", "id": "Meningkatkan Komponen Frekuensi Tinggi Sinyal." },
  { "en": "Apa Itu De-Emphasis Sinyal?", "id": "Meredam Komponen Frekuensi Tinggi Penerima." },
  { "en": "Apa Itu Ekualisasi Sinyal?", "id": "Kompensasi Distorsi Saluran Transmisi." },
  { "en": "Apa Itu FFE (Feed-Forward Equalization)?", "id": "Ekualisasi Umpan Maju (Filter Transmit)." },
  { "en": "Apa Itu DFE (Decision Feedback Equalization)?", "id": "Ekualisasi Umpan Balik Keputusan (Penerima)." },
  { "en": "Apa Itu SerDes (Serializer/Deserializer)?", "id": "IC Ubah Data Paralel Serial." },
  { "en": "Fungsi Serializer?", "id": "Mengubah Data Paralel Lebar Serial Cepat." },
  { "en": "Fungsi Deserializer?", "id": "Mengubah Data Serial Cepat Paralel Lebar." },
  { "en": "Mengapa Pakai SerDes (Serializer/Deserializer)?", "id": "Mengurangi Jumlah Pin Kabel Kecepatan Tinggi." },
  { "en": "Apa Itu Pemulihan Jam (CDR)?", "id": "Ekstraksi Sinyal Jam Dari Data Serial." },
  { "en": "Apa Itu NRZ (Non-Return-to-Zero)?", "id": "Skema Pengkodean Sinyal Serial Sederhana." },
  { "en": "Apa Itu PAM4 (Pulse Amplitude Modulation 4-level)?", "id": "Skema Pengkodean Empat Level Tegangan." },
  { "en": "Kelebihan PAM4 (Pulse Amplitude Modulation 4-level)?", "id": "Dua Bit Per Simbol (Lebar Pita Efisien)." },
  { "en": "Apa Itu Kode 8b/10b?", "id": "Pengkodean Jalur Ubah 8 Bit 10 Bit." },
  { "en": "Tujuan Kode 8b/10b?", "id": "Pastikan Transisi Sinyal (CDR) DC Balance." },
  { "en": "Apa Itu DC (Direct Current) Balance?", "id": "Jumlah Bit Nol Satu Seimbang." },
  { "en": "Apa Itu Kode 64b/66b?", "id": "Pengkodean Jalur Efisien (Contoh: 10G Ethernet)." },
  { "en": "Apa Itu Scrambling Data?", "id": "Mengacak Pola Data Cegah Pola Berulang." },
  { "en": "Apa Itu PRBS (Pseudo-Random Binary Sequence)?", "id": "Urutan Biner Acak Semu Uji Sinyal." },
  { "en": "Apa Itu BERT (Bit Error Rate Test)?", "id": "Pengujian Hitung Tingkat Kesalahan Bit Komunikasi." },
  { "en": "Apa Itu Bathtub Curve Jitter?", "id": "Grafik BERT Versus Waktu Sampling." },
  { "en": "Apa Itu Optical Transceiver?", "id": "Modul Ubah Sinyal Listrik Optik." },
  { "en": "Apa Itu SFP (Small Form-factor Pluggable)?", "id": "Modul Transceiver Optik Hot-Pluggable Kecil." },
  { "en": "Apa Itu QSFP (Quad Small Form-factor Pluggable)?", "id": "Modul SFP Empat Saluran (Quad)." },
  { "en": "Apa Itu TOSA (Transmitter Optical Sub-Assembly)?", "id": "Sub-Rakitan Optik Pemancar (Laser)." },
  { "en": "Apa Itu ROSA (Receiver Optical Sub-Assembly)?", "id": "Sub-Rakitan Optik Penerima (Fotodioda)." },
  { "en": "Apa Itu TIA (Transimpedance Amplifier)?", "id": "Penguat Ubah Arus Foto Tegangan." },
  { "en": "Apa Itu Penguat Pembatas?", "id": "Penguat Hasilkan Sinyal Digital Amplitudo Tetap." },
  { "en": "Apa Itu Backplane?", "id": "Papan Sirkuit Penghubung Banyak Papan (Kartu)." },
  { "en": "Apa Itu Midplane?", "id": "Backplane Konektor Dua Sisi." },
  { "en": "Apa Itu Mezzanine Connector?", "id": "Konektor Hubungkan Dua PCB Paralel." },
  { "en": "Apa Itu Kabel DAC (Direct Attach Copper)?", "id": "Kabel Tembaga Kecepatan Tinggi Transceiver Terpasang." },
  { "en": "Apa Itu Kabel AOC (Active Optical Cable)?", "id": "Kabel Optik Aktif Transceiver Terpasang." },
  { "en": "Apa Itu PoE (Power over Ethernet)?", "id": "Daya Listrik Lewat Kabel Ethernet." },
  { "en": "Apa IC Kontroler PoE (Power over Ethernet) PSE (Power Sourcing Equipment)?", "id": "IC Sisi Sumber Daya (Contoh: Switch)." },
  { "en": "Apa IC Kontroler PoE (Power over Ethernet) PD (Powered Device)?", "id": "IC Sisi Perangkat Terima Daya (Contoh: Kamera)." },
  { "en": "Apa Deteksi Tanda Tangan PoE (Power over Ethernet)?", "id": "Proses PSE Deteksi Perangkat PD." },
  { "en": "Apa Itu Klasifikasi PoE (Power over Ethernet)?", "id": "Proses PSE Tentukan Kebutuhan Daya PD." },
  { "en": "Apa Standar PoE (Power over Ethernet) IEEE 802.3af?", "id": "Standar PoE Awal (Hingga 15.4W)." },
  { "en": "Apa Standar PoE (Power over Ethernet) IEEE 802.3at?", "id": "Standar PoE Plus (PoE+) (Hingga 30W)." },
  { "en": "Apa Standar PoE (Power over Ethernet) IEEE 802.3bt?", "id": "Standar PoE (PoE++) (Hingga 60W 100W)." },
  { "en": "Apa Itu Ethernet Otomotif?", "id": "Standar Ethernet Jaringan Kendaraan." },
  { "en": "Apa Itu BroadR-Reach?", "id": "Standar Ethernet Otomotif Satu Pasang Kabel." },
  { "en": "Apa Itu IC PHY (Physical Layer) Ethernet?", "id": "IC Antarmuka Lapisan Fisik Ethernet." },
  { "en": "Apa Itu Antarmuka MII (Media Independent Interface)?", "id": "Antarmuka Standar Antara MAC PHY." },
  { "en": "Apa Itu Antarmuka RGMII (Reduced Gigabit MII)?", "id": "Antarmuka MII Pin Dikurangi Gigabit." },
  { "en": "Apa Itu Antarmuka SGMII (Serial Gigabit MII)?", "id": "Antarmuka MII Serial Gigabit." },
  { "en": "Apa Itu Sinkronisasi Waktu Ethernet?", "id": "Sinkronisasi Jam Presisi Jaringan Ethernet." },
  { "en": "Apa Itu PTP (Precision Time Protocol)?", "id": "Protokol Waktu Presisi (IEEE 1588)." },
  { "en": "Apa Itu SyncE (Synchronous Ethernet)?", "id": "Sinkronisasi Jam Lapisan Fisik Ethernet." },
  { "en": "Apa Itu IC Audio Codec?", "id": "IC Gabungan ADC DAC Audio." },
  { "en": "Apa Itu IC Penguat Audio Kelas D?", "id": "Penguat Audio Switching Efisiensi Tinggi." },
  { "en": "Apa Itu IC DSP (Digital Signal Processor) Audio?", "id": "IC Proses Sinyal Digital Khusus Audio." },
  { "en": "Apa Itu IC Mikrofon MEMS (Micro-Electro-Mechanical Systems)?", "id": "Mikrofon Miniatur Dibuat Chip Silikon." },
  { "en": "Apa Itu PDM (Pulse Density Modulation)?", "id": "Modulasi Kepadatan Pulsa (Output Mikrofon MEMS)." },
  { "en": "Apa Itu ASRC (Asynchronous Sample Rate Converter)?", "id": "Pengubah Laju Sampel Asinkron." },
  { "en": "Apa Itu Filter Dekimasi Digital?", "id": "Filter Turunkan Laju Sampel (Contoh: PDM ke PCM)." },
  { "en": "Apa Itu PCM (Pulse Code Modulation)?", "id": "Representasi Digital Sinyal Audio Analog Standar." },
  { "en": "Apa Itu Sistem Pembangkit Jam IC?", "id": "Rangkaian Hasilkan Distribusikan Sinyal Jam." },
  { "en": "Apa Itu PLL (Phase-Locked Loop) Integer-N?", "id": "PLL Faktor Pembagi Bilangan Bulat." },
  { "en": "Apa Itu PLL (Phase-Locked Loop) Fractional-N?", "id": "PLL Faktor Pembagi Bilangan Pecahan." },
  { "en": "Apa Itu Jitter RMS (Root Mean Square)?", "id": "Ukuran Total Energi Jitter Acak." },
  { "en": "Apa Itu Jitter Puncak-ke-Puncak?", "id": "Perbedaan Maksimal Tepi Jam Tercepat Terlambat." },
  { "en": "Apa Itu Kebisingan Fasa Osilator?", "id": "Kebisingan Domain Frekuensi (Ketidakstabilan Osilator)." },
  { "en": "Apa Itu Jitter Siklus-ke-Siklus?", "id": "Variasi Waktu Antara Dua Siklus Jam." },
  { "en": "Apa Itu XO (Crystal Oscillator)?", "id": "Osilator Kristal Sederhana." },
  { "en": "Apa Itu TCXO (Temperature-Compensated Crystal Oscillator)?", "id": "Osilator Kristal Kompensasi Suhu." },
  { "en": "Apa Itu VCXO (Voltage-Controlled Crystal Oscillator)?", "id": "Osilator Kristal Frekuensi Diatur Tegangan." },
  { "en": "Apa Itu OCXO (Oven-Controlled Crystal Oscillator)?", "id": "Osilator Kristal Kontrol Oven (Sangat Stabil)." },
  { "en": "Apa Itu Osilator MEMS (Micro-Electro-Mechanical Systems)?", "id": "Resonator Silikon MEMS Gantikan Kristal." },
  { "en": "Apa Itu Pembersih Jitter?", "id": "IC PLL Redam Jitter Sinyal Jam." },
  { "en": "Apa Itu Fanout Buffer Jam?", "id": "IC Distribusi Satu Sinyal Jam Banyak Beban." },
  { "en": "Apa Itu Penundaan Pin-ke-Pin Nol?", "id": "Fitur Buffer Jam Sinkronkan Output Input." },
  { "en": "Apa Itu Sensor Arus Efek Hall?", "id": "Sensor Arus Non-Kontak Medan Magnet." },
  { "en": "Apa Itu Resistor Shunt Arus?", "id": "Resistor Presisi Rendah Ukur Arus (Penurunan Tegangan)." },
  { "en": "Apa Itu Penguat Indera Arus?", "id": "Penguat Ukur Penurunan Tegangan Kecil Shunt." },
  { "en": "Apa Itu Penginderaan Sisi Tinggi?", "id": "Ukur Arus Antara Catu Daya Positif Beban." },
  { "en": "Apa Itu Penginderaan Sisi Rendah?", "id": "Ukur Arus Antara Beban Ground." },
  { "en": "Apa Itu Isolasi Penginderaan Arus?", "id": "Pengukuran Arus Aman Tegangan Common-Mode Tinggi." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Isolasi?", "id": "ADC Barrier Isolasi Internal (Contoh: Shunt)." },
  { "en": "Apa Itu IC Pengukur Energi?", "id": "IC Ukur Konsumsi Daya Energi Listrik." },
  { "en": "Apa Itu CT (Current Transformer)?", "id": "Trafo Arus Sensor Arus AC Non-Kontak." },
  { "en": "Apa Itu Koil Rogowski?", "id": "Sensor Arus AC Fleksibel Non-Kontak." },
  { "en": "Apa Itu Efek Seebeck?", "id": "Prinsip Fisik Dibalik Termokopel (TEG)." },
  { "en": "Apa Itu Efek Peltier?", "id": "Prinsip Fisik Pendingin Termoelektrik (TEC)." },
  { "en": "Apa Itu TEC (Thermoelectric Cooler)?", "id": "Perangkat Semikonduktor Pompa Panas (Pendingin/Pemanas)." },
  { "en": "Apa Itu IC Kontroler TEC (Thermoelectric Cooler)?", "id": "IC Driver Jembatan-H Kontrol Suhu TEC." },
  { "en": "Apa Itu Sel Beban Jembatan?", "id": "Sensor Berat Gunakan Konfigurasi Jembatan Wheatstone." },
  { "en": "Apa Itu IC Front-End Sel Beban?", "id": "IC PGA ADC Presisi Sel Beban." },
  { "en": "Apa Itu Rangkaian RC (Resistor-Capacitor)?", "id": "Rangkaian Resistor Dan Kapasitor." },
  { "en": "Apa Itu Konstanta Waktu RC?", "id": "Waktu Pengisian Kapasitor (63.2%)." },
  { "en": "Apa Itu Rangkaian RL (Resistor-Inductor)?", "id": "Rangkaian Resistor Dan Induktor." },
  { "en": "Apa Itu Rangkaian RLC (Resistor-Inductor-Capacitor)?", "id": "Rangkaian Resistor, Induktor, Kapasitor." },
  { "en": "Apa Itu Resonansi Rangkaian RLC?", "id": "Frekuensi Reaktansi Induktif Kapasitif Sama." },
  { "en": "Apa Itu Faktor Q Rangkaian RLC?", "id": "Ukuran Kualitas Faktor Resonansi." },
  { "en": "Apa Itu Rangkaian Tangki LC?", "id": "Rangkaian Resonansi Induktor Kapasitor Paralel." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Alat Matematika Analisis Rangkaian Domain-S." },
  { "en": "Apa Itu Domain-S?", "id": "Domain Frekuensi Kompleks (Analisis Rangkaian)." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Terhadap Input Domain-S." },
  { "en": "Apa Itu Sistem LTI (Linear Time-Invariant)?", "id": "Sistem Linear Tidak Berubah Waktu." },
  { "en": "Apa Itu Konvolusi Sinyal?", "id": "Operasi Matematika Respon Sistem LTI." },
  { "en": "Apa Itu Respon Impuls?", "id": "Output Sistem LTI Terhadap Input Impuls." },
  { "en": "Apa Itu Respon Langkah?", "id": "Output Sistem LTI Terhadap Input Langkah." },
  { "en": "Apa Itu Sistem Orde Pertama?", "id": "Sistem Deskripsi Satu Persamaan Diferensial." },
  { "en": "Apa Itu Sistem Orde Kedua?", "id": "Sistem Deskripsi Dua Persamaan Diferensial." },
  { "en": "Apa Itu Damping Sistem?", "id": "Ukuran Redaman Osilasi Respon Sistem." },
  { "en": "Apa Itu Underdamped?", "id": "Respon Sistem Berosilasi Menuju Stabil." },
  { "en": "Apa Itu Overdamped?", "id": "Respon Sistem Stabil Lambat Tanpa Osilasi." },
  { "en": "Apa Itu Critically Damped?", "id": "Respon Sistem Stabil Cepat Tanpa Osilasi." },
  { "en": "Apa Itu Rangkaian Clamper?", "id": "Rangkaian Geser Level DC Sinyal AC." },
  { "en": "Apa Itu Rangkaian Clipper?", "id": "Rangkaian Potong Bagian Sinyal Amplitudo." },
  { "en": "Apa Itu Pengganda Tegangan?", "id": "Rangkaian Dioda Kapasitor Lipatgandakan Tegangan DC." },
  { "en": "Apa Itu Detektor Puncak?", "id": "Rangkaian Penyimpan Nilai Puncak Tegangan Sinyal." },
  { "en": "Apa Itu Detektor Selubung?", "id": "Rangkaian Ekstraksi Selubung Sinyal Modulasi AM." },
  { "en": "Apa Itu Kontrol Volume Digital?", "id": "Pengaturan Level Sinyal Audio Digital." },
  { "en": "Apa Itu Potensiometer Digital?", "id": "IC Resistor Variabel Kontrol Digital." },
  { "en": "Apa Itu Saklar Analog?", "id": "IC Saklar Elektronik Sinyal Analog." },
  { "en": "Apa Itu Multiplexer Analog?", "id": "Saklar Analog Pilih Satu Sinyal Banyak Input." },
  { "en": "Apa Itu Demultiplexer Analog?", "id": "Saklar Analog Salurkan Satu Input Banyak Output." },
  { "en": "Apa Itu Crosstalk Saklar Analog?", "id": "Kebocoran Sinyal Antar Saluran Saklar." },
  { "en": "Apa Itu Isolasi Saklar Analog?", "id": "Ukuran Redaman Sinyal Saklar Terbuka." },
  { "en": "Apa Itu Injeksi Muatan Saklar?", "id": "Error Muatan Terinjeksi Sinyal Saat Saklar Beralih." },
  { "en": "Apa Itu Rangkaian Sample-and-Hold (S/H)?", "id": "Rangkaian Ambil Sampel Tahan Tegangan Analog." },
  { "en": "Apa Itu Waktu Akuisisi S/H?", "id": "Waktu Diperlukan Sampel Tegangan Akurat." },
  { "en": "Apa Itu Waktu Aperture S/H?", "id": "Waktu Transisi Mode Sampel Ke Tahan." },
  { "en": "Apa Itu Droop Rate S/H?", "id": "Laju Penurunan Tegangan Mode Tahan." },
  { "en": "Apa Itu V/F Converter (Pengubah V/F)?", "id": "Rangkaian Ubah Tegangan Analog Frekuensi." },
  { "en": "Apa Itu F/V Converter (Pengubah F/V)?", "id": "Rangkaian Ubah Frekuensi Tegangan Analog." },
  { "en": "Apa Itu Penguat Logaritmik?", "id": "Penguat Output Proporsional Logaritma Input." },
  { "en": "Apa Itu Penguat Antilogaritmik?", "id": "Penguat Output Proporsional Eksponensial Input." },
  { "en": "Apa Itu Pengganda Analog?", "id": "Rangkaian Hasilkan Output Perkalian Sinyal Analog." },
  { "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Modulasi Variasikan Amplitudo Sinyal Pembawa." },
  { "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Modulasi Variasikan Frekuensi Sinyal Pembawa." },
  { "en": "Apa Itu Modulasi Fasa (PM)?", "id": "Modulasi Variasikan Fasa Sinyal Pembawa." },
  { "en": "Apa Itu Demodulasi (Deteksi)?", "id": "Proses Ekstraksi Sinyal Informasi Pembawa." },
  { "en": "Apa Itu Superheterodyne Receiver?", "id": "Arsitektur Penerima Radio Turunkan Frekuensi IF." },
  { "en": "Apa Itu Frekuensi Gambar?", "id": "Frekuensi Palsu Dihasilkan Mixer Superheterodyne." },
  { "en": "Apa Itu Filter Penolak Gambar?", "id": "Filter RF Hapus Frekuensi Gambar." },
  { "en": "Apa Itu Direct Conversion Receiver?", "id": "Arsitektur Penerima Langsung Konversi RF Baseband." },
  { "en": "Apa Itu Masalah Offset DC Penerima?", "id": "Offset DC Besar Penerima Konversi Langsung." },
  { "en": "Apa Itu Ketidakseimbangan I/Q?", "id": "Ketidakcocokan Jalur I (In-phase) Q (Quadrature)." },
  { "en": "Apa Itu Rangkaian Catu Daya?", "id": "Sirkuit Penyedia Tegangan Arus Listrik." },
  { "en": "Apa Itu Efisiensi Catu Daya?", "id": "Rasio Daya Output Terhadap Daya Input." },
  { "en": "Apa Itu Arus Diam (Quiescent Current)?", "id": "Arus Konsumsi Rangkaian Mode Siaga." },
  { "en": "Apa Itu Arus Shutdown?", "id": "Arus Konsumsi Rangkaian Mode Mati." },
  { "en": "Apa Itu Mode Hemat Daya?", "id": "Mode Operasi Konsumsi Daya Rendah." },
  { "en": "Apa Itu Cacat Kristal?", "id": "Ketidaksempurnaan Struktur Kristal Semikonduktor." },
  { "en": "Apa Itu Dislokasi Kristal?", "id": "Cacat Garis Struktur Kristal." },
  { "en": "Apa Itu Cacat Titik?", "id": "Cacat Atom Tunggal (Contoh: Kekosongan)." },
  { "en": "Apa Itu Kontaminasi Partikulat?", "id": "Partikel Debu Asing Rusak Pola IC." },
  { "en": "Apa Itu Kontaminasi Ionik?", "id": "Ion (Contoh: Sodium) Ubah Karakteristik Transistor." },
  { "en": "Apa Itu Model Yield Poisson?", "id": "Model Statistik Prediksi Yield Chip." },
  { "en": "Apa Itu Area Kritis?", "id": "Area Chip Rentan Cacat Partikel." },
  { "en": "Apa Itu D0 (D-Zero)?", "id": "Kepadatan Cacat Per Satuan Luas." },
  { "en": "Apa Itu Struktur Tes Kelvin?", "id": "Struktur Uji Ukur Resistansi Kontak Akurat." },
  { "en": "Apa Itu Struktur Tes Cross-Bridge?", "id": "Struktur Uji Ukur Resistansi Lembar Lebar Jalur." },
  { "en": "Apa Itu Pengujian Parametrik Wafer?", "id": "Pengujian Karakteristik Listrik Struktur Tes." },
  { "en": "Apa Itu Backgrinding Wafer?", "id": "Proses Penipisan Wafer Dari Belakang." },
  { "en": "Mengapa Wafer Ditipiskan?", "id": "Kemasan Tipis Tumpukan Tiga Dimensi." },
  { "en": "Apa Itu Stres Wafer?", "id": "Stres Mekanis Wafer Akibat Proses." },
  { "en": "Apa Itu Pembengkokan Wafer?", "id": "Kelengkungan Wafer Akibat Stres." },
  { "en": "Apa Itu BSM (Backside Metal)?", "id": "Lapisan Logam Sisi Belakang Wafer." },
  { "en": "Apa Itu Penanganan Wafer Tipis?", "id": "Tantangan Penanganan Wafer Tipis Rapuh." },
  { "en": "Apa Itu Pemasangan Wafer Pita?", "id": "Pemasangan Wafer Pita Perekat Pemotongan." },
  { "en": "Apa Itu Pelepasan Pita?", "id": "Proses Pelepasan Die Potong Pita Perekat." },
  { "en": "Apa Itu IC (Integrated Circuit) Tiga Dimensi (3D)?", "id": "Integrasi Vertikal Beberapa Die Chip." },
  { "en": "Apa Itu TSV (Through-Silicon Via)?", "id": "Koneksi Listrik Vertikal Tembus Silikon." },
  { "en": "Apa Itu Ikatan Wafer-ke-Wafer?", "id": "Menyatukan Dua Wafer Utuh Vertikal." },
  { "en": "Apa Itu Ikatan Chip-ke-Wafer?", "id": "Menyatukan Die Individual Ke Wafer Utuh." },
  { "en": "Apa Itu Ikatan Hibrida?", "id": "Ikatan Langsung Tembaga-Tembaga Dielektrik-Dielektrik." },
  { "en": "Apa Itu Interposer 2.5D (Dua Koma Lima Dimensi)?", "id": "Substrat Silikon Koneksi Antar Chip Horizontal." },
  { "en": "Kelebihan Interposer 2.5D (Dua Koma Lima Dimensi)?", "id": "Integrasi Chiplet Kinerja Sangat Tinggi." },
  { "en": "Apa Itu HBM (High Bandwidth Memory)?", "id": "Memori Tumpuk Tiga Dimensi Lebar Pita Tinggi." },
  { "en": "Bagaimana HBM (High Bandwidth Memory) Terhubung?", "id": "Melalui Interposer Silikon TSV." },
  { "en": "Apa Itu Fan-Out Level Wafer?", "id": "Teknologi Kemasan Interkoneksi Luar Area Die." },
  { "en": "Apa Itu RDL (Redistribution Layer)?", "id": "Lapisan Perutean Logam Tipis Kemasan Fan-Out." },
  { "en": "Kelebihan Fan-Out?", "id": "Kemasan Sangat Tipis Jumlah Pin Tinggi." },
  { "en": "Apa Itu Pilar Tembaga?", "id": "Benjolan Solder Tembaga Tinggi Koneksi Flip-Chip." },
  { "en": "Apa Itu Benjolan Solder?", "id": "Bola Solder Kecil Koneksi Flip-Chip." },
  { "en": "Apa Itu Retak Die?", "id": "Kegagalan Mekanis Die Silikon Retak." },
  { "en": "Apa Itu Void Enkapsulasi?", "id": "Gelembung Udara Terperangkap Bahan Kemasan Plastik." },
  { "en": "Apa Itu Void Solder?", "id": "Gelembung Udara Terperangkap Sambungan Solder." },
  { "en": "Mengapa Void Solder Buruk?", "id": "Kurangi Kekuatan Mekanis Termal." },
  { "en": "Apa Itu Jembatan Solder?", "id": "Hubung Singkat Solder Antara Dua Pad Berdekatan." },
  { "en": "Apa Itu Batu Nisan Komponen?", "id": "Komponen SMD Berdiri Vertikal Saat Solder." },
  { "en": "Apa Itu Desain Pad Asimetris?", "id": "Penyebab Umum Batu Nisan (Tegangan Permukaan)." },
  { "en": "Apa Itu Pembersihan Papan PCB?", "id": "Menghilangkan Residu Fluks Sisa Solder." },
  { "en": "Apa Itu Residu Fluks No-Clean?", "id": "Residu Fluks Aman Ditinggalkan Papan." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
