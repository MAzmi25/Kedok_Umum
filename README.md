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


  { "en": "Apa Fungsi Utama Jantung?", "id": "Memompa Darah Ke Seluruh Tubuh." },
  { "en": "Dimana Letak Organ Paru-Paru?", "id": "Di Dalam Rongga Dada Manusia." },
  { "en": "Apa Nama Lain Sel Darah Merah?", "id": "Eritrosit, Pembawa Oksigen Dalam Darah." },
  { "en": "Apa Itu Tekanan Darah Tinggi (Hipertensi)?", "id": "Kondisi Tekanan Darah Di Atas Normal." },
  { "en": "Berapa Jumlah Tulang Pada Orang Dewasa?", "id": "Sekitar Dua Ratus Enam Tulang." },
  { "en": "Apa Fungsi Utama Organ Ginjal?", "id": "Menyaring Limbah Dari Darah Manusia." },
  { "en": "Apa Yang Dimaksud Dengan Denyut Nadi?", "id": "Ukuran Detak Jantung Per Menit." },
  { "en": "Siapa Penemu Penisilin Yang Terkenal?", "id": "Alexander Fleming, Seorang Ilmuwan Skotlandia." },
  { "en": "Apa Itu Penyakit Diabetes Melitus (DM)?", "id": "Penyakit Kadar Gula Darah Tinggi." },
  { "en": "Apa Fungsi Utama Dari Organ Hati?", "id": "Memetabolisme Racun Dalam Tubuh Manusia." },
  { "en": "Apa Nama Tulang Terpanjang Di Tubuh?", "id": "Tulang Paha Atau Biasa Disebut Femur." },
  { "en": "Apa Itu Vaksinasi Atau Imunisasi?", "id": "Proses Peningkatan Kekebalan Tubuh Seseorang." },
  { "en": "Sebutkan Gejala Umum Penyakit Demam Berdarah?", "id": "Demam Tinggi, Nyeri Otot, Ruam." },
  { "en": "Apa Yang Dimaksud Dengan Sistem Saraf?", "id": "Jaringan Komunikasi Tubuh, Mengatur Gerakan." },
  { "en": "Apa Fungsi Dari Sel Darah Putih?", "id": "Melawan Infeksi Dan Penyakit Masuk." },
  { "en": "Apa Itu Antibiotik Dalam Dunia Medis?", "id": "Obat Untuk Melawan Infeksi Bakteri." },
  { "en": "Apa Penyebab Utama Penyakit TBC (Tuberkulosis)?", "id": "Bakteri Mycobacterium Tuberculosis Menyerang Paru." },
  { "en": "Apa Fungsi Kulit Bagi Tubuh Manusia?", "id": "Melindungi Organ Internal, Mengatur Suhu." },
  { "en": "Apa Itu Anemia Dalam Istilah Medis?", "id": "Kekurangan Sel Darah Merah Sehat." },
  { "en": "Sebutkan Lima Panca Indera Manusia?", "id": "Penglihatan, Pendengaran, Penciuman, Perasa, Peraba." },
  { "en": "Apa Fungsi Otak Besar Atau Cerebrum?", "id": "Mengontrol Pikiran, Gerakan, Dan Indra." },
  { "en": "Apa Itu Penyakit Asma Yang Umum?", "id": "Peradangan Kronis Pada Saluran Pernapasan." },
  { "en": "Apa Fungsi Dari Sumsum Tulang Belakang?", "id": "Menghubungkan Otak Dengan Saraf Tubuh." },
  { "en": "Apa Yang Dimaksud Dengan DNA (Deoxyribonucleic Acid)?", "id": "Materi Genetik Pembawa Sifat Keturunan." },
  { "en": "Apa Itu Golongan Darah Sistem ABO?", "id": "Klasifikasi Darah: A, B, AB, O." },
  { "en": "Apa Fungsi Lambung Dalam Sistem Pencernaan?", "id": "Mencerna Makanan Dengan Asam Lambung." },
  { "en": "Apa Itu Patah Tulang Atau Fraktur?", "id": "Kondisi Terputusnya Kontinuitas Jaringan Tulang." },
  { "en": "Apa Yang Dimaksud Dengan Metabolisme Tubuh?", "id": "Proses Kimia Untuk Mengubah Makanan." },
  { "en": "Sebutkan Fungsi Utama Dari Usus Halus?", "id": "Menyerap Sebagian Besar Nutrisi Makanan." },
  { "en": "Apa Itu Penyakit Kanker Secara Umum?", "id": "Pertumbuhan Sel Tidak Normal, Tidak Terkendali." },
  { "en": "Apa Fungsi Dari Kelenjar Tiroid Manusia?", "id": "Menghasilkan Hormon Pengatur Metabolisme Tubuh." },
  { "en": "Apa Itu Virus Dalam Konteks Medis?", "id": "Agen Infeksius Berukuran Sangat Kecil." },
  { "en": "Apa Yang Dimaksud Dengan Kolesterol Jahat?", "id": "Low-Density Lipoprotein Atau Biasa Disebut LDL." },
  { "en": "Apa Fungsi Dari Sistem Limfatik Tubuh?", "id": "Bagian Penting Dari Sistem Kekebalan." },
  { "en": "Apa Itu Penyakit Jantung Koroner (PJK)?", "id": "Penyempitan Pembuluh Darah Arteri Koroner." },
  { "en": "Apa Fungsi Dari Otot Rangka Manusia?", "id": "Memungkinkan Pergerakan Tubuh Secara Aktif." },
  { "en": "Apa Itu Demam Dalam Istilah Medis?", "id": "Peningkatan Suhu Tubuh Di Atas Normal." },
  { "en": "Apa Fungsi Utama Dari Organ Pankreas?", "id": "Menghasilkan Enzim Pencernaan Dan Insulin." },
  { "en": "Apa Yang Dimaksud Dengan Dehidrasi Tubuh?", "id": "Kondisi Kehilangan Banyak Cairan Tubuh." },
  { "en": "Apa Itu Pemeriksaan Rontgen Atau Sinar-X?", "id": "Tes Pencitraan Melihat Bagian Dalam." },
  { "en": "Apa Fungsi Dari Alveolus Di Paru-Paru?", "id": "Tempat Pertukaran Oksigen Dan Karbon Dioksida." },
  { "en": "Apa Itu Penyakit Stroke Yang Berbahaya?", "id": "Gangguan Pasokan Darah Menuju Ke Otak." },
  { "en": "Apa Yang Dimaksud Dengan Tekanan Darah?", "id": "Kekuatan Darah Mendorong Dinding Arteri." },
  { "en": "Sebutkan Tiga Jenis Pembuluh Darah Utama?", "id": "Arteri, Vena, Dan Juga Kapiler." },
  { "en": "Apa Fungsi Dari Keping Darah Trombosit?", "id": "Membantu Dalam Proses Pembekuan Darah." },
  { "en": "Apa Itu Penyakit Maag Atau Gastritis?", "id": "Peradangan Pada Lapisan Dinding Lambung." },
  { "en": "Apa Fungsi Dari Lensa Mata Manusia?", "id": "Memfokuskan Cahaya Ke Bagian Retina." },
  { "en": "Apa Yang Dimaksud Dengan Sistem Endokrin?", "id": "Sistem Kontrol Kelenjar Penghasil Hormon." },
  { "en": "Apa Itu Alergi Dalam Dunia Medis?", "id": "Reaksi Sistem Kekebalan Terhadap Zat Asing." },
  { "en": "Apa Fungsi Utama Dari Usus Besar?", "id": "Menyerap Air Dan Membentuk Feses." },
  { "en": "Apa Itu Penyakit Cacar Air (Varicella)?", "id": "Infeksi Virus Menyebabkan Ruam Gatal." },
  { "en": "Apa Fungsi Dari Gendang Telinga Manusia?", "id": "Mengirim Getaran Suara Ke Dalam." },
  { "en": "Apa Yang Dimaksud Dengan Hormon Insulin?", "id": "Hormon Pengatur Kadar Gula Darah." },
  { "en": "Apa Itu Penyakit Radang Sendi (Artritis)?", "id": "Peradangan Pada Satu Atau Beberapa Sendi." },
  { "en": "Apa Fungsi Dari Empedu Di Hati?", "id": "Membantu Pencernaan Dan Penyerapan Lemak." },
  { "en": "Apa Itu Biopsi Dalam Prosedur Medis?", "id": "Pengambilan Sampel Jaringan Untuk Diperiksa." },
  { "en": "Apa Fungsi Kerongkongan Atau Esofagus?", "id": "Menyalurkan Makanan Dari Mulut Ke Lambung." },
  { "en": "Apa Itu Penyakit Malaria Yang Berbahaya?", "id": "Disebabkan Oleh Parasit Plasmodium Falciparum." },
  { "en": "Apa Yang Dimaksud Dengan Sistem Imun?", "id": "Sistem Pertahanan Tubuh Melawan Penyakit." },
  { "en": "Apa Fungsi Dari Otak Kecil Cerebellum?", "id": "Mengontrol Keseimbangan Dan Koordinasi Gerak." },
  { "en": "Apa Itu Penyakit Tifus Atau Tifoid?", "id": "Infeksi Bakteri Salmonella Typhi Menular." },
  { "en": "Apa Fungsi Dari Kornea Mata Manusia?", "id": "Bagian Terluar Mata, Membiaskan Cahaya." },
  { "en": "Apa Yang Dimaksud Dengan Proses Ovulasi?", "id": "Pelepasan Sel Telur Dari Ovarium." },
  { "en": "Apa Itu Penyakit Influenza Atau Flu?", "id": "Infeksi Virus Menyerang Sistem Pernapasan." },
  { "en": "Apa Fungsi Dari Katup Jantung Manusia?", "id": "Menjaga Aliran Darah Satu Arah." },
  { "en": "Apa Itu USG (Ultrasonografi) Dalam Medis?", "id": "Pencitraan Menggunakan Gelombang Suara Tinggi." },
  { "en": "Apa Fungsi Dari Lidah Sebagai Indra?", "id": "Merasakan Manis, Asin, Pahit, Asam." },
  { "en": "Apa Itu Penyakit Osteoporosis Yang Umum?", "id": "Pengeroposan Tulang, Membuat Tulang Rapuh." },
  { "en": "Apa Yang Dimaksud Dengan Masa Inkubasi?", "id": "Waktu Antara Infeksi Dan Gejala." },
  { "en": "Apa Fungsi Dari Sistem Saraf Otonom?", "id": "Mengatur Fungsi Tubuh Tidak Sadar." },
  { "en": "Apa Itu Penyakit Hepatitis Secara Umum?", "id": "Peradangan Pada Organ Hati Manusia." },
  { "en": "Apa Fungsi Dari Pupil Mata Kita?", "id": "Mengatur Jumlah Cahaya Masuk Ke Mata." },
  { "en": "Apa Yang Dimaksud Dengan Denaturasi Protein?", "id": "Perubahan Struktur Protein Akibat Panas." },
  { "en": "Apa Itu Penyakit Gondok Yang Terjadi?", "id": "Pembengkakan Kelenjar Tiroid Di Leher." },
  { "en": "Apa Fungsi Neuron Atau Sel Saraf?", "id": "Mengirimkan Sinyal Listrik Dan Kimia." },
  { "en": "Apa Itu Elektrokardiogram Atau EKG (ECG)?", "id": "Tes Merekam Aktivitas Listrik Jantung." },
  { "en": "Apa Fungsi Dari Kelenjar Adrenal Manusia?", "id": "Menghasilkan Hormon Adrenalin Dan Kortisol." },
  { "en": "Apa Itu Penyakit Katarak Pada Mata?", "id": "Kekeruhan Pada Lensa Mata Manusia." },
  { "en": "Apa Yang Dimaksud Dengan Proses Fertilisasi?", "id": "Peleburan Sel Sperma Dan Sel Telur." },
  { "en": "Apa Fungsi Dari Sistem Perkemihan Manusia?", "id": "Menghasilkan, Menyimpan, Mengeluarkan Urin (Air Seni)." },
  { "en": "Apa Itu Penyakit Glaukoma Pada Mata?", "id": "Kerusakan Saraf Mata Akibat Tekanan." },
  { "en": "Apa Fungsi Utama Tulang Rawan (Kartilago)?", "id": "Jaringan Ikat Fleksibel, Pelindung Sendi." },
  { "en": "Apa Yang Dimaksud Dengan Zat Besi?", "id": "Mineral Penting Pembentuk Sel Darah." },
  { "en": "Apa Itu Penyakit Epilepsi Atau Ayan?", "id": "Gangguan Aktivitas Listrik Otak Berlebih." },
  { "en": "Apa Fungsi Dari Kelenjar Getah Bening?", "id": "Menyaring Cairan Limfe, Melawan Infeksi." },
  { "en": "Apa Itu Endoskopi Dalam Dunia Medis?", "id": "Prosedur Melihat Organ Dalam Tubuh." },
  { "en": "Apa Fungsi Dari Diafragma Pernapasan Manusia?", "id": "Otot Utama Dalam Proses Pernapasan." },
  { "en": "Apa Itu Penyakit Gagal Ginjal Kronis?", "id": "Kerusakan Ginjal Progresif Jangka Panjang." },
  { "en": "Apa Yang Dimaksud Dengan Detoksifikasi Hati?", "id": "Proses Hati Menetralkan Racun Berbahaya." },
  { "en": "Apa Fungsi Dari Cairan Sinovial Sendi?", "id": "Cairan Pelumas Pada Sendi Gerak." },
  { "en": "Apa Itu Penyakit Meningitis Atau Radang?", "id": "Peradangan Selaput Pelindung Otak Manusia." },
  { "en": "Apa Yang Dimaksud Dengan Vitamin C?", "id": "Antioksidan Penting, Meningkatkan Sistem Imun." },
  { "en": "Apa Fungsi Retina Di Dalam Mata?", "id": "Mendeteksi Cahaya, Mengirim Sinyal Visual." },
  { "en": "Apa Itu MRI (Magnetic Resonance Imaging)?", "id": "Pencitraan Medis Menggunakan Medan Magnet." },
  { "en": "Apa Fungsi Dari Rahim Atau Uterus?", "id": "Tempat Berkembangnya Janin Selama Kehamilan." },
  { "en": "Apa Itu Penyakit Asam Urat (Gout)?", "id": "Radang Sendi Akibat Tumpukan Kristal." },
  { "en": "Apa Yang Dimaksud Dengan Transfusi Darah?", "id": "Proses Transfer Darah Ke Orang." },
  { "en": "Apa Fungsi Alat Medis Bernama Stetoskop?", "id": "Mendengarkan Suara Jantung, Paru, Usus." },
  { "en": "Apa Yang Dimaksud Dengan Gen Manusia?", "id": "Unit Pewarisan Sifat Kepada Keturunan." },
  { "en": "Sebutkan Spesialisasi Dokter Tulang Manusia?", "id": "Dokter Spesialis Ortopedi Dan Traumatologi." },
  { "en": "Apa Itu Penyakit Autoimun Secara Umum?", "id": "Sistem Imun Menyerang Jaringan Tubuh." },
  { "en": "Apa Fungsi Dari Kelenjar Pituitari Otak?", "id": "Kelenjar Master, Mengatur Kelenjar Lain." },
  { "en": "Apa Yang Dimaksud Dengan Reaksi Anafilaksis?", "id": "Reaksi Alergi Berat, Mengancam Nyawa." },
  { "en": "Apa Fungsi Dari Kandung Kemih Manusia?", "id": "Menampung Urin Sebelum Dikeluarkan Tubuh." },
  { "en": "Apa Itu CT Scan (Computed Tomography Scan)?", "id": "Pencitraan Rontgen Lintas Bagian Tubuh." },
  { "en": "Apa Penyebab Penyakit Cacingan Pada Anak?", "id": "Infeksi Cacing Parasit Di Usus." },
  { "en": "Apa Fungsi Utama Dari Mitokondria Sel?", "id": "Pembangkit Tenaga, Menghasilkan Energi (ATP)." },
  { "en": "Apa Itu Prosedur Operasi Caesar Medis?", "id": "Metode Persalinan Melalui Pembedahan Perut." },
  { "en": "Apa Yang Dimaksud Dengan Tekanan Darah Rendah?", "id": "Hipotensi, Tekanan Darah Di Bawah Normal." },
  { "en": "Sebutkan Fungsi Utama Dari Tengkorak Manusia?", "id": "Melindungi Organ Otak Dari Cedera." },
  { "en": "Apa Itu Penyakit Demensia Secara Umum?", "id": "Penurunan Fungsi Kognitif, Daya Ingat." },
  { "en": "Apa Fungsi Dari Trakea Sistem Pernapasan?", "id": "Saluran Udara Menuju Ke Paru-Paru." },
  { "en": "Apa Yang Dimaksud Dengan Angka Kematian Bayi?", "id": "Jumlah Kematian Bayi Per Kelahiran." },
  { "en": "Apa Itu Pemeriksaan Darah Lengkap (CBC)?", "id": "Tes Mengukur Komponen Sel Darah." },
  { "en": "Apa Fungsi Dari Otot Jantung Manusia?", "id": "Memompa Darah Secara Terus-Menerus Otomatis." },
  { "en": "Apa Itu Penyakit Sifilis Atau Raja Singa?", "id": "Infeksi Menular Seksual Bakteri Treponema." },
  { "en": "Apa Yang Dimaksud Dengan Proses Pembekuan Darah?", "id": "Mekanisme Alami Menghentikan Pendarahan Hebat." },
  { "en": "Apa Fungsi Dari Saraf Optik Mata?", "id": "Mengirim Informasi Visual Dari Retina." },
  { "en": "Apa Itu Anestesi Dalam Tindakan Medis?", "id": "Obat Menghilangkan Rasa Sakit Sementara." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Anak?", "id": "Dokter Spesialis Anak Atau Pediatri." },
  { "en": "Apa Yang Dimaksud Dengan Kalori Makanan?", "id": "Satuan Energi Dalam Makanan Minuman." },
  { "en": "Apa Fungsi Dari Tulang Belakang Manusia?", "id": "Menopang Tubuh, Melindungi Sumsum Tulang." },
  { "en": "Apa Itu Penyakit Herpes Simpleks Virus (HSV)?", "id": "Infeksi Virus Menyebabkan Luka Melepuh." },
  { "en": "Apa Fungsi Dari Rambut Dan Kuku?", "id": "Melindungi Kulit, Memiliki Fungsi Sensorik." },
  { "en": "Apa Yang Dimaksud Dengan Koma Medis?", "id": "Keadaan Tidak Sadar Jangka Panjang." },
  { "en": "Sebutkan Bagian Utama Dari Sel Manusia?", "id": "Membran Sel, Sitoplasma, Dan Nukleus." },
  { "en": "Apa Itu Penyakit Parkinson Yang Dikenal?", "id": "Gangguan Sistem Saraf Pusat Progresif." },
  { "en": "Apa Fungsi Dari Kelenjar Ludah Mulut?", "id": "Memproduksi Air Liur, Bantu Pencernaan." },
  { "en": "Apa Itu Tensimeter Atau Sfigmomanometer Medis?", "id": "Alat Untuk Mengukur Tekanan Darah." },
  { "en": "Apa Yang Dimaksud Dengan Sel Punca?", "id": "Sel Belum Berdiferensiasi, Bisa Berubah." },
  { "en": "Apa Itu HIV (Human Immunodeficiency Virus)?", "id": "Virus Merusak Sistem Kekebalan Tubuh." },
  { "en": "Apa Fungsi Dari Batang Otak Manusia?", "id": "Mengatur Fungsi Vital, Pernapasan, Detak." },
  { "en": "Apa Itu Prosedur Kemoterapi Pada Kanker?", "id": "Pengobatan Kanker Menggunakan Obat Kimia." },
  { "en": "Apa Yang Dimaksud Dengan Indeks Massa Tubuh?", "id": "Ukuran Lemak Tubuh Berdasarkan Tinggi." },
  { "en": "Apa Itu Penyakit Skizofrenia Secara Umum?", "id": "Gangguan Mental Mempengaruhi Pikiran, Perasaan." },
  { "en": "Apa Fungsi Dari Ovarium Pada Wanita?", "id": "Menghasilkan Sel Telur Dan Hormon." },
  { "en": "Apa Itu Proses Inflamasi Atau Peradangan?", "id": "Respon Alami Tubuh Terhadap Cedera." },
  { "en": "Apa Yang Dimaksud Dengan Patogen Berbahaya?", "id": "Mikroorganisme Penyebab Penyakit, Infeksi Berat." },
  { "en": "Apa Itu Penyakit Gagal Jantung Kongestif?", "id": "Jantung Tidak Dapat Memompa Darah." },
  { "en": "Apa Fungsi Dari Tulang Rusuk Manusia?", "id": "Melindungi Jantung Dan Juga Paru-Paru." },
  { "en": "Apa Itu Obat Antipiretik Yang Umum?", "id": "Obat Yang Digunakan Untuk Menurunkan Demam." },
  { "en": "Apa Yang Dimaksud Dengan Masa Nifas?", "id": "Periode Pemulihan Setelah Melahirkan Bayi." },
  { "en": "Apa Itu Penyakit Usus Buntu (Apendisitis)?", "id": "Peradangan Pada Kantong Usus Buntu." },
  { "en": "Apa Fungsi Dari Enzim Pencernaan Manusia?", "id": "Mempercepat Pemecahan Molekul Makanan Kompleks." },
  { "en": "Apa Itu Deoksigenasi Darah Dalam Sirkulasi?", "id": "Darah Yang Kekurangan Kandungan Oksigen." },
  { "en": "Apa Yang Dimaksud Dengan Kode Genetik?", "id": "Aturan Penerjemahan Informasi Genetik DNA." },
  { "en": "Apa Itu Penyakit Chikungunya Yang Terkenal?", "id": "Infeksi Virus Ditularkan Nyamuk Aedes." },
  { "en": "Apa Fungsi Dari Testis Pada Pria?", "id": "Memproduksi Sperma Dan Hormon Testosteron." },
  { "en": "Apa Itu Kontrasepsi Dalam Keluarga Berencana?", "id": "Metode Mencegah Kehamilan Tidak Diinginkan." },
  { "en": "Apa Yang Dimaksud Dengan Asam Amino?", "id": "Unit Dasar Pembangun Molekul Protein." },
  { "en": "Apa Itu Penyakit Leukemia Atau Kanker Darah?", "id": "Kanker Jaringan Pembentuk Sel Darah." },
  { "en": "Apa Fungsi Dari Hipotalamus Di Otak?", "id": "Mengatur Suhu, Lapar, Haus, Emosi." },
  { "en": "Apa Itu Obat Analgesik Dalam Farmasi?", "id": "Obat Pereda Rasa Nyeri Atau Sakit." },
  { "en": "Apa Yang Dimaksud Dengan Donor Darah?", "id": "Menyumbangkan Darah Untuk Transfusi Medis." },
  { "en": "Apa Itu Penyakit Polio Yang Melumpuhkan?", "id": "Infeksi Virus Menyerang Sistem Saraf." },
  { "en": "Apa Fungsi Dari Plasenta Selama Kehamilan?", "id": "Memberi Nutrisi Dan Oksigen Janin." },
  { "en": "Apa Itu Radioterapi Dalam Pengobatan Kanker?", "id": "Penggunaan Radiasi Tinggi Membunuh Kanker." },
  { "en": "Apa Yang Dimaksud Dengan Refleks Tubuh?", "id": "Gerakan Otomatis Tanpa Kesadaran Penuh." },
  { "en": "Apa Itu Penyakit Kusta Atau Lepra?", "id": "Infeksi Bakteri Kronis Menyerang Kulit." },
  { "en": "Apa Fungsi Dari Vena Kava Manusia?", "id": "Pembuluh Vena Terbesar Bawa Darah." },
  { "en": "Apa Itu Jarum Suntik Dalam Medis?", "id": "Alat Memasukkan Cairan Ke Tubuh." },
  { "en": "Apa Yang Dimaksud Dengan Antiseptik Medis?", "id": "Zat Mencegah Pertumbuhan Mikroorganisme Berbahaya." },
  { "en": "Apa Itu Penyakit Campak Atau Morbili?", "id": "Infeksi Virus Sangat Menular, Ruam." },
  { "en": "Apa Fungsi Dari Uretra Sistem Perkemihan?", "id": "Saluran Mengeluarkan Urin Dari Tubuh." },
  { "en": "Apa Itu Obat Antasida Yang Dijual?", "id": "Obat Menetralkan Asam Lambung Berlebih." },
  { "en": "Apa Yang Dimaksud Dengan Mati Batang Otak?", "id": "Berhentinya Semua Fungsi Batang Otak." },
  { "en": "Apa Itu Penyakit Rabies Atau Anjing Gila?", "id": "Infeksi Virus Fatal, Menyerang Otak." },
  { "en": "Apa Fungsi Dari Getah Lambung Manusia?", "id": "Mengandung Asam, Enzim Untuk Pencernaan." },
  { "en": "Apa Itu Termometer Dalam Dunia Medis?", "id": "Alat Untuk Mengukur Suhu Tubuh." },
  { "en": "Apa Yang Dimaksud Dengan Kromosom Manusia?", "id": "Struktur Pembawa DNA Di Inti Sel." },
  { "en": "Apa Itu Penyakit Tetanus Yang Berbahaya?", "id": "Infeksi Bakteri Menyebabkan Kejang Otot." },
  { "en": "Apa Fungsi Dari Sfingter Dalam Tubuh?", "id": "Otot Cincin, Mengatur Aliran Zat." },
  { "en": "Apa Itu Obat Diuretik Dalam Farmakologi?", "id": "Obat Meningkatkan Produksi Urin Tubuh." },
  { "en": "Apa Yang Dimaksud Dengan Janin Manusia?", "id": "Calon Bayi Dalam Kandungan Ibu." },
  { "en": "Apa Itu Penyakit Ambeien Atau Wasir?", "id": "Pembengkakan Pembuluh Darah Di Anus." },
  { "en": "Apa Fungsi Dari Cairan Serebrospinal Otak?", "id": "Melindungi Otak Dan Sumsum Tulang." },
  { "en": "Apa Itu Jahitan Dalam Prosedur Medis?", "id": "Benang Medis Menyatukan Jaringan Tubuh." },
  { "en": "Apa Yang Dimaksud Dengan Antibodi Tubuh?", "id": "Protein Melawan Patogen Spesifik Asing." },
  { "en": "Apa Itu Penyakit Sariawan Di Mulut?", "id": "Luka Kecil Menyakitkan Di Mulut." },
  { "en": "Apa Fungsi Dari Ureter Sistem Ginjal?", "id": "Menyalurkan Urin Dari Ginjal Ke Kandung." },
  { "en": "Apa Itu Obat Antihistamin Bagi Alergi?", "id": "Obat Meredakan Gejala Reaksi Alergi." },
  { "en": "Apa Yang Dimaksud Dengan Ovum Manusia?", "id": "Sel Telur Betina Untuk Reproduksi." },
  { "en": "Apa Itu Penyak-it Biduran Atau Urtikaria?", "id": "Reaksi Kulit Gatal, Bentol Kemerahan." },
  { "en": "Apa Fungsi Dari Serabut Saraf Mielin?", "id": "Mempercepat Transmisi Impuls Saraf Listrik." },
  { "en": "Apa Itu Zat Adiktif Yang Berbahaya?", "id": "Zat Menyebabkan Ketergantungan Fisik, Mental." },
  { "en": "Apa Yang Dimaksud Dengan Enzim Katalase?", "id": "Enzim Memecah Hidrogen Peroksida Beracun." },
  { "en": "Apa Itu Penyakit Batu Ginjal Medis?", "id": "Endapan Keras Terbentuk Di Ginjal." },
  { "en": "Apa Fungsi Utama Dari Tulang Panggul?", "id": "Menopang Tulang Belakang, Melindungi Organ." },
  { "en": "Apa Itu Autopsi Dalam Ilmu Forensik?", "id": "Pemeriksaan Medis Mayat, Menentukan Sebab." },
  { "en": "Apa Yang Dimaksud Dengan Psikotropika Ilegal?", "id": "Zat Mempengaruhi Fungsi Mental, Perilaku." },
  { "en": "Apa Itu Penyakit Hernia Atau Turun Berok?", "id": "Tonjolan Organ Melalui Jaringan Lemah." },
  { "en": "Apa Fungsi Dari Rongga Hidung Manusia?", "id": "Menyaring, Melembapkan, Menghangatkan Udara Masuk." },
  { "en": "Apa Itu Gips Dalam Perawatan Medis?", "id": "Pembalut Keras Untuk Imobilisasi Tulang." },
  { "en": "Apa Yang Dimaksud Dengan Placebo Medis?", "id": "Pengobatan Kosong, Tanpa Zat Aktif." },
  { "en": "Apa Itu Otoskop Dalam Pemeriksaan THT?", "id": "Alat Medis Untuk Memeriksa Telinga." },
  { "en": "Apa Fungsi Hormon Pertumbuhan Manusia (HGH)?", "id": "Merangsang Pertumbuhan, Regenerasi Sel Tubuh." },
  { "en": "Apa Yang Dimaksud Dengan Jaringan Epitel?", "id": "Jaringan Penutup Permukaan Tubuh, Organ." },
  { "en": "Apa Itu Penyakit Psoriasis Pada Kulit?", "id": "Penyakit Autoimun, Kulit Bersisik, Merah." },
  { "en": "Apa Fungsi Dari Kelenjar Keringat Manusia?", "id": "Mengeluarkan Keringat, Mendinginkan Suhu Tubuh." },
  { "en": "Apa Itu Prosedur Lumbal Pungsi Medis?", "id": "Pengambilan Cairan Serebrospinal Dari Punggung." },
  { "en": "Apa Yang Dimaksud Dengan Sel Kuman?", "id": "Sel Reproduksi, Sperma Atau Sel Telur." },
  { "en": "Apa Itu Fobia Dalam Istilah Psikologi?", "id": "Rasa Takut Berlebihan Terhadap Sesuatu." },
  { "en": "Apa Fungsi Dari Otot Polos Manusia?", "id": "Mengontrol Organ Dalam Secara Tidak Sadar." },
  { "en": "Apa Itu AIDS (Acquired Immunodeficiency Syndrome)?", "id": "Tahap Akhir Infeksi Virus HIV." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Saraf?", "id": "Dokter Spesialis Saraf Atau Neurolog." },
  { "en": "Apa Yang Dimaksud Dengan Malnutrisi Gizi?", "id": "Kekurangan, Kelebihan, Atau Ketidakseimbangan Gizi." },
  { "en": "Apa Fungsi Dari Ribosom Di Dalam Sel?", "id": "Tempat Terjadinya Sintesis Molekul Protein." },
  { "en": "Apa Itu Penyakit Disentri Yang Menular?", "id": "Infeksi Usus, Menyebabkan Diare Berdarah." },
  { "en": "Apa Fungsi Hormon Testosteron Pada Pria?", "id": "Mengatur Perkembangan Karakteristik Seksual Pria." },
  { "en": "Apa Itu Alat Bantu Dengar Medis?", "id": "Perangkat Elektronik Membantu Gangguan Pendengaran." },
  { "en": "Apa Yang Dimaksud Dengan Jaringan Ikat?", "id": "Jaringan Penyokong, Pengikat Jaringan Lain." },
  { "en": "Apa Itu Penyakit Konjungtivitis Atau Mata Merah?", "id": "Peradangan Pada Selaput Konjungtiva Mata." },
  { "en": "Apa Fungsi Utama Dari Enzim Amilase?", "id": "Memecah Zat Pati Menjadi Gula." },
  { "en": "Apa Itu Eutanasia Dalam Etika Medis?", "id": "Tindakan Mengakhiri Hidup Untuk Meringankan." },
  { "en": "Apa Yang Dimaksud Dengan Asam Folat?", "id": "Vitamin B9, Penting Bagi Ibu Hamil." },
  { "en": "Apa Itu Skoliosis Pada Tulang Belakang?", "id": "Kelainan Tulang Belakang Melengkung Samping." },
  { "en": "Apa Fungsi Utama Dari Tulang Kering?", "id": "Menopang Berat Badan, Menstabilkan Gerakan." },
  { "en": "Apa Itu Obat Antikoagulan Atau Pengencer Darah?", "id": "Obat Mencegah Terjadinya Pembekuan Darah." },
  { "en": "Apa Yang Dimaksud Dengan Sistem Kardiovaskular?", "id": "Sistem Peredaran Darah, Jantung, Pembuluh." },
  { "en": "Apa Itu Penyakit Bronkitis Sistem Pernapasan?", "id": "Peradangan Pada Lapisan Saluran Bronkial." },
  { "en": "Apa Fungsi Hormon Estrogen Pada Wanita?", "id": "Mengatur Siklus Menstruasi, Karakteristik Seksual." },
  { "en": "Apa Itu Oftalmoskop Dalam Pemeriksaan Mata?", "id": "Alat Medis Untuk Melihat Retina." },
  { "en": "Apa Yang Dimaksud Dengan Jaringan Saraf?", "id": "Jaringan Terdiri Dari Sel-Sel Saraf." },
  { "en": "Apa Itu Penyakit Sinusitis Atau Radang Sinus?", "id": "Peradangan Pada Dinding Rongga Sinus." },
  { "en": "Apa Fungsi Dari Gigi Geraham Manusia?", "id": "Mengunyah Dan Menggiling Makanan Keras." },
  { "en": "Apa Itu Defibrilator Dalam Gawat Darurat?", "id": "Alat Memberi Kejutan Listrik Jantung." },
  { "en": "Apa Yang Dimaksud Dengan Antigen Asing?", "id": "Zat Merangsang Respon Imun Tubuh." },
  { "en": "Apa Itu Penyakit Vertigo Yang Mengganggu?", "id": "Sensasi Pusing Berputar Hebat Tiba-Tiba." },
  { "en": "Apa Fungsi Dari Kelenjar Paratiroid Leher?", "id": "Mengatur Kadar Kalsium Dalam Darah." },
  { "en": "Apa Itu Obat Ekspektoran Untuk Batuk?", "id": "Obat Membantu Mengencerkan Dan Mengeluarkan Dahak." },
  { "en": "Apa Yang Dimaksud Dengan Sistem Integumen?", "id": "Sistem Organ Pelindung Luar Tubuh." },
  { "en": "Apa Itu Penyakit Depresi Gangguan Mental?", "id": "Gangguan Suasana Hati, Menyebabkan Kesedihan." },
  { "en": "Apa Fungsi Dari Tulang Selangka Manusia?", "id": "Menghubungkan Lengan Dengan Kerangka Tubuh." },
  { "en": "Apa Itu Proses Sitokinesis Dalam Sel?", "id": "Pembelahan Sitoplasma Sel Menjadi Dua." },
  { "en": "Apa Yang Dimaksud Dengan Kematian Klinis?", "id": "Berhentinya Detak Jantung Dan Pernapasan." },
  { "en": "Apa Itu Penyakit Limfoma Atau Kanker Getah?", "id": "Kanker Yang Berasal Dari Sistem Limfatik." },
  { "en": "Apa Fungsi Dari Sfingter Ani Manusia?", "id": "Otot Mengontrol Pengeluaran Feses (Tinja)." },
  { "en": "Apa Itu Obat Vasodilator Dalam Farmasi?", "id": "Obat Melebarkan Pembuluh Darah Manusia." },
  { "en": "Apa Yang Dimaksud Dengan Karbohidrat Kompleks?", "id": "Sumber Energi, Serat, Vitamin, Mineral." },
  { "en": "Apa Itu Penyakit Gusi Atau Gingivitis?", "id": "Peradangan Pada Jaringan Gusi Manusia." },
  { "en": "Apa Fungsi Hormon Adrenalin Saat Stres?", "id": "Meningkatkan Detak Jantung, Tekanan Darah." },
  { "en": "Apa Itu Inkubator Untuk Bayi Prematur?", "id": "Alat Menjaga Suhu Bayi Stabil." },
  { "en": "Apa Yang Dimaksud Dengan Jaringan Otot?", "id": "Jaringan Terdiri Dari Sel-Sel Otot." },
  { "en": "Apa Itu Penyakit Bipolar Gangguan Mental?", "id": "Perubahan Suasana Hati Ekstrem, Mania." },
  { "en": "Apa Fungsi Dari Tulang Belikat Manusia?", "id": "Menstabilkan Bahu, Memungkinkan Gerakan Lengan." },
  { "en": "Apa Itu Proses Meiosis Pembelahan Sel?", "id": "Pembelahan Sel Menghasilkan Sel Kelamin." },
  { "en": "Apa Yang Dimaksud Dengan Mati Suri?", "id": "Pengalaman Mendekati Kematian Secara Klinis." },
  { "en": "Apa Itu Penyakit Pneumonia Atau Paru-Paru Basah?", "id": "Infeksi Menyebabkan Peradangan Kantung Udara." },
  { "en": "Apa Fungsi Dari Selaput Lendir Hidung?", "id": "Menjebak Debu, Bakteri, Partikel Asing." },
  { "en": "Apa Itu Obat Dekongestan Untuk Hidung?", "id": "Obat Meredakan Hidung Tersumbat Pilek." },
  { "en": "Apa Yang Dimaksud Dengan Lemak Jenuh?", "id": "Jenis Lemak Buruk, Meningkatkan Kolesterol." },
  { "en": "Apa Itu Penyakis Karies Gigi Berlubang?", "id": "Kerusakan Jaringan Keras Gigi Manusia." },
  { "en": "Apa Fungsi Hormon Kortisol Atau Hormon Stres?", "id": "Mengatur Metabolisme, Respon Stres Tubuh." },
  { "en": "Apa Itu Kateter Dalam Prosedur Medis?", "id": "Tabung Fleksibel Memasukkan, Mengeluarkan Cairan." },
  { "en": "Apa Yang Dimaksud Dengan Lisosom Sel?", "id": "Organel Sel, Mencerna Limbah Seluler." },
  { "en": "Apa Itu Penyakit Celiac Atau Intoleransi Gluten?", "id": "Reaksi Imun Terhadap Konsumsi Gluten." },
  { "en": "Apa Fungsi Dari Tulang Pengumpil Lengan?", "id": "Memungkinkan Gerakan Rotasi Lengan Bawah." },
  { "en": "Apa Itu Proses Mitosis Pembelahan Sel?", "id": "Pembelahan Sel Menghasilkan Sel Identik." },
  { "en": "Apa Yang Dimaksud Dengan Otot Lurik?", "id": "Otot Rangka, Bekerja Di Bawah Kesadaran." },
  { "en": "Apa Itu Penyakit Anoreksia Nervosa Medis?", "id": "Gangguan Makan, Takut Berat Badan." },
  { "en": "Apa Fungsi Dari Hormon Melatonin Manusia?", "id": "Mengatur Siklus Tidur Dan Bangun." },
  { "en": "Apa Itu Pipet Dalam Laboratorium Medis?", "id": "Alat Memindahkan Sejumlah Kecil Cairan." },
  { "en": "Apa Yang Dimaksud Dengan Aparatus Golgi?", "id": "Organel Memproses, Mengemas Protein, Lipid." },
  { "en": "Apa Itu Penyakit Sirosis Hati Kronis?", "id": "Kerusakan Hati Jangka Panjang, Jaringan Parut." },
  { "en": "Apa Fungsi Dari Tulang Pipi Wajah?", "id": "Membentuk Struktur Wajah, Melindungi Mata." },
  { "en": "Apa Itu Zigot Hasil Dari Fertilisasi?", "id": "Sel Tunggal Hasil Penyatuan Gamet." },
  { "en": "Apa Yang Dimaksud Dengan Denyut Jantung Janin?", "id": "Detak Jantung Janin Selama Kehamilan." },
  { "en": "Apa Itu Penyakit Endometriosis Pada Wanita?", "id": "Jaringan Rahim Tumbuh Di Luar." },
  { "en": "Apa Fungsi Hormon Progesteron Pada Wanita?", "id": "Mempersiapkan Rahim Untuk Proses Kehamilan." },
  { "en": "Apa Itu Mikroskop Dalam Dunia Medis?", "id": "Alat Melihat Objek Sangat Kecil." },
  { "en": "Apa Yang Dimaksud Dengan Retikulum Endoplasma?", "id": "Jaringan Membran, Tempat Sintesis Protein." },
  { "en": "Apa Itu Penyakit Batu Empedu Kolelitiasis?", "id": "Endapan Keras Di Dalam Kantung Empedu." },
  { "en": "Apa Fungsi Dari Tulang Rahang Bawah?", "id": "Memungkinkan Gerakan Mengunyah Dan Berbicara." },
  { "en": "Apa Itu Embrio Tahap Awal Perkembangan?", "id": "Organisme Eukariotik Diploid Tahap Awal." },
  { "en": "Apa Yang Dimaksud Dengan Resusitasi Jantung Paru?", "id": "Tindakan Pertolongan Pertama Henti Jantung." },
  { "en": "Apa Itu Penyakit Hemofilia Kelainan Darah?", "id": "Gangguan Pembekuan Darah Secara Genetik." },
  { "en": "Apa Fungsi Dari Hormon Oksitosin Manusia?", "id": "Merangsang Kontraksi Rahim, Produksi ASI." },
  { "en": "Apa Itu Pinset Dalam Peralatan Medis?", "id": "Alat Menjepit Benda Kecil, Jaringan." },
  { "en": "Apa Yang Dimaksud Dengan Vakuola Sel?", "id": "Organel Penyimpan Air, Makanan, Limbah." },
  { "en": "Apa Itu Penyakit Hipertiroidisme Kelenjar Tiroid?", "id": "Kelenjar Tiroid Terlalu Aktif, Berlebihan." },
  { "en": "Apa Fungsi Dari Tulang Dahi Manusia?", "id": "Melindungi Bagian Depan Organ Otak." },
  { "en": "Apa Itu Proses Diferensiasi Seluler Biologi?", "id": "Proses Sel Menjadi Lebih Khusus." },
  { "en": "Apa Yang Dimaksud Dengan Puskemas Kesehatan?", "id": "Pusat Kesehatan Masyarakat Tingkat Kecamatan." },
  { "en": "Apa Itu Penyakit Talasemia Kelainan Darah?", "id": "Kelainan Genetik, Produksi Hemoglobin Kurang." },
  { "en": "Apa Fungsi Dari Kelenjar Pineal Otak?", "id": "Menghasilkan Hormon Melatonin Pengatur Tidur." },
  { "en": "Apa Itu Gunting Bedah Dalam Operasi?", "id": "Gunting Tajam Untuk Memotong Jaringan." },
  { "en": "Apa Yang Dimaksud Dengan Sitoplasma Sel?", "id": "Cairan Di Dalam Sel, Mengelilingi Inti." },
  { "en": "Apa Itu Penyakit Hipotiroidisme Kelenjar Tiroid?", "id": "Kelenjar Tiroid Kurang Aktif, Lambat." },
  { "en": "Apa Itu Skalpel Dalam Peralatan Bedah?", "id": "Pisau Bedah Sangat Tajam, Presisi." },
  { "en": "Apa Yang Dimaksud Dengan Cairan Amnion?", "id": "Cairan Ketuban Pelindung Janin Kandungan." },
  { "en": "Apa Itu Penyakit Obsesif-Kompulsif (OCD)?", "id": "Gangguan Kecemasan, Pikiran Obsesif Berulang." },
  { "en": "Apa Fungsi Dari Tulang Tumit Kaki?", "id": "Menopang Berat Badan Saat Berdiri." },
  { "en": "Apa Itu EEG (Electroencephalogram) Pemeriksaan Saraf?", "id": "Tes Merekam Aktivitas Listrik Otak." },
  { "en": "Apa Yang Dimaksud Dengan Sistem Muskuloskeletal?", "id": "Sistem Rangka, Otot, Sendi, Jaringan." },
  { "en": "Apa Itu Penyakit Croup Pada Anak?", "id": "Infeksi Saluran Napas Atas, Batuk." },
  { "en": "Apa Fungsi Hormon Glukagon Dari Pankreas?", "id": "Meningkatkan Kadar Gula Darah Rendah." },
  { "en": "Apa Itu Infus Intravena (IV) Medis?", "id": "Pemberian Cairan Langsung Ke Pembuluh." },
  { "en": "Apa Yang Dimaksud Dengan Membran Sel?", "id": "Lapisan Luar Pelindung Isi Sel." },
  { "en": "Apa Itu Penyakit Gila Sapi Medis?", "id": "Penyakit Otak Sapi Menular Manusia." },
  { "en": "Apa Fungsi Dari Ligamen Pada Sendi?", "id": "Jaringan Ikat Penghubung Tulang Dengan Tulang." },
  { "en": "Sebutkan Spesialisasi Dokter Kulit Kelamin?", "id": "Dokter Spesialis Dermatologi Dan Venereologi." },
  { "en": "Apa Yang Dimaksud Dengan Epidemiologi Medis?", "id": "Studi Penyebaran, Pengendalian Penyakit Populasi." },
  { "en": "Apa Itu Penyakit Rosacea Pada Kulit?", "id": "Penyakit Kulit, Wajah Kemerahan, Benjolan." },
  { "en": "Apa Fungsi Dari Tendon Pada Otot?", "id": "Jaringan Ikat Penghubung Otot Ke Tulang." },
  { "en": "Apa Itu Obat Antivirus Dalam Farmakologi?", "id": "Obat Untuk Mengobati Infeksi Virus." },
  { "en": "Apa Yang Dimaksud Dengan Zat Karsinogenik?", "id": "Zat Yang Dapat Memicu Kanker." },
  { "en": "Apa Itu Penyakit Toksoplasmosis Dari Parasit?", "id": "Infeksi Parasit Toxoplasma Gondii Umum." },
  { "en": "Apa Fungsi Dari Tulang Ubun-Ubun Tengkorak?", "id": "Melindungi Bagian Atas Dan Samping." },
  { "en": "Apa Itu Intubasi Dalam Prosedur Medis?", "id": "Memasukkan Tabung Ke Trakea Pernapasan." },
  { "en": "Apa Yang Dimaksud Dengan Homeostasis Tubuh?", "id": "Kemampuan Menjaga Keseimbangan Lingkungan Internal." },
  { "en": "Apa Itu Penyakit Psikosomatis Dalam Psikologi?", "id": "Penyakit Fisik Disebabkan Faktor Mental." },
  { "en": "Apa Fungsi Dari Iris Pada Mata?", "id": "Mengatur Ukuran Pupil, Memberi Warna." },
  { "en": "Apa Itu Obat Imunosupresan Sistem Imun?", "id": "Obat Menekan Aktivitas Sistem Kekebalan." },
  { "en": "Apa Yang Dimaksud Dengan Sanitasi Lingkungan?", "id": "Upaya Mengendalikan Faktor Lingkungan Fisik." },
  { "en": "Apa Itu Penyakit Fibromyalgia Kronis Nyeri?", "id": "Nyeri Kronis Menyebar, Kelelahan, Gangguan." },
  { "en": "Apa Fungsi Dari Epiglotis Saluran Napas?", "id": "Mencegah Makanan Masuk Ke Trakea." },
  { "en": "Apa Itu Ventilator Mekanis Bantuan Napas?", "id": "Mesin Membantu Pasien Bernapas Normal." },
  { "en": "Apa Yang Dimaksud Dengan Osmosis Seluler?", "id": "Perpindahan Air Melalui Membran Semipermeabel." },
  { "en": "Apa Itu Penyakit Antraks Dari Bakteri?", "id": "Infeksi Bakteri Bacillus Anthracis Serius." },
  { "en": "Apa Fungsi Dari Tulang Kelangkang Manusia?", "id": "Menghubungkan Tulang Belakang Dengan Panggul." },
  { "en": "Apa Itu Obat Beta-Blocker Untuk Jantung?", "id": "Obat Mengurangi Beban Kerja Jantung." },
  { "en": "Apa Yang Dimaksud Dengan Angka Harapan Hidup?", "id": "Rata-Rata Tahun Hidup Seseorang." },
  { "en": "Apa Itu Penyakit Lupus Eritematosus Sistemik?", "id": "Penyakit Autoimun, Peradangan Kronis Terjadi." },
  { "en": "Apa Fungsi Dari Laring Atau Kotak Suara?", "id": "Menghasilkan Suara, Melindungi Saluran Napas." },
  { "en": "Apa Itu Forceps Dalam Alat Medis?", "id": "Alat Penjepit Untuk Memegang Jaringan." },
  { "en": "Apa Yang Dimaksud Dengan Difusi Molekul?", "id": "Pergerakan Molekul Dari Konsentrasi Tinggi." },
  { "en": "Apa Itu Penyakit Pes Atau Sampar?", "id": "Infeksi Bakteri Yersinia Pestis Berbahaya." },
  { "en": "Apa Fungsi Dari Tulang Ekor Manusia?", "id": "Sisa Evolusi, Titik Jangkar Otot." },
  { "en": "Apa Itu Obat Statin Untuk Kolesterol?", "id": "Obat Menurunkan Kadar Kolesterol Jahat." },
  { "en": "Apa Yang Dimaksud Dengan Pandemi Global?", "id": "Wabah Penyakit Menyebar Di Seluruh Dunia." },
  { "en": "Apa Itu Penyakit Multiple Sclerosis (MS)?", "id": "Penyakit Autoimun, Menyerang Selubung Saraf." },
  { "en": "Apa Fungsi Dari Faring Atau Tenggorokan?", "id": "Saluran Umum Untuk Makanan, Udara." },
  { "en": "Apa Itu Klem Dalam Peralatan Bedah?", "id": "Alat Menjepit Pembuluh Darah, Jaringan." },
  { "en": "Apa Yang Dimaksud Dengan Transpor Aktif Sel?", "id": "Pergerakan Molekul Membutuhkan Energi (ATP)." },
  { "en": "Apa Itu Penyakit Kolera Infeksi Usus?", "id": "Infeksi Bakteri Vibrio Cholerae, Diare." },
  { "en": "Apa Fungsi Dari Tulang Pergelangan Tangan?", "id": "Memungkinkan Gerakan Fleksibel Pada Tangan." },
  { "en": "Apa Itu Obat Kortikosteroid Untuk Peradangan?", "id": "Obat Anti-Inflamasi Kuat, Meredakan Peradangan." },
  { "en": "Apa Yang Dimaksud Dengan Vektor Penyakit?", "id": "Organisme Pembawa, Pemindah Agen Infeksi." },
  { "en": "Apa Itu Penyakit Huntington Kelainan Genetik?", "id": "Kerusakan Sel Saraf Otak Progresif." },
  { "en": "Apa Fungsi Dari Bronkus Sistem Pernapasan?", "id": "Cabang Trakea Menuju Ke Paru-Paru." },
  { "en": "Apa Itu Jarum Hecting Atau Jarum Jahit?", "id": "Jarum Khusus Untuk Menjahit Luka." },
  { "en": "Apa Yang Dimaksud Dengan Fagositosis Seluler?", "id": "Proses Sel Menelan Partikel Padat." },
  { "en": "Apa Itu Penyakit Demam Kuning (Yellow Fever)?", "id": "Penyakit Virus Ditularkan Oleh Nyamuk." },
  { "en": "Apa Fungsi Dari Tulang Pergelangan Kaki?", "id": "Menghubungkan Kaki Dengan Tungkai Bawah." },
  { "en": "Apa Itu Obat Antijamur Untuk Infeksi?", "id": "Obat Mengobati Infeksi Akibat Jamur." },
  { "en": "Apa Yang Dimaksud Dengan Zoonosis Medis?", "id": "Penyakit Menular Dari Hewan Manusia." },
  { "en": "Apa Itu Penyakit Fibrosis Kistik Genetik?", "id": "Kelainan Genetik, Lendir Kental Rusak." },
  { "en": "Apa Fungsi Dari Bronkiolus Pernapasan Manusia?", "id": "Cabang Kecil Bronkus Di Paru-Paru." },
  { "en": "Apa Itu Benang Bedah Dalam Operasi?", "id": "Benang Steril Untuk Menjahit Luka." },
  { "en": "Apa Yang Dimaksud Dengan Pinositosis Seluler?", "id": "Proses Sel Menelan Tetesan Cairan." },
  { "en": "Apa Itu Penyakit Leptospirosis Dari Bakteri?", "id": "Penyakit Akibat Bakteri Leptospira, Hewan." },
  { "en": "Apa Fungsi Dari Tulang Telinga Dalam?", "id": "Meneruskan Getaran Suara Ke Koklea." },
  { "en": "Apa Itu Obat Antiretroviral Untuk HIV?", "id": "Obat Menghambat Perkembangan Virus HIV." },
  { "en": "Apa Yang Dimaksud Dengan Morbiditas Medis?", "id": "Angka Kesakitan Akibat Penyakit Tertentu." },
  { "en": "Apa Itu Penyakit Distrofi Otot Genetik?", "id": "Kelompok Penyakit, Melemahkan Otot Progresif." },
  { "en": "Apa Fungsi Utama Dari Rongga Pleura?", "id": "Memungkinkan Paru-Paru Mengembang Tanpa Gesekan." },
  { "en": "Apa Itu Spekulum Dalam Pemeriksaan Medis?", "id": "Alat Membuka Rongga Tubuh Pemeriksaan." },
  { "en": "Apa Yang Dimaksud Dengan Potensial Aksi?", "id": "Sinyal Listrik Cepat Di Membran." },
  { "en": "Apa Itu Penyakit Filariasis Atau Kaki Gajah?", "id": "Infeksi Cacing Filaria, Pembengkakan Tungkai." },
  { "en": "Apa Fungsi Dari Tulang Metatarsal Kaki?", "id": "Membentuk Lengkungan Kaki, Menopang Berat." },
  { "en": "Apa Itu Obat NSAID (Nonsteroidal Anti-Inflammatory Drug)?", "id": "Obat Meredakan Nyeri, Demam, Peradangan." },
  { "en": "Apa Yang Dimaksud Dengan Mortalitas Medis?", "id": "Angka Kematian Akibat Penyakit Tertentu." },
  { "en": "Apa Itu Penyakit Skleroderma Autoimun Kronis?", "id": "Pengerasan, Penebalan Kulit, Jaringan Ikat." },
  { "en": "Apa Fungsi Dari Saluran Eustachius Telinga?", "id": "Menyeimbangkan Tekanan Udara Dalam Telinga." },
  { "en": "Apa Itu Tabung Reaksi Di Laboratorium?", "id": "Wadah Kaca Untuk Reaksi Kimia." },
  { "en": "Apa Yang Dimaksud Dengan Periosteum Tulang?", "id": "Membran Serat Pelapis Permukaan Luar." },
  { "en": "Apa Itu Penyakit Legionellosis Dari Bakteri?", "id": "Jenis Pneumonia Berat, Bakteri Legionella." },
  { "en": "Apa Fungsi Dari Tulang Metakarpal Tangan?", "id": "Menghubungkan Pergelangan Dengan Jari Tangan." },
  { "en": "Apa Itu Obat Antiemetik Untuk Mual?", "id": "Obat Mencegah Mual Dan Juga Muntah." },
  { "en": "Apa Yang Dimaksud Dengan Prevalensi Penyakit?", "id": "Jumlah Kasus Penyakit Dalam Populasi." },
  { "en": "Apa Itu Penyakit Miastenia Gravis Autoimun?", "id": "Kelemahan Otot Rangka, Gangguan Saraf." },
  { "en": "Apa Fungsi Dari Koklea Telinga Dalam?", "id": "Mengubah Getaran Suara Menjadi Sinyal." },
  { "en": "Apa Itu Cawan Petri Di Laboratorium?", "id": "Wadah Datar, Membiakkan Mikroorganisme, Sel." },
  { "en": "Apa Yang Dimaksud Dengan Endosteum Tulang?", "id": "Membran Tipis Melapisi Rongga Sumsum." },
  { "en": "Apa Itu Penyakit Listeriosis Dari Bakteri?", "id": "Infeksi Bakteri Listeria, Makanan Terkontaminasi." },
  { "en": "Apa Fungsi Dari Tulang Jari Falang?", "id": "Memungkinkan Gerakan Fleksibel Jari Kaki." },
  { "en": "Apa Itu Obat Antitrombotik Untuk Gumpalan?", "id": "Obat Mencegah Pembentukan Gumpalan Darah." },
  { "en": "Apa Yang Dimaksud Dengan Insidensi Penyakit?", "id": "Jumlah Kasus Baru Penyakit Tertentu." },
  { "en": "Apa Itu Penyakit Sindrom Guillain-Barre Langka?", "id": "Sistem Imun Menyerang Saraf Tepi." },
  { "en": "Apa Fungsi Kelenjar Sebasea Pada Kulit?", "id": "Menghasilkan Sebum, Minyak Alami Pelumas." },
  { "en": "Apa Itu Spatula Dalam Alat Laboratorium?", "id": "Alat Mengambil, Memindahkan Zat Padat." },
  { "en": "Apa Yang Dimaksud Dengan Osteoblas Tulang?", "id": "Sel Yang Bertugas Membentuk Tulang." },
  { "en": "Apa Itu Penyakit Trakoma Infeksi Mata?", "id": "Infeksi Bakteri Chlamydia, Menyebabkan Kebutaan." },
  { "en": "Apa Itu Posyandu Dalam Sistem Kesehatan?", "id": "Pos Pelayanan Terpadu Untuk Masyarakat." },
  { "en": "Apa Yang Dimaksud Dengan Osteoklas Tulang?", "id": "Sel Yang Berfungsi Merombak Tulang." },
  { "en": "Apa Itu Penyakit Chikungunya Akibat Virus?", "id": "Demam Virus Ditularkan Nyamuk Aedes." },
  { "en": "Apa Fungsi Dari Tulang Tempurung Lutut?", "id": "Melindungi Sendi Lutut, Meningkatkan Kekuatan." },
  { "en": "Apa Itu Obat Antipruritik Untuk Gatal?", "id": "Obat Meredakan Sensasi Gatal Kulit." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Kanker?", "id": "Dokter Spesialis Onkologi Atau Onkolog." },
  { "en": "Apa Itu Penyakit Buta Warna Genetik?", "id": "Ketidakmampuan Membedakan Warna Tertentu Akurat." },
  { "en": "Apa Fungsi Dari Medula Oblongata Otak?", "id": "Mengatur Pernapasan, Detak Jantung, Pencernaan." },
  { "en": "Apa Itu Kuretase Dalam Prosedur Medis?", "id": "Tindakan Mengikis Lapisan Rahim (Endometrium)." },
  { "en": "Apa Yang Dimaksud Dengan Asam Laktat?", "id": "Produk Sampingan Metabolisme Anaerobik Otot." },
  { "en": "Apa Itu Penyakit Sindrom Down Genetik?", "id": "Kelainan Genetik, Kelebihan Kromosom 21." },
  { "en": "Apa Fungsi Dari Lobus Frontal Otak?", "id": "Mengatur Gerakan, Bahasa, Memori, Kepribadian." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Jantung?", "id": "Dokter Spesialis Jantung Atau Kardiolog." },
  { "en": "Apa Yang Dimaksud Dengan Uji Tipes?", "id": "Tes Darah Mendeteksi Bakteri Salmonella." },
  { "en": "Apa Itu Penyakit Radang Panggul (PID)?", "id": "Infeksi Organ Reproduksi Wanita Atas." },
  { "en": "Apa Fungsi Dari Lobus Parietal Otak?", "id": "Memproses Informasi Sensorik, Sentuhan, Suhu." },
  { "en": "Apa Itu Obat Mukolitik Untuk Dahak?", "id": "Obat Mengencerkan Dahak Kental, Lengket." },
  { "en": "Apa Yang Dimaksud Dengan Triase Medis?", "id": "Proses Pemilahan Pasien Berdasarkan Prioritas." },
  { "en": "Apa Itu Penyakit Gangguan Stres Pascatrauma?", "id": "Gangguan Kecemasan Setelah Peristiwa Traumatis." },
  { "en": "Apa Fungsi Dari Lobus Temporal Otak?", "id": "Memproses Pendengaran, Memori, Emosi, Bahasa." },
  { "en": "Apa Itu Laparoskopi Dalam Prosedur Bedah?", "id": "Pembedahan Minimal Dengan Sayatan Kecil." },
  { "en": "Apa Yang Dimaksud Dengan Glikolisis Biokimia?", "id": "Pemecahan Glukosa Menjadi Piruvat, Energi." },
  { "en": "Apa Itu Penyakit Ebola Virus Mematikan?", "id": "Penyakit Virus, Menyebabkan Pendarahan Hebat." },
  { "en": "Apa Fungsi Dari Lobus Oksipital Otak?", "id": "Memproses Dan Menginterpretasikan Informasi Visual." },
  { "en": "Apa Itu Obat Trombolitik Untuk Sumbatan?", "id": "Obat Melarutkan Gumpalan Darah Berbahaya." },
  { "en": "Apa Yang Dimaksud Dengan Geriatri Medis?", "id": "Cabang Medis Fokus Kesehatan Lansia." },
  { "en": "Apa Itu Penyakit Tinea Atau Kurap?", "id": "Infeksi Jamur Pada Kulit Manusia." },
  { "en": "Apa Fungsi Dari Sistem Saraf Tepi?", "id": "Menghubungkan Sistem Saraf Pusat Tubuh." },
  { "en": "Apa Itu Angiografi Pemeriksaan Pembuluh Darah?", "id": "Tes Pencitraan Melihat Pembuluh Darah." },
  { "en": "Apa Yang Dimaksud Dengan Siklus Krebs?", "id": "Serangkaian Reaksi Kimia Hasilkan Energi." },
  { "en": "Apa Itu Penyakit Hantavirus Dari Tikus?", "id": "Virus Ditularkan Melalui Kotoran Tikus." },
  { "en": "Apa Fungsi Dari Saraf Kranial Manusia?", "id": "Dua Belas Pasang Saraf Dari Otak." },
  { "en": "Apa Itu Obat Ansiolitik Untuk Cemas?", "id": "Obat Mengurangi Gejala Gangguan Kecemasan." },
  { "en": "Apa Yang Dimaksud Dengan Perawatan Paliatif?", "id": "Perawatan Meringankan Gejala Penyakit Serius." },
  { "en": "Apa Itu Penyakit Panu Atau Tinea Versicolor?", "id": "Infeksi Jamur, Bercak Terang Kulit." },
  { "en": "Apa Fungsi Dari Saraf Spinal Manusia?", "id": "Tiga Puluh Satu Pasang Saraf." },
  { "en": "Apa Itu Kolonoskopi Pemeriksaan Usus Besar?", "id": "Prosedur Melihat Bagian Dalam Usus." },
  { "en": "Apa Yang Dimaksud Dengan Rantai Transpor Elektron?", "id": "Tahap Akhir Respirasi Seluler, Hasilkan." },
  { "en": "Apa Itu Penyakit Antraks Kulit Berbahaya?", "id": "Bentuk Paling Umum Infeksi Antraks." },
  { "en": "Apa Fungsi Dari Ganglion Sistem Saraf?", "id": "Kumpulan Badan Sel Saraf Tepi." },
  { "en": "Apa Itu Obat Antipsikotik Gangguan Jiwa?", "id": "Obat Mengelola Gejala Psikosis Berat." },
  { "en": "Apa Yang Dimaksud Dengan Informed Consent?", "id": "Persetujuan Pasien Setelah Mendapat Informasi." },
  { "en": "Apa Itu Penyakit Eksim Atau Dermatitis Atopik?", "id": "Peradangan Kulit, Gatal, Kering, Merah." },
  { "en": "Apa Fungsi Dari Cairan Pleura Paru?", "id": "Cairan Pelumas Antara Lapisan Pleura." },
  { "en": "Apa Itu Mammografi Pemeriksaan Payudara Medis?", "id": "Pemeriksaan Sinar-X Jaringan Payudara Wanita." },
  { "en": "Apa Yang Dimaksud Dengan Glukoneogenesis Biokimia?", "id": "Sintesis Glukosa Dari Senyawa Non-Karbohidrat." },
  { "en": "Apa Itu Penyakit MERS-CoV Virus Pernapasan?", "id": "Virus Korona Pernapasan Timur Tengah." },
  { "en": "Apa Fungsi Dari Saraf Sensorik Tubuh?", "id": "Membawa Sinyal Dari Indra Ke Otak." },
  { "en": "Apa Itu Obat Penstabil Suasana Hati?", "id": "Obat Mengobati Gangguan Bipolar, Mood." },
  { "en": "Apa Yang Dimaksud Dengan Rekam Medis?", "id": "Dokumen Berisi Riwayat Kesehatan Pasien." },
  { "en": "Apa Itu Penyakit Impetigo Infeksi Kulit?", "id": "Infeksi Bakteri Kulit, Sangat Menular." },
  { "en": "Apa Fungsi Dari Saraf Motorik Tubuh?", "id": "Membawa Sinyal Dari Otak Ke Otot." },
  { "en": "Apa Itu Pap Smear Tes Kanker Serviks?", "id": "Prosedur Deteksi Dini Kanker Serviks." },
  { "en": "Apa Yang Dimaksud Dengan Enzim Lipase?", "id": "Enzim Pemecah Lemak Menjadi Gliserol." },
  { "en": "Apa Itu Penyakit SARS-CoV Virus Pernapasan?", "id": "Sindrom Pernapasan Akut Parah Virus." },
  { "en": "Apa Fungsi Dari Sinaps Sistem Saraf?", "id": "Celah Antara Dua Sel Saraf." },
  { "en": "Apa Itu Obat Hipnotik Atau Obat Tidur?", "id": "Obat Menginduksi Tidur, Mengobati Insomnia." },
  { "en": "Apa Yang Dimaksud Dengan Etika Kedokteran?", "id": "Prinsip Moral Praktik Profesi Kedokteran." },
  { "en": "Apa Itu Penyakit Selulitis Infeksi Bakteri?", "id": "Infeksi Bakteri Pada Jaringan Kulit." },
  { "en": "Apa Fungsi Dari Neurotransmiter Di Otak?", "id": "Senyawa Kimia Pembawa Sinyal Antarsaraf." },
  { "en": "Apa Itu Biopsi Sumsum Tulang Belakang?", "id": "Pengambilan Sampel Sumsum Tulang Pemeriksaan." },
  { "en": "Apa Yang Dimaksud Dengan Enzim Protease?", "id": "Enzim Pemecah Protein Menjadi Asam." },
  { "en": "Apa Itu Penyakit Avian Influenza (Flu Burung)?", "id": "Jenis Influenza Disebabkan Virus Unggas." },
  { "en": "Apa Fungsi Dari Akson Sel Saraf?", "id": "Serabut Saraf Panjang, Menghantarkan Impuls." },
  { "en": "Apa Itu Obat Relaksan Otot Manusia?", "id": "Obat Mengurangi Ketegangan Otot Rangka." },
  { "en": "Apa Yang Dimaksud Dengan Malapraktik Medis?", "id": "Kelalaian Profesional Oleh Tenaga Medis." },
  { "en": "Apa Itu Penyakter Folikulitis Atau Radang Folikel?", "id": "Peradangan Pada Folikel Rambut Manusia." },
  { "en": "Apa Fungsi Dari Dendrit Sel Saraf?", "id": "Cabang Penerima Sinyal Dari Sel." },
  { "en": "Apa Itu Bronkoskopi Pemeriksaan Saluran Napas?", "id": "Prosedur Melihat Saluran Udara Paru-Paru." },
  { "en": "Apa Yang Dimaksud Dengan Trigliserida Darah?", "id": "Jenis Lemak Dalam Darah Manusia." },
  { "en": "Apa Itu Penyakit Demam Lassa Virus?", "id": "Demam Virus Akut, Ditularkan Tikus." },
  { "en": "Apa Fungsi Dari Selubung Mielin Saraf?", "id": "Lapisan Lemak Pelindung, Mempercepat Impuls." },
  { "en": "Apa Itu Terapi Perilaku Kognitif (CBT)?", "id": "Jenis Psikoterapi, Mengubah Pola Pikir." },
  { "en": "Apa Yang Dimaksud Dengan Kerahasiaan Medis?", "id": "Kewajiban Menjaga Informasi Pasien Rahasia." },
  { "en": "Apa Itu Penyakit Bisul Atau Furunkel?", "id": "Infeksi Bakteri Folikel Rambut, Bernanah." },
  { "en": "Apa Fungsi Dari Refleks Lutut Manusia?", "id": "Kontraksi Otot Paha Tidak Disengaja." },
  { "en": "Apa Itu Sistoskopi Pemeriksaan Kandung Kemih?", "id": "Prosedur Melihat Bagian Dalam Kandung." },
  { "en": "Apa Yang Dimaksud Dengan ATP (Adenosine Triphosphate)?", "id": "Molekul Pembawa Energi Utama Sel." },
  { "en": "Apa Itu Penyakit Demam Berdarah Krimea-Kongo?", "id": "Penyakit Virus Ditularkan Melalui Sengkenit." },
  { "en": "Apa Fungsi Dari Cairan Perikardial Jantung?", "id": "Cairan Pelumas Mengelilingi Organ Jantung." },
  { "en": "Apa Itu Terapi Kejut Listrik (ECT)?", "id": "Prosedur Medis, Kejang Singkat Distimulasi." },
  { "en": "Apa Yang Dimaksud Dengan Sumpah Hippokrates?", "id": "Sumpah Etika Diucapkan Para Dokter." },
  { "en": "Apa Itu Penyakit Kudis Atau Skabies?", "id": "Infestasi Tungau Gatal Sarcoptes Scabiei." },
  { "en": "Apa Fungsi Dari Sistem Saraf Simpatik?", "id": "Memicu Respon Lawan Atau Lari." },
  { "en": "Apa Itu Histeroskopi Pemeriksaan Rahim Medis?", "id": "Prosedur Melihat Bagian Dalam Rahim." },
  { "en": "Apa Yang Dimaksud Dengan HDL (High-Density Lipoprotein)?", "id": "Kolesterol Baik, Membersihkan Arteri, Sehat." },
  { "en": "Apa Itu Penyakit Virus Zika Nyamuk?", "id": "Virus Disebarkan Nyamuk Aedes Aegypti." },
  { "en": "Apa Fungsi Dari Sistem Saraf Parasimpatik?", "id": "Memicu Respon Istirahat Dan Cerna." },
  { "en": "Apa Itu Psikoanalisis Dalam Dunia Psikologi?", "id": "Teori, Terapi Mengungkapkan Pikiran Bawah." },
  { "en": "Apa Yang Dimaksud Dengan Prognosis Penyakit?", "id": "Prediksi Kemungkinan Perkembangan Suatu Penyakit." },
  { "en": "Apa Itu Penyakit Vitiligo Pada Kulit?", "id": "Kulit Kehilangan Pigmen, Bercak Putih." },
  { "en": "Apa Fungsi Dari Arkus Aorta Jantung?", "id": "Bagian Lengkung Aorta, Mendistribusikan Darah." },
  { "en": "Apa Itu EMG (Elektromiografi) Pemeriksaan Otot?", "id": "Tes Mendiagnosis Gangguan Otot, Saraf." },
  { "en": "Apa Itu PET Scan (Positron Emission Tomography)?", "id": "Tes Pencitraan Melihat Fungsi Jaringan." },
  { "en": "Apa Fungsi Dari Hormon Tiroksin (T4)?", "id": "Mengatur Metabolisme, Pertumbuhan, Suhu Tubuh." },
  { "en": "Apa Yang Dimaksud Dengan Genotipe Genetik?", "id": "Susunan Genetik Lengkap Suatu Organisme." },
  { "en": "Apa Itu Penyakit Keracunan Makanan Akut?", "id": "Penyakit Akibat Konsumsi Makanan Terkontaminasi." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Ginjal?", "id": "Dokter Spesialis Nefrologi Atau Nefrolog." },
  { "en": "Apa Yang Dimaksud Dengan Otot Diafragma?", "id": "Otot Pernapasan Utama, Pemisah Dada." },
  { "en": "Apa Itu Ablasi Dalam Prosedur Medis?", "id": "Penghancuran Jaringan Tubuh Menggunakan Energi." },
  { "en": "Apa Fungsi Dari Lobus Serebelum Otak?", "id": "Mengkoordinasikan Gerakan Halus, Keseimbangan, Postur." },
  { "en": "Apa Itu Penyakit Sindrom Iritasi Usus (IBS)?", "id": "Gangguan Usus Besar, Nyeri Perut." },
  { "en": "Apa Itu Obat Antitusif Untuk Batuk?", "id": "Obat Penekan Refleks Batuk Kering." },
  { "en": "Apa Yang Dimaksud Dengan Fenotipe Genetik?", "id": "Karakteristik Fisik Teramati Suatu Organisme." },
  { "en": "Apa Itu Penyakit Botulisme Dari Bakteri?", "id": "Keracunan Serius Akibat Toksin Bakteri." },
  { "en": "Sebutkan Spesialisasi Dokter Saluran Cerna?", "id": "Dokter Spesialis Gastroenterologi Atau Gastroenterolog." },
  { "en": "Apa Fungsi Dari Septum Nasi Hidung?", "id": "Dinding Pemisah Rongga Hidung Kiri." },
  { "en": "Apa Itu Amputasi Dalam Tindakan Bedah?", "id": "Pemotongan Sebagian Atau Seluruh Anggota." },
  { "en": "Apa Yang Dimaksud Dengan Alel Gen?", "id": "Bentuk Alternatif Dari Suatu Gen." },
  { "en": "Apa Itu Penyakit Listeriosis Dari Makanan?", "id": "Infeksi Bakteri Listeria, Makanan Tercemar." },
  { "en": "Apa Fungsi Dari Kelenjar Timus Manusia?", "id": "Mematangkan Sel Limfosit T Imunitas." },
  { "en": "Apa Itu Trakeostomi Prosedur Saluran Napas?", "id": "Pembuatan Lubang Di Trakea Bantuan." },
  { "en": "Apa Yang Dimaksud Dengan Gen Dominan?", "id": "Alel Mengekspresikan Sifatnya Secara Penuh." },
  { "en": "Apa Itu Penyakit Brucellosis Dari Hewan?", "id": "Infeksi Bakteri Hewan Ternak Manusia." },
  { "en": "Apa Fungsi Dari Otot Interkostal Dada?", "id": "Membantu Menggerakkan Dinding Dada Bernapas." },
  { "en": "Apa Itu Transplantasi Organ Dalam Bedah?", "id": "Pemindahan Organ Dari Satu Individu." },
  { "en": "Apa Yang Dimaksud Dengan Gen Resesif?", "id": "Alel Hanya Terlihat Saat Dominan." },
  { "en": "Apa Itu Penyakit Demam Tifoid Salmonella?", "id": "Infeksi Bakteri Salmonella Typhi Menular." },
  { "en": "Apa Fungsi Dari Sklera Mata Manusia?", "id": "Bagian Putih, Melindungi Struktur Dalam." },
  { "en": "Apa Itu Gastrektomi Atau Operasi Lambung?", "id": "Pengangkatan Sebagian Atau Seluruh Organ." },
  { "en": "Apa Yang Dimaksud Dengan Mutasi Genetik?", "id": "Perubahan Permanen Pada Urutan DNA." },
  { "en": "Apa Itu Penyakit Shigellosis Infeksi Bakteri?", "id": "Infeksi Usus, Menyebabkan Diare Parah." },
  { "en": "Apa Fungsi Dari Koroid Lapisan Mata?", "id": "Memasok Darah Dan Nutrisi Retina." },
  { "en": "Apa Itu Mastektomi Atau Operasi Payudara?", "id": "Pengangkatan Seluruh Jaringan Payudara Kanker." },
  { "en": "Apa Yang Dimaksud Dengan Individu Homozigot?", "id": "Memiliki Dua Alel Identik Suatu." },
  { "en": "Apa Itu Penyakit Vibrio Vulnificus Bakteri?", "id": "Infeksi Bakteri Air Laut Tercemar." },
  { "en": "Apa Fungsi Dari Badan Vitreous Mata?", "id": "Zat Mirip Jeli, Mengisi Bola." },
  { "en": "Apa Itu Histerektomi Atau Operasi Rahim?", "id": "Pengangkatan Rahim Atau Uterus Wanita." },
  { "en": "Apa Yang Dimaksud Dengan Individu Heterozigot?", "id": "Memiliki Dua Alel Berbeda Suatu." },
  { "en": "Apa Itu Penyakit Giardiasis Dari Parasit?", "id": "Infeksi Usus Halus, Parasit Giardia." },
  { "en": "Apa Fungsi Dari Bintik Buta Mata?", "id": "Area Retina Tanpa Fotoreseptor Sensitif." },
  { "en": "Apa Itu Nefrektomi Atau Operasi Ginjal?", "id": "Pengangkatan Satu Atau Kedua Organ." },
  { "en": "Apa Yang Dimaksud Dengan Pewarisan Mendel?", "id": "Prinsip Dasar Pewarisan Sifat Genetik." },
  { "en": "Apa Itu Penyakit Kriptosporidiosis Parasit Usus?", "id": "Penyakit Diare Akibat Parasit Cryptosporidium." },
  { "en": "Apa Fungsi Dari Fovea Retina Mata?", "id": "Area Penglihatan Paling Tajam, Detail." },
  { "en": "Apa Itu Splenektomi Atau Operasi Limpa?", "id": "Pengangkatan Organ Limpa Secara Bedah." },
  { "en": "Apa Yang Dimaksud Dengan Genetika Populasi?", "id": "Studi Distribusi Variasi Genetik Populasi." },
  { "en": "Apa Itu Penyakit Amubiasis Akibat Entamoeba?", "id": "Infeksi Usus Besar Parasit Entamoeba." },
  { "en": "Apa Fungsi Dari Otot Siliaris Mata?", "id": "Mengubah Bentuk Lensa Untuk Fokus." },
  { "en": "Apa Itu Prostatektomi Atau Operasi Prostat?", "id": "Pengangkatan Kelenjar Prostat Pada Pria." },
  { "en": "Apa Yang Dimaksud Dengan Rekayasa Genetika?", "id": "Manipulasi Langsung Gen Suatu Organisme." },
  { "en": "Apa Itu Penyakit Balantidiasis Parasit Usus?", "id": "Infeksi Usus Besar Langka Parasit." },
  { "en": "Apa Fungsi Dari Sel Kerucut Retina?", "id": "Mendeteksi Warna, Penglihatan Siang Hari." },
  { "en": "Apa Itu Kolesistektomi Atau Operasi Kantung Empedu?", "id": "Pengangkatan Kantung Empedu Secara Bedah." },
  { "en": "Apa Yang Dimaksud Dengan Terapi Gen?", "id": "Teknik Memperbaiki Gen Cacat Penyebab." },
  { "en": "Apa Itu Penyakit Askariasis Atau Cacing Gelang?", "id": "Infeksi Cacing Gelang Ascaris Lumbricoides." },
  { "en": "Apa Fungsi Dari Sel Batang Retina?", "id": "Mendeteksi Cahaya Redup, Penglihatan Malam." },
  { "en": "Apa Itu Apendektomi Atau Operasi Usus Buntu?", "id": "Pengangkatan Usus Buntu Yang Meradang." },
  { "en": "Apa Yang Dimaksud Dengan Kloning Biologi?", "id": "Proses Menciptakan Salinan Identik Genetik." },
  { "en": "Apa Itu Penyakit Sistiserkosis Cacing Pita?", "id": "Infeksi Jaringan Cacing Pita Babi." },
  { "en": "Apa Fungsi Dari Saluran Air Mata?", "id": "Mengalirkan Air Mata Dari Mata." },
  { "en": "Apa Itu Laminektomi Atau Operasi Tulang Belakang?", "id": "Mengangkat Bagian Tulang Belakang (Lamina)." },
  { "en": "Apa Yang Dimaksud Dengan Toksikologi Medis?", "id": "Ilmu Tentang Racun, Efek, Pengobatannya." },
  { "en": "Apa Itu Penyakit Trichuriasis Atau Cacing Cambuk?", "id": "Infeksi Usus Besar Cacing Cambuk." },
  { "en": "Apa Fungsi Dari Gigi Seri Manusia?", "id": "Untuk Memotong Makanan Dengan Mudah." },
  { "en": "Apa Itu Kraniotomi Atau Operasi Tempurung Kepala?", "id": "Pembedahan Membuka Tempurung Kepala Otak." },
  { "en": "Apa Yang Dimaksud Dengan Toksin Atau Racun?", "id": "Zat Beracun Dihasilkan Organisme Hidup." },
  { "en": "Apa Itu Penyakit Enterobiasis Atau Cacing Kremi?", "id": "Infeksi Cacing Kremi Umum Anak-Anak." },
  { "en": "Apa Fungsi Dari Gigi Taring Manusia?", "id": "Untuk Merobek Makanan Dengan Kuat." },
  { "en": "Apa Itu Torakotomi Atau Operasi Dinding Dada?", "id": "Sayatan Bedah Dinding Dada Manusia." },
  { "en": "Apa Yang Dimaksud Dengan Overdosis Obat?", "id": "Konsumsi Obat Melebihi Dosis Dianjurkan." },
  { "en": "Apa Itu Penyakit Ankilostomiasis Cacing Tambang?", "id": "Infeksi Usus Halus Cacing Tambang." },
  { "en": "Apa Fungsi Dari Gigi Premolar Manusia?", "id": "Menghancurkan, Menggiling Makanan Jadi Kecil." },
  { "en": "Apa Itu Tiroidektomi Atau Operasi Kelenjar Tiroid?", "id": "Pengangkatan Sebagian Atau Seluruh Kelenjar." },
  { "en": "Apa Yang Dimaksud Dengan Antidot Penawar Racun?", "id": "Zat Dapat Melawan Efek Racun." },
  { "en": "Apa Itu Penyakit Strongiloidiasis Cacing Benang?", "id": "Infeksi Cacing Benang Parasit Manusia." },
  { "en": "Apa Fungsi Dari Email Gigi Manusia?", "id": "Lapisan Terluar Keras, Melindungi Gigi." },
  { "en": "Apa Itu Lobektomi Atau Operasi Paru-Paru?", "id": "Pengangkatan Satu Lobus Paru-Paru Manusia." },
  { "en": "Apa Yang Dimaksud Dengan Bisa Hewan?", "id": "Cairan Beracun Disuntikkan Hewan Tertentu." },
  { "en": "Apa Itu Penyakit Toksokariasis Cacing Gelang?", "id": "Infeksi Larva Cacing Gelang Anjing." },
  { "en": "Apa Fungsi Dari Dentin Lapisan Gigi?", "id": "Jaringan Keras Di Bawah Email." },
  { "en": "Apa Itu Bypass Jantung (CABG) Operasi?", "id": "Membuat Jalur Baru Aliran Darah." },
  { "en": "Apa Yang Dimaksud Dengan Logam Berat Beracun?", "id": "Logam Kepadatan Tinggi, Beracun Lingkungan." },
  { "en": "Apa Itu Penyakit Dicrocoeliasis Cacing Hati?", "id": "Infeksi Cacing Pipih Hati Ruminansia." },
  { "en": "Apa Fungsi Dari Pulpa Gigi Manusia?", "id": "Jaringan Lunak, Berisi Saraf, Pembuluh." },
  { "en": "Apa Itu Angioplasti Prosedur Pembuluh Darah?", "id": "Melebarkan Pembuluh Darah Yang Tersumbat." },
  { "en": "Apa Itu Sianida Zat Beracun Kimia?", "id": "Zat Kimia Beracun, Bekerja Cepat." },
  { "en": "Apa Itu Penyakit Fascioliasis Cacing Hati?", "id": "Infeksi Cacing Hati Ternak Manusia." },
  { "en": "Apa Fungsi Dari Sementum Akar Gigi?", "id": "Melapisi Akar Gigi, Menghubungkan Tulang." },
  { "en": "Apa Itu Pemasangan Stent Pada Jantung?", "id": "Memasang Tabung Jala Menjaga Arteri." },
  { "en": "Apa Yang Dimaksud Dengan Pestisida Berbahaya?", "id": "Zat Kimia Mengendalikan Hama Tanaman." },
  { "en": "Apa Itu Penyakit Opisthorchiasis Cacing Hati?", "id": "Infeksi Cacing Hati Asia Tenggara." },
  { "en": "Apa Fungsi Dari Ligamen Periodontal Gigi?", "id": "Menahan Gigi Tetap Dalam Soketnya." },
  { "en": "Apa Itu Hemoroidektomi Atau Operasi Wasir?", "id": "Pengangkatan Wasir Atau Ambeien Meradang." },
  { "en": "Apa Yang Dimaksud Dengan Farmakokinetik Obat?", "id": "Studi Perjalanan Obat Dalam Tubuh." },
  { "en": "Apa Arti Awalan 'Hipo-' Dalam Medis?", "id": "Berarti Rendah, Di Bawah Normal." },
  { "en": "Apa Itu Jaringan Epitel Skuamosa Selapis?", "id": "Satu Lapis Sel Pipih Tipis." },
  { "en": "Apa Itu Penyakit Tetanus Neonatorum Bayi?", "id": "Tetanus Terjadi Pada Bayi Baru." },
  { "en": "Sebutkan Spesialisasi Dokter Bedah Saraf?", "id": "Dokter Spesialis Bedah Saraf Terlatih." },
  { "en": "Apa Itu Tindakan Herniorafi Operasi Hernia?", "id": "Prosedur Bedah Memperbaiki Penyakit Hernia." },
  { "en": "Apa Yang Dimaksud Dengan Absorpsi Obat?", "id": "Proses Obat Masuk Ke Sirkulasi." },
  { "en": "Apa Arti Akhiran '-Itis' Dalam Medis?", "id": "Menandakan Adanya Proses Peradangan (Inflamasi)." },
  { "en": "Apa Itu Jaringan Epitel Kuboid Selapis?", "id": "Satu Lapis Sel Berbentuk Kubus." },
  { "en": "Apa Itu Penyakit Pertusis Atau Batuk Rejan?", "id": "Infeksi Bakteri Saluran Napas Menular." },
  { "en": "Apa Fungsi Dari Sumsum Merah Tulang?", "id": "Tempat Produksi Sel-Sel Darah." },
  { "en": "Apa Itu Perikarditis Atau Radang Selaput Jantung?", "id": "Peradangan Pada Lapisan Tipis Perikardium." },
  { "en": "Apa Yang Dimaksud Dengan Distribusi Obat?", "id": "Proses Obat Menyebar Ke Seluruh." },
  { "en": "Apa Arti Awalan 'Hiper-' Dalam Medis?", "id": "Berarti Tinggi, Di Atas Normal." },
  { "en": "Apa Itu Jaringan Epitel Silindris Selapis?", "id": "Satu Lapis Sel Berbentuk Silinder." },
  { "en": "Apa Itu Penyakit Difteri Infeksi Bakteri?", "id": "Infeksi Serius Selaput Lendir Hidung." },
  { "en": "Apa Fungsi Dari Sumsum Kuning Tulang?", "id": "Menyimpan Cadangan Lemak Untuk Energi." },
  { "en": "Apa Itu Miokarditis Atau Radang Otot Jantung?", "id": "Peradangan Pada Lapisan Tengah Dinding." },
  { "en": "Apa Yang Dimaksud Dengan Metabolisme Obat?", "id": "Proses Tubuh Mengubah Obat Kimiawi." },
  { "en": "Apa Arti Akhiran '-Oma' Dalam Medis?", "id": "Menandakan Adanya Tumor Atau Benjolan." },
  { "en": "Apa Itu Jaringan Epitel Transisional Manusia?", "id": "Jaringan Berlapis, Dapat Meregang Kembali." },
  { "en": "Apa Itu Penyakit Rubela Atau Campak Jerman?", "id": "Infeksi Virus, Ruam Merah Muda." },
  { "en": "Apa Fungsi Dari Tulang Rawan Hialin?", "id": "Memberi Bentuk, Menopang, Fleksibilitas Sendi." },
  { "en": "Apa Itu Endokarditis Atau Radang Lapisan Jantung?", "id": "Peradangan Lapisan Dalam Ruang Jantung." },
  { "en": "Apa Yang Dimaksud Dengan Ekskresi Obat?", "id": "Proses Pengeluaran Obat Dari Tubuh." },
  { "en": "Apa Arti Awalan 'Bradikardi' Dalam Medis?", "id": "Berarti Lambat, Terutama Denyut Jantung." },
  { "en": "Apa Itu Jaringan Ikat Longgar Manusia?", "id": "Mengisi Ruang Antar Organ, Bantalan." },
  { "en": "Apa Itu Penyakit Gondongan Atau Mumps Virus?", "id": "Infeksi Virus, Pembengkakan Kelenjar Parotis." },
  { "en": "Apa Fungsi Tulang Rawan Elastis Manusia?", "id": "Memberi Fleksibilitas, Contoh Daun Telinga." },
  { "en": "Apa Itu Fibrilasi Atrium Gangguan Irama?", "id": "Irama Jantung Tidak Teratur, Cepat." },
  { "en": "Apa Yang Dimaksud Dengan Waktu Paruh Obat?", "id": "Waktu Diperlukan Setengah Obat Dieliminasi." },
  { "en": "Apa Arti Awalan 'Takikardi' Dalam Medis?", "id": "Berarti Cepat, Terutama Denyut Jantung." },
  { "en": "Apa Itu Jaringan Ikat Padat Manusia?", "id": "Jaringan Kuat, Membentuk Tendon, Ligamen." },
  { "en": "Apa Itu Penyakit Demam Reumatik Autoimun?", "id": "Komplikasi Radang Tenggorokan, Jantung, Sendi." },
  { "en": "Apa Fungsi Tulang Rawan Fibrosa Manusia?", "id": "Tahan Tekanan, Contoh Cakram Tulang." },
  { "en": "Apa Itu Fibrilasi Ventrikel Gangguan Irama?", "id": "Irama Jantung Kacau, Mengancam Nyawa." },
  { "en": "Apa Yang Dimaksud Dengan Farmakodinamik Obat?", "id": "Studi Efek Obat Pada Tubuh." },
  { "en": "Apa Arti Akhiran '-Pati' Dalam Medis?", "id": "Menandakan Adanya Suatu Penyakit (Patologi)." },
  { "en": "Apa Itu Jaringan Adiposa Atau Jaringan Lemak?", "id": "Menyimpan Energi, Isolator, Bantal Pelindung." },
  { "en": "Apa Itu Penyakit Kawasaki Pada Anak?", "id": "Peradangan Pembuluh Darah Seluruh Tubuh." },
  { "en": "Apa Itu Osteosit Atau Sel Tulang?", "id": "Sel Utama Dalam Jaringan Tulang." },
  { "en": "Apa Itu Penyakit Buerger Pada Perokok?", "id": "Peradangan, Pembengkakan Pembuluh Darah Tangan." },
  { "en": "Apa Yang Dimaksud Dengan Reseptor Obat?", "id": "Target Protein Tempat Obat Berikatan." },
  { "en": "Apa Arti Awalan 'Dis-' Dalam Medis?", "id": "Berarti Sulit, Sakit, Atau Abnormal." },
  { "en": "Apa Itu Jaringan Darah Sebagai Jaringan?", "id": "Jaringan Ikat Cair, Transportasi Nutrisi." },
  { "en": "Apa Itu Penyakit Tangan, Kaki, Dan Mulut?", "id": "Infeksi Virus, Luka Mulut, Ruam." },
  { "en": "Apa Itu Kondrosit Atau Sel Tulang Rawan?", "id": "Satu-Satunya Sel Tulang Rawan." },
  { "en": "Apa Itu Penyakit Raynaud Jari Tangan?", "id": "Pembuluh Darah Menyempit, Aliran Terbatas." },
  { "en": "Apa Yang Dimaksud Dengan Agonis Obat?", "id": "Obat Mengaktifkan Reseptor, Menghasilkan Respons." },
  { "en": "Apa Arti Akhiran '-Algi' Dalam Medis?", "id": "Menandakan Rasa Nyeri Atau Sakit." },
  { "en": "Apa Itu Jaringan Otot Jantung Involunter?", "id": "Otot Hanya Ditemukan Di Jantung." },
  { "en": "Apa Itu Penyakit Cacingan Kremi Enterobiasis?", "id": "Infeksi Cacing Kremi Paling Umum." },
  { "en": "Apa Itu Sel Darah Merah Eritrosit?", "id": "Membawa Oksigen Dari Paru-Paru Jaringan." },
  { "en": "Apa Itu Trombosis Vena Dalam (DVT)?", "id": "Gumpalan Darah Vena Bagian Dalam." },
  { "en": "Apa Yang Dimaksud Dengan Antagonis Obat?", "id": "Obat Menghalangi Aksi Agonis Reseptor." },
  { "en": "Apa Arti Awalan 'Poli-' Dalam Medis?", "id": "Berarti Banyak, Berlebihan, Atau Sering." },
  { "en": "Apa Itu Jaringan Otot Lurik Volunter?", "id": "Otot Melekat Pada Tulang, Sadar." },
  { "en": "Apa Itu Penyakit Kelima (Erythema Infectiosum)?", "id": "Infeksi Virus, Ruam Merah Pipi." },
  { "en": "Apa Itu Sel Darah Putih Leukosit?", "id": "Bagian Sistem Kekebalan, Melawan Infeksi." },
  { "en": "Apa Itu Emboli Paru Gumpalan Darah?", "id": "Penyumbatan Arteri Paru-Paru Gumpalan Darah." },
  { "en": "Apa Yang Dimaksud Dengan Bioavailabilitas Obat?", "id": "Fraksi Obat Mencapai Sirkulasi Sistemik." },
  { "en": "Apa Arti Akhiran '-Plegi' Dalam Medis?", "id": "Menandakan Kelumpuhan Atau Paralisis Total." },
  { "en": "Apa Itu Jaringan Otot Polos Involunter?", "id": "Otot Dinding Organ Berongga, Lambung." },
  { "en": "Apa Itu Penyakit Roseola Infantum Bayi?", "id": "Infeksi Virus, Demam Tinggi, Ruam." },
  { "en": "Apa Itu Keping Darah Atau Trombosit?", "id": "Fragmen Sel, Membantu Pembekuan Darah." },
  { "en": "Apa Itu Vaskulitis Atau Radang Pembuluh Darah?", "id": "Peradangan Pada Dinding Pembuluh Darah." },
  { "en": "Apa Yang Dimaksud Dengan Efek Samping Obat?", "id": "Efek Tidak Diinginkan Dari Pengobatan." },
  { "en": "Apa Arti Awalan 'Oligo-' Dalam Medis?", "id": "Berarti Sedikit Atau Kekurangan Sesuatu." },
  { "en": "Apa Itu Sel Saraf Atau Neuron?", "id": "Unit Dasar Sistem Saraf Manusia." },
  { "en": "Apa Itu Sindrom Kelelahan Kronis (CFS)?", "id": "Kelelahan Ekstrem, Tidak Membaik Istirahat." },
  { "en": "Apa Itu Plasma Darah Cairan Kuning?", "id": "Komponen Cair Darah, Mengangkut Sel." },
  { "en": "Apa Itu Aneurisma Otak Pembuluh Darah?", "id": "Tonjolan Lemah Dinding Pembuluh Darah." },
  { "en": "Apa Yang Dimaksud Dengan Interaksi Obat?", "id": "Efek Obat Berubah Obat Lain." },
  { "en": "Apa Arti Akhiran '-Skopi' Dalam Medis?", "id": "Pemeriksaan Visual Menggunakan Alat (Skop)." },
  { "en": "Apa Itu Sel Glial Sistem Saraf?", "id": "Sel Penunjang, Pelindung, Pemberi Nutrisi." },
  { "en": "Apa Itu Penyakit Infeksi Saluran Kemih (ISK)?", "id": "Infeksi Pada Bagian Sistem Perkemihan." },
  { "en": "Apa Itu Hemoglobin Protein Sel Darah?", "id": "Protein Mengikat Oksigen Sel Darah." },
  { "en": "Apa Itu Penyakit Arteri Perifer (PAD)?", "id": "Penyempitan Arteri, Mengurangi Aliran Darah." },
  { "en": "Apa Yang Dimaksud Dengan Dosis Terapeutik?", "id": "Jumlah Obat Memberi Efek Diinginkan." },
  { "en": "Apa Arti Awalan 'Endo-' Dalam Medis?", "id": "Berarti Di Dalam Atau Internal." },
  { "en": "Apa Itu Gastrulasi Dalam Perkembangan Embrio?", "id": "Pembentukan Tiga Lapisan Germinal Primer." },
  { "en": "Apa Itu Pielonefritis Atau Infeksi Ginjal?", "id": "Jenis ISK, Peradangan Jaringan Ginjal." },
  { "en": "Apa Itu Hematokrit Dalam Tes Darah?", "id": "Persentase Volume Sel Darah Merah." },
  { "en": "Apa Itu Limfedema Atau Pembengkakan Lengan?", "id": "Penumpukan Cairan Limfe Jaringan Lunak." },
  { "en": "Apa Yang Dimaksud Dengan Dosis Letal?", "id": "Dosis Obat Yang Menyebabkan Kematian." },
  { "en": "Apa Arti Akhiran '-Ektomi' Dalam Medis?", "id": "Tindakan Bedah Pengangkatan Atau Pemotongan." },
  { "en": "Apa Itu Blastula Dalam Perkembangan Embrio?", "id": "Bola Berongga Sel Setelah Pembelahan." },
  { "en": "Apa Itu Sistitis Atau Infeksi Kandung Kemih?", "id": "Peradangan Dinding Kandung Kemih Bakteri." },
  { "en": "Apa Itu Fibrinogen Protein Plasma Darah?", "id": "Protein Penting Proses Pembekuan Darah." },
  { "en": "Apa Itu Varises Atau Pembengkakan Vena?", "id": "Vena Bengkak, Berkelok, Terlihat Biru." },
  { "en": "Apa Yang Dimaksud Dengan Indeks Terapeutik?", "id": "Ukuran Keamanan Suatu Obat Tertentu." },
  { "en": "Apa Arti Awalan 'Peri-' Dalam Medis?", "id": "Berarti Di Sekitar Atau Mengelilingi." },
  { "en": "Apa Itu Neurulasi Dalam Perkembangan Embrio?", "id": "Pembentukan Bumbung Saraf, Cikal Bakal." },
  { "en": "Apa Itu Uretritis Atau Radang Uretra?", "id": "Peradangan Uretra, Saluran Keluar Urin." },
  { "en": "Apa Yang Dimaksud Dengan Teratogen Medis?", "id": "Zat Menyebabkan Cacat Lahir Janin." },
  { "en": "Apa Arti Akhiran '-Stomi' Dalam Medis?", "id": "Pembuatan Lubang Buatan Secara Bedah." },
  { "en": "Apa Itu Organogenesis Dalam Perkembangan Embrio?", "id": "Proses Pembentukan Organ-Organ Tubuh." },
  { "en": "Apa Itu Urosepsis Infeksi Darah Parah?", "id": "Sepsis Disebabkan Oleh Infeksi Saluran." },
  { "en": "Apa Itu Serum Darah Komponen Cair?", "id": "Plasma Darah Tanpa Faktor Pembekuan." },
  { "en": "Apa Itu Syok Kardiogenik Akibat Jantung?", "id": "Tubuh Syok, Jantung Gagal Memompa." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Darah?", "id": "Dokter Spesialis Hematologi Atau Hematolog." },
  { "en": "Apa Arti Awalan 'Neo-' Dalam Medis?", "id": "Berarti Baru, Terutama Bayi Baru." },
  { "en": "Apa Itu Sel Punca Embrionik Pluripoten?", "id": "Sel Berasal Embrio, Bisa Diferensiasi." },
  { "en": "Apa Itu Prostatitis Atau Radang Kelenjar Prostat?", "id": "Pembengkakan, Peradangan Kelenjar Prostat Pria." },
  { "en": "Apa Itu Apoptosis Atau Kematian Sel?", "id": "Proses Kematian Sel Terprogram Normal." },
  { "en": "Apa Itu Syok Hipovolemik Akibat Kehilangan Cairan?", "id": "Kondisi Kehilangan Banyak Darah, Cairan." },
  { "en": "Apa Yang Dimaksud Dengan Plasebo Efek?", "id": "Efek Positif Dari Perawatan Palsu." },
  { "en": "Apa Arti Akhiran '-Grafi' Dalam Medis?", "id": "Proses Merekam Atau Pencitraan Sesuatu." },
  { "en": "Apa Itu Lapisan Ektoderm Pada Embrio?", "id": "Lapisan Germinal Luar, Membentuk Kulit." },
  { "en": "Apa Itu Epididimitis Radang Saluran Sperma?", "id": "Peradangan Saluran Belakang Testis Pria." },
  { "en": "Apa Itu Nekrosis Atau Kematian Jaringan?", "id": "Kematian Sel Akibat Cedera, Penyakit." },
  { "en": "Apa Itu Syok Anafilaktik Akibat Alergi?", "id": "Reaksi Alergi Serius, Mengancam Nyawa." },
  { "en": "Apa Yang Dimaksud Dengan Nocebo Efek?", "id": "Efek Negatif Dari Perawatan Palsu." },
  { "en": "Apa Arti Awalan 'Para-' Dalam Medis?", "id": "Berarti Di Samping, Dekat, Abnormal." },
  { "en": "Apa Itu Lapisan Mesoderm Pada Embrio?", "id": "Lapisan Germinal Tengah, Membentuk Otot." },
  { "en": "Apa Itu Orkitis Atau Radang Testis?", "id": "Peradangan Satu Atau Kedua Testis." },
  { "en": "Apa Itu Atrofi Pengecilan Ukuran Jaringan?", "id": "Penyusutan Jaringan Akibat Hilangnya Sel." },
  { "en": "Apa Itu Syok Septik Akibat Infeksi?", "id": "Tekanan Darah Turun Akibat Infeksi." },
  { "en": "Sebutkan Spesialisasi Dokter Kesehatan Jiwa?", "id": "Dokter Spesialis Psikiatri Atau Psikiater." },
  { "en": "Apa Arti Akhiran '-Gram' Dalam Medis?", "id": "Hasil Rekaman Atau Gambar Pencitraan." },
  { "en": "Apa Itu Lapisan Endoderm Pada Embrio?", "id": "Lapisan Germinal Dalam, Membentuk Pencernaan." },
  { "en": "Apa Itu Striktur Uretra Penyempitan Saluran?", "id": "Penyempitan Uretra, Menghalangi Aliran Urin." },
  { "en": "Apa Itu Hipertrofi Pembesaran Ukuran Sel?", "id": "Peningkatan Ukuran Sel, Jaringan Membesar." },
  { "en": "Apa Itu Syok Neurogenik Akibat Kerusakan Saraf?", "id": "Disebabkan Kerusakan Sistem Saraf Pusat." },
  { "en": "Apa Itu Kekebalan Aktif Alami Tubuh?", "id": "Kekebalan Didapat Setelah Terinfeksi Penyakit." },
  { "en": "Apa Arti Awalan 'Trans-' Dalam Medis?", "id": "Berarti Melintasi, Melewati, Atau Menembus." },
  { "en": "Apa Itu Tali Pusat Penghubung Janin?", "id": "Menghubungkan Janin Dengan Plasenta Ibu." },
  { "en": "Apa Itu Batu Kandung Kemih Manusia?", "id": "Massa Keras Mineral Di Kandung." },
  { "en": "Apa Itu Hiperplasia Penambahan Jumlah Sel?", "id": "Peningkatan Jumlah Sel Dalam Jaringan." },
  { "en": "Apa Itu Pingsan Atau Sinkop Medis?", "id": "Kehilangan Kesadaran Sementara Aliran Darah." },
  { "en": "Apa Itu Kekebalan Pasif Alami Tubuh?", "id": "Kekebalan Bayi Dari Ibu (ASI)." },
  { "en": "Apa Arti Akhiran '-Lisis' Dalam Medis?", "id": "Berarti Kerusakan, Kehancuran, Atau Pemecahan." },
  { "en": "Apa Itu Kantung Kuning Telur Embrio?", "id": "Membran Pemberi Nutrisi Awal Embrio." },
  { "en": "Apa Itu Retensi Urin Tidak Bisa Kencing?", "id": "Ketidakmampuan Mengosongkan Kandung Kemih Sepenuhnya." },
  { "en": "Apa Itu Metaplasia Perubahan Jenis Sel?", "id": "Perubahan Satu Jenis Sel Dewasa." },
  { "en": "Apa Itu Luka Bakar Derajat Pertama?", "id": "Kerusakan Lapisan Luar Kulit (Epidermis)." },
  { "en": "Apa Itu Kekebalan Aktif Buatan Manusia?", "id": "Kekebalan Didapat Melalui Proses Vaksinasi." },
  { "en": "Apa Arti Awalan 'Sub-' Dalam Medis?", "id": "Berarti Di Bawah Atau Kurang." },
  { "en": "Apa Itu Cairan Ketuban Atau Amnion?", "id": "Cairan Pelindung Mengelilingi Janin Rahim." },
  { "en": "Apa Itu Inkontinensia Urin Atau Mengompol?", "id": "Kehilangan Kontrol Kandung Kemih, Bocor." },
  { "en": "Apa Itu Displasia Pertumbuhan Sel Abnormal?", "id": "Perkembangan Sel Abnormal, Potensi Kanker." },
  { "en": "Apa Itu Luka Bakar Derajat Kedua?", "id": "Kerusakan Epidermis Dan Sebagian Dermis." },
  { "en": "Apa Itu Kekebalan Pasif Buatan Manusia?", "id": "Kekebalan Suntikan Antibodi Siap Pakai." },
  { "en": "Apa Arti Akhiran '-Malasia' Dalam Medis?", "id": "Menandakan Pelunakan Jaringan Atau Organ." },
  { "en": "Apa Itu Periode Gestasi Atau Masa Kehamilan?", "id": "Lama Waktu Perkembangan Janin Kandungan." },
  { "en": "Apa Itu Nokturia Atau Sering Kencing Malam?", "id": "Kebutuhan Bangun Malam Hari Kencing." },
  { "en": "Apa Itu Neoplasia Pertumbuhan Jaringan Baru?", "id": "Pertumbuhan Baru Abnormal, Tumor Ganas." },
  { "en": "Apa Itu Luka Bakar Derajat Ketiga?", "id": "Kerusakan Seluruh Lapisan Kulit Manusia." },
  { "en": "Apa Yang Dimaksud Dengan Sel Memori?", "id": "Sel Limfosit, Mengingat Infeksi Sebelumnya." },
  { "en": "Apa Arti Awalan 'Supra-' Dalam Medis?", "id": "Berarti Di Atas Atau Melebihi." },
  { "en": "Apa Itu Trimester Pertama Masa Kehamilan?", "id": "Tiga Bulan Pertama Masa Kehamilan." },
  { "en": "Apa Itu Hematuria Atau Kencing Berdarah?", "id": "Adanya Sel Darah Merah Urin." },
  { "en": "Apa Itu Tumor Jinak Atau Benign?", "id": "Tumor Tidak Bersifat Kanker, Menyebar." },
  { "en": "Apa Itu Luka Bakar Derajat Keempat?", "id": "Kerusakan Meluas Hingga Otot, Tulang." },
  { "en": "Apa Yang Dimaksud Dengan Sistem Komplemen?", "id": "Bagian Sistem Imun, Meningkatkan Kemampuan." },
  { "en": "Apa Arti Akhiran '-Pen' Dalam Medis?", "id": "Menandakan Kekurangan Atau Penurunan Jumlah." },
  { "en": "Apa Itu Trimester Kedua Masa Kehamilan?", "id": "Bulan Keempat Hingga Keenam Kehamilan." },
  { "en": "Apa Itu Poliuria Produksi Urin Berlebih?", "id": "Produksi Urin Dalam Jumlah Besar." },
  { "en": "Apa Itu Tumor Ganas Atau Malignan?", "id": "Tumor Bersifat Kanker, Menyerang Jaringan." },
  { "en": "Apa Itu Serangan Jantung (Infark Miokard)?", "id": "Kematian Otot Jantung, Aliran Darah." },
  { "en": "Apa Yang Dimaksud Dengan Sitokin Imunologi?", "id": "Protein Pengatur Komunikasi Antar Sel." },
  { "en": "Apa Arti Awalan 'Intra-' Dalam Medis?", "id": "Berarti Di Dalam Atau Selama." },
  { "en": "Apa Itu Trimester Ketiga Masa Kehamilan?", "id": "Tiga Bulan Terakhir Masa Kehamilan." },
  { "en": "Apa Itu Disuria Atau Nyeri Saat Kencing?", "id": "Rasa Sakit, Tidak Nyaman, Terbakar." },
  { "en": "Apa Itu Karsinoma Jenis Kanker Umum?", "id": "Kanker Berasal Dari Jaringan Epitel." },
  { "en": "Apa Itu Henti Jantung Mendadak (SCA)?", "id": "Jantung Berhenti Berdetak Tiba-Tiba Efektif." },
  { "en": "Apa Itu Limfosit T Dalam Imunitas?", "id": "Sel Darah Putih, Peran Imunitas." },
  { "en": "Apa Arti Akhiran '-Ptosis' Dalam Medis?", "id": "Penurunan Atau Terkulainya Suatu Organ." },
  { "en": "Kapan Perkembangan Janin Dianggap Cukup Bulan?", "id": "Antara Minggu Ke Tiga Puluh Sembilan." },
  { "en": "Apa Itu Urinalisis Pemeriksaan Sampel Urin?", "id": "Analisis Sampel Urin Untuk Diagnosis." },
  { "en": "Apa Itu Sarkoma Jenis Kanker Langka?", "id": "Kanker Berasal Dari Jaringan Ikat." },
  { "en": "Apa Itu Manuver Heimlich Untuk Tersedak?", "id": "Teknik Pertolongan Pertama Mengatasi Tersedak." },
  { "en": "Apa Itu Limfosit B Dalam Imunitas?", "id": "Sel Darah Putih, Menghasilkan Antibodi." },
  { "en": "Apa Arti Awalan 'Inter-' Dalam Medis?", "id": "Berarti Di Antara Atau Antara." },
  { "en": "Apa Itu Bayi Prematur Lahir Dini?", "id": "Bayi Lahir Sebelum Minggu Ke-37." },
  { "en": "Apa Itu Kultur Urin Tes Bakteri?", "id": "Tes Membiakkan Bakteri Dari Sampel." },
  { "en": "Apa Itu Melanoma Jenis Kanker Kulit?", "id": "Kanker Berasal Dari Sel Melanosit." },
  { "en": "Bagaimana Cara Melakukan CPR (Cardiopulmonary Resuscitation)?", "id": "Kompresi Dada Dan Napas Bantuan." },
  { "en": "Apa Itu Sel Pembunuh Alami (NK)?", "id": "Limfosit, Membunuh Sel Terinfeksi Virus." },
  { "en": "Apa Arti Akhiran '-Tripsi' Dalam Medis?", "id": "Tindakan Menghancurkan Atau Meremukkan Sesuatu." },
  { "en": "Apa Itu Bayi Postmatur Lahir Terlambat?", "id": "Bayi Lahir Setelah Minggu Ke-42." },
  { "en": "Apa Itu Tes Darah Samar Feses?", "id": "Tes Mendeteksi Darah Tidak Terlihat." },
  { "en": "Apa Itu Metastasis Penyebaran Sel Kanker?", "id": "Penyebaran Kanker Ke Bagian Lain." },
  { "en": "Apa Itu Posisi Pemulihan Untuk Pasien?", "id": "Posisi Aman Untuk Orang Pingsan." },
  { "en": "Apa Itu Fagosit Dalam Sistem Imun?", "id": "Sel Menelan, Mencerna Partikel Asing." },
  { "en": "Apa Arti Awalan 'Ekstra-' Dalam Medis?", "id": "Berarti Di Luar Atau Tambahan." },
  { "en": "Apa Itu Skor APGAR Penilaian Bayi?", "id": "Penilaian Cepat Kesehatan Bayi Baru." },
  { "en": "Apa Itu Kultur Darah Tes Bakteri?", "id": "Tes Mencari Kuman Dalam Sampel." },
  { "en": "Apa Itu Biopsi Jarum Halus (FNA)?", "id": "Pengambilan Sel Menggunakan Jarum Sangat." },
  { "en": "Apa Itu Tahap Kanker Dalam Diagnosis?", "id": "Menjelaskan Ukuran, Penyebaran Tumor Kanker." },
  { "en": "Bagaimana Cara Menghentikan Pendarahan Arteri?", "id": "Berikan Tekanan Langsung Pada Lukanya." },
  { "en": "Apa Itu Makrofag Dalam Sistem Imun?", "id": "Sel Fagosit Besar, Pemakan Patogen." },
  { "en": "Apa Arti Akhiran '-Rrhagia' Dalam Medis?", "id": "Aliran Atau Pelepasan Darah Berlebihan." },
  { "en": "Apa Itu Abortus Spontan Atau Keguguran?", "id": "Berakhirnya Kehamilan Sebelum Minggu Ke-20." },
  { "en": "Apa Itu Tes Alergi Tusuk Kulit?", "id": "Tes Mencari Reaksi Alergi Cepat." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Tua?", "id": "Dokter Spesialis Geriatri Atau Geriatriawan." },
  { "en": "Apa Itu Remisi Dalam Penyakit Kanker?", "id": "Berkurangnya Atau Hilangnya Tanda Kanker." },
  { "en": "Kapan Harus Menggunakan Torniket Hentikan Pendarahan?", "id": "Hanya Untuk Pendarahan Ekstrem Mengancam." },
  { "en": "Apa Itu Sel Dendritik Sistem Imun?", "id": "Sel Penyaji Antigen, Mengaktifkan Limfosit." },
  { "en": "Apa Arti Awalan 'Retro-' Dalam Medis?", "id": "Berarti Di Belakang Atau Mundur." },
  { "en": "Apa Itu Kehamilan Ektopik Di Luar?", "id": "Kehamilan Terjadi Di Luar Rahim." },
  { "en": "Apa Itu Tes Fungsi Hati (LFT)?", "id": "Tes Darah Mengukur Enzim, Protein." },
  { "en": "Apa Itu Paliatif Dalam Perawatan Kanker?", "id": "Perawatan Mengurangi Gejala, Meningkatkan Kualitas." },
  { "en": "Apa Tanda-Tanda Utama Gegar Otak?", "id": "Sakit Kepala, Pusing, Mual, Kebingungan." },
  { en: "Apa Itu Neutrofil Jenis Sel Darah?", "id": "Jenis Leukosit Paling Banyak, Fagosit." },
  { "en": "Apa Arti Akhiran '-Rhea' Dalam Medis?", "id": "Aliran Atau Keluarnya Cairan Tubuh." },
  { "en": "Apa Itu Plasenta Previa Kondisi Kehamilan?", "id": "Plasenta Menutupi Sebagian Atau Seluruh." },
  { "en": "Apa Itu Tes Fungsi Tiroid (TFT)?", "id": "Tes Darah Mengukur Hormon Tiroid." },
  { "en": "Apa Itu Karsinogen Zat Penyebab Kanker?", "id": "Zat, Radiasi Mendorong Pembentukan Kanker." },
  { "en": "Bagaimana Menangani Mimisan Yang Benar?", "id": "Duduk Tegak, Condongkan Badan, Jepit." },
  { "en": "Apa Itu Eosinofil Jenis Sel Darah?", "id": "Melawan Infeksi Parasit, Reaksi Alergi." },
  { "en": "Apa Arti Awalan 'A-' Atau 'An-?'", "id": "Berarti Tidak, Tanpa, Atau Kurang." },
  { "en": "Apa Itu Solusio Plasenta Kondisi Kehamilan?", "id": "Plasenta Lepas Dari Dinding Rahim." },
  { "en": "Apa Itu Tes Elektrolit Dalam Darah?", "id": "Mengukur Mineral Penting, Natrium, Kalium." },
  { "en": "Apa Itu Onkogen Gen Penyebab Kanker?", "id": "Gen Termutasi, Potensi Menyebabkan Kanker." },
  { "en": "Apa Itu Bidai Dalam Pertolongan Pertama?", "id": "Alat Menstabilkan Tulang Patah, Cedera." },
  { "en": "Apa Itu Basofil Jenis Sel Darah?", "id": "Melepaskan Histamin Selama Reaksi Alergi." },
  { "en": "Apa Arti Akhiran '-Sclerosis' Dalam Medis?", "id": "Pengerasan Jaringan Abnormal, Penebalan Jaringan." },
  { "en": "Apa Itu Preeklamsia Pada Ibu Hamil?", "id": "Tekanan Darah Tinggi, Kerusakan Organ." },
  { "en": "Apa Itu Analisis Gas Darah (AGD)?", "id": "Mengukur Oksigen, Karbon Dioksida, Darah." },
  { "en": "Apa Itu Gen Supresor Tumor Pelindung?", "id": "Gen Memperlambat Pembelahan Sel Normal." },
  { "en": "Apa Itu Hipotermia Suhu Tubuh Rendah?", "id": "Suhu Tubuh Turun Drastis, Berbahaya." },
  { "en": "Apa Itu Sel Mast Dalam Imunitas?", "id": "Terlibat Dalam Proses Peradangan, Alergi." },
  { "en": "Apa Arti Awalan 'Dys-' Atau 'Dis-?'", "id": "Berarti Abnormal, Sulit, Atau Sakit." },
  { "en": "Apa Itu Diabetes Gestasional Saat Kehamilan?", "id": "Diabetes Terjadi Selama Masa Kehamilan." },
  { "en": "Apa Itu Laju Endap Darah (LED)?", "id": "Tes Mengukur Kecepatan Sel Darah." },
  { "en": "Apa Itu Angiogenesis Pembentukan Pembuluh Darah?", "id": "Pembentukan Pembuluh Darah Baru Tumor." },
  { "en": "Apa Itu Sengatan Panas Atau Heatstroke?", "id": "Kondisi Tubuh Terlalu Panas, Serius." },
  { "en": "Apa Itu Interferon Protein Anti-Virus?", "id": "Protein Dihasilkan Sel, Respon Virus." },
  { "en": "Apa Arti Akhiran '-Stenosis' Dalam Medis?", "id": "Penyempitan Abnormal Saluran Atau Lubang." },
  { "en": "Apa Itu Hiperemesis Gravidarum Mual Parah?", "id": "Mual, Muntah Parah Selama Kehamilan." },
  { "en": "Apa Itu Tes Waktu Protrombin (PT)?", "id": "Tes Mengukur Kemampuan Darah Membeku." },
  { "en": "Apa Yang Dimaksud Dengan Makronutrien Gizi?", "id": "Nutrien Dibutuhkan Jumlah Besar, Energi." },
  { "en": "Apa Itu Radang Tenggorokan Atau Faringitis?", "id": "Peradangan Bagian Belakang Tenggorokan Faring." },
  { "en": "Sebutkan Spesialisasi Dokter Telinga Hidung?", "id": "Dokter Spesialis THT Atau Otolaringolog." },
  { "en": "Apa Arti Awalan 'Hemi-' Dalam Medis?", "id": "Berarti Setengah Atau Sebagian Dari." },
  { "en": "Apa Itu Penyakit Celiac Intoleransi Gluten?", "id": "Penyakit Autoimun, Reaksi Terhadap Gluten." },
  { "en": "Apa Itu Tes Panel Metabolik Dasar?", "id": "Mengukur Gula, Kalsium, Elektrolit, Ginjal." },
  { "en": "Apa Yang Dimaksud Dengan Mikronutrien Gizi?", "id": "Nutrien Dibutuhkan Jumlah Kecil, Vitamin." },
  { "en": "Apa Itu Radang Amandel Atau Tonsilitis?", "id": "Peradangan Amandel, Dua Bantalan Jaringan." },
  { "en": "Apa Itu Penyakit Refluks Asam Lambung?", "id": "Asam Lambung Naik Ke Kerongkongan." },
  { "en": "Apa Arti Akhiran '-Tomi' Dalam Medis?", "id": "Sayatan Atau Pemotongan Ke Dalam." },
  { "en": "Apa Itu Penyakit Crohn Radang Usus?", "id": "Penyakit Radang Usus Kronis Autoimun." },
  { "en": "Apa Itu Tes Enzim Jantung Medis?", "id": "Mengukur Tingkat Enzim, Kerusakan Jantung." },
  { "en": "Apa Yang Dimaksud Dengan Serat Pangan?", "id": "Karbohidrat Tidak Dapat Dicerna Manusia." },
  { "en": "Apa Itu Laringitis Atau Radang Pita Suara?", "id": "Peradangan Laring, Suara Jadi Serak." },
  { "en": "Apa Itu Tukak Lambung Luka Lambung?", "id": "Luka Terbuka Lapisan Dalam Lambung." },
  { "en": "Apa Arti Awalan 'Pan-' Dalam Medis?", "id": "Berarti Semua, Menyeluruh, Atau Seluruh." },
  { "en": "Apa Itu Kolitis Ulserativa Radang Usus?", "id": "Penyakit Radang Usus Besar, Rektum." },
  { "en": "Apa Itu Tes Hemoglobin A1c (HbA1c)?", "id": "Mengukur Rata-Rata Gula Darah Tiga." },
  { "en": "Apa Itu Vitamin Larut Dalam Lemak?", "id": "Vitamin A, D, E, Dan K." },
  { "en": "Apa Itu Otitis Media Infeksi Telinga?", "id": "Infeksi Telinga Bagian Tengah Manusia." },
  { "en": "Apa Itu Gastroparesis Lambung Bergerak Lambat?", "id": "Kondisi Lambung Sulit Mengosongkan Diri." },
  { "en": "Apa Arti Akhiran '-Uria' Dalam Medis?", "id": "Kondisi Urin Atau Adanya Zat." },
  { "en": "Apa Itu Penyakit Divertikulitis Kantong Usus?", "id": "Peradangan Atau Infeksi Kantong Divertikula." },
  { "en": "Apa Itu Kadar Kreatinin Dalam Darah?", "id": "Tes Mengukur Fungsi Ginjal Manusia." },
  { "en": "Apa Itu Vitamin Larut Dalam Air?", "id": "Vitamin B Kompleks Dan Vitamin C." },
  { "en": "Apa Itu Tinnitus Atau Telinga Berdenging?", "id": "Persepsi Bunyi Tanpa Sumber Eksternal." },
  { "en": "Apa Itu Pankreatitis Atau Radang Pankreas?", "id": "Peradangan Organ Pankreas, Nyeri Hebat." },
  { "en": "Apa Itu Anamnesis Wawancara Riwayat Medis?", "id": "Pengambilan Riwayat Medis Pasien Terperinci." },
  { "en": "Apa Itu Sifilis Penyakit Menular Seksual?", "id": "Infeksi Bakteri, Bisa Sebabkan Komplikasi." },
  { "en": "Apa Itu Nitrogen Ureum Darah (BUN)?", "id": "Tes Darah Lainnya Ukur Fungsi Ginjal." },
  { "en": "Apa Fungsi Vitamin K Bagi Tubuh?", "id": "Penting Untuk Proses Pembekuan Darah." },
  { "en": "Apa Itu Epistaksis Atau Mimisan Medis?", "id": "Pendarahan Berasal Dari Dalam Hidung." },
  { "en": "Apa Itu Kolesistitis Atau Radang Kantung Empedu?", "id": "Peradangan Dinding Kantung Empedu Akut." },
  { "en": "Apa Itu Pemeriksaan Fisik Dalam Diagnosis?", "id": "Evaluasi Tubuh Pasien Mencari Tanda." },
  { "en": "Apa Itu Gonore Penyakit Menular Seksual?", "id": "Infeksi Bakteri, Menyerang Saluran Kemih." },
  { "en": "Apa Itu Panel Lipid Tes Kolesterol?", "id": "Tes Darah Mengukur Kadar Lemak." },
  { "en": "Apa Fungsi Kalsium Bagi Tubuh Manusia?", "id": "Membangun, Menjaga Tulang, Gigi Kuat." },
  { "en": "Apa Itu Polip Hidung Pertumbuhan Jinak?", "id": "Pertumbuhan Jaringan Lunak Lapisan Hidung." },
  { "en": "Apa Itu Peritonitis Radang Selaput Perut?", "id": "Peradangan Peritoneum, Lapisan Tipis Perut." },
  { "en": "Apa Itu Diagnosis Banding Dalam Medis?", "id": "Daftar Kemungkinan Penyakit, Gejala Serupa." },
  { "en": "Apa Itu Klamidia Penyakit Menular Seksual?", "id": "Infeksi Bakteri Umum, Sering Tanpa." },
  { "en": "Apa Itu Tes C-Reaktif Protein (CRP)?", "id": "Mengukur Tingkat Peradangan Dalam Tubuh." },
  { "en": "Apa Fungsi Zat Besi Bagi Tubuh?", "id": "Komponen Penting Hemoglobin Sel Darah." },
  { "en": "Apa Itu Septum Deviasi Dinding Hidung?", "id": "Dinding Pemisah Hidung Bergeser, Bengkok." },
  { "en": "Apa Itu Hernia Hiatus Kondisi Lambung?", "id": "Bagian Lambung Menonjol Melalui Diafragma." },
  { "en": "Apa Itu Diagnosis Akhir Atau Final?", "id": "Identifikasi Penyakit Final Setelah Evaluasi." },
  { "en": "Apa Itu Herpes Genital Infeksi Virus?", "id": "Infeksi Virus Menular, Luka Melepuh." },
  { "en": "Apa Itu Albumin Dalam Tes Darah?", "id": "Protein Utama, Menjaga Cairan, Nutrisi." },
  { "en": "Apa Fungsi Yodium Bagi Tubuh Manusia?", "id": "Dibutuhkan Kelenjar Tiroid Membuat Hormon." },
  { "en": "Apa Itu Kutil Kelamin (Human Papillomavirus)?", "id": "Infeksi HPV, Benjolan Area Genital." },
  { "en": "Apa Itu Bilirubin Dalam Tes Darah?", "id": "Pigmen Kuning, Hasil Pemecahan Sel." },
  { "en": "Apa Fungsi Magnesium Bagi Tubuh Manusia?", "id": "Mendukung Fungsi Saraf, Otot, Energi." },
  { "en": "Apa Itu Rinitis Alergi Radang Hidung?", "id": "Reaksi Alergi, Bersin, Hidung Tersumbat." },
  { "en": "Apa Itu Sindrom Brugada Kelainan Jantung?", "id": "Kelainan Irama Jantung Genetik, Berbahaya." },
  { "en": "Apa Itu Pencegahan Primer Dalam Kesehatan?", "id": "Mencegah Penyakit Sebelum Terjadi, Vaksinasi." },
  { "en": "Apa Itu Trikomoniasis Penyakit Menular Seksual?", "id": "Infeksi Parasit, Menyerang Saluran Urogenital." },
  { "en": "Apa Itu Ureum Dalam Tes Darah?", "id": "Produk Limbah Metabolisme Protein Hati." },
  { "en": "Apa Fungsi Fosfor Bagi Tubuh Manusia?", "id": "Komponen Utama Tulang, Gigi, DNA." },
  { "en": "Apa Itu Meniere Penyakit Telinga Dalam?", "id": "Gangguan Telinga Dalam, Vertigo, Tinnitus." },
  { "en": "Apa Itu Kardiomiopati Penyakit Otot Jantung?", "id": "Penyakit Membuat Jantung Sulit Memompa." },
  { "en": "Apa Itu Pencegahan Sekunder Dalam Kesehatan?", "id": "Deteksi Dini, Pengobatan Awal Penyakit." },
  { "en": "Apa Itu Bakterial Vaginosis Infeksi Vagina?", "id": "Infeksi Akibat Ketidakseimbangan Bakteri Vagina." },
  { "en": "Apa Itu Tes Golongan Darah Manusia?", "id": "Menentukan Tipe Darah (A, B, AB, O)." },
  { "en": "Apa Fungsi Kalium Bagi Tubuh Manusia?", "id": "Elektrolit Penting, Keseimbangan Cairan, Saraf." },
  { "en": "Apa Itu Otosklerosis Tulang Telinga Kaku?", "id": "Pertumbuhan Tulang Abnormal Telinga Tengah." },
  { "en": "Apa Itu Stenosis Aorta Penyempitan Katup?", "id": "Penyempitan Katup Aorta, Menghambat Aliran." },
  { "en": "Apa Itu Pencegahan Tersier Dalam Kesehatan?", "id": "Mengurangi Dampak Penyakit Kronis, Rehabilitasi." },
  { "en": "Apa Itu Kandidiasis Infeksi Jamur Ragi?", "id": "Infeksi Jamur Candida Albicans, Umum." },
  { "en": "Apa Itu Crossmatch Tes Kecocokan Darah?", "id": "Tes Mencocokkan Darah Donor, Resipien." },
  { "en": "Apa Fungsi Natrium Bagi Tubuh Manusia?", "id": "Mengatur Keseimbangan Cairan, Tekanan Darah." },
  { "en": "Apa Itu Presbikusis Gangguan Pendengaran Usia?", "id": "Kehilangan Pendengaran Bertahap Akibat Usia." },
  { "en": "Apa Itu Insufisiensi Mitral Katup Bocor?", "id": "Katup Mitral Tidak Menutup Rapat." },
  { "en": "Apa Itu Skrining Kesehatan Pemeriksaan Dini?", "id": "Tes Mengidentifikasi Penyakit Tanpa Gejala." },
  { "en": "Apa Itu Sifilis Kongenital Pada Bayi?", "id": "Sifilis Ditularkan Ibu Ke Janin." },
  { "en": "Apa Itu Tes Koagulasi Darah Lengkap?", "id": "Evaluasi Kemampuan Darah Untuk Membeku." },
  { "en": "Apa Fungsi Seng Bagi Tubuh Manusia?", "id": "Mendukung Sistem Imun, Penyembuhan Luka." },
  { "en": "Apa Itu Neuroma Akustik Tumor Saraf?", "id": "Tumor Jinak Saraf Pendengaran, Keseimbangan." },
  { "en": "Apa Itu Prolaps Katup Mitral Kondisi?", "id": "Katup Mitral Menonjol Ke Atrium." },
  { "en": "Apa Itu Uji Klinis Dalam Penelitian?", "id": "Studi Penelitian Menguji Keamanan, Efektivitas." },
  { "en": "Apa Itu Hepatitis B Penyakit Hati?", "id": "Infeksi Virus Hati, Bisa Kronis." },
  { "en": "Apa Itu D-dimer Tes Gumpalan Darah?", "id": "Mengukur Zat Sisa Saat Gumpalan." },
  { "en": "Apa Fungsi Selenium Bagi Tubuh Manusia?", "id": "Antioksidan, Penting Fungsi Tiroid, Imun." },
  { "en": "Apa Itu Kolesteatoma Pertumbuhan Kulit Abnormal?", "id": "Pertumbuhan Kulit Telinga Tengah, Destruktif." },
  { "en": "Apa Itu Penyakit Jantung Reumatik (PJR)?", "id": "Kerusakan Katup Jantung, Demam Reumatik." },
  { "en": "Apa Itu Studi Buta Ganda (Double-Blind)?", "id": "Pasien, Peneliti Tidak Tahu Perlakuan." },
  { "en": "Apa Itu Sindrom Alkohol Janin (FAS)?", "id": "Cacat Lahir Akibat Konsumsi Alkohol." },
  { "en": "Apa Itu Fibrinogen Dalam Tes Darah?", "id": "Protein Diubah Menjadi Fibrin Membeku." },
  { "en": "Apa Fungsi Tembaga Bagi Tubuh Manusia?", "id": "Membantu Pembentukan Sel Darah Merah." },
  { "en": "Apa Itu Barotrauma Telinga Akibat Tekanan?", "id": "Cedera Telinga Akibat Perubahan Tekanan." },
  { "en": "Apa Itu Koarktasio Aorta Penyempitan Aorta?", "id": "Penyempitan Aorta Bawaan Sejak Lahir." },
  { "en": "Apa Itu Kelompok Kontrol Dalam Studi?", "id": "Kelompok Tidak Menerima Perlakuan Uji." },
  { "en": "Apa Itu Sindrom Kematian Bayi Mendadak?", "id": "Kematian Bayi Sehat Tanpa Penjelasan." },
  { "en": "Apa Itu Kadar Amilase Dalam Darah?", "id": "Enzim Membantu Mencerna Karbohidrat, Pankreas." },
  { "en": "Apa Fungsi Mangan Bagi Tubuh Manusia?", "id": "Penting Untuk Pembentukan Tulang, Metabolisme." },
  { "en": "Apa Itu Mastoiditis Infeksi Tulang Mastoid?", "id": "Infeksi Tulang Belakang Telinga Manusia." },
  { "en": "Apa Itu Defek Septum Atrium (ASD)?", "id": "Lubang Dinding Antara Serambi Jantung." },
  { "en": "Apa Itu Meta-Analisis Dalam Penelitian Ilmiah?", "id": "Analisis Statistik Menggabungkan Hasil Studi." },
  { "en": "Apa Itu Spina Bifida Cacat Lahir?", "id": "Cacat Lahir Tulang Belakang, Saraf." },
  { "en": "Apa Itu Kadar Lipase Dalam Darah?", "id": "Enzim Membantu Mencerna Lemak, Pankreas." },
  { "en": "Apa Fungsi Kromium Bagi Tubuh Manusia?", "id": "Meningkatkan Aksi Insulin, Metabolisme Glukosa." },
  { "en": "Apa Itu Penyakit Keseimbangan Vestibular Sentral?", "id": "Masalah Keseimbangan Berasal Dari Otak." },
  { "en": "Apa Itu Defek Septum Ventrikel (VSD)?", "id": "Lubang Dinding Antara Bilik Jantung." },
  { "en": "Apa Itu Tinjauan Sistematis Dalam Penelitian?", "id": "Tinjauan Komprehensif Bukti Penelitian Relevan." },
  { "en": "Apa Itu Anensefali Cacat Lahir Fatal?", "id": "Bayi Lahir Tanpa Sebagian Otak." },
  { "en": "Apa Itu Elektroforesis Hemoglobin Tes Darah?", "id": "Memisahkan Jenis Hemoglobin Dalam Darah." },
  { "en": "Apa Itu Penyakit Akromegali Hormon Pertumbuhan?", "id": "Kelebihan Hormon Pertumbuhan Pada Dewasa." },
  { "en": "Sebutkan Spesialisasi Dokter Kaki Manusia?", "id": "Dokter Spesialis Podiatri Atau Podiatris." },
  { "en": "Apa Itu Tetralogi Fallot Kelainan Jantung?", "id": "Kombinasi Empat Cacat Jantung Bawaan." },
  { "en": "Apa Itu Angka Signifikansi (P-Value) Statistik?", "id": "Ukuran Kekuatan Bukti Terhadap Hipotesis." },
  { "en": "Apa Itu Bibir Sumbing Celah Bibir?", "id": "Celah Bibir Atas, Langit-Langit Mulut." },
  { "en": "Apa Itu Penyakit Addison Kekurangan Hormon?", "id": "Kelenjar Adrenal Tidak Cukup Hormon." },
  { "en": "Apa Itu Plantar Fasciitis Nyeri Tumit?", "id": "Peradangan Jaringan Ikat Telapak Kaki." },
  { "en": "Apa Itu Duktus Arteriosus Paten (PDA)?", "id": "Pembukaan Antara Aorta, Arteri Pulmonalis." },
  { "en": "Apa Itu Epidemiologi Studi Pola Penyakit?", "id": "Studi Penyebaran, Faktor Penentu Penyakit." },
  { "en": "Apa Itu Penyakit Celiac Sprue Autoimun?", "id": "Respons Imun Merusak Usus Halus." },
  { "en": "Apa Itu Penyakit Cushing Kelebihan Kortisol?", "id": "Kondisi Akibat Paparan Kortisol Tinggi." },
  { "en": "Apa Itu Bunion Benjolan Ibu Jari?", "id": "Benjolan Tulang Sendi Ibu Jari." },
  { "en": "Apa Itu Emfisema Kerusakan Kantung Udara?", "id": "Kerusakan Alveoli Paru-Paru, Sesak Napas." },
  { "en": "Apa Itu Kesehatan Masyarakat (Public Health)?", "id": "Ilmu Melindungi, Meningkatkan Kesehatan Komunitas." },
  { "en": "Apa Itu Preeklampsia Tekanan Darah Kehamilan?", "id": "Tekanan Darah Tinggi, Tanda Kerusakan." },
  { "en": "Apa Itu Diabetes Insipidus Haus Berlebih?", "id": "Ginjal Gagal Menghemat Air, Dehidrasi." },
  { "en": "Apa Itu Kaki Atlet Infeksi Jamur?", "id": "Infeksi Jamur Kulit Kaki Menular." },
  { "en": "Apa Itu Penyakit Paru Obstruktif Kronis?", "id": "Penyakit Paru Progresif, Sulit Bernapas." },
  { "en": "Apa Itu Promosi Kesehatan Upaya Edukasi?", "id": "Proses Memampukan Orang Mengontrol Kesehatan." },
  { "en": "Apa Itu Abses Gigi Kantung Nanah?", "id": "Kumpulan Nanah Akibat Infeksi Bakteri." },
  { "en": "Apa Itu Sindrom Ovarium Polikistik (PCOS)?", "id": "Gangguan Hormon Wanita, Kista Ovarium." },
  { "en": "Apa Itu Jari Kaki Palu (Hammertoe)?", "id": "Kelainan Bentuk Jari Kaki Membengkok." },
  { "en": "Apa Itu Fibrosis Paru Jaringan Parut?", "id": "Jaringan Parut Paru-Paru, Napas Pendek." },
  { "en": "Apa Itu Surveilans Kesehatan Pemantauan Penyakit?", "id": "Pengumpulan, Analisis, Interpretasi Data Kesehatan." },
  { "en": "Apa Itu Eklampsia Kejang Saat Hamil?", "id": "Komplikasi Preeklampsia, Menyebabkan Kejang Hebat." },
  { "en": "Apa Itu Hipopituitarisme Kekurangan Hormon Hipofisis?", "id": "Kelenjar Hipofisis Kurang Produksi Hormon." },
  { "en": "Apa Itu Keseleo Pergelangan Kaki Cedera?", "id": "Cedera Ligamen Pergelangan Kaki Tiba-Tiba." },
  { "en": "Apa Itu Asbestosis Penyakit Paru Akibat?", "id": "Penyakit Akibat Menghirup Serat Asbes." },
  { "en": "Apa Itu Imunisasi Program Pemberian Vaksin?", "id": "Pemberian Vaksin Untuk Mencegah Penyakit." },
  { "en": "Apa Itu Periodontitis Infeksi Gusi Serius?", "id": "Infeksi Gusi Merusak Tulang Penyokong." },
  { "en": "Apa Itu Insufisiensi Adrenal Kelenjar Adrenal?", "id": "Kelenjar Adrenal Kurang Produksi Hormon." },
  { "en": "Apa Itu Neuropati Perifer Kerusakan Saraf?", "id": "Kerusakan Saraf Tepi, Nyeri, Mati." },
  { "en": "Apa Itu Edema Paru Cairan Paru?", "id": "Kelebihan Cairan Di Dalam Paru-Paru." },
  { "en": "Apa Itu Disinfeksi Proses Membunuh Kuman?", "id": "Membunuh Mikroorganisme Di Benda Mati." },
  { "en": "Apa Itu Gigi Impaksi Tidak Bisa?", "id": "Gigi Gagal Tumbuh Sepenuhnya Normal." },
  { "en": "Apa Itu Hiperaldosteronisme Hormon Aldosteron Berlebih?", "id": "Produksi Aldosteron Berlebihan Kelenjar Adrenal." },
  { "en": "Apa Itu Ulkus Diabetik Luka Kaki?", "id": "Luka Terbuka Kaki Penderita Diabetes." },
  { "en": "Apa Itu Silikosis Penyakit Paru Debu?", "id": "Penyakit Paru Akibat Menghirup Debu." },
  { "en": "Apa Itu Sterilisasi Proses Membunuh Mikroba?", "id": "Menghilangkan Semua Bentuk Kehidupan Mikroba." },
  { "en": "Apa Itu Bruxism Atau Menggertakkan Gigi?", "id": "Kebiasaan Menggertakkan, Menekan Gigi Atas." },
  { "en": "Apa Itu Feokromositoma Tumor Kelenjar Adrenal?", "id": "Tumor Langka, Melepaskan Hormon Adrenalin." },
  { "en": "Apa Itu Charcot Foot Komplikasi Diabetes?", "id": "Kondisi Pelemahan Tulang Kaki Diabetes." },
  { "en": "Apa Itu Efusi Pleura Cairan Pleura?", "id": "Penumpukan Cairan Berlebih Di Rongga." },
  { "en": "Apa Itu Karantina Medis Isolasi Penyakit?", "id": "Pemisahan, Pembatasan Gerak Orang Sakit." },
  { "en": "Apa Itu Mulut Kering Atau Xerostomia?", "id": "Kondisi Kurangnya Air Liur Mulut." },
  { "en": "Apa Itu Hiperparatiroidisme Hormon Paratiroid Berlebih?", "id": "Kelebihan Hormon Paratiroid Dalam Darah." },
  { "en": "Apa Itu Cedera Ligamen Lutut Anterior?", "id": "Robeknya Ligamen Krusiatum Anterior (ACL)." },
  { "en": "Apa Itu Pneumotoraks Kolaps Paru-Paru Udara?", "id": "Udara Bocor, Mengumpul Ruang Pleura." },
  { "en": "Apa Yang Dimaksud Dengan Penyakit Endemik?", "id": "Penyakit Terus-Menerus Ada Wilayah Tertentu." },
  { "en": "Apa Itu Halitosis Atau Bau Mulut?", "id": "Bau Napas Tidak Sedap Kronis." },
  { "en": "Apa Itu Hipoparatiroidisme Hormon Paratiroid Rendah?", "id": "Kekurangan Hormon Paratiroid Dalam Darah." },
  { "en": "Apa Itu Robekan Meniskus Cedera Lutut?", "id": "Robeknya Tulang Rawan Meniskus Lutut." },
  { "en": "Apa Itu Atelektasis Kondisi Paru Mengempis?", "id": "Kolaps Sebagian Atau Seluruh Paru-Paru." },
  { "en": "Apa Yang Dimaksud Dengan Penyakit Epidemik?", "id": "Peningkatan Kasus Penyakit Luas, Cepat." },
  { "en": "Apa Itu Glositis Atau Radang Lidah?", "id": "Peradangan Lidah, Bengkak, Berubah Warna." },
  { "en": "Apa Itu Ginekomastia Pembesaran Payudara Pria?", "id": "Pembengkakan Jaringan Payudara Pada Pria." },
  { "en": "Apa Itu Tendinitis Atau Radang Tendon?", "id": "Peradangan Atau Iritasi Pada Tendon." },
  { "en": "Apa Itu Sindrom Gangguan Pernapasan Akut?", "id": "Kegagalan Pernapasan, Cairan Di Paru." },
  { "en": "Apa Itu Autonomi Pasien Hak Pasien?", "id": "Hak Pasien Membuat Keputusan Medisnya." },
  { "en": "Apa Arti Awalan 'Nefro-' Dalam Medis?", "id": "Berkaitan Dengan Organ Ginjal Manusia." },
  { "en": "Apa Itu Hipogonadisme Produksi Hormon Seks?", "id": "Produksi Hormon Seks Rendah, Testis." },
  { "en": "Apa Itu Bursitis Radang Kantung Sendi?", "id": "Peradangan Bursa, Kantung Cairan Sendi." },
  { "en": "Apa Itu Penyakit Legionnaires Jenis Pneumonia?", "id": "Pneumonia Berat Akibat Bakteri Legionella." },
  { "en": "Apa Itu Beneficence Prinsip Etika Medis?", "id": "Bertindak Demi Kebaikan Pasien Selalu." },
  { "en": "Apa Arti Awalan 'Hepato-' Dalam Medis?", "id": "Berkaitan Dengan Organ Hati Manusia." },
  { "en": "Apa Itu Kriptorkismus Testis Tidak Turun?", "id": "Testis Gagal Turun Kantung Skrotum." },
  { "en": "Apa Itu Sindrom Terowongan Karpal Tangan?", "id": "Saraf Median Terjepit Pergelangan Tangan." },
  { "en": "Apa Itu Mikosis Paru Infeksi Jamur?", "id": "Infeksi Jamur Menyerang Organ Paru-Paru." },
  { "en": "Apa Itu Non-Maleficence Prinsip Etika Medis?", "id": "Prinsip Untuk Tidak Merugikan Pasien." },
  { "en": "Apa Arti Awalan 'Kardio-' Dalam Medis?", "id": "Berkaitan Dengan Organ Jantung Manusia." },
  { "en": "Apa Itu Torsio Testis Terpelintirnya Testis?", "id": "Kondisi Darurat, Testis Terpelintir, Nyeri." },
  { "en": "Apa Itu Siku Tenis (Lateral Epicondylitis)?", "id": "Nyeri Bagian Luar Siku Lengan." },
  { "en": "Apa Itu Empiema Kumpulan Nanah Rongga?", "id": "Kumpulan Nanah Di Dalam Rongga." },
  { "en": "Apa Itu Keadilan Dalam Etika Medis?", "id": "Distribusi Perawatan Kesehatan Adil, Merata." },
  { "en": "Apa Arti Awalan 'Pulmo-' Dalam Medis?", "id": "Berkaitan Dengan Organ Paru-Paru Manusia." },
  { "en": "Apa Itu Hidrokel Kantung Berisi Cairan?", "id": "Penumpukan Cairan Sekitar Testis Skrotum." },
  { "en": "Apa Itu Pemicu Jari (Trigger Finger)?", "id": "Kondisi Jari Terkunci Posisi Bengkok." },
  { "en": "Apa Itu Hipertensi Pulmonal Tekanan Darah?", "id": "Tekanan Darah Tinggi Arteri Paru." },
  { "en": "Apa Itu DNR (Do Not Resuscitate)?", "id": "Perintah Medis Tidak Lakukan CPR." },
  { "en": "Apa Arti Awalan 'Gastro-' Dalam Medis?", "id": "Berkaitan Dengan Organ Lambung Manusia." },
  { "en": "Apa Itu Varikokel Pembengkakan Vena Skrotum?", "id": "Pembesaran Vena Dalam Kantung Skrotum." },
  { "en": "Apa Itu Kontraktur Dupuytren Jaringan Tangan?", "id": "Penebalan Jaringan Bawah Kulit Tangan." },
  { "en": "Apa Itu Apnea Tidur Henti Napas?", "id": "Gangguan Tidur, Napas Berhenti, Mulai." },
  { "en": "Apa Itu HIPAA (Health Insurance Portability Act)?", "id": "Undang-Undang Melindungi Informasi Kesehatan Pasien." },
  { "en": "Apa Arti Awalan 'Neuro-' Dalam Medis?", "id": "Berkaitan Dengan Sistem Saraf Manusia." },
  { "en": "Apa Itu Peyronie Penyakit Penis Bengkok?", "id": "Jaringan Parut Menyebabkan Penis Bengkok." },
  { "en": "Apa Itu Kista Ganglion Benjolan Sendi?", "id": "Benjolan Non-Kanker, Sendi, Pergelangan Tangan." },
  { "en": "Apa Itu Narkolepsi Serangan Tidur Mendadak?", "id": "Rasa Kantuk Berlebihan, Serangan Tidur." },
  { "en": "Apa Itu Kode Etik Kedokteran Indonesia?", "id": "Pedoman Etika Bagi Dokter Indonesia." },
  { "en": "Apa Arti Awalan 'Osteo-' Dalam Medis?", "id": "Berkaitan Dengan Tulang Rangka Manusia." },
  { "en": "Apa Itu Priapismus Ereksi Berkepanjangan Sakit?", "id": "Ereksi Tahan Lama, Menyakitkan, Darurat." },
  { "en": "Apa Itu Sindrom De Quervain Nyeri?", "id": "Peradangan Tendon Sisi Ibu Jari." },
  { "en": "Apa Itu Insomnia Kesulitan Tidur Kronis?", "id": "Sulit Tidur, Tetap Tidur, Kualitas." },
  { "en": "Apa Itu Visum Et Repertum Medis?", "id": "Laporan Tertulis Dokter Untuk Peradilan." },
  { "en": "Apa Arti Awalan 'Myo-' Dalam Medis?", "id": "Berkaitan Dengan Jaringan Otot Manusia." },
  { "en": "Apa Itu Balanitis Radang Kepala Penis?", "id": "Peradangan Pada Glans Atau Kepala." },
  { "en": "Apa Itu Fasciitis Nekrotikans Infeksi Daging?", "id": "Infeksi Bakteri Langka, Merusak Jaringan." },
  { "en": "Apa Itu Sindrom Kaki Gelisah (RLS)?", "id": "Dorongan Tak Terkendali Menggerakkan Kaki." },
  { "en": "Apa Itu Livor Mortis Dalam Forensik?", "id": "Perubahan Warna Kulit Setelah Kematian." },
  { "en": "Apa Arti Awalan 'Angio-' Dalam Medis?", "id": "Berkaitan Dengan Pembuluh Darah Atau Limfe." },
  { "en": "Apa Itu Fimosis Kulup Penis Ketat?", "id": "Kulup Tidak Dapat Ditarik Kembali." },
  { "en": "Apa Itu Gangren Kematian Jaringan Tubuh?", "id": "Kematian Jaringan Akibat Kehilangan Aliran." },
  { "en": "Apa Itu Parasomnia Perilaku Tidur Abnormal?", "id": "Perilaku Tidak Biasa Selama Tidur." },
  { "en": "Apa Itu Rigor Mortis Dalam Forensik?", "id": "Kekakuan Tubuh Yang Terjadi Setelah." },
  { "en": "Apa Arti Awalan 'Derma-' Dalam Medis?", "id": "Berkaitan Dengan Organ Kulit Manusia." },
  { "en": "Apa Itu Parafimosis Kulup Penis Terjebak?", "id": "Kulup Terjebak, Tidak Kembali Posisi." },
  { "en": "Apa Itu Kompartemen Sindrom Tekanan Otot?", "id": "Peningkatan Tekanan Kompartemen Otot, Berbahaya." },
  { "en": "Apa Itu Tidur Berjalan Atau Somnambulisme?", "id": "Bangun, Berjalan Saat Dalam Tidur." },
  { "en": "Apa Itu Algor Mortis Dalam Forensik?", "id": "Penurunan Suhu Tubuh Setelah Kematian." },
  { "en": "Apa Arti Awalan 'Orchi-' Dalam Medis?", "id": "Berkaitan Dengan Organ Testis Pria." },
  { "en": "Apa Itu Hipospadia Kelainan Lubang Uretra?", "id": "Lubang Uretra Berada Bagian Bawah." },
  { "en": "Apa Itu Rabdomiolisis Kerusakan Otot Parah?", "id": "Kerusakan Otot, Pelepasan Mioglobin Darah." },
  { "en": "Apa Itu Teror Tidur (Night Terrors)?", "id": "Episode Ketakutan, Berteriak Saat Tidur." },
  { "en": "Apa Itu Otopsi Pemeriksaan Setelah Kematian?", "id": "Pemeriksaan Bedah Mayat, Penyebab Kematian." },
  { "en": "Apa Arti Awalan 'Oophor-' Dalam Medis?", "id": "Berkaitan Dengan Organ Ovarium Wanita." },
  { "en": "Apa Itu Atresia Bilier Saluran Empedu?", "id": "Penyumbatan Saluran Empedu Bayi Baru." },
  { "en": "Apa Itu Fraktur Stres Retak Tulang?", "id": "Retak Kecil Tulang, Akibat Beban." },
  { "en": "Apa Itu Narkolepsi Dengan Katapleksi Kelemahan?", "id": "Kelemahan Otot Tiba-Tiba Dipicu Emosi." },
  { "en": "Apa Itu Toksikologi Forensik Studi Racun?", "id": "Aplikasi Toksikologi Untuk Tujuan Hukum." },
  { "en": "Apa Arti Awalan 'Salpingo-' Dalam Medis?", "id": "Berkaitan Dengan Tuba Falopi Wanita." },
  { "en": "Apa Itu Penyakit Hirschsprung Kelainan Usus?", "id": "Kelainan Usus Besar, Kurang Saraf." },
  { "en": "Apa Itu Dislokasi Sendi Tulang Bergeser?", "id": "Ujung Tulang Bergeser Dari Posisi." },
  { "en": "Apa Itu REM (Rapid Eye Movement) Sleep?", "id": "Tahap Tidur Mimpi, Gerakan Mata." },
  { "en": "Apa Itu Antropologi Forensik Identifikasi Rangka?", "id": "Aplikasi Antropologi Identifikasi Sisa Manusia." },
  { "en": "Apa Arti Awalan 'Hystero-' Dalam Medis?", "id": "Berkaitan Dengan Organ Rahim (Uterus)." },
  { "en": "Apa Itu Intususepsi Lipatan Usus Anak?", "id": "Bagian Usus Melipat Ke Usus." },
  { "en": "Apa Itu Osteomielitis Atau Infeksi Tulang?", "id": "Infeksi Pada Jaringan Tulang Manusia." },
  { "en": "Apa Itu Polisomnografi Atau Studi Tidur?", "id": "Tes Merekam Fungsi Tubuh Tidur." },
  { "en": "Apa Itu Odontologi Forensik Identifikasi Gigi?", "id": "Aplikasi Ilmu Gigi Untuk Hukum." },
  { "en": "Apa Itu Stenosis Pilorus Penyempitan Lambung?", "id": "Penyempitan Otot Antara Lambung, Usus." },
  { "en": "Apa Itu Nekrosis Avaskular Kematian Tulang?", "id": "Kematian Jaringan Tulang Kurangnya Darah." },
  { "en": "Apa Itu Skala Tidur Epworth (ESS)?", "id": "Kuesioner Mengukur Tingkat Kantuk Siang." },
  { "en": "Apa Itu Entomologi Forensik Studi Serangga?", "id": "Aplikasi Studi Serangga Investigasi Kriminal." },
  { "en": "Apa Arti Awalan 'Myringo-' Dalam Medis?", "id": "Berkaitan Dengan Gendang Telinga Manusia." },
  { "en": "Apa Itu Atresia Esofagus Kelainan Bawaan?", "id": "Esofagus Bayi Tidak Tersambung Lambung." },
  { "en": "Apa Itu Penyakit Paget Tulang Rapuh?", "id": "Gangguan Proses Regenerasi Tulang Normal." },
  { "en": "Apa Itu Hipersomnia Rasa Kantuk Berlebihan?", "id": "Rasa Kantuk Siang Hari Berlebihan." },
  { "en": "Apa Itu Balistik Forensik Ilmu Peluru?", "id": "Ilmu Proyektil, Senjata Api, Investigasi." },
  { "en": "Apa Arti Awalan 'Phlebo-' Dalam Medis?", "id": "Berkaitan Dengan Pembuluh Vena Manusia." },
  { "en": "Apa Itu Hernia Diafragmatika Bawaan (CDH)?", "id": "Lubang Diafragma, Organ Perut Masuk." },
  { "en": "Apa Itu Rakhitis Pelunakan Tulang Anak?", "id": "Pelunakan, Pelemahan Tulang Akibat Vitamin." },
  { "en": "Apa Itu Ritme Sirkadian Jam Internal?", "id": "Siklus Internal 24 Jam Tubuh." },
  { "en": "Apa Itu Patologi Forensik Pemeriksaan Medis?", "id": "Menentukan Penyebab Kematian Pemeriksaan Mayat." },
  { "en": "Apa Arti Awalan 'Pneumo-' Dalam Medis?", "id": "Berkaitan Dengan Udara, Gas, Paru-Paru." },
  { "en": "Apa Itu Gastroskisis Cacat Dinding Perut?", "id": "Usus Bayi Keluar Lubang Perut." },
  { "en": "Apa Itu Osteomalasia Pelunakan Tulang Dewasa?", "id": "Pelunakan Tulang, Kekurangan Vitamin D." },
  { "en": "Apa Itu Kebersihan Tidur (Sleep Hygiene)?", "id": "Kebiasaan, Praktik Mendukung Tidur Nyenyak." },
  { "en": "Apa Itu Psikiatri Forensik Kesehatan Jiwa?", "id": "Sub-Spesialisasi Psikiatri, Terkait Masalah Hukum." },
  { "en": "Apa Arti Awalan 'Adeno-' Dalam Medis?", "id": "Berkaitan Dengan Kelenjar Atau Struktur." },
  { "en": "Apa Itu Omfalokel Kelainan Dinding Perut?", "id": "Organ Perut Bayi Menonjol Pusar." },
  { "en": "Apa Itu Skurvi Penyakit Kekurangan Vitamin?", "id": "Penyakit Akibat Kekurangan Vitamin C." },
  { "en": "Apa Itu Hormon Pertumbuhan (Growth Hormone)?", "id": "Hormon Merangsang Pertumbuhan, Reproduksi Sel." },
  { "en": "Apa Itu Artritis Reumatoid Radang Sendi?", "id": "Penyakit Autoimun, Radang Kronis Sendi." },
  { "en": "Apa Arti Awalan 'Lipo-' Dalam Medis?", "id": "Berkaitan Dengan Lemak Atau Jaringan." },
  { "en": "Apa Itu Anemia Sel Sabit Genetik?", "id": "Kelainan Genetik Sel Darah Merah." },
  { "en": "Apa Itu Beri-Beri Kekurangan Vitamin B1?", "id": "Penyakit Kekurangan Tiamin (Vitamin B1)." },
  { "en": "Apa Itu Gigantisme Pertumbuhan Berlebih Anak?", "id": "Pertumbuhan Abnormal Anak, Hormon Berlebih." },
  { "en": "Apa Itu Lupus Eritematosus Sistemik (LES)?", "id": "Penyakit Autoimun, Menyerang Banyak Organ." },
  { "en": "Apa Arti Awalan 'Chole-' Dalam Medis?", "id": "Berkaitan Dengan Empedu Atau Kantung." },
  { "en": "Apa Itu Penyakit Tay-Sachs Kelainan Genetik?", "id": "Kelainan Genetik Merusak Sel Saraf." },
  { "en": "Apa Itu Pellagra Kekurangan Niasin (B3)?", "id": "Penyakit Akibat Kekurangan Vitamin B3." },
  { "en": "Apa Itu Dwarfisme Hipofisis Kekurangan Hormon?", "id": "Pertumbuhan Terhambat, Kurang Hormon Pertumbuhan." },
  { "en": "Apa Itu Sindrom Sjogren Autoimun Kelenjar?", "id": "Autoimun, Menyerang Kelenjar Air Mata." },
  { "en": "Apa Arti Awalan 'Cysto-' Dalam Medis?", "id": "Berkaitan Dengan Kandung Kemih Manusia." },
  { "en": "Apa Itu Fenilketonuria (PKU) Kelainan Genetik?", "id": "Kelainan Genetik, Tubuh Tak Urai." },
  { "en": "Apa Itu Anemia Pernisiosa Kekurangan Vitamin?", "id": "Anemia, Tubuh Tak Serap Vitamin." },
  { "en": "Apa Itu Prolaktinoma Tumor Kelenjar Hipofisis?", "id": "Tumor Jinak, Menghasilkan Hormon Prolaktin." },
  { "en": "Apa Itu Skleroderma Pengerasan Kulit Autoimun?", "id": "Penyakit Autoimun, Pengerasan Jaringan Ikat." },
  { "en": "Apa Arti Awalan 'Entero-' Dalam Medis?", "id": "Berkaitan Dengan Organ Usus Manusia." },
  { "en": "Apa Itu Galaktosemia Kelainan Metabolisme Gula?", "id": "Kelainan Genetik, Tak Urai Gula." },
  { "en": "Apa Itu Kwashiorkor Malnutrisi Protein Parah?", "id": "Bentuk Malnutrisi Protein-Energi Parah Anak." },
  { "en": "Apa Itu Tiroiditis Hashimoto Autoimun Tiroid?", "id": "Autoimun, Merusak Kelenjar Tiroid, Hipotiroidisme." },
  { "en": "Apa Itu Polimiositis Radang Otot Autoimun?", "id": "Peradangan Otot Kronis, Kelemahan Otot." },
  { "en": "Apa Arti Awalan 'Colpo-' Dalam Medis?", "id": "Berkaitan Dengan Organ Vagina Wanita." },
  { "en": "Apa Itu Sindrom Fragile X Genetik?", "id": "Kelainan Genetik, Kecacatan Intelektual, Perilaku." },
  { "en": "Apa Itu Marasmus Malnutrisi Energi Parah?", "id": "Bentuk Malnutrisi Energi Sangat Parah." },
  { "en": "Apa Itu Penyakit Graves Autoimun Hipertiroid?", "id": "Penyakit Autoimun, Menyebabkan Hipertiroidisme Kronis." },
  { "en": "Apa Itu Dermatomiositis Radang Kulit Otot?", "id": "Penyakit Radang, Kelemahan Otot, Ruam." },
  { "en": "Apa Arti Awalan 'Kerato-' Dalam Medis?", "id": "Berkaitan Dengan Kornea Atau Jaringan." },
  { "en": "Apa Itu Sindrom Klinefelter Kondisi Genetik?", "id": "Kondisi Genetik Pria, Kromosom X." },
  { "en": "Apa Itu Kaheksia Penurunan Berat Badan?", "id": "Penurunan Berat, Otot, Lemak Parah." },
  { "en": "Apa Itu Krisis Tiroid (Thyroid Storm)?", "id": "Kondisi Langka, Mengancam, Hipertiroidisme Parah." },
  { "en": "Apa Itu Vaskulitis Granulomatosis Wegener (GPA)?", "id": "Peradangan Pembuluh Darah Hidung, Paru." },
  { "en": "Apa Arti Awalan 'Mast-' Atau 'Masto-?'", "id": "Berkaitan Dengan Payudara Atau Kelenjar." },
  { "en": "Apa Itu Sindrom Turner Kondisi Genetik?", "id": "Kondisi Genetik Wanita, Kromosom X." },
  { "en": "Apa Itu Sianosis Warna Kulit Kebiruan?", "id": "Warna Kebiruan, Kurangnya Oksigen Darah." },
  { "en": "Apa Itu Goiter Endemik Kekurangan Yodium?", "id": "Pembesaran Tiroid Akibat Kekurangan Yodium." },
  { "en": "Apa Itu Artritis Psoriatik Radang Sendi?", "id": "Jenis Artritis, Mempengaruhi Penderita Psoriasis." },
  { "en": "Apa Arti Awalan 'Onco-' Dalam Medis?", "id": "Berkaitan Dengan Tumor, Massa, Kanker." },
  { "en": "Apa Itu Penyakit Jantung Bawaan Sianotik?", "id": "Cacat Jantung, Kadar Oksigen Rendah." },
  { "en": "Apa Itu Ikterus Atau Penyakit Kuning?", "id": "Menguningnya Kulit, Mata, Bilirubin Tinggi." },
  { "en": "Apa Itu Kretinisme Keterbelakangan Mental, Fisik?", "id": "Kondisi Akibat Kekurangan Hormon Tiroid." },
  { "en": "Apa Itu Spondilitis Ankilosa Radang Tulang?", "id": "Jenis Artritis, Peradangan Tulang Belakang." },
  { "en": "Apa Arti Awalan 'Patho-' Dalam Medis?", "id": "Berkaitan Dengan Penyakit Atau Penderitaan." },
  { "en": "Apa Itu Polisitemia Vera Kanker Darah?", "id": "Kanker Darah Lambat, Sumsum Tulang." },
  { "en": "Apa Itu Asites Penumpukan Cairan Perut?", "id": "Penumpukan Cairan Dalam Rongga Peritoneum." },
  { "en": "Apa Itu Oftalmopati Graves Mata Menonjol?", "id": "Kondisi Mata Menonjol Penyakit Graves." },
  { "en": "Apa Itu Artritis Reaktif Infeksi Bakteri?", "id": "Nyeri Sendi, Bengkak Dipicu Infeksi." },
  { "en": "Apa Arti Akhiran '-Genesis' Dalam Medis?", "id": "Berarti Pembentukan, Produksi, Atau Asal." },
  { "en": "Apa Itu Trombositopenia Jumlah Trombosit Rendah?", "id": "Kondisi Jumlah Trombosit Darah Rendah." },
  { "en": "Apa Itu Edema Pembengkakan Akibat Cairan?", "id": "Pembengkakan Jaringan Akibat Kelebihan Cairan." },
  { "en": "Apa Itu Tiroiditis Subakut Radang Tiroid?", "id": "Peradangan Kelenjar Tiroid, Diduga Virus." },
  { "en": "Apa Itu Penyakit Kawasaki Radang Pembuluh?", "id": "Penyakit Langka, Radang Dinding Pembuluh." },
  { "en": "Apa Arti Akhiran '-Blast' Dalam Medis?", "id": "Menandakan Sel Belum Matang (Imatur)." },
  { "en": "Apa Itu Leukopenia Jumlah Leukosit Rendah?", "id": "Penurunan Jumlah Sel Darah Putih." },
  { "en": "Apa Itu Efusi Perikardial Cairan Jantung?", "id": "Penumpukan Cairan Berlebih Kantung Jantung." },
  { "en": "Apa Itu Adenoma Hipofisis Tumor Jinak?", "id": "Tumor Jinak Pada Kelenjar Hipofisis." },
  { "en": "Apa Itu Henoch-Schonlein Purpura (HSP) Vaskulitis?", "id": "Peradangan Pembuluh Darah Kecil Kulit." },
  { "en": "Apa Arti Akhiran '-Cele' Dalam Medis?", "id": "Berarti Hernia, Tonjolan, Atau Pembengkakan." },
  { "en": "Apa Itu Pansitopenia Kekurangan Semua Sel?", "id": "Kekurangan Tiga Jenis Sel Darah." },
  { "en": "Apa Itu Dehidrasi Kekurangan Cairan Tubuh?", "id": "Kehilangan Cairan Tubuh Melebihi Asupan." },
  { "en": "Apa Itu Hipoglikemia Kadar Gula Rendah?", "id": "Kadar Gula Darah Di Bawah Normal." },
  { "en": "Apa Itu Polimialgia Reumatika Nyeri Otot?", "id": "Gangguan Radang, Nyeri, Kekakuan Otot." },
  { "en": "Apa Arti Akhiran '-Dipsia' Dalam Medis?", "id": "Berkaitan Dengan Rasa Haus Atau Minum." },
  { "en": "Apa Itu Anemia Aplastik Kegagalan Sumsum?", "id": "Sumsum Tulang Gagal Produksi Sel." },
  { "en": "Apa Itu Hipokalemia Kadar Kalium Rendah?", "id": "Kadar Kalium Darah Di Bawah." },
  { "en": "Apa Itu Ketoasidosis Diabetik Komplikasi Diabetes?", "id": "Komplikasi Serius, Tubuh Produksi Asam." },
  { "en": "Apa Itu Artritis Septik Infeksi Sendi?", "id": "Infeksi Menyakitkan Pada Sendi Manusia." },
  { "en": "Apa Arti Akhiran '-Emia' Dalam Medis?", "id": "Kondisi Darah Atau Adanya Zat." },
  { "en": "Apa Itu Mieloma Multipel Kanker Sumsum?", "id": "Kanker Sel Plasma Sumsum Tulang." },
  { "en": "Apa Itu Hiperkalemia Kadar Kalium Tinggi?", "id": "Kadar Kalium Darah Di Atas." },
  { "en": "Apa Itu Sindrom Metabolik Faktor Risiko?", "id": "Kumpulan Kondisi, Tingkatkan Risiko Penyakit." },
  { "en": "Apa Itu Demam Mediterania Familial (FMF)?", "id": "Penyakit Genetik, Demam Berulang, Peradangan." },
  { "en": "Apa Arti Akhiran '-Kinesia' Dalam Medis?", "id": "Berkaitan Dengan Gerakan Atau Aktivitas." },
  { "en": "Apa Itu Waldenstrom Macroglobulinemia Kanker Langka?", "id": "Jenis Limfoma Non-Hodgkin Sangat Langka." },
  { "en": "Apa Itu Hiponatremia Kadar Natrium Rendah?", "id": "Kadar Natrium Darah Di Bawah." },
  { "en": "Apa Itu Retinopati Diabetik Kerusakan Retina?", "id": "Komplikasi Diabetes, Mempengaruhi Mata, Retina." },
  { "en": "Apa Itu Amiloidosis Penumpukan Protein Abnormal?", "id": "Penumpukan Protein Amiloid Di Organ." },
  { "en": "Apa Arti Akhiran '-Lepsy' Dalam Medis?", "id": "Berarti Kejang Atau Serangan Mendadak." },
  { "en": "Apa Itu Limfoma Hodgkin Kanker Limfatik?", "id": "Kanker Sistem Limfatik, Sel Abnormal." },
  { "en": "Apa Itu Hipernatremia Kadar Natrium Tinggi?", "id": "Kadar Natrium Darah Di Atas." },
  { "en": "Apa Itu Nefropati Diabetik Kerusakan Ginjal?", "id": "Kerusakan Ginjal Akibat Penyakit Diabetes." },
  { "en": "Apa Itu Sarkoidosis Penyakit Peradangan Granuloma?", "id": "Pertumbuhan Benjolan Peradangan (Granuloma) Kecil." },
  { "en": "Apa Arti Akhiran '-Orexia' Dalam Medis?", "id": "Berkaitan Dengan Nafsu Makan Manusia." },
  { "en": "Apa Itu Limfoma Non-Hodgkin Kanker Limfatik?", "id": "Kelompok Kanker Darah Sistem Limfatik." },
  { "en": "Apa Itu Hipokalsemia Kadar Kalsium Rendah?", "id": "Kadar Kalsium Darah Di Bawah." },
  { "en": "Apa Itu Neuropati Diabetik Kerusakan Saraf?", "id": "Jenis Kerusakan Saraf Akibat Diabetes." },
  { "en": "Apa Itu Hemokromatosis Penumpukan Zat Besi?", "id": "Tubuh Menyerap Terlalu Banyak Zat." },
  { "en": "Apa Arti Akhiran '-Paresis' Dalam Medis?", "id": "Kelemahan Sebagian Atau Kelumpuhan Ringan." },
  { "en": "Apa Itu Sindrom Mielodisplastik (MDS) Kelainan?", "id": "Kelompok Kelainan Sel Darah Terganggu." },
  { "en": "Apa Itu Hiperkalsemia Kadar Kalsium Tinggi?", "id": "Kadar Kalsium Darah Di Atas." },
  { "en": "Apa Itu Kaki Diabetik Masalah Kaki?", "id": "Masalah Kaki Akibat Penyakit Diabetes." },
  { "en": "Apa Itu Penyakit Wilson Penumpukan Tembaga?", "id": "Kelainan Genetik, Tembaga Menumpuk Tubuh." },
  { "en": "Apa Arti Akhiran '-Phagia' Dalam Medis?", "id": "Berkaitan Dengan Makan Atau Menelan." },
  { "en": "Apa Itu Trombositosis Esensial (ET) Kelainan?", "id": "Sumsum Tulang Produksi Terlalu Banyak." },
  { "en": "Apa Itu Hipomagnesemia Magnesium Darah Rendah?", "id": "Kadar Magnesium Rendah Dalam Darah." },
  { "en": "Apa Itu Hiperglikemia Kadar Gula Tinggi?", "id": "Kadar Gula Darah Di Atas." },
  { "en": "Apa Itu Porfiria Kelainan Enzim Genetik?", "id": "Kelompok Kelainan, Penumpukan Zat Kimia." },
  { "en": "Apa Arti Akhiran '-Phasia' Dalam Medis?", "id": "Berkaitan Dengan Kemampuan Berbicara Manusia." },
  { "en": "Apa Itu Mielofibrosis Primer Kelainan Sumsum?", "id": "Jenis Kanker Darah Langka Progresif." },
  { "en": "Apa Itu Hipermagnesemia Magnesium Darah Tinggi?", "id": "Kadar Magnesium Tinggi Dalam Darah." },
  { "en": "Apa Itu Glikosuria Glukosa Dalam Urin?", "id": "Ekskresi Glukosa Ke Dalam Urin." },
  { "en": "Apa Itu Defisiensi Alfa-1 Antitripsin (AATD)?", "id": "Kondisi Genetik, Risiko Penyakit Paru." },
  { "en": "Apa Arti Akhiran '-Pnea' Dalam Medis?", "id": "Berkaitan Dengan Pernapasan Atau Bernapas." },
  { "en": "Apa Itu Gangguan Bipolare Mania Depresi?", "id": "Perubahan Suasana Hati Ekstrem, Episode." },
  { "en": "Apa Itu Asidosis Kondisi Darah Asam?", "id": "Kondisi Keasaman Berlebih Cairan Tubuh." },
  { "en": "Apa Itu Ketonuria Adanya Keton Urin?", "id": "Adanya Badan Keton Dalam Urin." },
  { "en": "Apa Itu Ataksia Friedreich Kelainan Genetik?", "id": "Penyakit Genetik, Kerusakan Sistem Saraf." },
  { "en": "Apa Arti Akhiran '-Praxia' Dalam Medis?", "id": "Berkaitan Dengan Tindakan Atau Performa." },
  { "en": "Apa Itu Gangguan Siklotimik Versi Ringan?", "id": "Versi Ringan Gangguan Bipolar, Mood." },
  { "en": "Apa Itu Alkalosis Kondisi Darah Basa?", "id": "Kondisi Kebasaan Berlebih Cairan Tubuh." },
  { "en": "Apa Itu Proteinuria Adanya Protein Urin?", "id": "Jumlah Protein Berlebih Dalam Urin." },
  { "en": "Apa Itu Korea Huntington Penyakit Genetik?", "id": "Penyakit Genetik, Kerusakan Sel Saraf." },
  { "en": "Apa Arti Akhiran '-Stasis' Dalam Medis?", "id": "Penghentian Aliran Cairan, Darah, Normal." },
  { "en": "Apa Itu Gangguan Depresi Mayor (MDD)?", "id": "Gangguan Mood, Kesedihan, Kehilangan Minat." },
  { "en": "Apa Itu Hipoksia Kekurangan Oksigen Jaringan?", "id": "Kondisi Kekurangan Pasokan Oksigen Jaringan." },
  { "en": "Apa Itu Piuria Adanya Nanah Urin?", "id": "Adanya Sel Darah Putih Urin." },
  { "en": "Apa Itu Ataksia Telangiektasia Penyakit Langka?", "id": "Penyakit Genetik Langka, Mempengaruhi Saraf." },
  { "en": "Apa Arti Akhiran '-Therapy' Dalam Medis?", "id": "Berarti Perawatan Atau Pengobatan Penyakit." },
  { "en": "Apa Itu Distimia Gangguan Depresi Persisten?", "id": "Bentuk Depresi Kronis Jangka Panjang." },
  { "en": "Apa Itu Hipoksemia Oksigen Darah Rendah?", "id": "Tingkat Oksigen Rendah Dalam Darah." },
  { "en": "Apa Itu Oliguria Produksi Urin Sedikit?", "id": "Produksi Urin Lebih Sedikit Normal." },
  { "en": "Apa Itu Sindrom Marfan Jaringan Ikat?", "id": "Kelainan Genetik Mempengaruhi Jaringan Ikat." },
  { "en": "Apa Arti Akhiran '-Trophy' Dalam Medis?", "id": "Berkaitan Dengan Nutrisi, Pertumbuhan, Ukuran." },
  { "en": "Apa Itu Gangguan Afektif Musiman (SAD)?", "id": "Depresi Terkait Perubahan Musim Tertentu." },
  { "en": "Apa Itu Iskemia Aliran Darah Terbatas?", "id": "Pembatasan Pasokan Darah Ke Jaringan." },
  { "en": "Apa Itu Anuria Kegagalan Produksi Urin?", "id": "Ginjal Gagal Memproduksi Urin Harian." },
  { "en": "Apa Itu Neurofibromatosis Pertumbuhan Tumor Saraf?", "id": "Kelainan Genetik, Pertumbuhan Tumor Jaringan." },
  { "en": "Apa Itu Skrining Neonatal Tes Bayi?", "id": "Tes Dilakukan Segera Setelah Bayi." },
  { "en": "Apa Itu Gangguan Panik Serangan Panik?", "id": "Serangan Rasa Takut Intens Tiba-Tiba." },
  { "en": "Apa Itu Infark Kematian Jaringan Lokal?", "id": "Kematian Jaringan Akibat Kekurangan Oksigen." },
  { "en": "Apa Itu Dislipidemia Kadar Lemak Abnormal?", "id": "Kadar Lemak Abnormal Dalam Darah." },
  { "en": "Apa Itu Distrofi Miotonik Penyakit Genetik?", "id": "Penyakit Genetik, Pengecilan Otot Progresif." },
  { "en": "Apa Itu Ensefalopati Kerusakan Fungsi Otak?", "id": "Istilah Umum Kerusakan Otak Abnormal." },
  { "en": "Apa Itu Gangguan Kecemasan Sosial (Fobia)?", "id": "Ketakutan, Kecemasan Situasi Sosial Berlebih." },
  { "en": "Apa Itu Sepsis Respon Peradangan Sistemik?", "id": "Respon Mengancam Nyawa Terhadap Infeksi." },
  { "en": "Apa Itu Aterosklerosis Pengerasan Arteri Plak?", "id": "Penumpukan Plak Di Dalam Arteri." },
  { "en": "Apa Itu Sklerosis Tuberosa Pertumbuhan Tumor?", "id": "Penyakit Genetik, Tumor Jinak Tumbuh." },
  { "en": "Apa Itu Afasia Gangguan Kemampuan Bahasa?", "id": "Kehilangan Kemampuan Memahami, Mengekspresikan Bahasa." },
  { "en": "Apa Itu Gangguan Kecemasan Umum (GAD)?", "id": "Kekhawatiran Berlebihan, Tidak Realistis, Kronis." },
  { "en": "Apa Itu Syok Keadaan Sirkulasi Gagal?", "id": "Kegagalan Sirkulasi, Aliran Darah Kurang." },
  { "en": "Apa Itu Arteriosklerosis Penebalan Dinding Arteri?", "id": "Penebalan, Pengerasan Dinding Arteri Manusia." },
  { "en": "Apa Itu Osteogenesis Imperfekta Tulang Rapuh?", "id": "Kelompok Kelainan Genetik, Tulang Mudah." },
  { "en": "Apa Itu Disartria Kesulitan Berbicara Jelas?", "id": "Kesulitan Mengucapkan Kata-Kata Dengan Jelas." },
  { "en": "Apa Itu Agorafobia Takut Ruang Terbuka?", "id": "Takut, Menghindari Tempat, Situasi Tertentu." },
  { "en": "Apa Itu Sindrom Disfungsi Organ Multipel?", "id": "Disfungsi Progresif Dua Organ Atau." },
  { "en": "Apa Itu Trombosis Pembentukan Gumpalan Darah?", "id": "Pembentukan Gumpalan Darah Pembuluh Darah." },
  { "en": "Apa Itu Sindrom Ehlers-Danlos (EDS) Genetik?", "id": "Kelompok Kelainan Genetik, Mempengaruhi Jaringan." },
  { "en": "Apa Itu Apraksia Gangguan Gerakan Terampil?", "id": "Ketidakmampuan Lakukan Gerakan Tujuan Tertentu." },
  { "en": "Apa Itu Fobia Spesifik Ketakutan Irasional?", "id": "Ketakutan Berlebihan, Tidak Rasional Obyek." },
  { "en": "Apa Itu Koagulasi Intravaskular Diseminata (KID)?", "id": "Aktivasi Luas Pembekuan Darah Sistemik." },
  { "en": "Apa Itu Embolisme Penyumbatan Pembuluh Darah?", "id": "Penyumbatan Arteri Oleh Gumpalan, Benda." },
  { "en": "Apa Itu Sindrom Noonan Kelainan Genetik?", "id": "Kelainan Genetik, Mencegah Perkembangan Normal." },
  { "en": "Apa Itu Agnosia Ketidakmampuan Mengenali Objek?", "id": "Kehilangan Kemampuan Mengenali Objek, Orang." },
  { "en": "Apa Itu Gangguan Penyesuaian Stres Emosional?", "id": "Kesulitan Mengatasi Peristiwa Hidup Stres." },
  { "en": "Apa Itu Anoksia Ketiadaan Oksigen Total?", "id": "Kondisi Ketiadaan Pasokan Oksigen Total." },
  { "en": "Apa Itu Aneurisma Pelebaran Pembuluh Darah?", "id": "Tonjolan Abnormal Dinding Pembuluh Darah." },
  { "en": "Apa Itu Sindrom Prader-Willi Kelainan Genetik?", "id": "Kelainan Genetik, Rasa Lapar Konstan." },
  { "en": "Apa Itu Amnesia Kehilangan Ingatan Parsial?", "id": "Kehilangan Ingatan, Fakta, Informasi, Pengalaman." },
  { "en": "Apa Itu Ataksia Kesulitan Koordinasi Gerakan?", "id": "Gangguan Gerakan Otot Sukarela Terkoordinasi." },
  { "en": "Apa Itu Gangguan Identitas Disosiatif (DID)?", "id": "Memiliki Dua Atau Lebih Kepribadian." },
  { "en": "Apa Itu Sianosis Warna Kulit Biru?", "id": "Warna Kebiruan Akibat Oksigen Kurang." },
  { "en": "Apa Itu Flebitis Atau Radang Vena?", "id": "Peradangan Pada Pembuluh Darah Vena." },
  { "en": "Apa Itu Sindrom Angelman Kelainan Genetik?", "id": "Kelainan Genetik, Keterlambatan Perkembangan, Saraf." },
  { "en": "Apa Itu Hemianopsia Kehilangan Setengah Lapang?", "id": "Kehilangan Penglihatan Setengah Lapang Pandang." },
  { "en": "Apa Itu Gangguan Konversi Gejala Neurologis?", "id": "Kondisi Mental, Gejala Sistem Saraf." },
  { "en": "Apa Itu Kongesti Penumpukan Cairan Berlebih?", "id": "Penumpukan Cairan Berlebihan Pembuluh Darah." },
  { "en": "Apa Itu Tromboflebitis Gumpalan Darah Vena?", "id": "Peradangan Vena Akibat Gumpalan Darah." },
  { "en": "Apa Itu Sindrom Williams Kelainan Genetik?", "id": "Kelainan Genetik, Masalah Perkembangan, Jantung." },
  { "en": "Apa Itu Parestesia Sensasi Abnormal Tusukan?", "id": "Sensasi Terbakar, Tertusuk Jarum, Kesemutan." },
  { "en": "Apa Itu Gangguan Buatan (Factitious Disorder)?", "id": "Sengaja Bertindak Sakit Tanpa Keuntungan." },
  { "en": "Apa Itu Hiperemia Peningkatan Aliran Darah?", "id": "Peningkatan Aliran Darah Ke Jaringan." },
  { "en": "Apa Itu Insufisiensi Vena Kronis (CVI)?", "id": "Vena Tungkai Sulit Kembalikan Darah." },
  { "en": "Apa Itu Penyakit Von Hippel-Lindau (VHL)?", "id": "Kelainan Genetik, Tumor, Kista Tumbuh." },
  { "en": "Apa Itu Tremor Gerakan Gemetar Involunter?", "id": "Gerakan Gemetar, Tidak Disengaja, Berirama." },
  { "en": "Apa Itu Gangguan Somatisasi Keluhan Fisik?", "id": "Fokus Ekstrem Gejala Fisik, Stres." },
  { "en": "Apa Itu Perdarahan Keluarnya Darah Pembuluh?", "id": "Keluarnya Darah Dari Sistem Peredaran." },
  { "en": "Apa Itu Teleangiektasia Pelebaran Pembuluh Darah?", "id": "Pelebaran Pembuluh Darah Kecil Kulit." },
  { "en": "Apa Itu Incontinentia Pigmenti Kelainan Genetik?", "id": "Kelainan Genetik, Mempengaruhi Kulit, Rambut." },
  { "en": "Apa Itu Distonia Kontraksi Otot Abnormal?", "id": "Kontraksi Otot Berulang, Gerakan Memutar." },
  { "en": "Apa Itu Hipokondriasis (Gangguan Kecemasan Penyakit)?", "id": "Kecemasan Berlebihan Memiliki Penyakit Serius." },
  { "en": "Apa Itu Hematoma Kumpulan Darah Lokal?", "id": "Kumpulan Darah Di Luar Pembuluh." },
  { "en": "Apa Itu Sindrom Klippel-Trenaunay (KTS) Langka?", "id": "Kelainan Pembuluh Darah, Pertumbuhan Tulang." },
  { "en": "Apa Itu Sindrom Rett Kelainan Genetik?", "id": "Kelainan Genetik Saraf, Perkembangan Langka." },
  { "en": "Apa Itu Mioklonus Sentakan Otot Tiba-Tiba?", "id": "Sentakan, Kedutan Otot Singkat Tiba-Tiba." },
  { "en": "Apa Itu Gangguan Dismorfik Tubuh (BDD)?", "id": "Kecacatan Penampilan, Cacat Sangat Kecil." },
  { "en": "Apa Itu Petekie Bintik Perdarahan Kecil?", "id": "Bintik Merah Kecil Akibat Pendarahan." },
  { "en": "Apa Itu Malformasi Arteri Vena (AVM)?", "id": "Kusutnya Pembuluh Darah Abnormal Otak." },
  { "en": "Apa Itu Xeroderma Pigmentosum (XP) Genetik?", "id": "Kelainan Genetik, Sensitivitas Ekstrem Sinar." },
  { "en": "Apa Itu Chorea Gerakan Menyentak Tak Teratur?", "id": "Gerakan Tak Disengaja, Tak Teratur." },
  { "en": "Apa Itu Kleptomania Dorongan Mencuri Berulang?", "id": "Gagal Menahan Dorongan Untuk Mencuri." },
  { "en": "Apa Itu Purpura Bintik Perdarahan Lebih?", "id": "Bintik Ungu Akibat Pendarahan Pembuluh." },
  { "en": "Apa Itu Hemangioma Pertumbuhan Pembuluh Darah?", "id": "Tanda Lahir Jinak, Pertumbuhan Pembuluh." },
  { "en": "Apa Itu Progeria Penuaan Dini Anak?", "id": "Kelainan Genetik, Penuaan Cepat Anak." },
  { "en": "Apa Itu Hemiballismus Gerakan Melempar Keras?", "id": "Gerakan Melempar, Mengayun Keras Tiba-Tiba." },
  { "en": "Apa Itu Piromania Dorongan Sengaja Menyalakan?", "id": "Dorongan Kuat Menyalakan Api, Kebakaran." },
  { "en": "Apa Itu Ekimosis Atau Memar Besar?", "id": "Perubahan Warna Kulit Akibat Pendarahan." },
  { "en": "Apa Itu Limfangioma Pertumbuhan Pembuluh Limfe?", "id": "Malformasi Sistem Limfatik, Kista Cair." },
  { "en": "Apa Itu Sindrom Cockayne Kelainan Genetik?", "id": "Kelainan Genetik, Penuaan Dini, Kerdil." },
  { "en": "Apa Itu Bradikinesia Perlambatan Gerakan Tubuh?", "id": "Gerakan Tubuh Sangat Lambat, Progresif." },
  { "en": "Apa Itu Trikotilomania Dorongan Cabut Rambut?", "id": "Dorongan Berulang Mencabut Rambut Sendiri." },
  { "en": "Apa Itu Hemostasis Proses Penghentian Pendarahan?", "id": "Proses Alami Menghentikan Aliran Darah." },
  { "en": "Apa Itu Ateroma Plak Kuning Arteri?", "id": "Penumpukan Material Lemak Dinding Arteri." },
  { "en": "Apa Itu Disautonomia Gangguan Saraf Otonom?", "id": "Gangguan Sistem Saraf Otonom Manusia." },
  { "en": "Apa Itu Ensefalitis Radang Jaringan Otak?", "id": "Peradangan Akut Pada Jaringan Otak." },
  { "en": "Apa Itu Gangguan Kepribadian Antisosial (ASPD)?", "id": "Pola Perilaku Mengabaikan Hak Orang." },
  { "en": "Apa Itu Koagulasi Proses Pembekuan Darah?", "id": "Proses Darah Berubah Jadi Gumpalan." },
  { "en": "Apa Itu Xantelasma Deposit Kolesterol Kelopak?", "id": "Deposit Kekuningan Kolesterol Bawah Kulit." },
  { "en": "Apa Itu Atrofi Otot Multisistem (MSA)?", "id": "Gangguan Neurodegeneratif Langka, Mempengaruhi Fungsi." },
  { "en": "Apa Itu Mielitis Radang Sumsum Tulang?", "id": "Peradangan Pada Sumsum Tulang Belakang." },
  { "en": "Apa Itu Gangguan Kepribadian Ambang (BPD)?", "id": "Ketidakstabilan Suasana Hati, Perilaku, Hubungan." },
  { "en": "Apa Itu Fibrinolisis Pemecahan Gumpalan Fibrin?", "id": "Proses Mencegah Gumpalan Darah Tumbuh." },
  { "en": "Apa Itu Xanthoma Deposit Lemak Kulit?", "id": "Deposit Lemak Bawah Permukaan Kulit." },
  { "en": "Apa Itu Kelumpuhan Supranuklear Progresif (PSP)?", "id": "Gangguan Otak, Keseimbangan, Gerakan Mata." },
  { "en": "Apa Itu Ensefalomielitis Radang Otak Sumsum?", "id": "Peradangan Otak Dan Sumsum Tulang." },
  { "en": "Apa Itu Gangguan Kepribadian Histrionik (HPD)?", "id": "Pola Perilaku Mencari Perhatian Berlebihan." },
  { "en": "Apa Itu Faktor Koagulasi Protein Pembekuan?", "id": "Protein Dalam Darah, Membantu Pembekuan." },
  { "en": "Apa Itu Lipoma Tumor Jinak Jaringan?", "id": "Benjolan Lemak Tumbuh Lambat, Jinak." },
  { "en": "Apa Itu Degenerasi Kortikobasal (CBD) Langka?", "id": "Penyakit Neurodegeneratif, Sel Saraf Mati." },
  { "en": "Apa Itu Abses Otak Kumpulan Nanah?", "id": "Kumpulan Nanah Akibat Infeksi Otak." },
  { "en": "Apa Itu Gangguan Kepribadian Narsistik (NPD)?", "id": "Pola Keagungan, Kebutuhan Akan Pujian." },
  { "en": "Apa Itu Jalur Intrinsik Pembekuan Darah?", "id": "Dipicu Ketika Darah Kontak Permukaan." },
  { "en": "Apa Itu Nevus Atau Tahi Lalat?", "id": "Istilah Medis Untuk Tahi Lalat." },
  { "en": "Apa Itu Penyakit Creutzfeldt-Jakob (CJD) Fatal?", "id": "Gangguan Otak Degeneratif, Fatal, Langka." },
  { "en": "Apa Itu Hidrosefalus Penumpukan Cairan Otak?", "id": "Penumpukan Cairan Serebrospinal Dalam Otak." },
  { "en": "Apa Itu Gangguan Kepribadian Menghindar (AvPD)?", "id": "Pola Hambatan Sosial, Perasaan Rendah." },
  { "en": "Apa Itu Jalur Ekstrinsik Pembekuan Darah?", "id": "Dipicu Oleh Kerusakan Jaringan Pembuluh." },
  { "en": "Apa Itu Keratosis Seboroik Pertumbuhan Kulit?", "id": "Pertumbuhan Kulit Umum Non-Kanker Usia." },
  { "en": "Apa Itu Kuru Penyakit Prion Langka?", "id": "Penyakit Neurodegeneratif Langka Akibat Prion." },
  { "en": "Apa Itu Peningkatan Tekanan Intrakranial (TIK)?", "id": "Tekanan Tinggi Dalam Tempurung Kepala." },
  { "en": "Apa Itu Gangguan Kepribadian Dependen (DPD)?", "id": "Kebutuhan Psikologis Dirawat Orang Lain." },
  { "en": "Apa Itu Protrombin Protein Pembekuan Darah?", "id": "Faktor Pembekuan Dibuat Hati Manusia." },
  { "en": "Apa Itu Keratosis Aktinik Bercak Kulit?", "id": "Bercak Kasar, Bersisik Akibat Matahari." },
  { "en": "Apa Itu Insomnia Familial Fatal (FFI)?", "id": "Penyakit Prion Genetik Sangat Langka." },
  { "en": "Apa Itu Herniasi Otak Jaringan Bergeser?", "id": "Jaringan Otak Bergeser Posisi Normal." },
  { "en": "Apa Itu Gangguan Kepribadian Obsesif-Kompulsif (OCPD)?", "id": "Preokupasi Keteraturan, Perfeksionisme, Kontrol Ketat." },
  { "en": "Apa Itu Trombin Enzim Pembekuan Darah?", "id": "Enzim Mengubah Fibrinogen Menjadi Fibrin." },
  { "en": "Apa Itu Dermatofibroma Benjolan Kulit Umum?", "id": "Benjolan Kecil, Jinak, Tumbuh Kulit." },
  { "en": "Apa Itu Sindrom Gerstmann-Straussler-Scheinker (GSS)?", "id": "Penyakit Neurodegeneratif Otak Sangat Langka." },
  { "en": "Apa Itu Edema Serebral Pembengkakan Otak?", "id": "Kelebihan Akumulasi Cairan Jaringan Otak." },
  { "en": "Apa Itu Gangguan Kepribadian Paranoid (PPD)?", "id": "Ketidakpercayaan, Kecurigaan Berlebihan Orang Lain." },
  { "en": "Apa Itu Fibrin Protein Pembentuk Bekuan?", "id": "Protein Serat, Membentuk Jaring Gumpalan." },
  { "en": "Apa Itu Kista Epidermoid Benjolan Kulit?", "id": "Benjolan Jinak, Tumbuh Lambat, Bawah." },
  { "en": "Apa Itu Sindrom Tourette Gangguan Tic?", "id": "Gangguan Saraf, Gerakan, Ucapan Berulang." },
  { "en": "Apa Itu Delirium Gangguan Mental Akut?", "id": "Perubahan Kesadaran, Kebingungan Mendadak Parah." },
  { "en": "Apa Itu Gangguan Kepribadian Skizoid (SPD)?", "id": "Pola Pelepasan Diri, Ekspresi Emosi." },
  { "en": "Apa Itu Vitamin K Peran Pembekuan?", "id": "Penting Sintesis Protein Pembekuan Darah." },
  { "en": "Apa Itu Milia Bintik Putih Kecil?", "id": "Benjolan Kecil, Putih, Biasanya Wajah." },
  { "en": "Apa Itu Adrenoleukodistrofi (ALD) Kelainan Genetik?", "id": "Kelainan Genetik, Merusak Mielin Saraf." },
  { "en": "Apa Itu Ataksia Serebelar Gangguan Koordinasi?", "id": "Ataksia Disebabkan Kerusakan Otak Kecil." },
  { "en": "Apa Itu Gangguan Kepribadian Skizotipal (STPD)?", "id": "Ketidaknyamanan Akut, Pola Pikir Eksentrik." },
  { "en": "Apa Itu Plasmin Enzim Pemecah Bekuan?", "id": "Enzim Mendegradasi Fibrin Gumpalan Darah." },
  { "en": "Apa Itu Kista Pilar Benjolan Kulit?", "id": "Kista Jinak, Berasal Folikel Rambut." },
  { "en": "Apa Itu Sklerosis Lateral Amiotrofik (ALS)?", "id": "Penyakit Sistem Saraf, Melemahkan Otot." },
  { "en": "Apa Itu Demensia Kebingungan Dan Disorientasi?", "id": "Penurunan Fungsi Kognitif, Memori, Berpikir." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Imun?", "id": "Dokter Spesialis Alergi Dan Imunologi." },
  { "en": "Apa Itu Tissue Plasminogen Activator (tPA)?", "id": "Protein Terlibat Pemecahan Gumpalan Darah." },
  { "en": "Apa Itu Granuloma Piogenik Pertumbuhan Kulit?", "id": "Pertumbuhan Jinak, Cepat, Pembuluh Darah." },
  { "en": "Apa Itu Atrofi Otot Spinal (SMA)?", "id": "Penyakit Genetik, Merusak Sel Saraf." },
  { "en": "Apa Itu Stupor Kondisi Tidak Responsif?", "id": "Kurangnya Fungsi Kognitif, Kesadaran Rendah." },
  { "en": "Apa Itu Uveitis Radang Lapisan Tengah?", "id": "Peradangan Uvea, Lapisan Tengah Mata." },
  { "en": "Apa Itu Antikoagulan Obat Pengencer Darah?", "id": "Mencegah Pembentukan Gumpalan Darah Berbahaya." },
  { "en": "Apa Itu Hemangioma Ceri Bintik Merah?", "id": "Benjolan Kulit Umum, Non-Kanker, Merah." },
  { "en": "Apa Itu Sindrom Pasca-Polio (PPS) Gejala?", "id": "Kondisi Mempengaruhi Penyintas Penyakit Polio." },
  { "en": "Apa Itu Sinkop Vasovagal Pingsan Umum?", "id": "Pingsan Akibat Pemicu, Penurunan Detak." },
  { "en": "Apa Itu Iritis Radang Iris Mata?", "id": "Peradangan Pada Iris Mata Berwarna." },
  { "en": "Apa Itu Obat Antiplatelet Pencegah Bekuan?", "id": "Mencegah Trombosit Saling Menempel, Membeku." },
  { "en": "Apa Itu Tanda Lahir (Nevus Kongenital)?", "id": "Tahi Lalat Ada Sejak Lahir." },
  { "en": "Apa Itu Mielopati Transversal Radang Sumsum?", "id": "Peradangan Kedua Sisi Sumsum Tulang." },
  { "en": "Apa Itu Kejang Tonik-Klonik (Grand Mal)?", "id": "Jenis Kejang, Kehilangan Kesadaran, Konvulsi." },
  { "en": "Apa Itu Skleritis Radang Dinding Putih?", "id": "Peradangan Parah, Merusak Dinding Putih." },
  { "en": "Apa Itu Heparin Antikoagulan Suntik Cepat?", "id": "Obat Pengencer Darah, Bekerja Cepat." },
  { "en": "Apa Itu Bintik Mongolian Tanda Lahir?", "id": "Tanda Lahir Datar, Kebiruan, Umum." },
  { "en": "Apa Itu Sindrom Cauda Equina Kompresi?", "id": "Kompresi Saraf Ujung Bawah Tulang." },
  { "en": "Apa Itu Kejang Absans (Petit Mal)?", "id": "Kejang, Gangguan Kesadaran Singkat, Tatapan." },
  { "en": "Apa Itu Episkleritis Radang Jaringan Tipis?", "id": "Peradangan Jaringan Antara Sklera, Konjungtiva." },
  { "en": "Apa Itu Warfarin Antikoagulan Oral Umum?", "id": "Obat Pengencer Darah, Mengganggu Vitamin." },
  { "en": "Apa Itu Tanda Lahir Port-Wine Noda?", "id": "Perubahan Warna Merah Muda, Merah." },
  { "en": "Apa Itu Radikulopati Saraf Terjepit Tulang?", "id": "Kompresi Atau Iritasi Saraf Tulang." },
  { "en": "Apa Itu Kejang Mioklonik Sentakan Otot?", "id": "Kejang, Sentakan, Kedutan Otot Singkat." },
  { "en": "Apa Itu Keratitis Radang Kornea Mata?", "id": "Peradangan Kornea, Bagian Depan Mata." },
  { "en": "Apa Itu Aspirin Dosis Rendah Pencegahan?", "id": "Mencegah Serangan Jantung, Stroke, Gumpalan." },
  { "en": "Apa Itu Kalazion Benjolan Kelopak Mata?", "id": "Benjolan Kecil, Lambat, Akibat Kelenjar." },
  { "en": "Apa Itu Neuropati Otonom Kerusakan Saraf?", "id": "Kerusakan Saraf Mengontrol Fungsi Otomatis." },
  { "en": "Apa Itu Kejang Atonik Kehilangan Tonus?", "id": "Kejang, Kehilangan Tonus Otot Tiba-Tiba." },
  { "en": "Apa Itu Ulkus Kornea Luka Kornea?", "id": "Luka Terbuka Pada Bagian Kornea." },
  { "en": "Apa Itu Klopidogrel Obat Antiplatelet Umum?", "id": "Obat Mencegah Gumpalan Darah Berbahaya." },
  { "en": "Apa Itu Hordeolum Atau Bintitan Merah?", "id": "Infeksi Kelenjar Minyak Kelopak Mata." },
  { "en": "Apa Itu Mononeuropati Kerusakan Satu Saraf?", "id": "Kerusakan Terjadi Pada Satu Saraf." },
  { "en": "Apa Itu Status Epileptikus Kejang Berkelanjutan?", "id": "Kejang Tunggal, Berlangsung Lima Menit." },
  { "en": "Apa Itu Abrasi Kornea Goresan Kornea?", "id": "Goresan Atau Kikisan Permukaan Kornea." },
  { "en": "Apa Itu Anemia Defisiensi Besi (ADB)?", "id": "Anemia Akibat Kurangnya Zat Besi." },
  { "en": "Apa Itu Blefaritis Radang Kelopak Mata?", "id": "Peradangan Pada Kelopak Mata Manusia." },
  { "en": "Apa Itu Mononeuritis Multipleks Kerusakan Saraf?", "id": "Kerusakan Dua Saraf Terpisah Serentak." },
  { "en": "Apa Itu Aura Dalam Konteks Kejang?", "id": "Sensasi Peringatan Sebelum Kejang Terjadi." },
  { "en": "Apa Itu Erosi Kornea Berulang (RCE)?", "id": "Lapisan Luar Kornea Lepas Berulang." },
  { "en": "Apa Itu Anemia Defisiensi Folat B9?", "id": "Anemia Akibat Kurangnya Vitamin B9." },
  { "en": "Apa Itu Ptosis Kelopak Mata Turun?", "id": "Penurunan Kelopak Mata Atas Abnormal." },
  { "en": "Apa Itu Polineuropati Kerusakan Banyak Saraf?", "id": "Kerusakan Beberapa Saraf Tepi Sekaligus." },
  { "en": "Apa Itu Periode Pascaiktal Fase Pemulihan?", "id": "Fase Pemulihan Setelah Kejang Selesai." },
  { "en": "Apa Itu Keratoconus Kornea Menipis Menonjol?", "id": "Kornea Menipis, Menonjol Keluar Kerucut." },
  { "en": "Apa Itu Anemia Defisiensi B12 Pernisiosa?", "id": "Anemia, Kekurangan Vitamin B12, Absorpsi." },
  { "en": "Apa Itu Entropion Kelopak Mata Melipat?", "id": "Kelopak Mata Melipat Ke Arah." },
  { "en": "Apa Itu Neuralgia Nyeri Tajam Saraf?", "id": "Nyeri Tajam, Menusuk Sepanjang Jalur." },
  { "en": "Apa Itu Kejang Demam Pada Anak?", "id": "Kejang Anak Akibat Demam Tinggi." },
  { "en": "Apa Itu Distrofi Fuchs Kornea Bengkak?", "id": "Penyakit Mata, Kornea Bengkak, Penglihatan." },
  { "en": "Apa Itu Anemia Hemolitik Sel Hancur?", "id": "Sel Darah Merah Hancur Cepat." },
  { "en": "Apa Itu Ektropion Kelopak Mata Keluar?", "id": "Kelopak Mata Membalik Ke Arah." },
  { "en": "Apa Itu Neuralgia Trigeminal Nyeri Wajah?", "id": "Nyeri Kronis Mempengaruhi Saraf Trigeminal." },
  { "en": "Apa Itu Skotoma Titik Buta Penglihatan?", "id": "Titik Buta Lapang Pandang Manusia." },
  { "en": "Apa Itu Sindrom Lisis Tumor (TLS)?", "id": "Komplikasi Kanker, Sel Kanker Hancur." },
  { "en": "Apa Itu Dakriosistitis Radang Kantung Air?", "id": "Infeksi Kantung Air Mata Manusia." },
  { "en": "Apa Itu Neuralgia Pasca-Herpetik Nyeri Saraf?", "id": "Komplikasi Herpes Zoster, Nyeri Saraf." },
  { "en": "Apa Itu Diplopia Atau Penglihatan Ganda?", "id": "Melihat Dua Gambar Satu Objek." },
  { "en": "Apa Itu Neutropenia Febrile Demam Neutrofil?", "id": "Demam, Penurunan Jumlah Neutrofil Darah." },
  { "en": "Apa Itu Selulitis Orbital Infeksi Jaringan?", "id": "Infeksi Jaringan Lemak, Otot Mata." },
  { "en": "Apa Itu Neuropati Perifer Idiopatik Penyebab?", "id": "Neuropati Tepi Tanpa Penyebab Jelas." },
  { "en": "Apa Itu Fotofobia Sensitivitas Terhadap Cahaya?", "id": "Sensitivitas Mata Berlebihan Terhadap Cahaya." },
  { "en": "Apa Itu Sindrom Vena Kava Superior?", "id": "Penyumbatan Vena Kava Superior, Darah." },
  { "en": "Apa Itu Oftalmoplegia Kelumpuhan Otot Mata?", "id": "Kelumpuhan Satu Atau Lebih Otot." },
  { "en": "Apa Itu Meralgia Paresthetica Saraf Terjepit?", "id": "Saraf Terjepit, Kesemutan, Mati Rasa." },
  { "en": "Apa Itu Nistagmus Gerakan Mata Involunter?", "id": "Gerakan Mata Cepat, Tidak Terkendali." },
  { "en": "Apa Itu Hiperviskositas Sindrom Darah Kental?", "id": "Darah Mengental, Aliran Jadi Lambat." },
  { "en": "Apa Itu Hifema Darah Ruang Depan?", "id": "Darah Mengumpul Ruang Depan Mata." },
  { "en": "Apa Itu Kausalgia Nyeri Terbakar Kronis?", "id": "Nyeri Terbakar Hebat, Akibat Cedera." },
  { "en": "Apa Itu Ambliopia Atau Mata Malas?", "id": "Penglihatan Berkurang, Perkembangan Visual Abnormal." },
  { "en": "Apa Itu Kompresi Sumsum Tulang Belakang?", "id": "Tekanan Abnormal Sumsum Tulang Belakang." },
  { "en": "Apa Itu Perdarahan Subkonjungtiva Pembuluh Darah?", "id": "Pembuluh Darah Pecah Bawah Konjungtiva." },
  { "en": "Apa Itu Refleks Babinski Abnormal Jari?", "id": "Refleks Abnormal, Ibu Jari Kaki." },
  { "en": "Apa Itu Strabismus Atau Mata Juling?", "id": "Ketidaksejajaran Mata, Tidak Melihat Arah." },
  { "en": "Apa Itu Efusi Maligna Cairan Kanker?", "id": "Penumpukan Cairan Akibat Kanker Pleura." },
  { "en": "Apa Itu Pterigium Pertumbuhan Jaringan Konjungtiva?", "id": "Pertumbuhan Jaringan Daging Konjungtiva Kornea." },
  { "en": "Apa Itu Klonus Kontraksi Otot Ritmis?", "id": "Serangkaian Kontraksi Otot Tidak Disengaja." },
  { "en": "Apa Itu Miopia Atau Rabun Jauh?", "id": "Sulit Melihat Objek Jauh Jelas." },
  { "en": "Apa Itu Hiperkalsemia Maligna Kalsium Kanker?", "id": "Kadar Kalsium Darah Tinggi Kanker." },
  { "en": "Apa Itu Pinguecula Benjolan Kekuningan Konjungtiva?", "id": "Benjolan Jinak Kekuningan Pada Konjungtiva." },
  { "en": "Apa Itu Tanda Romberg Uji Keseimbangan?", "id": "Tes Digunakan Pemeriksaan Neurologis Keseimbangan." },
  { "en": "Apa Itu Hipermetropia Atau Rabun Dekat?", "id": "Sulit Melihat Objek Dekat Jelas." },
  { "en": "Apa Itu Nyeri Tulang Akibat Kanker?", "id": "Nyeri Akibat Kanker Menyebar Tulang." },
  { "en": "Apa Itu Ablasi Retina Lepasnya Retina?", "id": "Kondisi Darurat, Retina Lepas Belakang." },
  { "en": "Apa Itu Grafestesia Kemampuan Mengenali Tulisan?", "id": "Kemampuan Mengenali Angka, Huruf Ditulis." },
  { "en": "Apa Itu Astigmatisme Penglihatan Kabur Distorsi?", "id": "Ketidaksempurnaan Lengkungan Kornea, Lensa Mata." },
  { "en": "Apa Itu Limfadenopati Pembengkakan Kelenjar Getah?", "id": "Pembesaran Ukuran Kelenjar Getah Bening." },
  { "en": "Apa Itu Degenerasi Makula Terkait Usia?", "id": "Penyebab Utama Kehilangan Penglihatan Lansia." },
  { "en": "Apa Itu Stereognosis Kemampuan Mengenali Objek?", "id": "Kemampuan Mengenali Objek Melalui Perabaan." },
  { "en": "Apa Itu Presbiopia Kesulitan Melihat Dekat?", "id": "Kehilangan Kemampuan Fokus Jarak Dekat." },
  { "en": "Apa Itu Sindrom Paraneoplastik Respon Imun?", "id": "Gangguan Akibat Respon Imun Tumor." },
  { "en": "Apa Itu Splenomegali Pembesaran Organ Limpa?", "id": "Pembesaran Limpa Melampaui Ukuran Normal." },
  { "en": "Apa Itu Retinopati Hipertensif Kerusakan Retina?", "id": "Kerusakan Retina Akibat Tekanan Darah." },
  { "en": "Apa Itu Dismetria Kesulitan Mengukur Jarak?", "id": "Ketidakmampuan Menilai Jarak Gerakan Akurat." },
  { "en": "Apa Itu Floaters Bintik Hitam Penglihatan?", "id": "Bintik, Benang Melayang Lapang Pandang." },
  { "en": "Apa Itu Kakeksia Penurunan Berat Badan?", "id": "Penurunan Berat Badan, Otot Parah." },
  { "en": "Apa Itu Hepatomegali Pembesaran Organ Hati?", "id": "Pembesaran Hati Melampaui Ukuran Normal." },
  { "en": "Apa Itu Oklusi Arteri Retina Sentral?", "id": "Penyumbatan Arteri Utama Retina Darurat." },
  { "en": "Apa Itu Disdiadokokinesia Gerakan Cepat Abnormal?", "id": "Ketidakmampuan Lakukan Gerakan Bolak-Balik Cepat." },
  { "en": "Apa Itu Kilatan Cahaya (Fotopsia) Mata?", "id": "Persepsi Kilatan Cahaya Lapang Pandang." },
  { "en": "Apa Itu Demam Asal Tidak Diketahui?", "id": "Demam Berkelanjutan Tanpa Penyebab Jelas." },
  { "en": "Apa Itu Asites Penumpukan Cairan Perut?", "id": "Akumulasi Cairan Berlebih Rongga Perut." },
  { "en": "Apa Itu Oklusi Vena Retina Sentral?", "id": "Penyumbatan Vena Utama Retina Mata." },
  { "en": "Apa Itu Niat Tremor Gemetar Saat?", "id": "Gemetar Semakin Parah Saat Gerakan." },
  { "en": "Apa Itu Buta Warna (Akromatopsia) Total?", "id": "Ketidakmampuan Total Melihat Warna Apapun." },
  { "en": "Apa Itu Bakteremia Adanya Bakteri Darah?", "id": "Keberadaan Bakteri Dalam Aliran Darah." },
  { "en": "Apa Itu Jaundice Menguningnya Kulit, Mata?", "id": "Warna Kuning Kulit Akibat Bilirubin." },
  { "en": "Apa Itu Neuritis Optik Radang Saraf?", "id": "Peradangan Saraf Optik, Gangguan Penglihatan." },
  { "en": "Apa Itu Gaya Berjalan Ataksik Tidak?", "id": "Pola Berjalan Tidak Stabil, Terhuyung-huyung." },
  { "en": "Apa Itu Hemokromatosis Kelebihan Zat Besi?", "id": "Kelebihan Zat Besi Dalam Tubuh." },
  { "en": "Apa Itu Viremia Adanya Virus Darah?", "id": "Keberadaan Virus Dalam Aliran Darah." },
  { "en": "Apa Itu Ensefalopati Hepatik Fungsi Otak?", "id": "Penurunan Fungsi Otak Penyakit Hati." },
  { "en": "Apa Itu Papiledema Pembengkakan Diskus Optikus?", "id": "Pembengkakan Saraf Optik Akibat Tekanan." },
  { "en": "Apa Itu Disartria Slurred Speech Bicara?", "id": "Bicara Tidak Jelas, Lambat, Sulit." },
  { "en": "Apa Itu Hipogonadisme Penurunan Fungsi Seksual?", "id": "Penurunan Produksi Hormon Seksual Normal." },
  { "en": "Apa Itu Fungemia Adanya Jamur Darah?", "id": "Keberadaan Jamur Dalam Aliran Darah." },
  { "en": "Apa Itu Hipertensi Portal Tekanan Vena?", "id": "Tekanan Darah Tinggi Vena Portal." },
  { "en": "Apa Itu Atrofi Optik Kerusakan Saraf?", "id": "Kerusakan Saraf Optik, Kehilangan Penglihatan." },
  { "en": "Apa Itu Disfagia Kesulitan Menelan Makanan?", "id": "Kesulitan Atau Rasa Sakit Saat." },
  { "en": "Apa Itu Galaktorea Produksi ASI Abnormal?", "id": "Keluarnya ASI Tidak Terkait Kehamilan." },
  { "en": "Apa Itu Parasitemia Adanya Parasit Darah?", "id": "Keberadaan Parasit Dalam Aliran Darah." },
  { "en": "Apa Itu Varises Esofagus Vena Bengkak?", "id": "Pembuluh Vena Bengkak Dinding Esofagus." },
  { "en": "Apa Itu Koloboma Cacat Jaringan Mata?", "id": "Celah Atau Lubang Jaringan Mata." },
  { "en": "Apa Itu Afonia Kehilangan Suara Total?", "id": "Ketidakmampuan Total Untuk Menghasilkan Suara." },
  { "en": "Apa Itu Amenorea Tidak Adanya Menstruasi?", "id": "Tidak Terjadinya Periode Menstruasi Normal." },
  { "en": "Apa Itu Sepsis Sindrom Respon Inflamasi?", "id": "Respon Peradangan Seluruh Tubuh Infeksi." },
  { "en": "Apa Itu Koagulopati Gangguan Pembekuan Darah?", "id": "Gangguan Kemampuan Darah Untuk Membeku." },
  { "en": "Apa Itu Aniridia Tidak Adanya Iris?", "id": "Kondisi Langka, Tidak Adanya Iris." },
  { "en": "Apa Itu Disfonia Gangguan Kualitas Suara?", "id": "Gangguan Suara, Serak, Lemah, Tegang." },
  { "en": "Apa Itu Oligomenorea Siklus Menstruasi Jarang?", "id": "Siklus Menstruasi Tidak Teratur, Jarang." },
  { "en": "Apa Itu Syok Septik Tekanan Darah?", "id": "Tekanan Darah Turun Drastis Sepsis." },
  { "en": "Apa Itu Trombositosis Peningkatan Jumlah Trombosit?", "id": "Jumlah Trombosit Darah Di Atas." },
  { "en": "Apa Itu Heterokromia Perbedaan Warna Iris?", "id": "Perbedaan Warna Antara Kedua Iris." },
  { "en": "Apa Itu Parestesia Sensasi Abnormal Kulit?", "id": "Sensasi Kesemutan, Tertusuk, Mati Rasa." },
  { "en": "Apa Itu Menoragia Pendarahan Menstruasi Hebat?", "id": "Pendarahan Menstruasi Berlebihan, Berkepanjangan, Abnormal." },
  { "en": "Apa Itu Anestesi Umum Hilangnya Kesadaran?", "id": "Membuat Pasien Tidak Sadar Selama." },
  { "en": "Apa Itu Leukositosis Peningkatan Jumlah Leukosit?", "id": "Jumlah Sel Darah Putih Atas." },
  { "en": "Apa Itu Astrocytoma Tumor Otak Ganas?", "id": "Jenis Kanker Otak, Sel Astrosit." },
  { "en": "Apa Itu Anestesi Regional Mati Rasa?", "id": "Membuat Sebagian Besar Tubuh Mati." },
  { "en": "Apa Itu Dismenore Nyeri Menstruasi Hebat?", "id": "Nyeri Kram Perut Selama Menstruasi." },
  { "en": "Apa Itu Anestesi Lokal Mati Rasa?", "id": "Membuat Area Kecil Tubuh Mati." },
  { "en": "Apa Itu Eritrositosis Peningkatan Jumlah Eritrosit?", "id": "Peningkatan Jumlah Sel Darah Merah." },
  { "en": "Apa Itu Glioblastoma Tumor Otak Agresif?", "id": "Jenis Tumor Otak Paling Agresif." },
  { "en": "Apa Itu Sedasi Penurunan Tingkat Kesadaran?", "id": "Membuat Pasien Rileks, Mengantuk, Tenang." },
  { "en": "Apa Itu Perdarahan Antar Menstruasi (Metroragia)?", "id": "Pendarahan Rahim Di Luar Siklus." },
  { "en": "Apa Itu Analgesia Penghilang Rasa Sakit?", "id": "Hilangnya Sensasi Nyeri Tanpa Kesadaran." },
  { "en": "Apa Itu Reaksi Leukemoid Respon Imun?", "id": "Peningkatan Sel Darah Putih Reaktif." },
  { "en": "Apa Itu Meningioma Tumor Selaput Otak?", "id": "Tumor Tumbuh Dari Selaput Pelindung." },
  { "en": "Apa Itu Anestesi Spinal Blok Saraf?", "id": "Suntikan Anestesi Ruang Tulang Belakang." },
  { "en": "Apa Itu Dispareunia Nyeri Saat Berhubungan?", "id": "Nyeri Genital Berulang Saat Berhubungan." },
  { "en": "Apa Itu Glasgow Coma Scale (GCS)?", "id": "Skala Menilai Tingkat Kesadaran Seseorang." },
  { "en": "Apa Itu Mielodisplasia Produksi Sel Abnormal?", "id": "Kelainan Sumsum Tulang, Produksi Abnormal." },
  { "en": "Apa Itu Ependimoma Tumor Otak Langka?", "id": "Tumor Langka, Berasal Sel Ependimal." },
  { "en": "Apa Itu Anestesi Epidural Blok Saraf?", "id": "Suntikan Anestesi Ruang Epidural Tulang." },
  { "en": "Apa Itu Vaginismus Kejang Otot Vagina?", "id": "Kejang Otot Vagina, Hubungan Seksual." },
  { "en": "Apa Itu Triage Proses Pemilahan Pasien?", "id": "Pemilahan Pasien Berdasarkan Tingkat Keparahan." },
  { "en": "Apa Itu Koagulopati Konsumtif Penggunaan Berlebih?", "id": "Penggunaan Faktor Pembekuan Berlebihan Darah." },
  { "en": "Apa Itu Medulloblastoma Tumor Otak Anak?", "id": "Tumor Otak Ganas Paling Umum." },
  { "en": "Apa Itu Intubasi Endotrakeal (ETI) Prosedur?", "id": "Memasukkan Tabung Melalui Mulut Trakea." },
  { "en": "Apa Itu Vulvodinia Nyeri Kronis Vulva?", "id": "Nyeri Kronis Area Vulva Wanita." },
  { "en": "Apa Itu Resusitasi Cairan Pemberian Cairan?", "id": "Pemberian Cairan Intravena Mengatasi Syok." },
  { "en": "Apa Itu Krioglobulinemia Protein Abnormal Dingin?", "id": "Adanya Protein Abnormal Darah Menggumpal." },
  { "en": "Apa Itu Oligodendroglioma Tumor Otak Langka?", "id": "Tumor Otak Langka, Sel Oligodendrosit." },
  { "en": "Apa Itu Laryngeal Mask Airway (LMA)?", "id": "Alat Jalan Napas Supraglotis Medis." },
  { "en": "Apa Itu Atrofi Vagina Penipisan Dinding?", "id": "Penipisan, Pengeringan Dinding Vagina Wanita." },
  { "en": "Apa Itu Kardioversi Prosedur Irama Jantung?", "id": "Mengembalikan Irama Jantung Normal Listrik." },
  { "en": "Apa Itu Hiperlipidemia Kadar Lemak Tinggi?", "id": "Tingkat Lemak (Lipid) Tinggi Darah." },
  { "en": "Apa Itu Neuroma Akustik (Schwannoma Vestibular)?", "id": "Tumor Jinak Saraf Vestibulocochlear Telinga." },
  { "en": "Apa Itu Blok Saraf Perifer Anestesi?", "id": "Suntikan Anestesi Dekat Saraf Spesifik." },
  { "en": "Apa Itu Kista Bartholin Kelenjar Bengkak?", "id": "Kantung Berisi Cairan Kelenjar Bartholin." },
  { "en": "Apa Itu Defibrilasi Prosedur Henti Jantung?", "id": "Menggunakan Sengatan Listrik Henti Jantung." },
  { "en": "Apa Itu Disproteinemia Protein Darah Abnormal?", "id": "Kandungan Protein Abnormal Dalam Darah." },
  { "en": "Apa Itu Kraniofaringioma Tumor Otak Anak?", "id": "Jenis Tumor Otak Jinak Langka." },
  { "en": "Apa Itu Anestesi Total Intravena (TIVA)?", "id": "Pemberian Obat Anestesi Melalui Infus." },
  { "en": "Apa Itu Mioma Uteri Tumor Jinak?", "id": "Pertumbuhan Jinak Otot Dinding Rahim." },
  { "en": "Apa Itu Pemasangan Kateter Vena Sentral?", "id": "Memasukkan Kateter Ke Vena Besar." },
  { "en": "Apa Itu Infiltrasi Kanker Penyebaran Lokal?", "id": "Proses Penyebaran Sel Kanker Jaringan." },
  { "en": "Apa Itu Tumor Pineal Tumor Otak?", "id": "Tumor Tumbuh Di Kelenjar Pineal." },
  { "en": "Apa Itu Monitoring Anestesi Pemantauan Pasien?", "id": "Pemantauan Tanda Vital Pasien Anestesi." },
  { "en": "Apa Itu Salpingitis Radang Tuba Falopi?", "id": "Peradangan Pada Saluran Tuba Falopi." },
  { "en": "Apa Itu Pemasangan Selang Nasogastrik (NGT)?", "id": "Memasukkan Selang Plastik Melalui Hidung." },
  { "en": "Apa Itu Asidemia Laktat Peningkatan Asam?", "id": "Peningkatan Asam Laktat Dalam Darah." },
  { "en": "Apa Itu Kista Dermoid Tumor Jinak?", "id": "Tumor Jinak Berisi Jaringan Matang." },
  { "en": "Apa Itu Malignant Hyperthermia Reaksi Anestesi?", "id": "Reaksi Parah Terhadap Obat Anestesi." },
  { "en": "Apa Itu Ooforitis Radang Ovarium Wanita?", "id": "Peradangan Pada Ovarium Atau Indung." },
  { "en": "Apa Itu Parasentesis Prosedur Pengambilan Cairan?", "id": "Mengambil Cairan Dari Rongga Perut." },
  { "en": "Apa Itu Gagal Napas Akut (ARF)?", "id": "Paru-Paru Gagal Pertukarkan Oksigen, Karbon." },
  { "en": "Apa Itu Teratoma Tumor Berisi Jaringan?", "id": "Tumor Berasal Sel Germinal Embrionik." },
  { "en": "Apa Itu Mual Muntah Pasca Operasi?", "id": "Mual, Muntah Terjadi Setelah Pembedahan." },
  { "en": "Apa Itu Kehamilan Anembrionik (Blighted Ovum)?", "id": "Kantong Kehamilan Berkembang Tanpa Embrio." },
  { "en": "Apa Itu Torasentesis Prosedur Pengambilan Cairan?", "id": "Mengambil Cairan Dari Rongga Pleura." },
  { "en": "Apa Itu Saturasi Oksigen (SpO2) Kadar?", "id": "Ukuran Persentase Oksigen Dalam Darah." },
  { "en": "Apa Itu Hemangioblastoma Tumor Pembuluh Darah?", "id": "Tumor Pembuluh Darah Otak, Retina." },
  { "en": "Apa Itu Delirium Pasca Operasi Kebingungan?", "id": "Kebingungan Akut Terjadi Setelah Operasi." },
  { "en": "Apa Itu Kehamilan Mola (Molar Pregnancy)?", "id": "Tumor Jinak Berkembang Di Rahim." },
  { "en": "Apa Itu Artrosentesis Prosedur Cairan Sendi?", "id": "Mengambil Cairan Sinovial Dari Sendi." },
  { "en": "Apa Itu Hiperkapnia Peningkatan Karbon Dioksida?", "id": "Peningkatan Kadar Karbon Dioksida Darah." },
  { "en": "Apa Itu Kordoma Tumor Tulang Langka?", "id": "Jenis Kanker Tulang Langka Primer." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Hati?", "id": "Dokter Spesialis Hepatologi Atau Hepatolog." },
  { "en": "Apa Itu Infertilitas Ketidakmampuan Hamil Kembali?", "id": "Ketidakmampuan Hamil Setelah Satu Tahun." },
  { "en": "Apa Itu Perikardiosentesis Prosedur Cairan Jantung?", "id": "Mengambil Cairan Dari Kantung Perikardial." },
  { "en": "Apa Itu Hipokapnia Penurunan Karbon Dioksida?", "id": "Penurunan Kadar Karbon Dioksida Darah." },
  { "en": "Apa Itu Tumor Sel Germinal Kanker?", "id": "Neoplasma Berasal Dari Sel Germinal." },
  { "en": "Sebutkan Spesialisasi Dokter Penyakit Sendi?", "id": "Dokter Spesialis Reumatologi Atau Reumatolog." },
  { "en": "Apa Itu Kehamilan Serviks Kehamilan Ektopik?", "id": "Kehamilan Ektopik, Embrio Menempel Serviks." },
  { "en": "Apa Itu Lavase Peritoneal Diagnostik (DPL)?", "id": "Prosedur Deteksi Pendarahan Rongga Perut." },
  { "en": "Apa Itu Gagal Hati Akut (ALF)?", "id": "Kehilangan Fungsi Hati Cepat, Parah." },
  { "en": "Apa Itu Kanker Esofagus Kanker Kerongkongan?", "id": "Kanker Tumbuh Di Lapisan Kerongkongan." },
  { "en": "Sebutkan Spesialisasi Dokter Bedah Jantung?", "id": "Dokter Spesialis Bedah Toraks Kardiovaskular." },
  { "en": "Apa Itu Sindrom Asherman Jaringan Parut?", "id": "Jaringan Parut Dalam Rahim, Infertilitas." },
  { "en": "Apa Itu Drainase Abses Prosedur Medis?", "id": "Mengeluarkan Nanah Dari Abses Terinfeksi." },
  { "en": "Apa Itu Sindrom Hepatorenal (HRS) Gagal?", "id": "Gagal Ginjal Progresif Penyakit Hati." },
  { "en": "Apa Itu Kanker Lambung Kanker Perut?", "id": "Pertumbuhan Sel Ganas Lapisan Lambung." },
  { "en": "Sebutkan Spesialisasi Dokter Bedah Plastik?", "id": "Dokter Spesialis Bedah Plastik, Rekonstruksi." },
  { "en": "Apa Itu Hematometra Darah Terkumpul Rahim?", "id": "Kumpulan Darah Dalam Rongga Rahim." },
  { "en": "Apa Itu Debridement Pembersihan Jaringan Luka?", "id": "Menghilangkan Jaringan Mati Dari Luka." },
  { "en": "Apa Itu Penyakit Hati Berlemak Non-Alkoholik?", "id": "Penumpukan Lemak Hati Bukan Alkohol." },
  { "en": "Apa Itu Kanker Kolorektal Kanker Usus?", "id": "Kanker Usus Besar Atau Rektum." },
  { "en": "Sebutkan Spesialisasi Dokter Bedah Anak?", "id": "Dokter Spesialis Bedah Anak Terlatih." },
  { "en": "Apa Itu Hematokolpos Darah Vagina Tertahan?", "id": "Kumpulan Darah Menstruasi Di Vagina." },
  { "en": "Apa Itu Perawatan Luka Lanjutan Medis?", "id": "Manajemen Luka Kompleks, Sulit Sembuh." },
  { "en": "Apa Itu Hepatitis Alkoholik Radang Hati?", "id": "Peradangan Hati Akibat Konsumsi Alkohol." },
  { "en": "Apa Itu Kanker Pankreas Kanker Organ?", "id": "Kanker Mulai Jaringan Organ Pankreas." },
  { "en": "Sebutkan Spesialisasi Dokter Kedokteran Olahraga?", "id": "Dokter Merawat Cedera Terkait Olahraga." },
  { "en": "Apa Itu Adenomiosis Penebalan Dinding Rahim?", "id": "Jaringan Endometrium Tumbuh Dinding Otot." },
  { "en": "Apa Itu Terapi Oksigen Hiperbarik (HBOT)?", "id": "Menghirup Oksigen Murni Ruang Bertekanan." },
  { "en": "Apa Itu Kolangitis Radang Saluran Empedu?", "id": "Peradangan Saluran Empedu, Biasanya Infeksi." },
  { "en": "Apa Itu Kanker Kandung Empedu Kanker?", "id": "Kanker Langka, Sel Abnormal Tumbuh." },
  { "en": "Sebutkan Spesialisasi Dokter Gawat Darurat?", "id": "Dokter Merawat Pasien Gawat Darurat." },
  { "en": "Apa Itu Polip Endometrium Pertumbuhan Jinak?", "id": "Pertumbuhan Jinak Lapisan Dalam Rahim." },
  { "en": "Apa Itu Skin Graft Atau Cangkok Kulit?", "id": "Memindahkan Kulit Sehat Area Terluka." },
  { "en": "Apa Itu Sirosis Bilier Primer Autoimun?", "id": "Penyakit Hati Autoimun, Merusak Saluran." },
  { "en": "Apa Itu Karsinoma Hepatoseluler Kanker Hati?", "id": "Jenis Kanker Hati Primer Paling." },
  { "en": "Sebutkan Spesialisasi Dokter Perawatan Intensif?", "id": "Dokter Merawat Pasien Kritis (ICU)." },
  { "en": "Apa Itu Hiperplasia Endometrium Penebalan Abnormal?", "id": "Penebalan Lapisan Rahim, Pra-Kanker." },
  { "en": "Apa Itu Rekonstruksi Flap Prosedur Bedah?", "id": "Memindahkan Jaringan Hidup Area Lain." },
  { "en": "Apa Itu Kolangitis Sklerosis Primer Radang?", "id": "Penyakit Saluran Empedu, Peradangan, Jaringan." },
  { "en": "Apa Itu Kanker Ginjal Karsinoma Sel?", "id": "Jenis Kanker Ginjal Paling Umum." },
  { "en": "Sebutkan Spesialisasi Dokter Ahli Patologi?", "id": "Dokter Mendiagnosis Penyakit Jaringan, Cairan." },
  { "en": "Apa Itu Salpingo-Ooforektomi Pengangkatan Ovarium Tuba?", "id": "Operasi Pengangkatan Tuba Falopi, Ovarium." },
  { "en": "Apa Itu Ekspansi Jaringan Prosedur Medis?", "id": "Menumbuhkan Kulit Ekstra Untuk Rekonstruksi." },
  { "en": "Apa Itu Sindrom Budd-Chiari Penyumbatan Vena?", "id": "Penyumbatan Vena Hepatik, Darah Hati." },
  { "en": "Apa Itu Kanker Kandung Kemih Karsinoma?", "id": "Kanker Mulai Sel Urotelial Kandung." },
  { "en": "Sebutkan Spesialisasi Dokter Ahli Radiologi?", "id": "Dokter Menginterpretasikan Gambar Medis (Sinar-X)." },
  { "en": "Apa Itu Ligasi Tuba Metode Kontrasepsi?", "id": "Metode Kontrasepsi Permanen, Mengikat Tuba." },
  { "en": "Apa Itu Terapi Vakum Luka (VAC)?", "id": "Menggunakan Tekanan Negatif Percepat Penyembuhan." },
  { "en": "Apa Itu Penyakit Hati Veno-Oklusif (VOD)?", "id": "Penyumbatan Vena Kecil Dalam Hati." },
  { "en": "Apa Itu Kanker Prostat Adenokarsinoma Umum?", "id": "Jenis Kanker Prostat Paling Umum." },
  { "en": "Sebutkan Spesialisasi Dokter Ahli Anestesi?", "id": "Dokter Memberikan Anestesi Selama Operasi." },
  { "en": "Apa Itu Ablasi Endometrium Prosedur Medis?", "id": "Menghancurkan Lapisan Rahim, Kurangi Pendarahan." },
  { "en": "Apa Itu Jahitan Subkutikular Teknik Jahit?", "id": "Teknik Jahit Di Bawah Lapisan." },
  { "en": "Apa Itu Abses Hati Kantung Nanah?", "id": "Kumpulan Nanah Dalam Hati Manusia." },
  { "en": "Apa Itu Kanker Testis Tumor Sel?", "id": "Kanker Testis Paling Sering Terjadi." },
  { "en": "Apa Itu Studi Kasus-Kontrol (Case-Control Study)?", "id": "Studi Membandingkan Pasien Dengan, Tanpa." },
  { "en": "Apa Itu Histerosalpingografi (HSG) Prosedur Rontgen?", "id": "Pemeriksaan Sinar-X Rahim, Tuba Falopi." },
  { "en": "Apa Itu Drainase Luka Prosedur Medis?", "id": "Mengeluarkan Cairan, Darah, Nanah Luka." },
  { "en": "Apa Itu Kista Hati Kantung Cairan?", "id": "Kantung Berdinding Tipis, Berisi Cairan." },
  { "en": "Apa Itu Kanker Serviks Akibat HPV?", "id": "Kanker Leher Rahim, Terkait HPV." },
  { "en": "Apa Itu Studi Kohort (Cohort Study)?", "id": "Studi Mengikuti Kelompok Orang Waktu." },
  { "en": "Apa Itu Sonohisterografi (SHG) Prosedur USG?", "id": "USG Rahim Setelah Diisi Cairan." },
  { "en": "Apa Itu Hemostat Agen Penghenti Pendarahan?", "id": "Agen Digunakan Menghentikan Pendarahan Bedah." },
  { "en": "Apa Itu Adenoma Hepatik Tumor Hati?", "id": "Tumor Hati Jinak, Jarang Terjadi." },
  { "en": "Apa Itu Kanker Ovarium Kanker Ganas?", "id": "Kanker Berasal Dari Sel Ovarium." },
  { "en": "Apa Itu Uji Coba Terkontrol Acak?", "id": "Studi Partisipan Ditempatkan Acak Kelompok." },
  { "en": "Apa Itu Kolposkopi Pemeriksaan Serviks Vagina?", "id": "Pemeriksaan Visual Serviks, Vagina, Vulva." },
  { "en": "Apa Itu Retraktor Alat Bedah Medis?", "id": "Alat Menahan Jaringan, Organ Terbuka." }



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
